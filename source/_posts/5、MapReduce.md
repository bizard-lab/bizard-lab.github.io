---
title: 5、MapReduce
date: 2022-11-12 20:52:23
top: 5
tags:
- Big Data

categories:
- Big Data

---

# MapReduce

## MapReduce与传统并行编程框架的区别

MapReduce可扩展性好，容错性好、廉价、编程容易（数据划分、任务分发、数据通讯、负载均衡等通用任务自动处理）、数据密集型计算（不是计算密集型应用）、移动计算而不是移动数据


补充：CPU、内存、磁盘的组合：共享内存（多个CPU共用一块内存）、共享磁盘（多个CPU共用一块磁盘）、非共享（每个CPU都独享内存和磁盘）、计算存储分离

分布式并行编程框架
MPI（Message Passing Interface，消息传递接口）：多进程通讯标准，主要有6个标准函数；
PThread：多线程并行编程框架；
编译制导：OpenMP

MapReduce体系结构
一主多从
Client：客户端，提交分布式应用程序给主节点（的JobTracker）；
JobTracker+Task Scheduler：在主节点运行的作业跟踪和调度（一个Job有多个Task，一个Task可以是Map计算任务或者Reduce计算任务）
TaskTracker：在从节点运行的具体任务执行（通过CPU、内存资源槽（slot）来分配Map或Reduce计算任务）

## MapReduce工作流程

主要分为四个阶段
（数据先上传到HDFS）
1）Split：对数据进行分片 -> (key, value)；

2）Map：每个数据分片上启动一个Map计算任务，计算该数据分片的数据；
(key, value) -> list(out_key, intermediate_value)

3）Shuffle：对数据进行混排，类似group by；
(out_key, intermediate_value) -> (out_key, list(intermediate_value))

（Combine：
           对相同out_key的值提前进行合并，即提前进行本地的Reduce，从而有效减少Shuffle的输出
           Map之后，Shuffle之前进行的本地Reduce操作）

4）Reduce：对混排后的数据进行聚集、约减
(out_key, list(intermediate_value)) -> (out_key, out_value) (其中out_value = Agg(list(intermediate_value))
（最终结果HDFS文件系统）

补充：分区一般分为：范围分区、哈希分区、列表分区
Shuffle：计算->溢写（分区、排序、合并（combine））->合并（多个文件）

Hadoop+Eclipse/IntelliJ IDEA（从而可以在IDE上用Java进行分布式计算应用程序开发，如：WordCount）



## Hadoop MapReduce框架中的数据类型

字符串：Text
整型：IntWritable （长整型：LongWritable）
浮点型：FloatWritable, DoubleWritable
布尔型：BooleanWritable
空类型：NullWritable

Writable：需要序列化和反序列化

序列化：A: object -> byte -> B: object ：反序列化


如何用Java程序定义Wordcount的map和reduce的输入和输出，即它们的输入输出类型和含义分别是什么？
map()
输入：<Object row, Text value>
输出：<Text word, IntWritable one>

reduce()
输入：<Text word, List(IntWritable value)>
输出：<Text word, IntWritable count>



## Wordcount部署

1）编译并打包（可用Eclipse的export打为jar包或命令行模式javac和jar进行编译和打包 ）；
（编译需要的Hadoop jar包：hadoop-common-***.jar, hadoop-mapreduce-core-***.jar, commons-cli-**.jar）

2）运行Hadoop Wordcount程序；（先要将数据上传到HDFS中）
（运行命令：hadoop jar WordCount.jar WordCount input output）

3）（到HDFS中）查看结果。
