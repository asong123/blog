---
title: SpringBoot Validator
urlname: ppvpxk
date: '2021-07-12 14:13:46 +0800'
tags: []
categories: []
abbrlink: 36055
---

# SpringBoot Validator

## 8.1 使用方法

引入依赖,如果引入了 web 模块则不需要引入

```java
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

使用方法，在类上加[**@Validated **](https://www.yuque.com/Validated)\*\* **注解，并且在 controller 方法参数前加[**@Valid **](https://www.yuque.com/Valid)** \*\*注解

注解使用：

> @Null，标注的属性值必须为空
> @NotNull，标注的属性值不能为空
> @AssertTrue，标注的属性值必须为 true
> @AssertFalse，标注的属性值必须为 false
> @Min，标注的属性值不能小于 min 中指定的值
> @Max，标注的属性值不能大于 max 中指定的值
> @DecimalMin，小数值，同上
> @DecimalMax，小数值，同上
> @Negative，负数
> @NegativeOrZero，0 或者负数
> @Positive，整数
> @PositiveOrZero，0 或者整数
> @Size，指定字符串长度，注意是长度，有两个值，min 以及 max，用于指定最小以及最大长度
> @Digits，内容必须是数字
> @Past，时间必须是过去的时间
> @PastOrPresent，过去或者现在的时间
> @Future，将来的时间
> @FutureOrPresent，将来或者现在的时间
> @Pattern，用于指定一个正则表达式
> @NotEmpty，字符串内容非空
> @NotBlank，字符串内容非空且长度大于 0
> @Email，邮箱
> @Range，用于指定数字，注意是数字的范围，有两个值，min 以及 max
> @Length(min, max) 被注解的元素长度必须在指定的范围内

**正则表达式**
![image-20200430143445141.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590054043796-9a3b9750-4d25-4f3b-9187-760985e03aa1.png#height=338&id=MeDz6&margin=%5Bobject%20Object%5D&name=image-20200430143445141.png&originHeight=338&originWidth=976&originalType=binary∶=1&size=34451&status=done&style=none&width=976)

**全局异常捕获处理**

```java
/**
     * 功能描述:  捕获 HttpMediaTypeNotSupportedException 异常，并处理，该异常由验证框架抛出
     * @param ex
     * @return
     */
    @ExceptionHandler(HttpMediaTypeNotSupportedException.class)
    public CommonResp handleHttpMediaTypeNotSupportedException(HttpMediaTypeNotSupportedException ex) {
        log.error("非法的MediaType请求 : ",ex);
        CommonResp errorResp = new CommonResp();
        errorResp.setCode(CodeEnum.EXEC_NOT_SUPPORT_MEDIATYPE_ERROR.getCode());
        errorResp.setMsg(CodeEnum.EXEC_NOT_SUPPORT_MEDIATYPE_ERROR.getMsg());
        return errorResp;
    }

    /**
     * 功能描述:  捕获 HttpRequestMethodNotSupportedException 异常，并处理，该异常由验证框架抛出
     * @param ex
     * @return
     */
    @ExceptionHandler(HttpRequestMethodNotSupportedException.class)
    public CommonResp handleHttpRequestMethodNotSupportedException(HttpRequestMethodNotSupportedException ex) {
        log.error("非法的http method请求 : ",ex);
        CommonResp errorResp = new CommonResp();
        errorResp.setCode(CodeEnum.EXEC_NOT_SUPPORT_HTTPMETHOD_ERROR.getCode());
        errorResp.setMsg(CodeEnum.EXEC_NOT_SUPPORT_HTTPMETHOD_ERROR.getMsg());
        return errorResp;
    }

    /**
     * 功能描述:  捕获 HttpMessageNotReadableException 异常，并处理，该异常由验证框架抛出
     * @param ex
     * @return
     */
    @ExceptionHandler(HttpMessageNotReadableException.class)
    @ResponseBody
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public CommonResp handleHttpMessageNotReadableException(HttpMessageNotReadableException ex) {
        log.error("非法的http请求 : ",ex);
        CommonResp errorResp = new CommonResp();
        errorResp.setCode(CodeEnum.EXEC_NOT_VAILD_HTTP_ERROR.getCode());
        errorResp.setMsg(CodeEnum.EXEC_NOT_VAILD_HTTP_ERROR.getMsg());
        return errorResp;
    }

    /**
     * @NotNull
     * 功能描述:  捕获 MethodArgumentNotValidException 异常，并处理，该异常由验证框架抛出
     * @param ex
     * @return
     */
    @ExceptionHandler(MissingServletRequestParameterException.class)
    @ResponseBody
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public CommonResp missingServletRequestParameterException(MissingServletRequestParameterException ex){
        log.error("参数缺失异常",ex);
        CommonResp errorResp = new CommonResp();
        errorResp.setCode(CodeEnum.EXEC_PARAM_MISS_ERROR.getCode());
        errorResp.setMsg(ex.getParameterName()+" "+CodeEnum.EXEC_PARAM_MISS_ERROR.getMsg());
        return errorResp;
    }

    /**
     * @NotBlank等不符合校验规则异常
     * 功能描述:  捕获 ConstraintViolationException 异常，并处理，该异常由验证框架抛出
     * @param ex
     * @return
     */
    @ExceptionHandler(value = {ConstraintViolationException.class})
    @ResponseBody
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public CommonResp handleResourceNotFoundException(ConstraintViolationException ex) {
        log.error("参数校验异常 : ",ex);
        List<String> errMsgs = Lists.newArrayList();
        Set<ConstraintViolation<?>> violations = ex.getConstraintViolations();
        for (ConstraintViolation<?> violation : violations) {
            errMsgs.add(violation.getMessage());
        }
        CommonResp errorResp = new CommonResp();
        errorResp.setCode(CodeEnum.EXEC_VAILD_ERROR.getCode());
        errorResp.setMsg(CodeEnum.EXEC_VAILD_ERROR.getMsg());
        errorResp.setErrorMsgs(errMsgs);
        return errorResp;
    }

    /**
     * @RequestBody @Vaild entity
     * 功能描述:  捕获 MethodArgumentNotValidException 异常，并处理，该异常由验证框架抛出
     * @param ex
     * @return
     */
    @ExceptionHandler({ MethodArgumentNotValidException.class })
    @ResponseBody
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public CommonResp processArgumentBindException(MethodArgumentNotValidException ex) {
        log.error("表单校验异常",ex);
        BindingResult bindingResult = ex.getBindingResult();
        List<FieldError> fieldError = bindingResult.getFieldErrors();
        CommonResp errorResp = new CommonResp();
        List<CommonResp.ErrorField> errorFields = fieldError.stream().map(e -> {
            CommonResp.ErrorField errorField = new CommonResp.ErrorField();
            errorField.setField(e.getField());
            errorField.setErrMsg(e.getDefaultMessage());
            return errorField;
        }).collect(Collectors.toList());
        errorResp.setCode(CodeEnum.EXEC_FORM_ERROR.getCode());
        errorResp.setMsg(CodeEnum.EXEC_FORM_ERROR.getMsg());
        errorResp.setErrorFields(errorFields);
        return errorResp;
    }
```
