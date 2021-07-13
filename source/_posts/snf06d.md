---
title: 框架设计
urlname: snf06d
date: '2021-07-12 14:19:33 +0800'
tags: []
categories: []
---

## 一，细节

### 1.防止空指针和下标越界

这是一个编写健壮程序的开发人员，在写每一行代码都应在潜意识中防止的异常。基本上要能确保每一次写完的代码，在不测试的情况下，都不会出现这两个异常才算合格。

### 2.保证线程安全性和可见性

对于开发人员，对线程安全性和可见性的深入理解是最基本的要求。需要开发人员，在写每一行代码时都应在潜意识中确保其正确性。因为这种代码，在小并发下做功能测试时，会显得很正常。但在高并发下就会出现莫明其妙的问题，而且场景很难重现，极难排查。

### 3.尽早失败和前置断言

尽早失败也应该成为潜意识，在有传入参数和状态变化时，均在入口处全部断言。一个不合法的值和状态，在第一时间就应报错，而不是等到要用时才报错。因为等到要用时，可能前面已经修改其它相关状态，而在程序中很少有人去处理回滚逻辑。这样报错后，其实内部状态可能已经混乱，极易在一个隐蔽分支上引发程序不可恢复。

### 4.分离可靠操作和不可靠操作

这里的可靠是狭义的指是否会抛出异常或引起状态不一致，比如，写入一个线程安全的 Map，可以认为是可靠的，而写入数据库等，可以认为是不可靠的。开发人员必须在写每一行代码时，都注意它的可靠性与否，在代码中尽量划分开，并对失败做异常处理，并为容错，自我保护，自动恢复或切换等补偿逻辑提供清晰的切入点，保证后续增加的代码不至于放错位置，而导致原先的容错处理陷入混乱。

### 5.异常防御，但不忽略异常

这里讲的异常防御，指的是对非必须途径上的代码进行最大限度的容忍，包括程序上的 BUG，比如：获取程序的版本号，会通过扫描 Manifest 和 jar 包名称抓取版本号，这个逻辑是辅助性的，但代码却不少，初步测试也没啥问题，但应该在整个 getVersion() 中加上一个全函数的 try-catch 打印错误日志，并返回基本版本，因为 getVersion() 可能存在未知特定场景异常，或被其他的开发人员误修改逻辑(但一般人员不会去掉 try-catch)，而如果它抛出异常会导致主流程异常，这是我们不希望看到的。但这里要控制个度，不要随意 try-catch，更不要无声无息的吃掉异常。

### 6.缩小可变域和尽量 final

如果一个类可以成为不变类(Immutable Class)，就优先将它设计成不变类。不变类有天然的并发共享优势，减少同步或复制，而且可以有效帮忙分析线程安全的范围。就算是可变类，对于从构造函数传入的引用，在类中持有时，最好将字段 final，以免被中途误修改引用。不要以为这个字段是私有的，这个类的代码都是我自己写的，不会出现对这个字段的重新赋值。要考虑的一个因素是，这个代码可能被其他人修改，他不知道你的这个弱约定，final 就是一个不变契约。

### 7.降低修改时的误解性，不埋雷

前面不停的提到代码被其他人修改，这也开发人员要随时紧记的。这个其他人包括未来的自己，你要总想着这个代码可能会有人去改它。我应该给修改的人一点什么提示，让他知道我现在的设计意图，而不要在程序里面加潜规则，或埋一些容易忽视的雷，比如：你用 null 表示不可用，size 等于 0 表示黑名单，这就是一个雷，下一个修改者，包括你自己，都不会记得有这样的约定，可能后面为了改某个其它 BUG，不小心改到了这里，直接引爆故障。对于这个例子，一个原则就是永远不要区分 null 引用和 empty 值。

### 8.提高代码的可测性

