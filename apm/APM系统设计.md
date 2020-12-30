# APM系统设计

## 第一章：如何着手打造公司的APM系统？

**概要：**

1. 整体认知APM系统
2. APM系统架构实战
3. 零侵入核心技术
   - Javaagent核心概念
   - javassist核心操作

 **1.整体认知APM系统** 

​	 APM全称Application Performance Management(应用性能管理),APM致力于监控和管理应用软件性能和可用性。通过监测和诊断复杂应用程序的性能问题，来保证软件应用程序的良好运行 .

**APM发展史**

![img](https://images-cdn.shimo.im/rqD9FsnWhcEBVrML/image.png!thumbnail)



APM 发展的三个阶段

**第一阶段:** 以网络为中心，网速即应用速度，APM 主要指基础组件的性能监控

**第二阶段**: 以 IT 部件/组件为中心，监控围绕着部件/组件健康状态，基础设施可用性，伴随 IT 基础架构组件发布

**第三阶段:** 以 IT 应用核心和业务交易为中心，IT 架构高度复杂性，专注 IT 应用，面向用户，提供应用生命周期管理



## 2.APM系统架构选型

### **架构目标：**

1. 功能性需求
   1. 系统和服务指标收集
   2. 日志集中存储与搜索
   3. 应用内部监控
   4. 图表展示
   5. 监控指标报警
2. 非功能性需求
   1. 高性能：支持TB级别监控日志收集
   2. 高可用：整个系统无核心单点
   3. 强伸缩：能根据公司规模灵活调整

### 开源方案

**Zabbix 与open-falcon**



主工功能与特点:

\- CPU负荷

\- 内存使用

\- 磁盘使用

\- 网络状况

\- 端口监视

\- 日志监视



**不同点：**

|        | Zabbix                                                       | open-falcon                                                  |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 用户群 | 85%泛化互联网企业                                            | 包括不限于：美团、金山云、快网、宜信、七牛、赶集、滴滴、金山办公、爱奇艺、一点资讯、借贷宝、百度、迅雷等等 |
| 优点   | 1、安装部署简单2、多数据集采集灵活集成3、用户群体大，使用时间长 | 1、数据采集免配置2、吞吐量高，支持单周期亿次数据采集、告警判定3、中文文档齐全4、水平扩展多IDC支持5、持策略模板、模板继承和覆盖、多种告警方式、支持callback调用 |
| 缺点   | 1、易用性不高、而且很多定制化监控实现起来很麻烦。2、应对超大流量有点力不从心 | 1、部署复杂，各个组件太零散                                  |

**Open-falcon 架构图**

![img](https://uploader.shimo.im/f/jE0Z4MzuMJ696TGa.png!thumbnail)



单纯服务节点监控与报警，以上两个开源软件都在一定程度上可以支持，但做为APM还缺失了日志集中搜索与查询的功能。这就需要另外一套技术来补充。

### 开源加自研发

**架构方案描述：**

整套架构在ELK基础上进行扩展升级，基主要分为日志信息**采集**、**传输、存储**与**展现**四部分。

**采集：**javaagent 采集应用内部性能指标并输出至日志、FileBeat采集文本日志、Metricbeat采集系统性能指标。

**传输：**beats**、**logstash**、**kafka

**存储：**Elasticsearch 

**展现：**Kibana



**架构图：**

![img](https://images-cdn.shimo.im/N02v6lj7ZdwsEvFh/image.png!thumbnail)



**应用内部性能采集说明：**

基于字节码插桩技术监控应用内部性能指标如：Http响应时间、服务响应、异常、SQL语句、外部接口响应。该方式特点是：即可以精准监控应用内部，又不会对应用造成侵入从而简化上线推广成本。

*注：字节码插桩技术指使用javaagent与 javassist动态重构Class字节已插入监控指令。*

**应用外部环境采集说明：**

基于metricbeat 主动采集系统信息如： CPU 使用率、内存、文件系统、磁盘 IO 和网络 IO 统计数据，以及获得如同系统上 top 命令类似的各个进程的统计数据。此外基于metricbeat  中提供的插件去主动采集 mySql、Redis、Nginx等状态信息。

**监控报警说明：**

如果报警条件比较简单，可以直接基于logstash当中logstash-output-http 插件输送数据流至自实现报警中心实现报警。如果报警条件复杂则可基于logstash-output-zabbix输送数据流至 zabbix系统触发报警。

​	1、设定日志过期时间，写脚本定期清理Elasticsearch 当中过期数据。

​	2、在logstash中设置过滤器，过滤debug 和info 日志。

**问题2：**logstash 和Elasticsearch  是分开部署的么，不想要这么复杂的架构怎么办？

​	可以直接去掉logstash ，数据直接从beats 发送至Elasticsearch 

**问题3：**日志信息吞吐量太大怎么办？

​	这种情况只能在整个传输管道中间加入一层 Broker作缓冲Redis或Kafak。当然这样做将会使整个系统变得更复杂，众多节点自身的健康状况维护也变得困难。具体看下图。

![img](https://uploader.shimo.im/f/niWUUqK4moK9qHs8.png!thumbnail)



## 3.零侵入核心技术

**插桩技术掌握对APM项目的意义：**

​	APM 整体实现 分为 数据**采集**、**传输**、**存储**、**图表展示**与**报警**。当中采集可以分为资源指标和系统指标两类。 一般监控系统调用操作系统命令可以轻松获得资源指标。而内部系统的指标要么就是应用自己采集数据发送数据监控系统，要么引入监控系统提供的JAR来修改，无论哪种都对应用造成了侵入，不利于实施。字节码插桩技术则可以解决监控系统侵入这一问题。



![img](https://images-cdn.shimo.im/FqhjymQQTnAoVSR1/image.png!thumbnail)





字节码插桩技术怎么实现呢?我们只需掌握两个技术即可：

1. 插桩的入口javaagnet。
2. 字节码修改工具javassist。



## **4.javaagent 核心操作**

​	javaagent 是java1.5之后引入的特性，其主要作用是在class 被加载之前对其拦截，已插入我们的监听字节码

​	**javaagent 应用场景**

​		监控、代码覆盖率分析

​		JProfiler、应用破解

​	**javaagent 使用**

​		javaagent 其实就是一个jar 包，通过-javaagent:xxx.jar 引入监控目标应用。那这个jar 和普通的jar 有什么区别么？

|                     | **运行时时JAR包** | **Javaagent JAR包** |
| ------------------- | ----------------- | ------------------- |
| 入口方法名称        | main              | premain             |
| Maninfe.MF 主要参数 | main-class        | premain-class       |
| 启动参数            | java -jar xxx.jar | -javaagent:xxx.jar  |
| 执行顺序            | 后                | 先                  |
| 是否独立启动        | 是                | 否                  |

**javaagent 使用演示**

- 演示静态装载agent

**javaagent 底层流程：**

javaagent 装载时序图（premain）：

![img](https://images-cdn.shimo.im/dbyTqsP6HtoUWSXH/image.png!thumbnail)



Class 装载时序图：

![img](https://images-cdn.shimo.im/qLIHqIkfLPkh5Lrj/image.png!thumbnail)



**javaagent jar 包**

**MANIFEST.MF 配置参数**

```java
#动态agent 类 

Agent-Class: com.tuling.javaagent.MyAgent

#agent 依懒包逗号分割

Boot-Class-Path: javassist-3.18.1-GA.jar

#是否允许重定义

Can-Redefine-Classes: true

#允许重载

Can-Retransform-Classes:true

#静agent 类 

Premain-Class: com.tuling.javaagent.MyAgent
```



## **5.javassist 核心操作**

​	Javassist是一个开源的分析、编辑和创建Java字节码的类库。其主要的优点，在于简单，而且快速。直接使用java编码的形式，而不需要了解虚拟机指令，就能动态改变类的结构，或者动态生成。



**Javassist 应用场景**

监控、动态代理

dubbo、myBatis、Spring 都有不同程度的应用。

**javassist 使用演示**

- 在运行时修改一个方法代码

**javassist API介绍**

API执行流程

![img](https://images-cdn.shimo.im/dv0bOCk7VOUmqD7d/image.png!thumbnail)



javassist UML类图

![img](https://images-cdn.shimo.im/efbvgLBcJ20ufDst/image.png!thumbnail)



**javassist 特殊符号** :

 ![img](https://uploader.shimo.im/f/7MzRVsNxjh6cLt6C.png!original) 

**javassist 特殊语法说明：**

  a) 不能引用在方法中其它地方定义的局部变量	

  b) 不会对类型进行强制检查：如 int start = System.currentTimeMillis(); 或 String i=”abc”;

  c) 使用特殊的项目语法符号

## 6.作业

1. 实现对某个服务方法的监听（时间，参数、异常）打印。

1. 实现对C3p0数据源的监听，类似Druid （附加）



![img](http://images2015.cnblogs.com/blog/859549/201704/859549-20170418221255977-960654626.png)



C3p0监控实现流程

**监控目标类：**

com.mchange.v2.c3p0.ComboPooledDataSource

- 编写c3p0 agent 

- 通过插桩方式获取 c3p0 DataSource 实例。

- System.getProperties().get("c3p0Source$agent")

- 对外提供Http 服务展示DataSource当前状态。



## 第二章：掌握字节码插桩核心技术

​	**概要：**

1. Service接口响应性能采集
2. Http响应性能采集实践

## Service接口响应性能采集

### **采集需求**

需求如下：

1. 接口指定时间段内调用统计包括：调用次数、平均执行时间、最快、最慢、成功次数、失败次数、慢执行次数。
2. 慢执行上报并报警。
3. 异常执行上报并报警。

### **service 采集方案**

**采集方式：**

1. 字节码插桩

**采集目标判定：**

1. 基于String“@Service"注解
2. 基于用户自定义配置include 与 exclude 
3. 用户需要配置
4. 加大了开发成本 com.coderead.*.**

### 数据模型

| **字段**    | **类型**       | **描述**       |
| ----------- | -------------- | -------------- |
| begin       | 时间搓（毫秒） | 开始时间       |
| userTime    | 用时（毫秒）   | 用时           |
| errorMsg    | 字符串         | 异常消息       |
| errorType   | 字符串         | 异常类型       |
| serviceName | 字符串         | 服务类名       |
| simpleName  | 字符串         | 服务类名简称   |
| methodName  | 字符串         | 方法名         |
| hostIp      | 字符串         | 主机IP         |
| appName     | 字符串         | 应用名称，标识 |
| traceId     | 字符串         | 追踪ID         |

## **Http 响应数据采集**

### 采集需求

1. http 指定时间段内调用统计包括：调用次数、平均执行时间、最快、最慢、成功次数、失败次数、慢执行次数。
2. 慢执行上报并报警。
3. 异常执行上报并报警。
4. URL分类统计

### 采集方案

1. **采集方式：**
   1. **字节码插桩**

1. **采集入口：**
   1. SpringDispacherServlet .doServer()
   2. 基于Spring @control   注解，监控对应方法@RequestMapping(url)
   3. javax.servlet.http.HttpServlet