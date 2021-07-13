---
title: SpringBoot Swagger
urlname: yxcv46
date: '2021-07-12 14:11:56 +0800'
tags: []
categories: []
---

## SpringBoot Swagger

## 1.1 集成

第一步，配置 pom.xml

```java
<dependencies>
    <!-- swagger -->
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger2</artifactId>
        <version>2.9.2</version>
    </dependency>
    <!-- swagger-ui -->
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger-ui</artifactId>
        <version>2.9.2</version>
    </dependency>
  </dependencies>
```

第二步，增加配置文件

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket createRestApi() {

        ParameterBuilder ticketPar = new ParameterBuilder();
        List<Parameter> pars = new ArrayList<Parameter>();
        ticketPar.name("Authorization").description("登录校验")
                .modelRef(new ModelRef("string")).parameterType("header")
                .required(false).defaultValue("Bearer ").build(); //header中的ticket参数非必填，传空也可以
        pars.add(ticketPar.build());    //根据每个方法名也知道当前方法在设置什么参数

        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("政企助手后台服务")
            	.globalOperationParameters(pars)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.cloud"))
                .paths(PathSelectors.any())
                .build();
    }

    //构建 api文档的详细信息函数,注意这里的注解引用的是哪个
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Spring Boot 使用 Swagger2 构建RESTful API") 	//页面标题
                .contact(new Contact("刘立俊", "", "")) 				 //创建人
                .version("1.0") 										//版本号
                .description("API 描述") 							 	  //描述
                .build();
    }
}
```

第三步，测试使用

```
@RestController
@Api("测试文档")
public class TestController {

    @RequestMapping(value = "/test", method= RequestMethod.GET)
    @ApiOperation(value = "测试文档", notes="author:llj")
    @ApiImplicitParams({
            @ApiImplicitParam(name="开始时间",value = "startTime"),
            @ApiImplicitParam(name="结束时间",value = "endTime")
    })
    public String test(@RequestParam("startTime") String startTime, @RequestParam("startTime") String endTime){
        return "test";
    }
}
```

第四步，swagger 地址

> [http://localhost:10001/swagger-ui.html](http://localhost:10001/swagger-ui.html)

## 1.2 最佳实践

### 1.2.1 使用 knife4j 优化 UI

传统 UI 过于难看，使用 knife4j 作为 swaggerUI

> 文档地址：[https://doc.xiaominfo.com/guide/useful.html](https://doc.xiaominfo.com/guide/useful.html)

step1 在 pom.xml 中加入依赖

```java
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>2.0.2</version>
</dependency>
```

step2 配置文件

```java
@Configuration
@EnableSwagger2
@EnableKnife4j
@Import(BeanValidatorPluginsConfiguration.class)
public class SwaggerConfig {

    @Bean(value = "defaultApi")
    public Docket defaultApi() {

        ParameterBuilder ticketPar = new ParameterBuilder();
        List<Parameter> pars = new ArrayList<Parameter>();
        ticketPar.name("Authorization").description("登录校验")
                .modelRef(new ModelRef("string")).parameterType("header")
                .required(false).defaultValue("Bearer ").build(); //header中的ticket参数非必填，传空也可以
        pars.add(ticketPar.build());    //根据每个方法名也知道当前方法在设置什么参数

        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("政企助手后台服务")
                .globalOperationParameters(pars)
                .select()
                .apis(RequestHandlerSelectors.basePackage("cn.chinaunicom"))
                .paths(PathSelectors.any())
                .build();
    }
}
```

文档请求地址

> [http://localhost:9081/doc.html](http://localhost:9081/doc.html)

### 1.2.2 生产使用

swaggerUI 还存在其他问题，比如排序、文档分组等问题，最佳实践使用 confluence+swagger,在 confluence 的宏中加入 swagger 的 URL。

> confluence 破解地址：[https://www.jianshu.com/p/8e81caca5f2a](https://www.jianshu.com/p/8e81caca5f2a)
