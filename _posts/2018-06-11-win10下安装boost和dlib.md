---
layout: post
title: win10下安装boost和dlib
description: 安装软件包
categories: Windows
keywords: boost, dlib 
---

安装软件包

# win10下安装boost和dlib
### 1.安装boost
1. 下载boost源码包并解压；
2. 进入vs2015开发者命令行，并切换目录至boost根目录；
3. 运行boostrap.bat；
4. 在命令行下添加python路径至PATH环境变量：set PAHT=%PATH%;C:\Anaconda3\；
5. 执行b2 -a --with-python address-model=64 toolset=msvc runtime-link=static
6. 添加环境变量：BOOST_ROOT=C:\local\boost_X_XX_X；BOOST_LIBRARYDIR=C:\local\boost_X_XX_X\stage\lib；

### 2.安装dlib python
1. 下载dlib源码并解压；
2. 在dlib根目录下打开cmd；
3. 执行：python setup.py install --yes USE_AVX_INSTRUCTIONS；