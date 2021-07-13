---
title: 幂等性总结
urlname: qylx1c
date: '2021-07-12 14:17:01 +0800'
tags: []
categories: []
---

# 一、背景

我们实际系统中有很多操作，是不管做多少次，都应该产生一样的效果或返回一样的结果。
例如：

> 1、前端重复提交选中的数据，应该后台只产生对应这个数据的一个反应结果。
> 2、我们发起一笔付款请求，应该只扣用户账户一次钱，当遇到网络重发或系统 bug 重发，也应该只扣一次钱；
> 3、发送消息，也应该只发一次，同样的短信发给用户，用户会哭的；
> 4、创建业务订单，一次业务请求只能创建一个，创建多个就会出大问题。

# 二、幂等性概念

幂等（idempotent、idempotence）是一个数学与计算机学概念，常见于抽象代数中。在编程中.一个幂等操作的特点是其任意多次执行所产生的影响均与一次执行的影响相同。幂等函数，或幂等方法，是指可以使用相同参数重复执行，并能获得相同结果的函数。这些函数不会影响系统状态，也不用担心重复执行会对系统造成改变。例如，“getUsername()和 setTrue()”函数就是一个幂等函数。更复杂的操作幂等保证是利用唯一交易号(流水号)实现.
我的理解：幂等就是一个操作，不论执行多少次，产生的效果和返回的结果都是一样的。

# 三、技术方案

## 3.1 查询操作

查询一次和查询多次，在数据不变的情况下，查询结果是一样的。select 是天然的幂等操作。

## 3.2 **删除操作**

删除操作也是幂等的，删除一次和多次删除都是把数据删除。(注意可能返回结果不一样，删除的数据不存在，返回 0，删除的数据多条，返回结果多个)。

## 3.3 唯一索引

防止新增脏数据，比如：支付宝的资金账户，支付宝也有用户账户，每个用户只能有一个资金账户，怎么防止给用户创建资金账户多个，那么给资金账户表中的用户 ID 加唯一索引，所以一个用户新增成功一个资金账户记录
要点： 唯一索引或唯一组合索引来防止新增数据存在脏数据 （当表存在唯一索引，并发时新增报错时，再查询一次就可以了，数据应该已经存在了，返回结果即可）

## 3.4 **token 机制（防止页面重复提交）**

业务要求：页面的数据只能被点击提交一次
发生原因：由于重复点击或者网络重发，或者 nginx 重发等情况会导致数据被重复提交
解决办法：集群环境：采用 token 加 redis（redis 单线程的，处理需要排队） 单 JVM 环境：采用 token 加 redis 或 token 加 jvm 内存。
处理流程：

> 1. 数据提交前要向服务的申请 token，token 放到 redis 或 jvm 内存，token 有效时间
> 1. 提交后后台校验 token，同时删除 token，生成新的 token 返回

token 特点：要申请，一次有效性，可以限流
注意：redis 要用删除操作来判断 token，删除成功代表 token 校验通过，如果用 select+delete 来校验 token，存在并发问题，不建议使用。

## 3.5 悲观锁（  获取数据的时候加锁获取）

select \* from table_xxx where id='xxx' for update;
注意：id 字段一定是主键或者唯一索引，不然是锁表，会死人的
悲观锁使用时一般伴随事务一起使用，数据锁定时间可能会很长，根据实际情况选用

## 3.6 乐观锁

乐观锁只是在更新数据那一刻锁表，其他时间不锁表，所以相对于悲观锁，效率更高。
乐观锁的实现方式多种多样可以通过 version 或者其他状态条件：

方式 1：通过版本号实现

