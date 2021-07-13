---
title: 拦截器、过滤器、监听器
urlname: gox3pd
date: '2021-07-12 14:14:23 +0800'
author: 阿松
top: true
hide: false
cover: false
toc: false
mathjax: false
summary: 拦截器、过滤器、监听器
categories: java
tags:
  - java
  - 后端
---

# 拦截器、过滤器、监听器

## 1.1 概述

![20180418180158535.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590054217691-7ecffa15-aa48-4848-b4e8-b1eb66f5ca9b.png#height=498&id=piwif&margin=%5Bobject%20Object%5D&name=20180418180158535.png&originHeight=498&originWidth=932&originalType=binary∶=1&size=466400&status=done&style=none&width=932)
![20180418181500252.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590054237195-50935630-873f-4161-a7ae-6dae23a6bedb.png#height=457&id=I5ddU&margin=%5Bobject%20Object%5D&name=20180418181500252.png&originHeight=457&originWidth=535&originalType=binary∶=1&size=8489&status=done&style=none&width=535)
![20180418181054330.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590054250551-1a53a786-9463-46b3-8bc1-aca89fd8eb8b.png#height=460&id=rsK7f&margin=%5Bobject%20Object%5D&name=20180418181054330.png&originHeight=460&originWidth=819&originalType=binary∶=1&size=201705&status=done&style=none&width=819)

## 1.2 过滤器

第一种方式：

先自定义 filter,然后进行注册

```java
public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        // do something 处理request 或response
        System.out.println("filter1");
        // 调用filter链中的下一个filter
        filterChain.doFilter(servletRequest,servletResponse);
    }
    @Override
    public void destroy() {

    }
}
```

```java
@Configuration
public class FilterConfig {

    @Bean
    public FilterRegistrationBean registrationBean() {
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(new MyFilter());
        filterRegistrationBean.addUrlPatterns("/*");
        filterRegistrationBean.setOrder(1);
        return filterRegistrationBean;
    }
}
```

第二种方式**(推荐)**：

```java
// 注入spring容器
@Component
// 定义filterName 和过滤的url
@WebFilter(filterName = "my2Filter" ,urlPatterns = "/*")
@Order(2)
public class My2Filter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("filter2");
    }
    @Override
    public void destroy() {

    }
}
```

## 1.3 拦截器

定义拦截器：

```java
public class MyInterceptor implements HandlerInterceptor{
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        System.out.println("preHandle");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
                           @Nullable ModelAndView modelAndView) {
        System.out.println("postHandle");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
                                @Nullable Exception ex) {
        System.out.println("afterCompletion");
    }
}
```

配置拦截器：

```java
@Configuration
public class InterceptorConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new MyInterceptor());
    }

}
```

## 1.4 监听器

### 1.4.1 SpringBoot 中的监听器

先定义监听器

```
@WebListener
public class ListenerDemo implements ServletContextListener {

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("*********** contextInitialized ***********");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("*********** contextDestroyed ***********");
    }
}
```

需加上 ServletComponentScan 注解

```
@SpringBootApplication
@ServletComponentScan
public class SpringbootSwaggerApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringbootSwaggerApplication.class, args);
	}

}
```

**SpringBoot 中提供哪些监听器？**

1、监听 Servlet 上下文对象 ServletContextListener

2、监听 Http 会话 Session 对象 HttpSessionListener

3、监听客户端请求 Servlet Request 对象 ServletRequestListener

4、监听 Servlet Attribute 对象 ServletContextAttributeListener

5、监听 Request Attribute 对象 ServletRequestAttributeListener

### 1.4.2 自定义监听器

第一步，自定义事件

```java
@Data
public class MyEvent extends ApplicationEvent {

    private String username;

    public MyEvent(Object source, String username) {
        super(source);
        this.username = username;
    }
}
```

第二步，自定义监听器

```java
@Component
public class MyEventListener implements ApplicationListener<MyEvent>  {
    @Override
    public void onApplicationEvent(MyEvent myEvent) {
        // 把事件中的信息获取到
        String username = myEvent.getUsername();
        // 处理事件，实际项目中可以通知别的微服务或者处理其他逻辑等
        System.out.println("用户名：" + username);

    }
}
```

第三步，发布监听事件

```java
@RequestMapping(value = "/testlistener", method= RequestMethod.GET)
public void testListener(){
	applicationContext.publishEvent(new MyEvent(this,"liulijun"));
}
```
