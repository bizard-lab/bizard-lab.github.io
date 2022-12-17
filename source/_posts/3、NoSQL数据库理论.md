---
title: 3、NoSQL数据库理论
date: 2022-11-12 20:50:29
top: 3
tags:
- Big Data

categories:
- Big Data

---

# NoSQL数据库理论

## NoSQL

Not Only SQL


可扩展性：
纵向扩展（垂直扩展）：装备升级（成员不增加，提升单位成员的效率）
横向扩展（水平扩展）：增加更多的成员（装备不一定升级；成员之间通讯增加，算力不是线性增长，近线性增长）


为什么NoSQL：
1. 计算和存储可扩展性；
2. 模式（schema）的可自定义；（模式指的是数据库中对象的集合，例如：表、视图、存储过程；元数据，对数据的描述）
3. Web 2.0下关系数据库的事务、范式等特性可放宽。



## NoSQL与数据库间的比较

1. 没有统一的数据模型；
2. 横向扩展；
3. 模式定义灵活；
4. 弱一致性（最终一致性）；
5. 没有标准的查询语言。



## NoSQL数据库分类

键值数据库：Redis、Amazon DynamoDB
列族数据库：HBase、Cassandra
文档数据库：MongoDB；
图数据库：Neo4J

自包含（self-contained）：数据和数据的定义在一起，数据与关联的数据也是在一起。（即无其它依赖项。）
e.g. age:25, name:xiaoming sex:m
     age:28, name:xiaoli sex:f university:kmust



## NoSQL数据库原理

CAP：
一致性（Consistency）：任何一个节点读到（或写到）的某个数据是相同的；
可用性（Availability）：可快速读到（或写到）数据；
分区容忍性（Partition tolerance）：可分区，可容错

e.g.
CA：MySQL、SQL Server
CP：HBase、Neo4J、MongoDB
AP：DynamoDB、Cassandra


BASE：
Basically Avaible：基本可用，允许分布式系统一部分出错
Soft state：软状态，允许有一段时间数据是不一致的

Eventually consistent：最终一致性，保证数据最终是一致的（弱一致性的特例）


给定N（数据副本数）、W（写数据需要完成的节点数）、R（读数据需要完成的节点数）
强一致性：W+R>N，达到强一致性；e.g.HDFS、HBase
弱一致性：W+R<=N,达到弱一致性

事务型应用：OldSQL（传统关系数据库）
互联网应用：NOSQL
分析型应用：NewSQL（关系模型，强一致性、高可扩展性），e.g. Amazon RDS, Micsoft SQL Azure
