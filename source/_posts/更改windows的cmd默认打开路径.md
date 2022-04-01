---
title: 更改windows的cmd默认打开路径
tags:
  - Windows
categories:
  - - Windows
  - - 所有
index_img: https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/R-C.png
date: 2022-04-01 11:49:10
---

1. ### 打开注册表

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220401115105930.png)

2. ### 找到下面的项

```
   HKEY_CURRENT_USER\Software\Microsoft\Command Processor
   或者
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor
   ```
   
3. ### 新增一个字符串

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220401115430752.png)

4. ### 查看效果

   ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20220401115457954.png)

   也可改成其他路径。