这里的可测性主要指 Mock 的容易程度，和测试的隔离性。至于测试的自动性，可重复性，非偶然性，无序性，完备性(全覆盖)，轻量性(可快速执行)，一般开发人员，加上 JUnit 等工具的辅助基本都能做到，也能理解它的好处，只是工作量问题。这里要特别强调的是测试用例的单一性(只测目标类本身)和隔离性(不传染失败)。现在的测试代码，过于强调完备性，大量重复交叉测试，看起来没啥坏处，但测试代码越多，维护代价越高。经常出现的问题是，修改一行代码或加一个判断条件，引起 100 多个测试用例不通过。时间一紧，谁有这个闲功夫去改这么多形态各异的测试用例？久而久之，这个测试代码就已经不能真实反应代码现在的状况，很多时候会被迫绕过。最好的情况是，修改一行代码，有且只有一行测试代码不通过。如果修改了代码而测试用例还能通过，那也不行，表示测试没有覆盖到。另外，可 Mock 性是隔离的基础，把间接依赖的逻辑屏蔽掉。可 Mock 性的一个最大的杀手就是静态方法，尽量少用。

## 二，框架设计常识

### 1.API 与 SPI 分离

框架或组件通常有两类客户，一个是使用者，一个是扩展者。API (Application Programming Interface) 是给使用者用的，而 SPI (Service Provide Interface) 是给扩展者用的。在设计时，尽量把它们隔离开，而不要混在一起。也就是说，使用者是看不到扩展者写的实现的。

比如：一个 Web 框架，它有一个 API 接口叫 Action，里面有个 execute() 方法，是给使用者用来写业务逻辑的。然后，Web 框架有一个 SPI 接口给扩展者控制输出方式，比如用 velocity 模板输出还是用 json 输出等。如果这个 Web 框架使用一个都继承 Action 的 VelocityAction 和一个 JsonAction 做为扩展方式，要用 velocity 模板输出的就继承 VelocityAction，要用 json 输出的就继承 JsonAction，这就是 API 和 SPI 没有分离的反面例子，SPI 接口混在了 API 接口中。

### 2.服务域/实体域/会话域分离

任何框架或组件，总会有核心领域模型，比如：Spring 的 Bean，Struts 的 Action，Dubbo 的 Service，Napoli 的 Queue 等等。这个核心领域模型及其组成部分称为实体域，它代表着我们要操作的目标本身。实体域通常是线程安全的，不管是通过不变类，同步状态，或复制的方式。

服务域也就是行为域，它是组件的功能集，同时也负责实体域和会话域的生命周期管理， 比如 Spring 的 ApplicationContext，Dubbo 的 ServiceManager 等。服务域的对象通常会比较重，而且是线程安全的，并以单一实例服务于所有调用。

什么是会话？就是一次交互过程。会话中重要的概念是上下文，什么是上下文？比如我们说：“老地方见”，这里的“老地方”就是上下文信息。为什么说“老地方”对方会知道，因为我们前面定义了“老地方”的具体内容。所以说，上下文通常持有交互过程中的状态变量等。会话对象通常较轻，每次请求都重新创建实例，请求结束后销毁。简而言之：把元信息交由实体域持有，把一次请求中的临时状态由会话域持有，由服务域贯穿整个过程。

### 3.在重要的过程上设置拦截接口

如果你要写个远程调用框架，那远程调用的过程应该有一个统一的拦截接口。如果你要写一个 ORM 框架，那至少 SQL 的执行过程，Mapping 过程要有拦截接口；如果你要写一个 Web 框架，那请求的执行过程应该要有拦截接口，等等。没有哪个公用的框架可以 Cover 住所有需求，允许外置行为，是框架的基本扩展方式。这样，如果有人想在远程调用前，验证下令牌，验证下黑白名单，统计下日志；如果有人想在 SQL 执行前加下分页包装，做下数据权限控制，统计下 SQL 执行时间；如果有人想在请求执行前检查下角色，包装下输入输出流，统计下请求量，等等，就可以自行完成，而不用侵入框架内部。拦截接口，通常是把过程本身用一个对象封装起来，传给拦截器链，比如：远程调用主过程为 invoke()，那拦截器接口通常为 invoke(Invocation)，Invocation 对象封装了本来要执行过程的上下文，并且 Invocation 里有一个 invoke() 方法，由拦截器决定什么时候执行，同时，Invocation 也代表拦截器行为本身，这样上一拦截器的 Invocation 其实是包装的下一拦截器的过程，直到最后一个拦截器的 Invocation 是包装的最终的 invoke() 过程；同理，SQL 主过程为 execute()，那拦截器接口通常为 execute(Execution)，原理一样。当然，实现方式可以任意，上面只是举例。

