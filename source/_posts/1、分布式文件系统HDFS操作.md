---
title: 1、分布式文件系统HDFS操作
date: 2022-11-12 20:40:01
top: 1
tags:
- Big Data
categories:
- Big Data
---

# 分布式文件系统HDFS操作

## Hadoop安装：

Mater Hadoop虚拟机：Linux操作系统，主节点（名字节点，存储管理（元数据），作业（Job）调度））
Slave Hadoop虚拟机：Linux操作系统，从节点（数据节点：数据存储、实际计算）。

建议大家采用Hadoop分布式模式。

## 日志的作用

（记录了对数据增、删、改等操作）：
1）数据恢复
2）提高效率


该示例中：
1）每个数据节点有多少个数据块？
每个数据节点有3-4个数据块 -> 每个数据节点存储的块的个数基本一致 -> 保证负载平衡

2）每个数据块的副本分到了几个数据节点？
每个数据块的副本分到了2个数据节点 -> 2副本冗余（Hadoop一般是3副本冗余）

3）请自己设计另一种存储方案（三个数据节点分布保存1-5数据块，每数据块是2副本）。
DN1：1 2 3 5
DN2：1 3 4
DN3：2 4 5
...

## HDFS

HDFS系统中，一个数据块一般有几个数据副本？
一个数据块一般有3个数据副本。


HDFS的一个数据块默认是多大？
64MB

## Linux常用操作命令

Ctrl + Shift + T：启动命令终端
~ ：用户主目录
/ ：根目录
./：当前目录
../：上一级目录
pwd：查看当前目录名称

ls：查看文件/目录信息
（ll：查看文件/目录详细信息）
cd：进入目录
rm：删除文件
mv：移动文件
cat：查看文件内容
cp：拷贝文件

vi、vim：创建和编辑文件
gedit：创建和编辑文件 （相当于Windows的记事本）

clear：清屏
history：显示历史命令


Hadoop常用操作命令
start-all.sh (Hadoop启动批处理命令，start-dfs.sh, start-yarn.sh)
stop-all.sh（Hadoop结束批处理命令）
jps：查看Java进程
（master共有四个进程，如下：
5022 NameNode
5199 SecondaryNameNode
5345 ResourceManager
5664 Jps

slave共有三个进程：
4794 DataNode
4898 NodeManager
5056 Jps
）



## HDFS常用操作命令

hadoop dfs -ls <path> ：查看指定目录下文件信息
e.g. hadoop dfs -ls /

hadoop dfs -mkdir <path> ：创建指定目录

hadoop dfs -cat <path> ：查看制定文件的内容
e.g. hadoop dfs -cat /input-20200324/hello.txt

hadoop dfs -put <localsrc> <dfsdst>：将本地文件上传到分布式文件HDFS
hadoop dfs -copyfromlocal <localsrc> <dfsdst>
e.g. hadoop dfs -put ./logs/*.* /input-20200324

hadoop dfs -get <dfsdst> <localsrc>：将分布式文件HDFS的指定文件下载到本地文件

hadoop dfs -rm <path>：删除HDFS的指定目录或文件
e.g. hadoop dfs -rm /input-20200324/*.out
