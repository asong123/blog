---
title: SpringBoot集成Email
urlname: whf77l
date: '2021-07-12 14:14:04 +0800'
tags: []
categories: []
---

# SpringBoot 集成 Email

## 1.1 集成

基础知识

> **什么是 SMTP？**SMTP 全称为 Simple Mail Transfer Protocol（简单邮件传输协议），它是一组用于从源地址到目的地址传输邮件的规范，通过它来控制邮件的中转方式。SMTP 认证要求必须提供账号和密码才能登陆服务器，其设计目的在于避免用户受到垃圾邮件的侵扰。
> **什么是 IMAP？**IMAP 全称为 Internet Message Access Protocol（互联网邮件访问协议），IMAP 允许从邮件服务器上获取邮件的信息、下载邮件等。IMAP 与 POP 类似，都是一种邮件获取协议。
> **什么是 POP3？**POP3 全称为 Post Office Protocol 3（邮局协议），POP3 支持客户端远程管理服务器端的邮件。POP3 常用于“离线”邮件处理，即允许客户端下载服务器邮件，然后服务器上的邮件将会被删除。目前很多 POP3 的邮件服务器只提供下载邮件功能，服务器本身并不删除邮件，这种属于改进版的 POP3 协议。

进阶知识

> **什么是 JavaMailSender 和 JavaMailSenderImpl？**javaMailSender 和 JavaMailSenderImpl 是 Spring 官方提供的集成邮件服务的接口和实现类，以简单高效的设计著称，目前是 Java 后端发送邮件和集成邮件服务的主流工具。

第一步，登录 163 邮箱开启服务，并且通过发送短信获取到一个密码，例如：PWCAXXTUWVRQQSXU ,这个密码是配置在配置文件中的密码，这个密码不是登录密码

![20190408161736164.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590054117696-c2f5b3ae-b2b8-4bd2-9d00-8f55a5eb18bd.png#height=506&id=jNoqn&margin=%5Bobject%20Object%5D&name=20190408161736164.png&originHeight=506&originWidth=801&originalType=binary∶=1&size=44575&status=done&style=none&width=801)

第二步，加入 maven 依赖

```java
<!-- mail -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

第三步，在 application.yml 中配置

```java
spring:
  mail:
    host: smtp.163.com
    username: lijunzhangdale@163.com
    password: PWCAXXTUWVRQQSXU  #这里是授权密码，需要到163上生成，不是自己的邮件登录密码
    default-encoding: UTF-8
```

第四步，定义实体和 service

```java
@Data
public class MailVo {
    private String id;//邮件id
    private String from;//邮件发送人
    private String to;//邮件接收人
    private String subject;//邮件主题
    private String text;//邮件内容
    private Date sentDate;//发送时间
    private String cc; //抄送
    private String bcc; //密送
    private String status; //状态
    private String error; //报错信息
    @JsonIgnore
    private MultipartFile[] multipartFiles;//邮件附件
}
```

```java
@Service
@Slf4j
public class MailServiceImpl {

    @Autowired
    private JavaMailSenderImpl mailSender;//注入邮件工具类

    @Async
    public MailVo sendMail(MailVo mailVo) {
        try {
            checkMail(mailVo); //1.检测邮件
            sendMimeMail(mailVo); //2.发送邮件
            return mailVo;
        } catch (Exception e) {
            log.error(e.getMessage());
            mailVo.setStatus("fail");
            mailVo.setError(e.getMessage());
            return mailVo;
        }
    }

    /**
     * 检测邮件信息类
     * @param mailVo
     */
    private void checkMail(MailVo mailVo) {
        if (StringUtils.isEmpty(mailVo.getTo())) {
            throw new RuntimeException("邮件收信人不能为空");
        }
        if (StringUtils.isEmpty(mailVo.getSubject())) {
            throw new RuntimeException("邮件主题不能为空");
        }
        if (StringUtils.isEmpty(mailVo.getText())) {
            throw new RuntimeException("邮件内容不能为空");
        }
    }

    /**
     * 构建复杂邮件信息类
     * @param mailVo
     */
    private void sendMimeMail(MailVo mailVo) {
        try {
            MimeMessageHelper messageHelper = new MimeMessageHelper(mailSender.createMimeMessage(), true);//true表示支持复杂类型
            messageHelper.setFrom(mailVo.getFrom());//邮件发送人
            messageHelper.setTo(mailVo.getTo().split(","));//邮件收信人
            messageHelper.setSubject(mailVo.getSubject());//邮件主题
            messageHelper.setText(mailVo.getText());//邮件内容
            if (!StringUtils.isEmpty(mailVo.getCc())) {//抄送
                messageHelper.setCc(mailVo.getCc().split(","));
            }
            if (!StringUtils.isEmpty(mailVo.getBcc())) {//密送
                messageHelper.setCc(mailVo.getBcc().split(","));
            }
            if (mailVo.getMultipartFiles() != null) {//添加邮件附件
                for (MultipartFile multipartFile : mailVo.getMultipartFiles()) {
                    messageHelper.addAttachment(Objects.requireNonNull(multipartFile.getOriginalFilename()), multipartFile);
                }
            }
            if (StringUtils.isEmpty(mailVo.getSentDate())) {//发送时间
                mailVo.setSentDate(new Date());
                messageHelper.setSentDate(mailVo.getSentDate());
            }
            mailSender.send(messageHelper.getMimeMessage());//正式发送邮件
            mailVo.setStatus("ok");
        } catch (Exception e) {
            throw new RuntimeException(e);//发送失败
        }
    }

}
```

第五步，测试和调用

```java
@RequestMapping(value = "/testmail", method= RequestMethod.GET)
    public void testMail(){
        //发送邮件
        MailVo mailVo = new MailVo();
        mailVo.setFrom("lijunzhangdale@163.com");
        mailVo.setTo("lijunzhangdale@163.com");
        mailVo.setSubject("收到您朋友llj的信件,Ta的邮箱:" + mailVo.getFrom());
        mailVo.setText("测试邮件");
        mailService.sendMail(mailVo);
    }
```
