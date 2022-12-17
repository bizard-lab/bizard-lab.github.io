---
title: 2、分布式数据库HBase理论
date: 2022-11-12 20:49:48
top: 2
tags:
- Big Data
categories:
- Big Data
---

# 分布式数据库HBase理论

## 介绍

HDFS：分布式文件系统。它是Google的GFS的开源实现。

HBase：分布式数据库，存储半结构化或非结构化数据，提供了高效访问。它是Google的BigTable的开源实现。

Zookeeper：分布式协调管理框架。它是Google的Chubby的开源实现。

Hadoop MapReduce：分布式计算框架。它是Google的MapReduce的开源实现。

## 大数据/云计算计算模式

批处理计算（离线计算）：小时级响应, e.g. Hadoop (HDFS, MapReduce)
交互计算（在线计算）：秒级、分钟级响应 
实时计算（流数据计算）：毫秒、秒级响应 e.g. HBase



## HBase与传统数据库

传统数据库的问题（两个可扩展性差）：
1）计算可扩展性差，例如：Oracle RAC扩展多达到100台机器。
2）数据库定义可扩展性差，例如：增加列属性不便且有限。



## HBase访问接口

1）Java API
2）Shell命令
3）SQL语句：Hive，Pig



## HBase数据模型

传统关系数据库：（行、列）-> （单元格）值
HBase：（行键、列族、列（限定符）、时间戳）-> （单元格）值

e.g.

id=20 姓名：张三（time stamp 1）

id=20 姓名：李四 （time stamp 2）

## 存储方式

行存储（Row Storage）：   高事务处理效率        适用OLTP（联机事务处理），e.g.传统数据库
列存储（Column Storage）：高压缩率、高查询效率，适用OLAP（联机分析处理），e.g. HBase
HTAP（混合行列存储）：

e.g.
25 
28
30
32
36

run length code：25 3 2 2 4

select xh, name from student;



## HBase实现

一主多从架构,例如：
HDFS（NameNode， DataNodes），
HBase（HMaster， HRegionServer），
MapReduce（Master， Slaves）

HBase的表由多个Region组成。

HBase的寻址访问：
.Meta：Data Region id -> Region Server
.Root：Root id -> Meta Region id

形成了三级寻址访问结构：
.Root -> .Meta -> Data



## HBase架构

客户端：提供HBase的访问接口；
Zookeeper：提供集群的协调管理服务；
HMaster：元数据管理；数据的管理（增删改查请求，负载均衡、Region的管理）
HRegionServer：提供了实际数据的读写访问

HRegionServer存储结构：
包括10 - 1000个Region和一个HLog，每个Region包括：MemStore、StoreFile（HFile）



## 启动HBase命令

start-hbase.sh
stop-hbase.sh

确认启动成功：
jps
Web UI：http://master:60010


hbase shell：进入hbase的shell交互界面
exit：退出hbase的shell交互界面

create：创建表，create '表名','列族名1','列族名2','列族名N'

put：添加记录，put '表名','rowkey','列族：列'，'值'

get：查看记录rowkey下的所有数据，get '表名','rowkey'
（
get：获取某个列族，get  '表名','rowkey','列族'
get：获取某个列族的某个列，get '表名','rowkey','列族：列'）

list：查看所有表	
desc：描述表信息，desc '表名'
scan：查看所有记录，scan '表名'
（scan：查看某个表某个列中所有数据，scan '表名',{COLUMNS=>'列族名：列名'}）

count：查看表中的记录总数，count '表名'

delete：删除记录，delete '表名','行名','列族：列'
（删除整行，deleteall '表名','rowkey'）
drop：删除一张表	
（先要屏蔽该表，才能对该表进行删除：第一步 disable '表名'，第二步 drop '表名'）
truncate：清空表，truncate '表名'


更新记录	就是重新一遍，进行覆盖，hbase没有修改，都是追加

exists：判断表存在，exists '表名'

is_enabled / is_disabled：判断是否禁用启用表
is_enabled '表名'
is_disabled '表名'

status：查看hbase状态