### 4.重要的状态的变更发送事件并留出监听接口

这里先要讲一个事件和上面拦截器的区别，拦截器是干预过程的，它是过程的一部分，是基于过程行为的，而事件是基于状态数据的，任何行为改变的相同状态，对事件应该是一致的。事件通常是事后通知，是一个 Callback 接口，方法名通常是过去式的，比如 onChanged()。比如远程调用框架，当网络断开或连上应该发出一个事件，当出现错误也可以考虑发出一个事件，这样外围应用就有可能观察到框架内部的变化，做相应适应。

### 5.扩展接口职责尽可能单一，具有可组合性

比如，远程调用框架它的协议是可以替换的。如果只提供一个总的扩展接口，当然可以做到切换协议，但协议支持是可以细分为底层通讯，序列化，动态代理方式等等。如果将接口拆细，正交分解，会更便于扩展者复用已有逻辑，而只是替换某部分实现策略。当然这个分解的粒度需要把握好。

### 6.微核插件式，平等对待第三方

大凡发展的比较好的框架，都遵守微核的理念。Eclipse 的微核是 OSGi， Spring 的微核是 BeanFactory，Maven 的微核是 Plexus。通常核心是不应该带有功能性的，而是一个生命周期和集成容器，这样各功能可以通过相同的方式交互及扩展，并且任何功能都可以被替换。如果做不到微核，至少要平等对待第三方，即原作者能实现的功能，扩展者应该可以通过扩展的方式全部做到。原作者要把自己也当作扩展者，这样才能保证框架的可持续性及由内向外的稳定性。

### 7.不要控制外部对象的生命周期

比如上面说的 Action 使用接口和 Renderer 扩展接口。框架如果让使用者或扩展者把 Action 或 Renderer 实现类的类名或类元信息报上来，然后在内部通过反射 newInstance() 创建一个实例，这样框架就控制了 Action 或 Renderer 实现类的生命周期，Action 或 Renderer 的生老病死，框架都自己做了，外部扩展或集成都无能为力。好的办法是让使用者或扩展者把 Action 或 Renderer 实现类的实例报上来，框架只是使用这些实例，这些对象是怎么创建的，怎么销毁的，都和框架无关，框架最多提供工具类辅助管理，而不是绝对控制。

### 8.可配置一定可编程，并保持友好的 CoC 约定

因为使用环境的不确定因素很多，框架总会有一些配置，一般都会到 classpath 直扫某个指定名称的配置，或者启动时允许指定配置路径。做为一个通用框架，应该做到凡是能配置文件做的一定要能通过编程方式进行，否则当使用者需要将你的框架与另一个框架集成时就会带来很多不必要的麻烦。

另外，尽可能做一个标准约定，如果用户按某种约定做事时，就不需要该配置项。比如：配置模板位置，你可以约定，如果放在 templates 目录下就不用配了，如果你想换个目录，就配置下。

### 9.区分命令与查询，明确前置条件与后置条件

这个是契约式设计的一部分，尽量遵守有返回值的方法是查询方法，void 返回的方法是命令。查询方法通常是幂等性的，无副作用的，也就是不改变任何状态，调 n 次结果都是一样的，比如 get 某个属性值，或查询一条数据库记录。命令是指有副作用的，也就是会修改状态，比如 set 某个值，或 update 某条数据库记录。如果你的方法即做了修改状态的操作，又做了查询返回，如果可能，将其拆成写读分离的两个方法，比如：User deleteUser(id)，删除用户并返回被删除的用户，考虑改为 getUser() 和 void 的 deleteUser()。 另外，每个方法都尽量前置断言传入参数的合法性，后置断言返回结果的合法性，并文档化。

### 10.增量式扩展，而不要扩充原始核心概念

