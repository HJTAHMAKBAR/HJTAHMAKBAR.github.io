---
title: 关于OracleDBConsoleorcl服务无法启动问题原因分析与解决办法
tags:
  - Oracle
categories:
  - - Oracle
  - - 所有
index_img: 'https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/oracle.png'
date: 2022-03-11 10:33:44
---

## 关于OracleDBConsoleorcl服务无法启动问题原因分析与解决办法

### 问题描述

不久前我在windows电脑上成功安装了Oracle10g，并顺利地启动了OracleDBConsoleorcl并且登上了OEM(Oracle企业管理器)网页。

但是在今天，我发现OracleDBConsoleorcl怎么也没有办法启动，不管是通过管理员命令行输入命令启动还是通过任务管理的服务手动打开。

### 原因分析

经过上网搜索问题相关资料，我发现原因在于OracleDBConsoleorcl服务会根据当前计算机所连接到的ip地址信息来配置一些文件，启动这个服务的时候就会用到这些文件，而问题就出在了这里，每次重新联网后ip地址都会重新动态分配，这样分配的ip和第一次安装oracle时使用的ip信息不一样，所以导致oracleconsoleorcl服务无法启动了。

### 解决办法

1. 删除资料档案库

   会同时删除OracleDBConsoleORCL服务，并删除用户SYSMAN及其所属对象

2. 重新创建资料档案库

   会重新添加OracleDBConsoleORCL服务，并创建SYSMAN用户及其所属对象。

### 解决步骤(方法1)

1. 此时Oracle的监听器服务和数据库服务必须处于启动状态

2. 以管理员身份运行CMD

   ```shell
   emca -repos drop
   ```

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220305201200469.png)

   输入以下信息:

   - 数据库 SID: orcl
   - 监听程序端口号: 1521
   - SYS 用户的口令:
   - SYSMAN 用户的口令:
   - SYSMAN 用户的口令:
   - 是否继续? [是(Y)/否(N)]: y

3. 启动数据库配置助手

   ```shell
   dbca
   ```

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220305201351570.png)

   - 第一步时选择“配置数据库选件”

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220305201554141.png)

   - 后面的操作只需要一直点击下一步
   - 在选择是否使用EM资料档案库时，一定要选中，默认没有选中

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220305201616361.png)

   - dbca检查到系统中已经没有EM资料档案库，于是就会重新创建
   - OracleDBConsoleORCL服务也会重新添加了

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220305202803186.png)

![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220305204411971.png)

### 解决步骤(方法2)

重新配置

```
emca -config dbcontrol db -repos recreate
```

启动

```
emctl start dbconsole
```

