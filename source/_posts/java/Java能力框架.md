---
title: Java能力框架
categories: Java系列
author: _山海
tags:
  - Java
keywords: java
abbrlink: 2914746800
data: 2024-01-01 00:00:00
---


JAVA从初学到资深再到专家，是一个循序渐进的过程，不仅要coding，还要对IT知识体系有认知。以下是笔者认为高阶JAVA技术人员需要掌握的整体能力框架。相关内容的掌握的深浅直接反映其技术水平。

> 改为文字，这样后面调整好修改

## 基础知识
### 计算机基础


{% mermaid %}
graph TD

计算机基础 --> 操作系统
计算机基础 --> 数据结构
计算机基础 --> 网络
计算机基础 --> 算法

{% endmermaid  %}

### Java

{% mermaid %}
graph TD

Java --> JVM
Java --> 语言特性
Java --> 多线程
Java --> IO编程
{% endmermaid %}



## 项目经验

{% mermaid %}
graph TD
项目经验 --> 项目描述
项目经验 --> 项目难点
项目经验 --> 项目问题
项目经验 --> 项目改进
项目经验 --> 理论知识
理论知识 --> WEB
理论知识 --> 敏捷
{% endmermaid %}

## 架构能力

### 基础架构能力
{% mermaid %}
graph TD
基础架构能力 --> Docker
基础架构能力 --> K8S
基础架构能力 --> Prometheus
基础架构能力 --> CAP理论
基础架构能力 --> 领域驱动设计
{% endmermaid %}

### 微服务架构

{% mermaid %}
graph TD
微服务架构  --> 注册中心
微服务架构  --> 流量控制
微服务架构  --> 分布式事务
微服务架构  --> 链路跟踪
微服务架构  --> ...
{% endmermaid %}

## 应用知识

### 常用工具

{% mermaid %}
graph TD
常用工具 --> 排查类
常用工具 --> 协作类
常用工具 --> 保障类
常用工具 --> 系统类
{% endmermaid %}
### 常用框架
{% mermaid %}
graph TD
常用框架 --> Spring
常用框架 --> Netty
常用框架 --> Dubbo
常用框架 --> Mybatic
常用框架 --> 细分领域
常用框架 --> ...
{% endmermaid %}

### 队列
{% mermaid %}
graph TD
常用框架 --> Kafka
常用框架 --> ActiveMQ
常用框架 --> RabbitMQ
常用框架 --> ...
{% endmermaid %}

### 数据库
{% mermaid %}
graph TD
数据库 --> RMDB
数据库 --> NoSql
数据库 --> 图数据库
数据库 --> 向量数据库
数据库 --> ...
{% endmermaid %}

### 缓存
{% mermaid %}
graph TD
缓存 --> redis
缓存 --> ehcache
缓存 --> ...
{% endmermaid %}

### 云平台
{% mermaid %}
graph TD
云平台 --> 阿里
云平台 --> 腾讯
云平台 --> 华为
云平台 --> Ucloud
云平台 --> 百度
云平台 --> AWS
云平台 --> AZure
云平台 --> GoogleCloud
{% endmermaid %}

## 文档能力
{% mermaid %}
graph TD
文档能力 --> 方案编写
文档能力 --> 概要设计
文档能力 --> 详细设计
{% endmermaid %}

<br><br>

我们抛开JAVA语言本身，将其他语言生态的相关功能置入，同样适用。
> 当时较懒，对于图中的内容，笔者只更新了两个文章, 不久之后将由其他博客搬至JJ。再有后续的有时间再添加上。


[Jvm原理](https://juejin.cn/post/7296017029705318419)  
[Jvm调优](https://juejin.cn/post/7296017029705318419)  
...
