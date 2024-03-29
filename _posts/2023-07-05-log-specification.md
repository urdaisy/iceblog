---
layout: post
title: mall tiny
date: 2023-07-05
tags: [Spring]
comments: false
---
## 一、常用的日志框架
SLF4J 是一个为各种日志框架（如 java.util.logging、logback、log4j）提供简单门面或抽象的日志框架，允许用户在部署时插入所需的日志框架。

1、java.util.logging 这是Java自带的日志框架，与JDK紧密集成，适合在Java平台上开发应用程序，但功能相对较弱，配置相对复杂。
2、logback Spring Boot的默认日志框架，基于Log4j的技术演化而来，但相比于 log4j，Logback 在性能方面非常高效，并支持异步日志记录，可提供较高的吞吐量和低延迟。它还具有灵活的配置选项和广泛的文档支持。。
3、log4j 这是Log4j的升级版本，相对于Logback来说，性能更高，配置更简单，支持异步日志记录和自动重载配置文件等特性。

## 二、logback使用
Logback 提供了以下主要组件：

Logger：用于记录日志信息的主要接口。
Appender：用于将日志信息输出到不同的目标（例如控制台、文件、数据库等）。
Layout：用于格式化日志信息的组件。
Filter：用于过滤特定类型的日志信息。
Logback 的配置文件采用 XML 格式，可以在配置文件中定义不同的 logger、appender、layout 和 filter。配置文件中还可以设置日志级别、日志转储策略等。
```
参考链接：https://github.com/YLongo/logback-chinese-manual/blob/master/03%E7%AC%AC%E4%B8%89%E7%AB%A0%EF%BC%9Alogback%20%E7%9A%84%E9%85%8D%E7%BD%AE.md
```

## 三、日志规约

1、【强制】应用中不可直接使用日志系统（Log4j、Logback）中的API，而应依赖使用日志框架SLF4J中的API，使用门面模式的日志框架，有利于维护和各个类的日志处理方式统一。
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
private static final Logger logger = LoggerFactory.getLogger(Abc.class);  

2、【强制】日志文件推荐至少保存15天，因为有些异常具备以“周”为频次发生的特点。

3、【强制】应用中的扩展日志（如打点、临时监控、访问日志等）命名方式：appName_logType_logName.log。logType:日志类型，推荐分类有stats/monitor/visit等；logName:日志描述。这种命名的好处：通过文件名就可知道日志文件属于什么应用，什么类型，什么目的，也有利于归类查找。
正例：mppserver应用中单独监控时区转换异常，如：
mppserver_monitor_timeZoneConvert.log
说明：推荐对日志进行分类，如将错误日志和业务日志分开存放，便于开发人员查看，也便于通过日志对系统进行及时监控。

4、【强制】对trace/debug/info级别的日志输出，必须使用条件输出形式或者使用占位符的方式。

5、【强制】避免重复打印日志，浪费磁盘空间，务必在log4j.xml中设置additivity=false。
正例： <logger name="com.taobao.dubbo.config" additivity="false">

6、【强制】异常信息应该包括两类信息：案发现场信息和异常堆栈信息。如果不处理，那么通过关键字throws往上抛出。
正例：
logger.error(各类参数或者对象toString + "_" + e.getMessage(), e);

7、【推荐】谨慎地记录日志。生产环境禁止输出debug日志；有选择地输出info日志；如果使用warn来记录刚上线时的业务行为信息，一定要注意日志输出量的问题，避免把服务器磁盘撑爆，并记得及时删除这些观察日志。
说明：大量地输出无效日志，不利于系统性能提升，也不利于快速定位错误点。记录日志时请思考：这些日志真的有人看吗？看到这条日志你能做什么？能不能给问题排查带来好处？

8、【推荐】可以使用warn日志级别来记录用户输入参数错误的情况，避免用户投诉时，无所适从。如非必要，请不要在此场景打出error级别，避免频繁报警。
说明：注意日志输出的级别，error级别只记录系统逻辑出错、异常或者重要的错误信息。