我们平台的产品越来越多，产品的功能也越来越多。平台的产品为了适应各 BU 和部门以及产品线的需求，势必会将很多不相干的功能凑在一起，客户可以选择性的使用。为了兼容更多的需求，每个产品，每个框架，都在不停的扩展，而我们经常会选择一些扩展的扩展方式，也就是将新旧功能扩展成一个通用实现。我想讨论是，有些情况下也可以考虑增量式的扩展方式，也就是保留原功能的简单性，新功能独立实现。我最近一直做分布式服务框架的开发，就拿我们项目中的问题开涮吧。

比如：远程调用框架，肯定少不了序列化功能，功能很简单，就是把流转成对象，对象转成流。但因有些地方可能会使用 osgi，这样序列化时，IO 所在的 ClassLoader 可能和业务方的 ClassLoader 是隔离的。需要将流转换成 byte[] 数组，然后传给业务方的 ClassLoader 进行序列化。为了适应 osgi 需求，把原来非 osgi 与 osgi 的场景扩展了一下，这样，不管是不是 osgi 环境，都先将流转成 byte[] 数组，拷贝一次。然而，大部分场景都用不上 osgi，却为 osgi 付出了代价。而如果采用增量式扩展方式，非 osgi 的代码原封不动，再加一个 osgi 的实现，要用 osgi 的时候，直接依赖 osgi 实现即可。

再比如：最开始，远程服务都是基于接口方法，进行透明化调用的。这样，扩展接口就是， invoke(Method method, Object[] args)，后来，有了无接口调用的需求，就是没有接口方法也能调用，并将 POJO 对象都转换成 Map 表示。因为 Method 对象是不能直接 new 出来的，我们不自觉选了一个扩展式扩展，把扩展接口改成了 invoke(String methodName, String[] parameterTypes, String returnTypes, Object[] args)，导致不管是不是无接口调用，都得把 parameterTypes 从 Class[] 转成 String[]。如果选用增量式扩展，应该是保持原有接口不变，增加一个 GeneralService 接口，里面有一个通用的 invoke() 方法，和其它正常业务上的接口一样的调用方式，扩展接口也不用变，只是 GeneralServiceImpl 的 invoke() 实现会将收到的调用转给目标接口，这样就能将新功能增量到旧功能上，并保持原来结构的简单性。

再再比如：无状态消息发送，很简单，序列化一个对象发过去就行。后来有了同步消息发送需求，需要一个 Request/Response 进行配对，采用扩展式扩展，自然想到，无状态消息其实是一个没有 Response 的 Request，所以在 Request 里加一个 boolean 状态，表示要不要返回 Response。如果再来一个会话消息发送需求，那就再加一个 Session 交互，然后发现，原来同步消息发送是会话消息的一种特殊情况，所有场景都传 Session，不需要 Session 的地方无视即可。

如果采用增量式扩展，无状态消息发送原封不动，同步消息发送，在无状态消息基础上加一个 Request/Response 处理，会话消息发送，再加一个 SessionRequest/SessionResponse 处理。

## 三，稳定性

### 1.日志

日志是发现问题、查看问题一个最常用的手段。日志质量往往被忽视，没有日志使用上的明确约定。重视 Log 的使用，提高 Log 的信息浓度。日志过多、过于混乱，会导致有用的信息被淹没。

要有效利用这个工具要注意：

#### 1)严格约定 WARN、ERROR 级别记录的内容

WARN 表示可以恢复的问题，无需人工介入。

ERROR 表示需要人工介入问题。

有了这样的约定，监管系统发现日志文件的中出现 ERROR 字串就报警，又尽量减少了发生。过多的报警会让人疲倦，使人对报警失去警惕性，使 ERROR 日志失去意义。再辅以人工定期查看 WARN 级别信息，以评估系统的“亚健康”程度。

#### 2)日志中，尽量多的收集关键信息

哪些是关键信息呢？

出问题时的现场信息，即排查问题要用到的信息。如服务调用失败时，要给出使用 Dubbo 的版本、服务提供者的 IP、使用的是哪个注册中心；调用的是哪个服务、哪个方法等等。这些信息如果不给出，那么事后人工收集的，问题过后现场可能已经不能复原，加大排查问题的难度。
如果可能，给出问题的原因和解决方法。这让维护和问题解决变得简单，而不是寻求精通者（往往是实现者）的帮助。

