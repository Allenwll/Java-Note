# 从小工到专家系列之-LOGGER BEST PRACTICE

## 1. rules
- no System.out.println() & System.out.err()
- alwarys log 
- log contain full detail
- use slf4j & logback
- log exception stacktrace
	
```
LOGGER.error("handle something error",e);
```
	
- no split to seperate lines 



## 2. concept
1. log level
	- error:运行时异常以及预期之外的错误，需要立即采取行动  
		
		1.	database can't connection
		2. 系统间interface 不可访问
		3. 接收到无效、超出范围或错误的数据
		4. 超时或响应时间超长
		5. RuntimeException
	- warn:预期之外的运行时状况，不一定是错误的情况，不需要立即采取行动   
		
		1. 处理过程返回状态未定义，但无错
		2. 处理延迟，但最终成功
		3. 其它非错误问题
	- info:运行时产生的事件   
		
		1. 任务启动、结束
		2. 事务开始、结束
		3. 发送短信、邮件
		4. 提供服务接口接收参数以及处理结果
		5. 新事件的产生
		6. 载入新文件或新存储文件
		7. 用户相关信息
		
	- debug:与程序运行时的流程相关的详细信息   
		1.	方法接收参数以及返回结果
		2. 方法处理时间，读取记录数量
		3. 堆栈跟踪详情


2. config file  
	1. logback.xml:log配置文件 /resource/logback.xml
	2. appender:写日志组件
		- ConsoleAppender:控制台输出
		- FileAppender：文件输出
		- RollingFileAppender：滚动文件输出，按时间生成新的文件

	
3. pattern:
	
	输出格式
		
	- 包名
	- 类名
	- 方法名
	- 行数
	- 对齐



## 3. how to use
1. init log
```java
private static final Logger LOGGER = LoggerFactory.getLogger(*****);
```	   
	
	   
2. log what
	
- method input parameter
- return result
- method or code segment time cost (ms)
- event occurs
- user id,session id
- http request
- db get size or detail



3. seperate log files

```xml
private static final Logger LOGGER = LoggerFactory.getLogger(Test.class);
private static final Logger LOGGER_DB = LoggerFactory.getLogger("connection." + Test.class.getName());


<root level="DEBUG">
	<appender-ref ref="ASYNC_TOTAL"/>
</root>
<logger name="connection" level="DEBUG">
	<appender-ref ref="ASYNC1"/>
</logger>

```


4.	use MDC : web app log user & session id

```java
LOGGER.info("begin test");
MDC.put(SESSION_ID, "userSessionId");
LOGGER_DB.info("begin");
for (int i = 0; i < 2; i++) {
LOGGER_DB.info("begin db connection" + i);
}
MDC.remove(SESSION_ID);
LOGGER_DB.info("end");

2016-08-10 14:11:44.156 userSessionId [main] INFO  c.c.w.e.epon.config.logger.Test - begin
2016-08-10 14:11:44.156 userSessionId [main] INFO  c.c.w.e.epon.config.logger.Test - begin db connection0
2016-08-10 14:11:44.156 userSessionId [main] INFO  c.c.w.e.epon.config.logger.Test - begin db connection1
2016-08-10 14:11:44.158  [main] INFO  c.c.w.e.epon.config.logger.Test - end
		
```



## 4.	performance


1.	isDebugEnabled

	
```
if (LOGGER.isDebugEnabled()) {
	LOGGER.debug("******");
}	
```		




2.	use async log


```
ch.qos.logback.classic.AsyncAppender

```


3.	not output method row number




## 5. sample

```xml
<appender name="FILE" class= "ch.qos.logback.core.rolling.RollingFileAppender">  
			<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">  
				 <fileNamePattern>/opt/log/test.%d{yyyy-MM-dd}.log</fileNamePattern>  
				 <maxHistory>30</maxHistory>  
			<layout class="ch.qos.logback.classic.PatternLayout">  
				 <Pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} -%msg%n</Pattern>  
			</layout>  
</appender>  
 <appender name ="ASYNC" class= "ch.qos.logback.classic.AsyncAppender"> 
		<discardingThreshold >0</discardingThreshold>  
		<queueSize>512</queueSize>  
	 <appender-ref ref ="FILE"/>  
 </appender>  
   
 <root level ="trace">  
		<appender-ref ref ="ASYNC"/>  
 </root>  


```

		