```
update table_xxx set name=#name#,version=version+1 where version=#version#
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1598604135430-1066d562-9ebd-406f-a0af-53da5888b9e2.png#align=left&display=inline&height=276&id=Mm4V3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=276&originWidth=632&size=69860&status=done&style=none&width=632)
方式 2：通过条件限制

```
update tablexxx set avaiamount=avaiamount-#subAmount# where avaiamount-#subAmount# >= 0
```

要求：quality-#subQuality# >= ，这个情景适合不用版本号，只更新是做数据安全校验，适合库存模型，扣份额和回滚份额，性能更高
注意：乐观锁的更新操作，最好用主键或者唯一索引来更新,这样是行锁，否则更新时会锁表，上面两个 sql 改成下面的两个更好：

```
update tablexxx set name=#name#,version=version+1 where id=#id# and version=#version#
update tablexxx set avaiamount=avaiamount-#subAmount# where id=#id# and avai_amount-#subAmount# >= 0
```

## 3.7 **分布式锁**

还是拿插入数据的例子，如果是分布是系统，构建全局唯一索引比较困难，例如唯一性的字段没法确定，这时候可以引入分布式锁，通过第三方的系统(redis 或 zookeeper)，在业务系统插入数据或者更新数据，获取分布式锁，然后做操作，之后释放锁，这样其实是把多线程并发的锁的思路，引入多多个系统，也就是分布式系统中得解决思路。
要点：某个长流程处理过程要求不能并发执行，可以在流程执行之前根据某个标志(用户 ID+后缀等)获取分布式锁，其他流程执行时获取锁就会失败，也就是同一时间该流程只能有一个能执行成功，执行完成后，释放分布式锁(分布式锁要第三方系统提供)

## 3.8 select + insert

并发不高的后台系统，或者一些任务 JOB，为了支持幂等，支持重复执行，简单的处理方法是，先查询下一些关键数据，判断是否已经执行过，在进行业务处理，就可以了
注意：核心高并发流程不要用这种方法

## 3.9 **状态机幂等**

在设计单据相关的业务，或者是任务相关的业务，肯定会涉及到状态机(状态变更图)，就是业务单据上面有个状态，状态在不同的情况下会发生变更，一般情况下存在有限状态机，如果状态机已经处于下一个状态，这时候来了一个上一个状态的变更，理论上是不能够变更的，这样的话，保证了有限状态机的幂等。
注意：订单等单据类业务，存在很长的状态流转，一定要深刻理解状态机，对业务系统设计能力提高有很大帮助。

## 3.10 对外提供接口的 api 如何保证幂等

如银联提供的付款接口：需要接入商户提交付款请求时附带：source 来源，seq 序列号
source+seq 在数据库里面做唯一索引，防止多次付款，(并发时，只能处理一个请求)
重点对外提供接口为了支持幂等调用，接口有两个字段必须传，一个是来源 source，一个是来源方序列号 seq，这个两个字段在提供方系统里面做联合唯一索引，这样当第三方调用时，先在本方系统里面查询一下，是否已经处理过，返回相应处理结果；没有处理过，进行相应处理，返回结果。
注意，为了幂等友好，一定要先查询一下，是否处理过该笔业务，不查询直接插入业务系统，会报错，但实际已经处理了。

# 四、Token 机制的例子

作用：防止表单重复提交
缺点：性能消耗比较多，需要多一次 http 请求
![image.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1598604549954-14706b39-a977-4387-a8ae-87fc7d5004c9.png#align=left&display=inline&height=376&id=OnXq6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=376&originWidth=704&size=112470&status=done&style=none&width=704)

## 4.1 搭建 redis 的服务 Api

```
/**
 * redis工具类
 */
@Component
public class RedisService {

    @Autowired
    private RedisTemplate redisTemplate;

