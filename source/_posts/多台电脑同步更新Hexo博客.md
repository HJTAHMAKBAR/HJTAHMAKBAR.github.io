---
title: 多台电脑同步更新Hexo博客
date: 2021-06-27 16:28:15
tags: [Hexo, Github Pages, Git]
categories:
- [Hexo]
- [所有]
index_img: https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/hexo.png
---

在利用Hexo+Github Pages写我们的博客的时候，真正的原始Hexo文件在我们的电脑本地，而GitHub上传的只是Hexo生成的静态网页，即public文件夹里面的内容。

那么假如我们有两台电脑工作，Hexo最开始搭建在其中一台电脑上，而我们需要在另外一台电脑上同时更新我们的博客，该怎么做呢？

也就是说，我们需要实现多台电脑间博客项目的迁移与同步。为实现这一点，我们可以利用Git的分支。

# 创建分支

博客搭建好后(博客搭建教程见——[利用Hexo框架从零开始搭建个人博客 - 江客 (jettsblog.top)](https://jettsblog.top/Hexo/利用Hexo框架从零开始搭建个人博客/))，我们在Github上创建分支

![image-20210627092559451](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210627092559451.png)

创建一个名为hexo的分支

# 设置hexo分支为默认分支

![image-20210627093723305](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210627093723305.png)

将博客项目仓库的Settings->Branches->Default branch修改为hexo

![image-20210627093748365](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210627093748365.png)

# 将创建的分支的远程仓库克隆到本地

![image-20210627093914404](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210627093914404.png)

# 删去除.git文件夹以外的所有你内容

1. 进入克隆到本地的仓库

   ![image-20210627094256807](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210627094256807.png)

2. 勾选查看隐藏的文件

   ![image-20210627094400276](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210627094400276.png)

3. 删去除.git文件夹以外的所有文件

4. 在克隆的仓库下分别执行以下命令更新删除操作到远程

   ```
   git add -A
   git commit -m "--"
   git push origin hexo
   ```

   

   ```
   jett@HUAWEI-Huang MINGW64 /d/Github/HJTAHMAKBAR.github.io (hexo)
   $ git branch
   * hexo
   
   jett@HUAWEI-Huang MINGW64 /d/Github/HJTAHMAKBAR.github.io (hexo)
   $ git add -A
   
   jett@HUAWEI-Huang MINGW64 /d/Github/HJTAHMAKBAR.github.io (hexo)
   $ git commit -m "--"
   [hexo 91e562f] --
    42 files changed, 8343 deletions(-)
    delete mode 100644 404.html
    delete mode 100644 CNAME
    delete mode 100644 about/index.html
    delete mode 100644 archives/2021/06/index.html
    delete mode 100644 archives/2021/index.html
    delete mode 100644 archives/index.html
    delete mode 100644 categories/index.html
    delete mode 100644 "categories/\346\211\200\346\234\211/index.html"
    delete mode 100644 css/gitalk.css
    delete mode 100644 css/main.css
    delete mode 100644 img/avatar.png
    delete mode 100644 img/bg/library.jpg
    delete mode 100644 img/bg/pen.jpg
    delete mode 100644 img/bg/planet.png
    delete mode 100644 img/bg/road.jpg
    delete mode 100644 img/bg/skateBoarding.jpg
    delete mode 100644 img/bg/sleepyCat.jpg
    delete mode 100644 img/cover/cover.png
    delete mode 100644 img/cover/helloworld.png
    delete mode 100644 img/default.png
    delete mode 100644 img/favicon.png
    delete mode 100644 img/img/avatar.png
    delete mode 100644 img/loading.gif
    delete mode 100644 img/police_beian.png
    delete mode 100644 index.html
    delete mode 100644 js/boot.js
    delete mode 100644 js/color-schema.js
    delete mode 100644 js/events.js
    delete mode 100644 js/img-lazyload.js
    delete mode 100644 js/leancloud.js
    delete mode 100644 js/local-search.js
    delete mode 100644 js/plugins.js
    delete mode 100644 js/utils.js
    delete mode 100644 lib/hint/hint.min.css
    delete mode 100644 links/index.html
    delete mode 100644 local-search.xml
    delete mode 100644 tags/index.html
    delete mode 100644 "tags/\345\205\266\344\273\226/index.html"
    delete mode 100644 "tags/\351\232\217\346\203\263/index.html"
    delete mode 100644 xml/local-search.xml
    delete mode 100644 "\346\211\200\346\234\211/\344\275\240\345\245\275\344\270\226\347\225\214/helloworld.png"
    delete mode 100644 "\346\211\200\346\234\211/\344\275\240\345\245\275\344\270\226\347\225\214/index.html"
   
   jett@HUAWEI-Huang MINGW64 /d/Github/HJTAHMAKBAR.github.io (hexo)
   $ git push origin hexo
   Enumerating objects: 3, done.
   Counting objects: 100% (3/3), done.
   Delta compression using up to 12 threads
   Compressing objects: 100% (1/1), done.
   Writing objects: 100% (2/2), 186 bytes | 186.00 KiB/s, done.
   Total 2 (delta 0), reused 0 (delta 0), pack-reused 0
   To https://github.com/HJTAHMAKBAR/HJTAHMAKBAR.github.io.git
      c5be9db..91e562f  hexo -> hexo
   ```

5. 将分支克隆到本地的仓库中的.git文件夹复制到博客文件夹中

   ![image-20210627101258655](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210627101258655.png)

6. 在博客目录下执行命令同步到远程的hexo分支

   ```
   git add -A
   git commit -m "备份Hexo(提交的描述)"
   git push origin hexo
   ```

   ```
   jett@HUAWEI-Huang MINGW64 /d/Github/JettsBlog (hexo)
   $ git add -A
   warning: LF will be replaced by CRLF in package.json.
   The file will have its original line endings in your working directory
   warning: LF will be replaced by CRLF in source/_posts/你好世界.md.
   The file will have its original line endings in your working directory
   warning: LF will be replaced by CRLF in source/about/index.md.
   The file will have its original line endings in your working directory
   
   jett@HUAWEI-Huang MINGW64 /d/Github/JettsBlog (hexo)
   $ git commit -m "备份Hexo"
   [hexo 3f03216] 备份Hexo
    24 files changed, 3404 insertions(+)
    create mode 100644 .github/dependabot.yml
    create mode 100644 .gitignore
    create mode 100644 _config.fluid.yml
    create mode 100644 _config.landscape.yml
    create mode 100644 _config.yml
    create mode 100644 package-lock.json
    create mode 100644 package.json
    create mode 100644 scaffolds/draft.md
    create mode 100644 scaffolds/page.md
    create mode 100644 scaffolds/post.md
    create mode 100644 source/CNAME
    create mode 100644 "source/_posts/\344\275\240\345\245\275\344\270\226\347\225\214.md"
    create mode 100644 "source/_posts/\344\275\240\345\245\275\344\270\226\347\225\214/helloworld.png"
    create mode 100644 source/about/index.md
    create mode 100644 source/img/bg/library.jpg
    create mode 100644 source/img/bg/pen.jpg
    create mode 100644 source/img/bg/planet.png
    create mode 100644 source/img/bg/road.jpg
    create mode 100644 source/img/bg/skateBoarding.jpg
    create mode 100644 source/img/bg/sleepyCat.jpg
    create mode 100644 source/img/cover/cover.png
    create mode 100644 source/img/cover/helloworld.png
    create mode 100644 source/img/img/avatar.png
    create mode 100644 themes/.gitkeep
   
   jett@HUAWEI-Huang MINGW64 /d/Github/JettsBlog (hexo)
   $ git push origin hexo
   Enumerating objects: 36, done.
   Counting objects: 100% (36/36), done.
   Delta compression using up to 12 threads
   Compressing objects: 100% (26/26), done.
   Writing objects: 100% (35/35), 13.91 MiB | 777.00 KiB/s, done.
   Total 35 (delta 0), reused 14 (delta 0), pack-reused 0
   To https://github.com/HJTAHMAKBAR/HJTAHMAKBAR.github.io.git
      91e562f..3f03216  hexo -> hexo
   ```

7. 查看hexo分支的仓库

   ![image-20210627101634009](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210627101634009.png)

# 另一台电脑的操作

1. **git** **bash**将远程仓库克隆到本地

   ```
   git clone 仓库地址
   ```

2. 然后进入项目目录，安装依赖启动博客服务器，生成静态文件

   ```
   npm install
   hexo g
   hexo s
   ```

   执行以上指令后，便可以在浏览器通过http://localhost:4000/访问博客

3. 另一台电脑发布文章

   同之前的教程一样，写好文章后

   ```
   hexo clean
   hexo d -g
   ```

# 两台电脑同步写博客

我们的博客仓库有两个分支，*master*分支和*hexo*分支

其中，master分支用于存放Hexo生成的静态资源文件，hexo分支用于存放网站的原始文件

所以，我们在一台设备上写好一篇文章或进行了博客的修改后

执行以下命令，将master中的静态资源文件更新

在博客目录下的cmd中

```
hexo clean
hexo d -g
```

![image-20210627142332846](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210627142332846.png)

执行以下命令，将hexo中的网站原始文件更新

在博客目录下的git bash中

```
git pull
git add -A
git commit -m "描述"
git push origin hexo
```

![image-20210627143023286](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210627143023286.png)

**大功告成！**

# 注意事项

每次有新的操作的时候，别忘了在另一台电脑上更新

```
git pull
```

