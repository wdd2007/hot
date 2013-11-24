=========================
Spark
=========================

:Author: Gao Peng <funky.gao@gmail.com>
:Description: NA
:Revision: $Id$

.. contents:: Table Of Contents
.. section-numbering::


sbt/sbt update gen-idea 


UseCases
========

- Foursquare

- Yahoo

- taobao

- douban

Scheduling
==========

- Standalone

- Yarn

  MapReduceV2

  JobTracker + TaskTracker => ResourceManager + ApplicationManager + NodeManager


DataSource
==========

Load and save data in Spark.


- RDD

- counters


::

    rs.addFile("spam.data") // populate the file to all nodes in the cluster
    val inFile = sc.textFile("spam.data")


InputFormat
-----------

该interface主要用来定义两件事情：

- 数据分割(Data splits)
- 记录读取器(Record reader)

::

    InputSplit[] getSplits(JobConf job, int numSplits) throws IOException;
    RecordReader<K, V> getRecordReader(InputSplit split, JobConf job, Reporter reporter) throws IOException;

Abstraction
===========

- RDD

  是对分布式内存的抽象使用，实现了以操作本地集合的方式来操作分布式数据集的抽象实现

  它表示已被分区，不可变的并能够被并行操作的数据集合，不同的数据集格式对应不同的RDD实现

  RDD必须是可序列化的，失败自动重建，内存不足时可自动降级为磁盘存储，把RDD存储于磁盘上(spill)

  是静态类型的

  RDD数据集通过所谓的血统关系(Lineage)记住了它是如何从其它RDD中演变过来的，Lineage记录的是粗颗粒度的特定数据转换(Transformation)操作(filter, map, join etc.)行为

- parallel operations

  - Transformations

    map, filter, groupBy, join等

  - Actions

    count, collect, save等