    /**
     * 写入缓存
     * @param key
     * @param value
     * @return
     */
    public boolean set(final String key, Object value) {
        boolean result = false;
        try {
            ValueOperations<Serializable, Object> operations = redisTemplate.opsForValue();
            operations.set(key, value);
            result = true;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }


    /**
     * 写入缓存设置时效时间
     * @param key
     * @param value
     * @return
     */
    public boolean setEx(final String key, Object value, Long expireTime) {
        boolean result = false;
        try {
            ValueOperations<Serializable, Object> operations = redisTemplate.opsForValue();
            operations.set(key, value);
            redisTemplate.expire(key, expireTime, TimeUnit.SECONDS);
            result = true;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }


    /**
     * 判断缓存中是否有对应的value
     * @param key
     * @return
     */
    public boolean exists(final String key) {
        return redisTemplate.hasKey(key);
    }

    /**
     * 读取缓存
     * @param key
     * @return
     */
    public Object get(final String key) {
        Object result = null;
        ValueOperations<Serializable, Object> operations = redisTemplate.opsForValue();
        result = operations.get(key);
        return result;
    }

    /**
     * 删除对应的value
     * @param key
     */
    public boolean remove(final String key) {
        if (exists(key)) {
            Boolean delete = redisTemplate.delete(key);
            return delete;
        }
        return false;

    }

}
```

## 4.2 自定义注解 AutoIdempotent

自定义一个注解，定义此注解的主要目的是把它添加在需要实现幂等的方法上，凡是某个方法注解了它，都会实现自动幂等。后台利用反射如果扫描到这个注解，就会处理这个方法实现自动幂等，使用元注解 ElementType.METHOD 表示它只能放在方法上，etentionPolicy.RUNTIME 表示它在运行时。

```
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface AutoIdempotent {

}
```

## 4.3 token 创建和检验

### 4.3.1 token 服务接口

我们新建一个接口，创建 token 服务，里面主要是两个方法，一个用来创建 token，一个用来验证 token。创建 token 主要产生的是一个字符串，检验 token 的话主要是传达 request 对象，为什么要传 request 对象呢？主要作用就是获取 header 里面的 token,然后检验，通过抛出的 Exception 来获取具体的报错信息返回给前端。

```
public interface TokenService {

    /**
     * 创建token
     * @return
     */
    public  String createToken();

    /**
     * 检验token
     * @param request
     * @return
     */
    public boolean checkToken(HttpServletRequest request) throws Exception;

}
```

### 4.3.2 token 的服务实现类

token 引用了 redis 服务，创建 token 采用随机算法工具类生成随机 uuid 字符串,然后放入到 redis 中(为了防止数据的冗余保留,这里设置过期时间为 10000 秒,具体可视业务而定)，如果放入成功，最后返回这个 token 值。checkToken 方法就是从 header 中获取 token 到值(如果 header 中拿不到，就从 paramter 中获取)，如若不存在,直接抛出异常。这个异常信息可以被拦截器捕捉到，然后返回给前端。

```
@Service
public class TokenServiceImpl implements TokenService {

    @Autowired
    private RedisService redisService;


    /**
     * 创建token
     *
     * @return
     */
    @Override
    public String createToken() {
        String str = RandomUtil.randomUUID();
        StrBuilder token = new StrBuilder();
        try {
            token.append(Constant.Redis.TOKEN_PREFIX).append(str);
            redisService.setEx(token.toString(), token.toString(),10000L);
            boolean notEmpty = StrUtil.isNotEmpty(token.toString());
            if (notEmpty) {
                return token.toString();
            }
        }catch (Exception ex){
            ex.printStackTrace();
        }
        return null;
    }


    /**
     * 检验token
     *
     * @param request
     * @return
     */
    @Override
    public boolean checkToken(HttpServletRequest request) throws Exception {

        String token = request.getHeader(Constant.TOKEN_NAME);
        if (StrUtil.isBlank(token)) {// header中不存在token
            token = request.getParameter(Constant.TOKEN_NAME);
            if (StrUtil.isBlank(token)) {// parameter中也不存在token
                throw new ServiceException(Constant.ResponseCode.ILLEGAL_ARGUMENT, 100);
            }
        }

        if (!redisService.exists(token)) {
            throw new ServiceException(Constant.ResponseCode.REPETITIVE_OPERATION, 200);
        }

        boolean remove = redisService.remove(token);
        if (!remove) {
            throw new ServiceException(Constant.ResponseCode.REPETITIVE_OPERATION, 200);
        }
        return true;
    }
}
```

## 4.4 拦截器的配置

**1、**web 配置类，实现 WebMvcConfigurerAdapter，主要作用就是添加 autoIdempotentInterceptor 到配置类中，这样我们到拦截器才能生效，注意使用@Configuration 注解，这样在容器启动是时候就可以添加进入 context 中。

```
@Configuration
public class WebConfiguration extends WebMvcConfigurerAdapter {

    @Resource
   private AutoIdempotentInterceptor autoIdempotentInterceptor;

    /**
     * 添加拦截器
     * @param registry
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(autoIdempotentInterceptor);
        super.addInterceptors(registry);
    }
}
```

**2、**拦截处理器：主要的功能是拦截扫描到 AutoIdempotent 到注解到方法,然后调用 tokenService 的 checkToken()方法校验 token 是否正确，如果捕捉到异常就将异常信息渲染成 json 返回给前端。

```
/**
 * 拦截器
 */
@Component
public class AutoIdempotentInterceptor implements HandlerInterceptor {

    @Autowired
    private TokenService tokenService;

    /**
     * 预处理
     *
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        if (!(handler instanceof HandlerMethod)) {
            return true;
        }
        HandlerMethod handlerMethod = (HandlerMethod) handler;
        Method method = handlerMethod.getMethod();
        //被ApiIdempotment标记的扫描
        AutoIdempotent methodAnnotation = method.getAnnotation(AutoIdempotent.class);
        if (methodAnnotation != null) {
            try {
                return tokenService.checkToken(request);// 幂等性校验, 校验通过则放行, 校验失败则抛出异常, 并通过统一异常处理返回友好提示
            }catch (Exception ex){
                ResultVo failedResult = ResultVo.getFailedResult(101, ex.getMessage());
                writeReturnJson(response, JSONUtil.toJsonStr(failedResult));
                throw ex;
            }
        }
        //必须返回true,否则会被拦截一切请求
        return true;
    }


    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }

    /**
     * 返回的json值
     * @param response
     * @param json
     * @throws Exception
     */
    private void writeReturnJson(HttpServletResponse response, String json) throws Exception{
        PrintWriter writer = null;
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html; charset=utf-8");
        try {
            writer = response.getWriter();
            writer.print(json);

        } catch (IOException e) {
        } finally {
            if (writer != null)
                writer.close();
        }
    }

}
```

## 4.5 测试用例

**1、**模拟业务请求类
首先我们需要通过/get/token 路径通过 getToken()方法去获取具体的 token，然后我们调用 testIdempotence 方法，这个方法上面注解了@AutoIdempotent，拦截器会拦截所有的请求，当判断到处理的方法上面有该注解的时候，就会调用 TokenService 中的 checkToken()方法，如果捕获到异常会将异常抛出调用者，下面我们来模拟请求一下：
1）先请求/get/token 获取 token 2) 再请求多次/test/Idempotence

```
@RestController
public class BusinessController {


    @Resource
    private TokenService tokenService;

    @Resource
    private TestService testService;


    @PostMapping("/get/token")
    public String  getToken(){
        String token = tokenService.createToken();
        if (StrUtil.isNotEmpty(token)) {
            ResultVo resultVo = new ResultVo();
            resultVo.setCode(Constant.code_success);
            resultVo.setMessage(Constant.SUCCESS);
            resultVo.setData(token);
            return JSONUtil.toJsonStr(resultVo);
        }
        return StrUtil.EMPTY;
    }


    @AutoIdempotent
    @PostMapping("/test/Idempotence")
    public String testIdempotence() {
        String businessResult = testService.testIdempotence();
        if (StrUtil.isNotEmpty(businessResult)) {
            ResultVo successResult = ResultVo.getSuccessResult(businessResult);
            return JSONUtil.toJsonStr(successResult);
        }
        return StrUtil.EMPTY;
    }
}
```
