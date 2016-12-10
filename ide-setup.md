---
title: 搭建Spark源码研读和代码调试的开发环境
date: 2016-12-10 10:00:00
tags: spark-notes
---

搭建Spark源码研读和代码调试的开发环境
-------

工欲善其事，必先利其器，第一篇笔记介绍如何搭建源码研读和代码调试的开发环境。
一些必要的开发工具，请自行提前安装：

* scala 2.11.8
* sbt 0.13.12
* maven 3.3.9
* git 2.10.2
* IntelliJ IDEA 2016.3 (scala plugin) 

## 源码获取与编译

### 从Github上获取Spark源码
可以直接从Spark官方Github仓库拉取。本系列笔记基于**Spark 2.0.2**这个版本，所以先checkout这个tag，再进行之后的步骤：

```bash
$ git clone git@github.com:apache/spark.git
$ cd spark
$ git tag
$ git checkout v2.0.2 
$ git checkout -b pin-tag-202
```

如果想要push自己的commits，也可以fork到自己的Github账号下，再拉取到本地，可以参考我之前的文章：[Reading Spark Souce Code in IntelliJ IDEA](https://linbojin.github.io/2016/01/09/Reading-Spark-Souce-Code-in-IntelliJ-IDEA/)

### 编译Spark项目
参考[官方文档](https://github.com/apache/spark#building-spark)，编译很简单，这里使用4个线程，跳过tests，以此加速编译。这个编译会产生一些必要的源代码，如Catalyst项目下的，所以是必要的一步：

```bash
$ build/mvn -T 4 -DskipTests clean package
# 编译完成后，测试一下
$ ./bin/spark-shell
```
 
## 导入源码到Intellij IDEA 16
现在IDEA对scala支持已经比较完善，导入Spark工程非常简单：
> Menu -> File -> **Open** -> {spark dir}/**pom.xml** 
















	
Useful IDEA Shortcuts:

	command + o : search classes
	command + b : go to implementation
	command + [ : go back to the previous location
	shift + command + F : search files 

Several important classes:

	SparkContext.scala 
	DAGScheduler.scala
	TaskSchedulerImpl.scala
	BlockManager.scala