#### 3)同一个或是一类问题不要重复记录多次

同一个或是一类异常日志连续出现几十遍的情况，还是常常能看到的。人眼很容易漏掉淹没在其中不一样的重要日志信息。要尽量避免这种情况。在可以预见会出现的情况，有必要加一些逻辑来避免。

如为一个问题准备一个标志，出问题后打日志后设置标志，避免重复打日志。问题恢复后清除标志。

虽然有点麻烦，但是这样做保证日志信息浓度，让监控更有效。

### 2.界限设置

资源是有限的，CPU、内存、IO 等等。不要因为外部的请求、数据不受限的而崩溃。

#### 1)线程池(ExectorService)的大小和饱和策略

Server 端用于处理请求的 ExectorService 设置上限。ExecutorService 的任务等待队列使用有限队列，避免资源耗尽。当任务等待队列饱和时，选择一个合适的饱和策略。这样保证平滑劣化。

在 Dubbo 中，饱和策略是丢弃数据，等待结果也只是请求的超时。

达到饱和时，说明已经达到服务提供方的负荷上限，要在饱和策略的操作中日志记录这个问题，以发出监控警报。记得注意不要重复多次记录哦。（注意，缺省的饱和策略不会有这些附加的操作。）根据警报的频率，已经决定扩容调整等等，避免系统问题被忽略。

#### 2)集合容量

如果确保进入集合的元素是可控的且是足够少，则可以放心使用。这是大部分的情况。如果不能保证，则使用有有界的集合。当到达界限时，选择一个合适的丢弃策略。

### 3.容错-重试-恢复

高可用组件要容忍其依赖组件的失败。

## 四，框架拍错

### 1.检查重复的 jar 包

多个版本的相同 jar 包，会出现新版本的 A 类，调用了旧版本的 B 类，而且和 JVM 加载顺序有关，问题带有偶然性，误导性，遇到这种莫名其妙的问题，最头疼，所以，第一条，先把它防住

### 2.检查重复的配置文件

配置文件加载错，也是经常碰到的问题。很多产品都会在 classpath 下放一个约定的配置，如果项目中有多个，通常会取 JVM 加载的第一个

### 3.检查所有可选配置

必填配置估计大家都会检查，因为没有的话，根本没法运行。但对一些可选参数，也应该做一些检查，比如：服务框架允许通过注册中心关联服务消费者和服务提供者，也允许直接配置服务提供者地址点对点直连，这时候，注册中心地址是可选的，但如果没有配点对点直连配置，注册中心地址就一定要配，这时候也要做相应检查。

### 4.异常信息给出解决方案

异常最基本要带有上下文信息，包括操作者，操作目标，原因等，最好的异常信息，应给出解决方案

### 5.日志信息包含环境信息

日志中最好把需要的环境信息一并打进去，最好给日志输出做个包装，统一处理掉，免得忘了。

**获取版本号工具类**

```java
public final class Version {

    private Version() {}

    private static final Logger logger = LoggerFactory.getLogger(Version.class);

    private static final Pattern VERSION_PATTERN = Pattern.compile("([0-9][0-9\\.\\-]*)\\.jar");

    private static final String VERSION = getVersion(Version.class, "2.0.0");

    public static String getVersion(){
        return VERSION;
    }

    public static String getVersion(Class cls, String defaultVersion) {
        try {
            // 首先查找MANIFEST.MF规范中的版本号
            String version = cls.getPackage().getImplementationVersion();
            if (version == null || version.length() == 0) {
                version = cls.getPackage().getSpecificationVersion();
            }
            if (version == null || version.length() == 0) {
                // 如果MANIFEST.MF规范中没有版本号，基于jar包名获取版本号
                String file = cls.getProtectionDomain().getCodeSource().getLocation().getFile();
                if (file != null && file.length() > 0 && file.endsWith(".jar")) {
                    Matcher matcher = VERSION_PATTERN.matcher(file);
                    while (matcher.find() && matcher.groupCount() > 0) {
                        version = matcher.group(1);
                    }
                }
            }
            // 返回版本号，如果为空返回缺省版本号
            return version == null || version.length() == 0 ? defaultVersion : version;
        } catch (Throwable e) { // 防御性容错
            // 忽略异常，返回缺省版本号
            logger.error(e.getMessage(), e);
            return defaultVersion;
        }
    }

}
```

