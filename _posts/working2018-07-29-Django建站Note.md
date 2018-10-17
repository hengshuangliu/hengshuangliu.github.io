---
layout: post
title: Django建站Note
categories: Python
description: 学习Django建站
keywords: Python, Django
---
周末闲暇之余学习网易云课堂之Django零基础项目实战。

## virtualenv安装python虚拟环境
* pip install virtualenv（anaconda下conda install virtualenv）
* vitualenv env-name （在当前目录下创建虚拟环境）
* virtualenv -p /usr/bin/python2.7 env-name（指定python版本创建虚拟环境）
* source env-name/bin/activate （win下执行activate脚本即可）
* deactivate （退出当前的虚拟环境）

## virtualenvwrapper
virtualenv使用过程中需要找寻虚拟环境路径，麻烦！virutalenvwrapper则统一管理，不需要找寻路径！（基于virtualenv）
* pip install virtualenvwrapper（win下virtualenvwrapper-win）
* mkvirtualenv env-name （特定目录下创建虚拟环境，统一管理）
* mkvirtualenv --python=/path/python.exe env-name（指定python版本）
* workon env-name （进入虚拟环境）
* deactivate （退出）
* lsvirtualenv（列出当前存在的虚拟环境）
* rmvirtualenv env-name（删除虚拟环境）

## Django

