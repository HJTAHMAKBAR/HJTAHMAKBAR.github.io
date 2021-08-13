---
title: VSCode配置C++环境超简单过程
date: 2021-06-30 00:28:09
tags: [c++, VSCode]
categories:
- [C++]
index_img: https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/vscode.png
---

# 在VSCode中写C++的相关配置

## 懒人福音

[20秒 一键配置 VSCode (Visual Studio Code) C/C++开发环境 !_12 26 25 的博客-CSDN博客](https://blog.csdn.net/qq_41523096/article/details/104628484)

参考上述教程配置好VSCode后，便可以使用VSCode写C++程序了，但是这样的配置会导致.cpp代码文件和.exe二进制可执行文件全部都与.vscode文件夹放在一起，显得十分的混乱而不整洁。

## 强迫症福音

通过对配置文件的一些修改便可以实现cpp代码文件与exe程序文件分离，显得更加的工整。

在.vscode同级目录下建立exe文件夹用于存放编译生成的可执行文件

在.vscode同级目录下建立代码文件夹用于存放cpp文件

1. task.json

   ```
   {
       "version": "2.0.0",
       "command": "g++",
       "type": "shell",
       "presentation": {
         "echo": true,
         "reveal": "always",
         "focus": false,
         "panel": "shared",
         "showReuseMessage": true,
         "clear": false
       },
       "args": [
         "-m32",
         "-g",
         "${file}",
         "-o",
         //"${fileDirname}/${fileBasenameNoExtension}.exe"
         "${workspaceFolder}/exe/${fileBasenameNoExtension}.exe"],
       "problemMatcher": {
         "owner": "cpp",
         "fileLocation": ["relative", "${workspaceRoot}"],
         "pattern": {
           "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
           "file": 1,
           "line": 2,
           "column": 3,
           "severity": 4,
           "message": 5
         }
       }
   }
   ```

   

2. launch.json

   ```
   {
       "version": "0.2.0",
       "configurations": [
           {
               "name": "(gdb) Launch",
               "type": "cppdbg",
               "request": "launch",
               "targetArchitecture": "x86",
               //"program": "${workspaceFolder}/exe/${fileBasenameNoExtension}.exe",
               "program": "${fileDirname}/${fileBasenameNoExtension}.exe",
               "miDebuggerPath": "D:/MinGW/bin/gdb.exe",
               "args": [],
               "stopAtEntry": false,
               "cwd": "${fileDirname}",
               "externalConsole": true,
               "preLaunchTask": "g++"
           }
       ]
   }
   ```

## 实现效果

![image-20210630000926160-1624984786477](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210630000926160-1624984786477.png)

![image-20210630000945867](https://jett-image-host.oss-cn-shanghai.aliyuncs.com/img/image-20210630000945867.png)