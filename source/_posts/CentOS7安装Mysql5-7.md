---
title: CentOS7安装Mysql5.7
tags:
  - MySQL
categories:
  - - MySQL
  - - 所有
index_img: 'https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/centos.png'
date: 2022-02-25 11:13:08
---

### CentOS安装Mysql5.7

1. ##### 下载并安装Mysql官方的 Yum Repository

   ```
   wget http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
   ```

2. ##### 安装MySQL的软件仓库

   ```
   rpm -ivh mysql57-community-release-el7-11.noarch.rpm
   ```

3. ##### 安装MySQL服务

   ```
   yum -y install mysql-server
   ```

   **可能遇到的问题**

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220225112053832.png)

   **问题描述：**源MySQL 5.7 Community Server” 的 GPG 密钥已安装，但是不适用于此软件包。

   **解决办法：**

   1. 执行以下命令：

      ```
      rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
      ```

   2. 重试安装命令

      ```
      yum -y install mysql-server
      ```

4. ##### 安装成功

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220225112334882.png)

### 启动Mysql服务

1. ##### 启动命令

   ```
   systemctl start  mysqld.service
   ```

2. ##### 查看MySQL运行状态

   ```
   systemctl status mysqld.service
   ```

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220225112442932.png)