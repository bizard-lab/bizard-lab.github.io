---
title: 6、数据仓库Hive理论和实践
date: 2022-11-12 20:53:32
top: 6
tags:
- Big Data

categories:
- Big Data

---

# 数据仓库Hive理论和实践

## Hive

查询接口，提供SQL -> MapReduce的转换。
Hive依托底层的HDFS进行分布式数据存储、MapReduce进行分布式数据处理或计算。


数据仓库：将异构的来自不同应用的数据源进行抽取、转换、加载到中心数据库，以提供决策分析支持。

数据仓库四大特色：面向主题、集成的、时变的、非易失的。

传统的数据仓库面临的挑战：海量数据、数据类型多样、可扩展性有限



## Hive架构

Hive访问接口：命令行（CLI）、Web页面（HWI）、开放数据库连接接口（ODBC或JDBC）、Thrift Server

Driver：SQL -> MapReduce

元数据：MetaStore，存储表的定义等元数据，这些元数据存储在Derby或MySQL等关系数据库中。


Hive QL（Hive 语句）
/etc/init.d/mysqld restart
hive

create database *** ;
show databases;

use database;

create table [if not exists] ... 
[partitioned by ...]
[row format delimited fields terminated by '\t']
location ' ';
e.g. 
create table student(id int, name string, age int) row format delimited fields terminated by '\t' location "/data";

alter table *** add partition (...) 
location '...';

show tables;

load data [local] inpth '' overwrite into table *** ;
e.g.
load data local inpath 'student_data' overwrite into table student;

insert overwrite table *** select *** from ... ;


另一个例子（来自https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.3.4/bk_dataintegration/content/new-feature-insert-values-update-delete.html）：
CREATE TABLE students (name VARCHAR(64), age INT, gpa DECIMAL(3,2)) CLUSTERED BY (age) INTO 2 BUCKETS STORED AS ORC; 

INSERT INTO TABLE students VALUES ('fred flintstone', 35, 1.28), ('barney rubble', 32, 2.32); 

CREATE TABLE pageviews (userid VARCHAR(64), link STRING, from STRING) PARTITIONED BY (datestamp STRING) CLUSTERED BY (userid) INTO 256 BUCKETS STORED AS ORC; 

INSERT INTO TABLE pageviews PARTITION (datestamp = '2014-09-23') VALUES ('jsmith', 'mail.com', 'sports.com'), ('jdoe', 'mail.com', null); 

INSERT INTO TABLE pageviews PARTITION (datestamp) VALUES ('tjohnson', 'sports.com', 'finance.com', '2014-09-23'), ('tlee', 'finance.com', null, '2014-09-21'); 


echo ... > file.txt
e.g. 
echo "Hello Hadoop!" > westart.txt
...
echo "Bye Hadoop!" > wecomplete.txt

hope every guy enjoys our special time ... 
