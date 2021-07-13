---
title: SpringBoot集成定时任务
urlname: zieqid
date: '2021-07-12 14:14:48 +0800'
tags: []
categories: []
---

# SpringBoot 集成定时任务

## 1.1 Spring Task

cron 表达式中，每个位置的约束如下：
![1010726-20190919020734351-1528359096.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590054291587-de30da84-85bb-49ba-b11b-f04703b284c4.png#height=482&id=piuDg&margin=%5Bobject%20Object%5D&name=1010726-20190919020734351-1528359096.png&originHeight=482&originWidth=1230&originalType=binary∶=1&size=215563&status=done&style=none&width=1230)

> 星号(\**)：可用在所有字段中，表示对应时间域的每一个时刻，例如，*在分钟字段时，表示“每分钟”；
> 问号（?）：该字符只在日期和星期字段中使用，它通常指定为“无意义的值”，相当于占位符；
> 减号(-)：表达一个范围，如在小时字段中使用“10-12”，则表示从 10 到 12 点，即 10,11,12；
> 逗号(,)：表达一个列表值，如在星期字段中使用“MON,WED,FRI”，则表示星期一，星期三和星期五；
> 斜杠(/)：x/y 表达一个等步长序列，x 为起始值，y 为增量步长值。如在秒数字段中使用 0/15，则表示为 0,15,30 和 45 秒，而 5/15 在分钟字段中表示 5,20,35,50，你也可以使用\*/y，它等同于 0/y；

### 1.1.1 单线程执行

```java
@Configuration      //1.主要用于标记配置类，兼备Component的效果。
@EnableScheduling   // 2.开启定时任务
public class SaticScheduleTask {
    //3.添加定时任务
    @Scheduled(cron = "0/5 * * * * ?")
    //或直接指定时间间隔，例如：5秒
    //@Scheduled(fixedRate=5000)
    private void configureTasks() {
        System.err.println("执行静态定时任务时间: " + LocalDateTime.now());
    }
}
```

缺点：基于注解@Scheduled 默认为单线程，开启多个任务时，任务的执行时机会受上一个任务执行时间的影响。举个例子：

```java
    //3.添加定时任务
    //@Scheduled(cron = "0/5 * * * * ?")
    //或直接指定时间间隔，例如：5秒
    @Scheduled(fixedRate=5000)
    private void configureTasks1() throws InterruptedException {
        System.err.println("执行静态定时任务时间1: " + LocalDateTime.now());
        Thread.sleep(3000);
    }

    @Scheduled(fixedRate=5000)
    private void configureTasks2() {
        System.err.println("执行静态定时任务时间2: " + LocalDateTime.now());
    }
```

### 1.1.2 多线程执行

```java
@Slf4j
@Configuration      //1.主要用于标记配置类，兼备Component的效果。
@EnableScheduling   //2.开启定时任务
@EnableAsync        //3.开启异步执行
public class SaticScheduleTask {

    //3.添加定时任务
    //@Scheduled(cron = "0/5 * * * * ?")
    //或直接指定时间间隔，例如：5秒
    @Async
    @Scheduled(fixedRate=5000)
    public void configureTasks1() throws InterruptedException {
        System.err.println("执行静态定时任务时间1: " + LocalDateTime.now());
        Thread.sleep(3000);
    }

    @Async
    @Scheduled(fixedRate=5000)
    public void configureTasks2() {
        System.err.println("执行静态定时任务时间2: " + LocalDateTime.now());
    }

}
```

优点：解决单线程带来的任务延时
缺点：更加消耗系统资源

task 任务执行线程池配置：

> spring.task.execution.pool.keep-alive=10s
> spring.task.execution.pool.queue-capacity=1000
> spring.task.execution.pool.max-size=100
> spring.task.execution.pool.core-size=50

####

### 1.1.3 动态：基于接口

```
@Configuration      //1.主要用于标记配置类，兼备Component的效果。
@EnableScheduling   // 2.开启定时任务
public class DynamicScheduleTask implements SchedulingConfigurer {

    @Override
    public void configureTasks(ScheduledTaskRegistrar scheduledTaskRegistrar) {
        //增加任务1
        addTask1(scheduledTaskRegistrar);
    }

    private void addTask1(ScheduledTaskRegistrar scheduledTaskRegistrar) {
        scheduledTaskRegistrar.addTriggerTask(
                //1.添加任务内容(Runnable)
                () -> System.out.println("执行动态定时任务: " + LocalDateTime.now().toLocalTime()),
                //2.设置执行周期(Trigger)
                triggerContext -> {
                    //2.1 从数据库获取执行周期（可以把cron配在数据库里，这里不做演示）
                    String cron = "0/5 * * * * ?";
                    //2.2 合法性校验.
                    if (StringUtils.isEmpty(cron)) {
                        throw new RuntimeException("cron表达式不正确");
                    }
                    //2.3 返回执行周期(Date)
                    return new CronTrigger(cron).nextExecutionTime(triggerContext);
                }
        );

    }
}
```

## 1.2 Quartz

### 1.2.1 集成

> quartz 三要素：Scheduler、Trigger、JobDetai&Job

step1，添加依赖

```java
<!--quartz依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-quartz</artifactId>
</dependency>
```

step2，增加配置

```java
spring:
  main:
    allow-bean-definition-overriding: true
  quartz:
    properties:
      org:
        quartz:
          scheduler:
            instanceName: clusteredScheduler #调度器实例名称
            instanceId: AUTO #调度器实例编号自动生成
          jobStore:
            class: org.quartz.impl.jdbcjobstore.JobStoreTX #持久化方式配置
            driverDelegateClass: org.quartz.impl.jdbcjobstore.StdJDBCDelegate #持久化方式配置数据驱动，MySQL数据库
            tablePrefix: qrtz_ #quartz相关数据表前缀名
            isClustered: true #开启分布式部署
            clusterCheckinInterval: 10000 #分布式节点有效性检查时间间隔，单位：毫秒
            useProperties: false #配置是否使用
          threadPool:
            class: org.quartz.simpl.SimpleThreadPool #线程池实现类
            threadCount: 10 #执行最大并发线程数量
            threadPriority: 5 #线程优先级
            threadsInheritContextClassLoaderOfInitializingThread: true #配置是否启动自动加载数据库内的定时任务，默认true
    job-store-type: jdbc
  #数据源配置
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    url: jdbc:mysql://${DEV.HOST:192.168.1.114}:3306/test?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true&pinGlobalTxToPhysicalConnection=true&useSSL=false
    username: root
    password: root
```

step3，生成表

```
DROP TABLE IF EXISTS QRTZ_FIRED_TRIGGERS;
DROP TABLE IF EXISTS QRTZ_PAUSED_TRIGGER_GRPS;
DROP TABLE IF EXISTS QRTZ_SCHEDULER_STATE;
DROP TABLE IF EXISTS QRTZ_LOCKS;
DROP TABLE IF EXISTS QRTZ_SIMPLE_TRIGGERS;
DROP TABLE IF EXISTS QRTZ_SIMPROP_TRIGGERS;
DROP TABLE IF EXISTS QRTZ_CRON_TRIGGERS;
DROP TABLE IF EXISTS QRTZ_BLOB_TRIGGERS;
DROP TABLE IF EXISTS QRTZ_TRIGGERS;
DROP TABLE IF EXISTS QRTZ_JOB_DETAILS;
DROP TABLE IF EXISTS QRTZ_CALENDARS;

CREATE TABLE QRTZ_JOB_DETAILS(
SCHED_NAME VARCHAR(120) NOT NULL,
JOB_NAME VARCHAR(190) NOT NULL,
JOB_GROUP VARCHAR(190) NOT NULL,
DESCRIPTION VARCHAR(250) NULL,
JOB_CLASS_NAME VARCHAR(250) NOT NULL,
IS_DURABLE VARCHAR(1) NOT NULL,
IS_NONCONCURRENT VARCHAR(1) NOT NULL,
IS_UPDATE_DATA VARCHAR(1) NOT NULL,
REQUESTS_RECOVERY VARCHAR(1) NOT NULL,
JOB_DATA BLOB NULL,
PRIMARY KEY (SCHED_NAME,JOB_NAME,JOB_GROUP))
ENGINE=InnoDB;

CREATE TABLE QRTZ_TRIGGERS (
SCHED_NAME VARCHAR(120) NOT NULL,
TRIGGER_NAME VARCHAR(190) NOT NULL,
TRIGGER_GROUP VARCHAR(190) NOT NULL,
JOB_NAME VARCHAR(190) NOT NULL,
JOB_GROUP VARCHAR(190) NOT NULL,
DESCRIPTION VARCHAR(250) NULL,
NEXT_FIRE_TIME BIGINT(13) NULL,
PREV_FIRE_TIME BIGINT(13) NULL,
PRIORITY INTEGER NULL,
TRIGGER_STATE VARCHAR(16) NOT NULL,
TRIGGER_TYPE VARCHAR(8) NOT NULL,
START_TIME BIGINT(13) NOT NULL,
END_TIME BIGINT(13) NULL,
CALENDAR_NAME VARCHAR(190) NULL,
MISFIRE_INSTR SMALLINT(2) NULL,
JOB_DATA BLOB NULL,
PRIMARY KEY (SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP),
FOREIGN KEY (SCHED_NAME,JOB_NAME,JOB_GROUP)
REFERENCES QRTZ_JOB_DETAILS(SCHED_NAME,JOB_NAME,JOB_GROUP))
ENGINE=InnoDB;

CREATE TABLE QRTZ_SIMPLE_TRIGGERS (
SCHED_NAME VARCHAR(120) NOT NULL,
TRIGGER_NAME VARCHAR(190) NOT NULL,
TRIGGER_GROUP VARCHAR(190) NOT NULL,
REPEAT_COUNT BIGINT(7) NOT NULL,
REPEAT_INTERVAL BIGINT(12) NOT NULL,
TIMES_TRIGGERED BIGINT(10) NOT NULL,
PRIMARY KEY (SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP),
FOREIGN KEY (SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP)
REFERENCES QRTZ_TRIGGERS(SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP))
ENGINE=InnoDB;

CREATE TABLE QRTZ_CRON_TRIGGERS (
SCHED_NAME VARCHAR(120) NOT NULL,
TRIGGER_NAME VARCHAR(190) NOT NULL,
TRIGGER_GROUP VARCHAR(190) NOT NULL,
CRON_EXPRESSION VARCHAR(120) NOT NULL,
TIME_ZONE_ID VARCHAR(80),
PRIMARY KEY (SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP),
FOREIGN KEY (SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP)
REFERENCES QRTZ_TRIGGERS(SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP))
ENGINE=InnoDB;

CREATE TABLE QRTZ_SIMPROP_TRIGGERS
  (
    SCHED_NAME VARCHAR(120) NOT NULL,
    TRIGGER_NAME VARCHAR(190) NOT NULL,
    TRIGGER_GROUP VARCHAR(190) NOT NULL,
    STR_PROP_1 VARCHAR(512) NULL,
    STR_PROP_2 VARCHAR(512) NULL,
    STR_PROP_3 VARCHAR(512) NULL,
    INT_PROP_1 INT NULL,
    INT_PROP_2 INT NULL,
    LONG_PROP_1 BIGINT NULL,
    LONG_PROP_2 BIGINT NULL,
    DEC_PROP_1 NUMERIC(13,4) NULL,
    DEC_PROP_2 NUMERIC(13,4) NULL,
    BOOL_PROP_1 VARCHAR(1) NULL,
    BOOL_PROP_2 VARCHAR(1) NULL,
    PRIMARY KEY (SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP),
    FOREIGN KEY (SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP)
    REFERENCES QRTZ_TRIGGERS(SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP))
ENGINE=InnoDB;

CREATE TABLE QRTZ_BLOB_TRIGGERS (
SCHED_NAME VARCHAR(120) NOT NULL,
TRIGGER_NAME VARCHAR(190) NOT NULL,
TRIGGER_GROUP VARCHAR(190) NOT NULL,
BLOB_DATA BLOB NULL,
PRIMARY KEY (SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP),
INDEX (SCHED_NAME,TRIGGER_NAME, TRIGGER_GROUP),
FOREIGN KEY (SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP)
REFERENCES QRTZ_TRIGGERS(SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP))
ENGINE=InnoDB;

CREATE TABLE QRTZ_CALENDARS (
SCHED_NAME VARCHAR(120) NOT NULL,
CALENDAR_NAME VARCHAR(190) NOT NULL,
CALENDAR BLOB NOT NULL,
PRIMARY KEY (SCHED_NAME,CALENDAR_NAME))
ENGINE=InnoDB;

CREATE TABLE QRTZ_PAUSED_TRIGGER_GRPS (
SCHED_NAME VARCHAR(120) NOT NULL,
TRIGGER_GROUP VARCHAR(190) NOT NULL,
PRIMARY KEY (SCHED_NAME,TRIGGER_GROUP))
ENGINE=InnoDB;

CREATE TABLE QRTZ_FIRED_TRIGGERS (
SCHED_NAME VARCHAR(120) NOT NULL,
ENTRY_ID VARCHAR(95) NOT NULL,
TRIGGER_NAME VARCHAR(190) NOT NULL,
TRIGGER_GROUP VARCHAR(190) NOT NULL,
INSTANCE_NAME VARCHAR(190) NOT NULL,
FIRED_TIME BIGINT(13) NOT NULL,
SCHED_TIME BIGINT(13) NOT NULL,
PRIORITY INTEGER NOT NULL,
STATE VARCHAR(16) NOT NULL,
JOB_NAME VARCHAR(190) NULL,
JOB_GROUP VARCHAR(190) NULL,
IS_NONCONCURRENT VARCHAR(1) NULL,
REQUESTS_RECOVERY VARCHAR(1) NULL,
PRIMARY KEY (SCHED_NAME,ENTRY_ID))
ENGINE=InnoDB;

CREATE TABLE QRTZ_SCHEDULER_STATE (
SCHED_NAME VARCHAR(120) NOT NULL,
INSTANCE_NAME VARCHAR(190) NOT NULL,
LAST_CHECKIN_TIME BIGINT(13) NOT NULL,
CHECKIN_INTERVAL BIGINT(13) NOT NULL,
PRIMARY KEY (SCHED_NAME,INSTANCE_NAME))
ENGINE=InnoDB;

CREATE TABLE QRTZ_LOCKS (
SCHED_NAME VARCHAR(120) NOT NULL,
LOCK_NAME VARCHAR(40) NOT NULL,
PRIMARY KEY (SCHED_NAME,LOCK_NAME))
ENGINE=InnoDB;

CREATE INDEX IDX_QRTZ_J_REQ_RECOVERY ON QRTZ_JOB_DETAILS(SCHED_NAME,REQUESTS_RECOVERY);
CREATE INDEX IDX_QRTZ_J_GRP ON QRTZ_JOB_DETAILS(SCHED_NAME,JOB_GROUP);

CREATE INDEX IDX_QRTZ_T_J ON QRTZ_TRIGGERS(SCHED_NAME,JOB_NAME,JOB_GROUP);
CREATE INDEX IDX_QRTZ_T_JG ON QRTZ_TRIGGERS(SCHED_NAME,JOB_GROUP);
CREATE INDEX IDX_QRTZ_T_C ON QRTZ_TRIGGERS(SCHED_NAME,CALENDAR_NAME);
CREATE INDEX IDX_QRTZ_T_G ON QRTZ_TRIGGERS(SCHED_NAME,TRIGGER_GROUP);
CREATE INDEX IDX_QRTZ_T_STATE ON QRTZ_TRIGGERS(SCHED_NAME,TRIGGER_STATE);
CREATE INDEX IDX_QRTZ_T_N_STATE ON QRTZ_TRIGGERS(SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP,TRIGGER_STATE);
CREATE INDEX IDX_QRTZ_T_N_G_STATE ON QRTZ_TRIGGERS(SCHED_NAME,TRIGGER_GROUP,TRIGGER_STATE);
CREATE INDEX IDX_QRTZ_T_NEXT_FIRE_TIME ON QRTZ_TRIGGERS(SCHED_NAME,NEXT_FIRE_TIME);
CREATE INDEX IDX_QRTZ_T_NFT_ST ON QRTZ_TRIGGERS(SCHED_NAME,TRIGGER_STATE,NEXT_FIRE_TIME);
CREATE INDEX IDX_QRTZ_T_NFT_MISFIRE ON QRTZ_TRIGGERS(SCHED_NAME,MISFIRE_INSTR,NEXT_FIRE_TIME);
CREATE INDEX IDX_QRTZ_T_NFT_ST_MISFIRE ON QRTZ_TRIGGERS(SCHED_NAME,MISFIRE_INSTR,NEXT_FIRE_TIME,TRIGGER_STATE);
CREATE INDEX IDX_QRTZ_T_NFT_ST_MISFIRE_GRP ON QRTZ_TRIGGERS(SCHED_NAME,MISFIRE_INSTR,NEXT_FIRE_TIME,TRIGGER_GROUP,TRIGGER_STATE);

CREATE INDEX IDX_QRTZ_FT_TRIG_INST_NAME ON QRTZ_FIRED_TRIGGERS(SCHED_NAME,INSTANCE_NAME);
CREATE INDEX IDX_QRTZ_FT_INST_JOB_REQ_RCVRY ON QRTZ_FIRED_TRIGGERS(SCHED_NAME,INSTANCE_NAME,REQUESTS_RECOVERY);
CREATE INDEX IDX_QRTZ_FT_J_G ON QRTZ_FIRED_TRIGGERS(SCHED_NAME,JOB_NAME,JOB_GROUP);
CREATE INDEX IDX_QRTZ_FT_JG ON QRTZ_FIRED_TRIGGERS(SCHED_NAME,JOB_GROUP);
CREATE INDEX IDX_QRTZ_FT_T_G ON QRTZ_FIRED_TRIGGERS(SCHED_NAME,TRIGGER_NAME,TRIGGER_GROUP);
CREATE INDEX IDX_QRTZ_FT_TG ON QRTZ_FIRED_TRIGGERS(SCHED_NAME,TRIGGER_GROUP);

commit;
```

表结构说明：

```xml
1.1. qrtz_blob_triggers : 以Blob 类型存储的触发器。
1.2. qrtz_calendars：存放日历信息， quartz可配置一个日历来指定一个时间范围。
1.3. qrtz_cron_triggers：存放cron类型的触发器。
1.4. qrtz_fired_triggers：存放已触发的触发器。
1.5. qrtz_job_details：存放一个jobDetail信息。
1.6. qrtz_job_listeners：job监听器。
1.7. qrtz_locks： 存储程序的悲观锁的信息(假如使用了悲观锁)。
1.8. qrtz_paused_trigger_graps：存放暂停掉的触发器。
1.9. qrtz_scheduler_state：调度器状态。
1.10. qrtz_simple_triggers：简单触发器的信息。
1.11. qrtz_trigger_listeners：触发器监听器。
1.12. qrtz_triggers：触发器的基本信息。
```

step4，工具类（自己补全）

```
public class JobUtils {

    /**
     *
     * @param scheduler quartz调度器
     * @param startAtTime 任务执行时刻
     * @param name 任务名称
     * @param group 任务组名称
     * @param jobBean 具体任务
     */
    public static void createJobByStartAt(Scheduler scheduler, long startAtTime, String name, String group, Class jobBean){
        //创建任务触发器
        Trigger trigger = TriggerBuilder.newTrigger().withIdentity(name,group).startAt(new Date(startAtTime)).build();
        createJob(scheduler, name, group, trigger,jobBean);

    }

    /**
     *
     * @param scheduler quartz调度器
     * @param name 任务名称
     * @param group 任务组名称
     * @param cron cron表达式
     * @param jobBean 具体任务
     */
    public static void createJobByCron(Scheduler scheduler, String name, String group,String cron,Class jobBean){
        CronScheduleBuilder scheduleBuilder = CronScheduleBuilder.cronSchedule(cron);
        //创建任务触发器
        Trigger trigger = TriggerBuilder.newTrigger().withIdentity(name,group).withSchedule(scheduleBuilder).build();
       createJob(scheduler,name,group,trigger,jobBean);
    }


    private static void createJob(Scheduler scheduler, String name, String group, Trigger trigger,Class jobBean) {
        //创建任务
        JobDetail jobDetail = JobBuilder.newJob(jobBean).withIdentity(name,group).build();
        try {
            //将触发器与任务绑定到调度器内
            scheduler.scheduleJob(jobDetail, trigger);
        } catch (SchedulerException e) {
            e.printStackTrace();
        }
    }
}
```

step5，测试

```
@RequestMapping(value = "/testquartz", method= RequestMethod.GET)
public void testQuartz(){
//JobUtils.createJobByStartAt(scheduler,new Date().getTime(),"firstJob","myGroup", MyJob.class);
JobUtils.createJobByCron(scheduler,"firstJob","myGroup","0/5 * * * * ?", MyJob.class);
}
```

### 1.2.2 增加自定义表管理定时任务

```mysql
//这个表可自行定义
CREATE TABLE `sys_job` (
  `job_id` varchar(64) NOT NULL,
  `job_name` varchar(64) DEFAULT NULL,
  `job_group` varchar(64) DEFAULT NULL,
  `cron_expression` varchar(64) DEFAULT NULL,
  `job_status` varchar(64) DEFAULT NULL,
  `job_class` varchar(200) DEFAULT NULL,
  `prevfire_time` date DEFAULT NULL,
  PRIMARY KEY (`job_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

工具类如下：

```java
@Component
@Slf4j
public class QuartzManager {

    //注入调度器
	@Autowired
	private  Scheduler scheduler;



	/**
	 * @Description: 添加一个定时任务，使用默认的任务组名，触发器名，触发器组名
	 *
	 * @param myJob
	 *
	 */
	@SuppressWarnings({ "unchecked", "rawtypes" })
	public  void addJob(SysJob myJob, Class cls) {
		log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:addJob  start");
		try {
			// 唯一主键
			String jobName = myJob.getJobName();
			String jobGroup = myJob.getJobGroup();
			TriggerKey triggerKey = TriggerKey.triggerKey(jobName, jobGroup);
			if(scheduler==null){ log.info("scheduler is null");}
			CronTrigger cronTrigger = (CronTrigger) scheduler.getTrigger(triggerKey);
			if (null == cronTrigger) {
				JobDetail jobDetail = JobBuilder.newJob(cls).withIdentity(jobName,jobGroup).build();

				cronTrigger = TriggerBuilder.newTrigger().withIdentity(jobName, jobGroup)
						.withSchedule(CronScheduleBuilder.cronSchedule(myJob.getCronExpression())).build();
//						cronTrigger.getJobDataMap().put("jobEntity", myJob);

				scheduler.scheduleJob(jobDetail, cronTrigger);
				//启动一个定时器
				if (!scheduler.isShutdown()) {
					scheduler.start();
				}
			} else {
				cronTrigger = cronTrigger.getTriggerBuilder().withIdentity(triggerKey).withSchedule(CronScheduleBuilder.cronSchedule(myJob.getCronExpression()))
						.build();
				scheduler.rescheduleJob(triggerKey, cronTrigger);
			}
			//等待启动完成
//			Thread.sleep(TimeUnit.SECONDS.toMillis(10));
		} catch (Exception e) {
			e.printStackTrace();
			log.info("定时任务启动失败：" + e.getMessage());
		}
		log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:addJob  end");
	}

	/**
	 * @Description: 暂停一个任务
	 *
	 * @param myJob
	 *
	 */
	public  void pasueOneJob(SysJob myJob) {
		log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:pasueOneJob  start");
		try {
			// 唯一主键
			String jobName = myJob.getJobName();
			String jobGroup = myJob.getJobGroup();
			TriggerKey triggerKey = TriggerKey.triggerKey(jobName, jobGroup);
			Trigger trigger = scheduler.getTrigger(triggerKey);
//			if(null==trigger){
//				log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:removeJob trigger is NULL ");
//				return;
//			}
			JobKey jobKey = trigger.getJobKey();
			scheduler.pauseJob(jobKey);
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
		log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:pasueOneJob  end");
	}

	/**
	 * @Description: 重启一个任务
	 *
	 * @param myJob
	 *
	 */
	public  void resOneJob(SysJob myJob) {
		log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:resOneJob  start");
		try {
			// 唯一主键
			String jobName = myJob.getJobName();
			String jobGroup = myJob.getJobGroup();
			TriggerKey triggerKey = TriggerKey.triggerKey(jobName, jobGroup);
			Trigger trigger = scheduler.getTrigger(triggerKey);
//			if(null==trigger){
//				log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:removeJob trigger is NULL ");
//				return;
//			}
			scheduler.rescheduleJob(triggerKey, trigger);

//			Thread.sleep(TimeUnit.MINUTES.toMillis(10));
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
		log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:resOneJob  end");
	}

	/**
	 * @Description: 修改一个任务的触发时间
	 *
	 */
	public  void modifyJobTime(SysJob myJob, String time) {
		log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:modifyJobTime  start");
		try {
			// 唯一主键
			String jobName = myJob.getJobName();
			String jobGroup = myJob.getJobGroup();
			TriggerKey triggerKey = TriggerKey.triggerKey(jobName, jobGroup);
			CronTrigger cronTrigger = (CronTrigger) scheduler.getTrigger(triggerKey);
			myJob.setCronExpression(time);
			CronScheduleBuilder cronScheduleBuilder = CronScheduleBuilder.cronSchedule(myJob.getCronExpression());
			cronTrigger = cronTrigger.getTriggerBuilder().withIdentity(triggerKey).withSchedule(cronScheduleBuilder)
					.build();
			scheduler.rescheduleJob(triggerKey, cronTrigger);

//			Thread.sleep(TimeUnit.MINUTES.toMillis(60));
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
		log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:modifyJobTime  end");
	}

	/**
	 * @Description: 移除一个任务(使用默认的任务组名，触发器名，触发器组名)
	 *
	 *
	 */
	public  void removeJob(SysJob myJob) {
		log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:removeJob  start");
		try {
			// 唯一主键
			String jobName = myJob.getJobName();
			String jobGroup = myJob.getJobGroup();
			TriggerKey triggerKey = TriggerKey.triggerKey(jobName, jobGroup);
			Trigger trigger = scheduler.getTrigger(triggerKey);
			if(null==trigger){
				log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:removeJob trigger is NULL ");
				return;
			}
			JobKey jobKey = trigger.getJobKey();
			scheduler.pauseTrigger(triggerKey);// 停止触发器
			scheduler.unscheduleJob(triggerKey);// 移除触发器
			scheduler.deleteJob(jobKey);// 删除任务

			//Thread.sleep(TimeUnit.MINUTES.toMillis(10));
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
		log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:removeJob  end");
	}

	/**
	 *
	 * 方法表述  获得执行器状态
	 * @return
	 * String
	 */
	public  String getStatus(SysJob myJob){
		String state = "NONE";
		try {
			// 唯一主键
			String jobName = myJob.getJobName();
			String jobGroup = myJob.getJobGroup();
			TriggerKey triggerKey = TriggerKey.triggerKey(jobName, jobGroup);
			//trigger state
			TriggerState triggerState = scheduler.getTriggerState(triggerKey);
			state = triggerState.toString();
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
		return state;
	}

	/**
	 * 是否存在任务
	 * 方法表述
	 * @param myJob
	 * @return
	 * boolean
	 */
	public  boolean hasTrigger(SysJob myJob){
		boolean isHas = true;
		try {
			// 唯一主键
			String jobName = myJob.getJobName();
			String jobGroup = myJob.getJobGroup();
			TriggerKey triggerKey = TriggerKey.triggerKey(jobName, jobGroup);
			Trigger trigger = scheduler.getTrigger(triggerKey);
			if(null==trigger){
				log.info(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>method:removeJob trigger is NULL ");
				isHas = false;
			}
		} catch (SchedulerException e) {
			isHas = false;
			e.printStackTrace();
		}
		return isHas;
	}

	/**
	 * @Description:启动所有定时任务
	 *
	 */
	public  void startJobs() {
		try {
			scheduler.start();
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}

	/**
	 * @Description:关闭所有定时任务
	 *
	 */
	public  void shutdownJobs() {
		try {
			if (!scheduler.isShutdown()) {
				scheduler.shutdown();
			}
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}

}
```

编辑/修改 job 例子：

```java
	@PostMapping("editJob")
    public void editJob(SysJob job){
        try {
            if(job.getJobId()!=null && StringUtils.isNotBlank(job.getJobId().toString())){

                SysJob t = jobService.queryById(job.getJobId());
                if(null != t){
                    quartzManager.removeJob(t);
                }
                jobService.updJob(job);
               log.info("成功","修改成功");
            }else{
                job.setJobId(ToolsUtils.generatorUUID());
                jobService.addJob(job);
                log.info("成功","创建成功");
            }
            //新增或者重新添加job
            quartzManager.addJob(job,Class.forName(job.getJobClass()));
        } catch (Exception e) {
            log.info("失败","修改失败");
        }
    }
}
```

## 1.3 xxl-job

### 1.3.1 官方文档地址

> [https://www.xuxueli.com/xxl-job/](https://www.xuxueli.com/xxl-job/)

### 1.3.2 搭建 xxl-job-admin

第一步，初始化 sql 脚本

> /xxl-job/doc/db/tables_xxl_job.sql

第二步，配置 xxl-job-admin 模块

> 配置 application.properties 具体的数据库和邮件地址

第三步，启动 xxl-job-admin 和访问

> [http://localhost:8080/xxl-job-admin](http://localhost:8080/xxl-job-admin)

### 1.3.3 搭建接入端

第一步，加入 pom.xml 依赖

```java
		<!-- xxl-job-core -->
		<dependency>
			<groupId>com.xuxueli</groupId>
			<artifactId>xxl-job-core</artifactId>
			<version>2.2.0</version>
		</dependency>
```

第二步，修改配置

```java
xxl:
  job:
    admin:
      addresses: http://127.0.0.1:8080/xxl-job-admin/
    accessToken:
    executor:
      appname: xxl-job1
      address:
      ip: 127.0.0.1
      port: 9999
      logpath: E:/springboot/springboot-project/springboot-swagger/log
      logretentiondays: 30
```

第三步，加入配置

```java
@Configuration
public class XxlJobConfig {
    private Logger logger = LoggerFactory.getLogger(XxlJobConfig.class);

    @Value("${xxl.job.admin.addresses}")
    private String adminAddresses;

    @Value("${xxl.job.accessToken}")
    private String accessToken;

    @Value("${xxl.job.executor.appname}")
    private String appname;

    @Value("${xxl.job.executor.address}")
    private String address;

    @Value("${xxl.job.executor.ip}")
    private String ip;

    @Value("${xxl.job.executor.port}")
    private int port;

    @Value("${xxl.job.executor.logpath}")
    private String logPath;

    @Value("${xxl.job.executor.logretentiondays}")
    private int logRetentionDays;


    @Bean
    public XxlJobSpringExecutor xxlJobExecutor() {
        logger.info(">>>>>>>>>>> xxl-job config init.");
        XxlJobSpringExecutor xxlJobSpringExecutor = new XxlJobSpringExecutor();
        xxlJobSpringExecutor.setAdminAddresses(adminAddresses);
        xxlJobSpringExecutor.setAppname(appname);
        xxlJobSpringExecutor.setAddress(address);
        xxlJobSpringExecutor.setIp(ip);
        xxlJobSpringExecutor.setPort(port);
        xxlJobSpringExecutor.setAccessToken(accessToken);
        xxlJobSpringExecutor.setLogPath(logPath);
        xxlJobSpringExecutor.setLogRetentionDays(logRetentionDays);

        return xxlJobSpringExecutor;
    }
}
```

第四步，配置 job

```java
@Slf4j
@Component
public class SampleXxlJob {

    @XxlJob("myJob")
    public ReturnT<String> myJobHandler(String param) throws Exception {
        log.info("----------------------");
        return ReturnT.SUCCESS;
    }

}
```

第五步，在 xxl-job-admin 上新增执行器路径和配置 Job

> 略

## 1.4 集成 @SchedulerLock 分布式锁

> [https://blog.csdn.net/a767815662/article/details/104459814](https://blog.csdn.net/a767815662/article/details/104459814)
