---
title: Gateway核心源码分析
urlname: vvp1vn
date: '2021-07-12 14:20:05 +0800'
tags: []
categories: []
---

## 1，源码入口

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=GatewayAutoConfiguration
```

```java
@AutoConfigureAfter({GatewayLoadBalancerClientAutoConfiguration.class, GatewayClassPathWarningAutoConfiguration.class})
public class GatewayAutoConfiguration {

    //查找匹配到Route并进行处理
    @Bean
    public RoutePredicateHandlerMapping routePredicateHandlerMapping(FilteringWebHandler webHandler, RouteLocator routeLocator, GlobalCorsProperties globalCorsProperties, Environment environment) {
        return new RoutePredicateHandlerMapping(webHandler, routeLocator, globalCorsProperties, environment);
    }

    //加载网关配置
    @Bean
    public GatewayProperties gatewayProperties() {
        return new GatewayProperties();
    }

    //创建一个根据RouteDefinition转换的路由定位器
    @Bean
    public RouteLocator routeDefinitionRouteLocator(GatewayProperties properties, List<GatewayFilterFactory> gatewayFilters, List<RoutePredicateFactory> predicates, RouteDefinitionLocator routeDefinitionLocator, ConfigurationService configurationService) {
        return new RouteDefinitionRouteLocator(routeDefinitionLocator, predicates, gatewayFilters, properties, configurationService);
    }
}
```

```java
public class GatewayLoadBalancerClientAutoConfiguration {

    //初始化路由负载均衡过滤器
    //实现了全局过滤器接口 GlobalFilter
    @Bean
    @ConditionalOnBean({LoadBalancerClient.class})
    @ConditionalOnMissingBean({LoadBalancerClientFilter.class, ReactiveLoadBalancerClientFilter.class})
    public LoadBalancerClientFilter loadBalancerClientFilter(LoadBalancerClient client, LoadBalancerProperties properties) {
        return new LoadBalancerClientFilter(client, properties);
    }

}
```

## 2，从请求开始

**DispatcherHandler** 请求的调度器，负责请求分发。

类似 DispatcherServlet 的处理逻辑，exchange 就是 request 请求上下文的封装。

```java
	@Override
	public Mono<Void> handle(ServerWebExchange exchange) {
		if (this.handlerMappings == null) {
			return createNotFoundError();
		}
		if (CorsUtils.isPreFlightRequest(exchange.getRequest())) {
			return handlePreFlight(exchange);
		}
		return Flux.fromIterable(this.handlerMappings)
            	 //获取路由
				.concatMap(mapping -> mapping.getHandler(exchange))
				.next()
				.switchIfEmpty(createNotFoundError())
           		 //代理路由
				.flatMap(handler -> invokeHandler(exchange, handler))
            	//处理结果
				.flatMap(result -> handleResult(exchange, result));
	}
```

```java
	@Override
	public Mono<Object> getHandler(ServerWebExchange exchange) {
        //getHandlerInternal（）
		return getHandlerInternal(exchange).map(handler -> {
			if (logger.isDebugEnabled()) {
				logger.debug(exchange.getLogPrefix() + "Mapped to " + handler);
			}
			ServerHttpRequest request = exchange.getRequest();
			if (hasCorsConfigurationSource(handler) || CorsUtils.isPreFlightRequest(request)) {
				CorsConfiguration config = (this.corsConfigurationSource != null ?
						this.corsConfigurationSource.getCorsConfiguration(exchange) : null);
				CorsConfiguration handlerConfig = getCorsConfiguration(handler, exchange);
				config = (config != null ? config.combine(handlerConfig) : handlerConfig);
				if (config != null) {
					config.validateAllowCredentials();
				}
				if (!this.corsProcessor.process(config, exchange) || CorsUtils.isPreFlightRequest(request)) {
					return NO_OP_HANDLER;
				}
			}
			return handler;
		});
	}