### 6.kill 之前先 dump

每次线上环境一出问题，大家就慌了，通常最直接的办法回滚重启，以减少故障时间，这样现场就被破坏了，要想事后查问题就麻烦了，有些问题必须在线上的大压力下才会发生，线下测试环境很难重现，不太可能让开发或 Appops 在重启前，先手工将出错现场所有数据备份一下，所以最好在 kill 脚本之前调用 dump，进行自动备份，这样就不会有人为疏忽。

**dump 脚本**

```shell
JAVA_HOME=/usr/java
OUTPUT_HOME=~/output
DEPLOY_HOME=`dirname $0`
HOST_NAME=`hostname`

DUMP_PIDS=`ps  --no-heading -C java -f --width 1000 | grep "$DEPLOY_HOME" |awk '{print $2}'`
if [ -z "$DUMP_PIDS" ]; then
    echo "The server $HOST_NAME is not started!"
    exit 1;
fi

DUMP_ROOT=$OUTPUT_HOME/dump
if [ ! -d $DUMP_ROOT ]; then
    mkdir $DUMP_ROOT
fi

DUMP_DATE=`date +%Y%m%d%H%M%S`
DUMP_DIR=$DUMP_ROOT/dump-$DUMP_DATE
if [ ! -d $DUMP_DIR ]; then
    mkdir $DUMP_DIR
fi

echo -e "Dumping the server $HOST_NAME ...\c"
for PID in $DUMP_PIDS ; do
    $JAVA_HOME/bin/jstack $PID > $DUMP_DIR/jstack-$PID.dump 2>&1
    echo -e ".\c"
    $JAVA_HOME/bin/jinfo $PID > $DUMP_DIR/jinfo-$PID.dump 2>&1
    echo -e ".\c"
    $JAVA_HOME/bin/jstat -gcutil $PID > $DUMP_DIR/jstat-gcutil-$PID.dump 2>&1
    echo -e ".\c"
    $JAVA_HOME/bin/jstat -gccapacity $PID > $DUMP_DIR/jstat-gccapacity-$PID.dump 2>&1
    echo -e ".\c"
    $JAVA_HOME/bin/jmap $PID > $DUMP_DIR/jmap-$PID.dump 2>&1
    echo -e ".\c"
    $JAVA_HOME/bin/jmap -heap $PID > $DUMP_DIR/jmap-heap-$PID.dump 2>&1
    echo -e ".\c"
    $JAVA_HOME/bin/jmap -histo $PID > $DUMP_DIR/jmap-histo-$PID.dump 2>&1
    echo -e ".\c"
    if [ -r /usr/sbin/lsof ]; then
    /usr/sbin/lsof -p $PID > $DUMP_DIR/lsof-$PID.dump
    echo -e ".\c"
    fi
done
if [ -r /usr/bin/sar ]; then
/usr/bin/sar > $DUMP_DIR/sar.dump
echo -e ".\c"
fi
if [ -r /usr/bin/uptime ]; then
/usr/bin/uptime > $DUMP_DIR/uptime.dump
echo -e ".\c"
fi
if [ -r /usr/bin/free ]; then
/usr/bin/free -t > $DUMP_DIR/free.dump
echo -e ".\c"
fi
if [ -r /usr/bin/vmstat ]; then
/usr/bin/vmstat > $DUMP_DIR/vmstat.dump
echo -e ".\c"
fi
if [ -r /usr/bin/mpstat ]; then
/usr/bin/mpstat > $DUMP_DIR/mpstat.dump
echo -e ".\c"
fi
if [ -r /usr/bin/iostat ]; then
/usr/bin/iostat > $DUMP_DIR/iostat.dump
echo -e ".\c"
fi
if [ -r /bin/netstat ]; then
/bin/netstat > $DUMP_DIR/netstat.dump
echo -e ".\c"
fi
echo "OK!"
```
