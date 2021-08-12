---
title: 关于Hexo写博客时的图片插入问题
tags:
  - Hexo
  - OSS
  - 阿里云
categories:
  - - Hexo
  - - 所有
index_img: /img/cover/helloworld.png
date: 2021-08-12 16:14:35
---

#### 关于MarkDown

`MarkDown`是一种轻量级标记语言，它可以导出`HTML`、`WORD`、`图像`、`PDF`、`EPUB`等多种格式的文档。使用`MarkDown`写文章有如下好处：

- MarkDown可以在任何地方使用，兼容`MacOS`、`Windows`、`Linux`等平台
- 博客网站比如简书、`GitHub`、`CSDN`，`GitChat`等都支持`MarkDown`，所以只需要写一个`MarkDown`文件，并通过将文件上传至各平台就可以实现在多个博客网站发表文章
- 专注文字内容而不是排版样式，不同于`Word`等软件需要考虑设置排版
- 正宗的官方技术文档都是使用`MarkDown`书写的，使用`MarkDown`写文章有助于熟悉其语法，提升自己写文档的能力
- 写作的过程也是思考的过程，写文章不仅有助于加强自己对所涉及内容的进一步熟悉与认知，还能提升自己的语言组织能力与写作水平

综上所述，收益无穷！

#### 使用MarkDown的相关问题

在写文章的时候，我们可以插入图片和相关资源使得文章更加丰富生动，但是在本地写`MarkDown`文件时，上传的本地图片是无法实现在网络上或是在另外一台设备上访问的。

![image-20210812164957225](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210812164957225.png)

比如上面这张图片是我截图然后粘贴的，文件存放在电脑本地，如果直接发布到`Hexo`自己的博客网站，或者是`CSDN`，`简书`等博客平台是无法访问的，又或者是我们将MarkDown文件发给别人时，对方也是无法查看插入的本地图片的。

那么在使用`Hexo`框架写我们的文章时，怎么实现在网络上看到我们插入的图片呢？这里我主要介绍两种方法，分别都是我实现过的，在这里总结经验。

#### 办法一：同名目录 + 相对路径

1. 在使用`Hexo`写博客时，每一篇文章创建一个同名目录，并在这个同名目录中放置需要插入的图片文件，在`MarkDown`文章中插入使用图片的相对路径，然后在部署博客的时候也一同将这个文件夹上传到服务器。

   1. 修改配置文件`_config.yml`使得每一次新建文章的时候自动生成同名目录。
      ![image-20210812170117912](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210812170117912.png)

      ```
      post_asset_folder: true
      ```

   2. 修改MarkDown编辑器配置，实现粘贴图片时自动将图片文件复制一份到所对应的同名目录。

      我使用的Typora编辑MarkDown文件，通过`ctrl + ,` 打开Typora的设置，并按图片中完成相关设置。

      ```
      ./${filename}
      ```

      ![image-20210812171134932](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210812171134932.png)

      完成这样的设置后，每当我们在`Typora`中粘贴图片的时候就会将图片复制到同名目录下。

      ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/20210812171437.png)

      但是需要注意的是，这样上传到`Hexo`依旧不能解决图片无法访问的问题，因为图片的路径还是本地的路径，只是这个图片所属的本地的文件夹是和文章同名的。

      ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/20210812171655.png)

      解决办法：将绝对路径变成相对路径，这样在`Hexo`服务器上就会自动的在与文章同名的那个文件夹里面找图片文件，所以只需要删去前面的一部分，只留下图片的文件名。

      ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/20210812172606.png)

      由于当前`MarkDown`文件内的所有插入的图片都自动地放在一个文件夹里，所以他们的路径的前缀都是一样的，可以使用`Typora`的快捷操作将前缀全部替换成空白。

      具体方法：`ctrl + H`打开替换

      ![](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/20210812172540.png)

      总结这个方法：

      1. 本地看不到图片的效果，但是上传至`Hexo`后是可以查看的，能解决`Hexo`上查看本地图片的问题。
      2. 但是仍然不能解决`Hexo`以外的诸多博客平台的本地图片访问问题，对于`CSDN`、简书来说它都有自己的图床，所以可以上传到对应平台的图床，然后用网络的链接访问图片。
      3. 使得`Hexo`文件变得臃肿，随着博客文章的积累，用到的图片会在每次部署和上传到服务器时一并被携带上传，造成部署和上传的压力，使得上传的速度变得很慢。
      4. 由于博客部署在`GitHub Pages`，就会导致加载博客文章时，一大部分时间用于请求`Github Pages`服务器加载图片文件，再加上`Github Pages`服务器在大陆访问有时很慢的情况，使得博客网页加载奇慢无比。

      综上，这个办法并不是长久有效的。

#### 方法二：OSS对象存储 + PicGo（图床）

这个方法其实就是自己搭建一个`图床`，当然你也可以将图片放在网上的各种免费的图床上。

但是我们需要面临一些问题

1. 网络图床有可能突然就不维护了，那么我们放的图片也就不翼而飞了，而如果你的图片又没有备份的话，那么所有用到这些图片的文件都会被涉及到，这些图片也就访问不了，消失在网络的大海里。
2. 免费的图床有额度限制，一般可用的空间不会太大。
3. 本地写文章时不够简便，需要每张图片手动上传。

为了解决上诉问题，可以使用OSS对象存储配合`PicGo`自己搭建一个图床供自己使用，岂不美哉。

关于`OSS`的概念就不介绍了，可以理解为利用它创建一个自己的云盘，然后通过链接访问这些资源。

具体可在阿里云的`OSS`介绍里了解：[阿里云帮助中心-阿里云，领先的云计算服务提供商 (aliyun.com)](https://help.aliyun.com/product/31815.html)

1. ##### 购买OSS服务

   ![image-20210812180005563](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210812180005563.png)

   博客加上日常使用的话40G就够了，而且9块钱一年，可以说并不贵。

   ![image-20210812180045807](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210812180045807.png)

2. ##### 创建Bucket

   ![image-20210812180159497](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210812180159497.png)

3. ##### 下载PicGo

   下载地址：[Releases · Molunerfinn/PicGo (github.com)](https://github.com/Molunerfinn/PicGo/releases)

4. ##### 配置PicGo

   ![image-20210812180706635](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210812180706635.png)

5. ##### 配置Typora

   在设置里面将插入图片时的操作设置为上传图片，然后上传服务设置为PicGo，那么每次粘贴图片的时候就会自动的上传到图床上，并生成网络连接

   ![image-20210812180816894](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210812180816894.png)

6. ##### 大功告成

   自动上传的文件

   ![image-20210812181038227](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210812181038227.png)

   生成的链接

   ![image-20210812181115779](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210812181115779.png)

#### 总结

1. 使用自己搭建图床的方法可以长久的存储自己的资源
2. 操作更加的简单
3. 会产生一些费用，不过轻量使用的话，一年要不了多少钱
4. 后续还可以配合自己的域名使用，不过考虑到域名备案的问题，我并没有这样实现。
5. 欢迎访问我的博客！[江客 (jettsblog.top)](https://jettsblog.top/)