```

```java
protected Mono<?> getHandlerInternal(ServerWebExchange exchange) {
    if (this.managementPortType == RoutePredicateHandlerMapping.ManagementPortType.DIFFERENT && this.managementPort != null && exchange.getRequest().getURI().getPort() == this.managementPort) {
        return Mono.empty();
    } else {
        exchange.getAttributes().put(ServerWebExchangeUtils.GATEWAY_HANDLER_MAPPER_ATTR, this.getSimpleName());
        //lookupRoute（）
        return this.lookupRoute(exchange).flatMap((r) -> {
            exchange.getAttributes().remove(ServerWebExchangeUtils.GATEWAY_PREDICATE_ROUTE_ATTR);
            if (this.logger.isDebugEnabled()) {
                this.logger.debug("Mapping [" + this.getExchangeDesc(exchange) + "] to " + r);
            }

            exchange.getAttributes().put(ServerWebExchangeUtils.GATEWAY_ROUTE_ATTR, r);

            return Mono.just(this.webHandler);
        }).switchIfEmpty(Mono.empty().then(Mono.fromRunnable(() -> {
            exchange.getAttributes().remove(ServerWebExchangeUtils.GATEWAY_PREDICATE_ROUTE_ATTR);
            if (this.logger.isTraceEnabled()) {
                this.logger.trace("No RouteDefinition found for [" + this.getExchangeDesc(exchange) + "]");
            }

        })));
    }
}
```

```java
protected Mono<Route> lookupRoute(ServerWebExchange exchange) {
    //routeLocator.getRoutes()
    return this.routeLocator.getRoutes().concatMap((route) -> {
        //根据请求url和路由渭词条件匹配路由route
        return Mono.just(route).filterWhen((r) -> {
            //找到路由信息设置到请求的上下文环境中
            exchange.getAttributes().put(ServerWebExchangeUtils.GATEWAY_PREDICATE_ROUTE_ATTR, r.getId());
            return (Publisher)r.getPredicate().apply(exchange);
        }).doOnError((e) -> {
            this.logger.error("Error applying predicate for route: " + route.getId(), e);
        }).onErrorResume((e) -> {
            return Mono.empty();
        });
    }).next().map((route) -> {
        if (this.logger.isDebugEnabled()) {
            this.logger.debug("Route matched: " + route.getId());
        }

        this.validateRoute(route, exchange);
        return route;
    });
}
```

## 3，获取路由

```java
@Override
public Flux<Route> getRoutes() {
    //用stream流的方式将getRouteDefinitions转换为Route
   Flux<Route> routes = this.routeDefinitionLocator.getRouteDefinitions()
       	//转换逻辑
         .map(this::convertToRoute);

   if (!gatewayProperties.isFailOnRouteDefinitionError()) {
      // instead of letting error bubble up, continue
      routes = routes.onErrorContinue((error, obj) -> {
         if (logger.isWarnEnabled()) {
            logger.warn("RouteDefinition id " + ((RouteDefinition) obj).getId()
                  + " will be ignored. Definition has invalid configs, "
                  + error.getMessage());
         }
      });
   }

   return routes.map(route -> {
      if (logger.isDebugEnabled()) {
         logger.debug("RouteDefinition matched: " + route.getId());
      }
      return route;
   });
}
```

```java
private Route convertToRoute(RouteDefinition routeDefinition) {
    //从routeDefinition获取Predicates
   AsyncPredicate<ServerWebExchange> predicate = combinePredicates(routeDefinition);
    //从routeDefinition获取filters
   List<GatewayFilter> gatewayFilters = getFilters(routeDefinition);
	//将routeDefinition彻底转化为route
   return Route.async(routeDefinition).asyncPredicate(predicate)
         .replaceFilters(gatewayFilters).build();
}
```

### 1）获取 Predicates

```java
	private AsyncPredicate<ServerWebExchange> combinePredicates(
			RouteDefinition routeDefinition) {
		List<PredicateDefinition> predicates = routeDefinition.getPredicates();
		AsyncPredicate<ServerWebExchange> predicate = lookup(routeDefinition,
				predicates.get(0));

		for (PredicateDefinition andPredicate : predicates.subList(1,
				predicates.size())) {
			AsyncPredicate<ServerWebExchange> found = lookup(routeDefinition,
					andPredicate);
			predicate = predicate.and(found);
		}

		return predicate;
	}
```

### 2）获取 filters

```java
private List<GatewayFilter> getFilters(RouteDefinition routeDefinition) {
   List<GatewayFilter> filters = new ArrayList<>();


   if (!this.gatewayProperties.getDefaultFilters().isEmpty()) {
      filters.addAll(loadGatewayFilters(DEFAULT_FILTERS,
            this.gatewayProperties.getDefaultFilters()));
   }

   if (!routeDefinition.getFilters().isEmpty()) {
      filters.addAll(loadGatewayFilters(routeDefinition.getId(),
            routeDefinition.getFilters()));
   }

   AnnotationAwareOrderComparator.sort(filters);
   return filters;
}
```

### 3）彻底转化为 route

```java
return Route.async(routeDefinition).asyncPredicate(predicate)
         .replaceFilters(gatewayFilters).build();
```

## 4，代理路由

```java
private Mono<HandlerResult> invokeHandler(ServerWebExchange exchange, Object handler) {
   if (ObjectUtils.nullSafeEquals(exchange.getResponse().getStatusCode(), HttpStatus.FORBIDDEN)) {
      return Mono.empty();  // CORS rejection
   }
   if (this.handlerAdapters != null) {
      for (HandlerAdapter handlerAdapter : this.handlerAdapters) {
         if (handlerAdapter.supports(handler)) {
             //真正的处理逻辑
            return handlerAdapter.handle(exchange, handler);
         }
      }
   }
   return Mono.error(new IllegalStateException("No HandlerAdapter: " + handler));
}
```

```java
public Mono<HandlerResult> handle(ServerWebExchange exchange, Object handler) {
   WebHandler webHandler = (WebHandler) handler;
   Mono<Void> mono = webHandler.handle(exchange);
   return mono.then(Mono.empty());
}
```

```java
public Mono<Void> handle(ServerWebExchange exchange) {

   Route route = exchange.getRequiredAttribute(GATEWAY_ROUTE_ATTR);
   List<GatewayFilter> gatewayFilters = route.getFilters();

   List<GatewayFilter> combined = new ArrayList<>(this.globalFilters);
    //添加配置的过滤器和全局过滤器到filter链
   combined.addAll(gatewayFilters);

   AnnotationAwareOrderComparator.sort(combined);

   if (logger.isDebugEnabled()) {
      logger.debug("Sorted gatewayFilterFactories: " + combined);
   }

   //执行过滤器链
   return new DefaultGatewayFilterChain(combined).filter(exchange);
}
```

## 5，执行过滤器链

```java
    public Mono<Void> filter(ServerWebExchange exchange) {
      return Mono.defer(() -> {
         if (this.index < filters.size()) {
            GatewayFilter filter = filters.get(this.index);
            DefaultGatewayFilterChain chain = new DefaultGatewayFilterChain(this,
                  this.index + 1);
             //过滤逻辑
            return filter.filter(exchange, chain);
         }
         else {
            return Mono.empty(); // complete
         }
      });
   }

}
```

```java
public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
   return this.delegate.filter(exchange, chain);
}
```

```java
public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
   URI url = exchange.getAttribute(GATEWAY_REQUEST_URL_ATTR);
   String schemePrefix = exchange.getAttribute(GATEWAY_SCHEME_PREFIX_ATTR);
   if (url == null
         || (!"lb".equals(url.getScheme()) && !"lb".equals(schemePrefix))) {
      return chain.filter(exchange);
   }
   // preserve the original url
   addOriginalRequestUrl(exchange, url);

   if (log.isTraceEnabled()) {
      log.trace("LoadBalancerClientFilter url before: " + url);
   }
   //通过ribbon去nacos里找服务名对应的实例，并用负载均衡算法选出一台实例机器调用
   final ServiceInstance instance = choose(exchange);

   if (instance == null) {
      throw NotFoundException.create(properties.isUse404(),
            "Unable to find instance for " + url.getHost());
   }

   URI uri = exchange.getRequest().getURI();

   // if the `lb:<scheme>` mechanism was used, use `<scheme>` as the default,
   // if the loadbalancer doesn't provide one.
   String overrideScheme = instance.isSecure() ? "https" : "http";
   if (schemePrefix != null) {
      overrideScheme = url.getScheme();
   }

   URI requestUrl = loadBalancer.reconstructURI(
         new DelegatingServiceInstance(instance, overrideScheme), uri);

   if (log.isTraceEnabled()) {
      log.trace("LoadBalancerClientFilter url chosen: " + requestUrl);
   }

   exchange.getAttributes().put(GATEWAY_REQUEST_URL_ATTR, requestUrl);
    //继续执行调用链
   return chain.filter(exchange);
}
```

### 1）获取 server

```java
public ServiceInstance choose(String serviceId, Object hint) {
    //获取server
   Server server = getServer(getLoadBalancer(serviceId), hint);
   if (server == null) {
      return null;
   }
   return new RibbonServer(serviceId, server, isSecure(server, serviceId),
         serverIntrospector(serviceId).getMetadata(server));
}
```

### 2）继续执行调用链

```
 return chain.filter(exchange);
```

最终使用 httpclient 发送请求到下游实例。

## 6，总结

1，配置文件加载组件

2，请求过来之后，经过 DispatcherHandler 的 handler 方法

3，先去获取处理器，根据请求 url 和路由条件匹配 Route，并将路由信息设置到请求的上下文环境。

4，获取到 handler 之后，对 handler 进行代理,。

5，加载所有的过滤器组成一条过滤器链，

6，执行过滤器链，通过 ribbon 去 nacos 找到一台服务实例，然后执行全局过滤器的 NettyRoutingFilter 的过滤逻辑，并使用 httpclient 发送请求到下游的微服务实例。
