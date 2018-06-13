---
layout: post
title: 'Python中setup编写与使用'
subtitle: 'setup'
date: 2018-06-11
categories: Python
tags: setup 
---

# setup.py
## 1.Python模块的安装
1. 单文件模块：直接把文件拷贝到$python_dir/lib
2. 多文件模块，带setup.py：python setup.py install
3. 使用pip或easy_pip安装：pip install module

## 2.setup.py文件的使用
1. python setup.py build #编译
2. python setup.py install    #安装
3. python setup.py sdist     #制作分发包
4. python setup.py bdist_wininst #制作windows下的分发包
5. python setup.py bdist_rpm

## 3.python包管理工具setuptools
setuptools是Python内置的distutils增强版的集合，它可以帮助我们更简单的创建和分发Python包，尤其是拥有依赖关系的。用户在使用setuptools创建的包时，并不需要已安装setuptools，只要一个启动模块即可。

功能亮点：
-  利用EasyInstall自动查找、下载、安装、升级依赖包
-  创建Python Eggs
-  包含包目录内的数据文件
-  自动包含包目录内的所有的包，而不用在setup.py中列举
-  自动包含包内和发布有关的所有相关文件，而不用创建一个MANIFEST.in文件
-  自动生成经过包装的脚本或Windows执行文件

最简单setup.py使用示例：
```
from setuptools import setup, find_packages
setup(
    name = "demo",
    version = "0.1",
    packages = find_packages(),
)
```
1. 执行**python setup.py bdist_egg**即可打包一个test的包了。

2. **python setup.py install --prefix=xx/xxx --record files.txt**
    - --prefix=xx/xxx    选择安装目录
    - --record files.txt   记录所有安装文件的路径
```
$ python setup.py install --record files.txt
$ cat files.txt | xargs rm -rf          #删除这些文件
```

## 4.setuptools进阶
#### 使用find_packages()
它默认在和setup.py同一目录下搜索各个含有__init__.py的包。其实我们可以将包统一放在一个src目录中，另外，这个包内可能还有aaa.txt文件和data数据文件夹。修改setup.py文件：
```
"""
demo
├── setup.py
└── src
    └── demo
        ├── __init__.py
        ├── aaa.txt
        └── data
            ├── abc.dat
            └── abcd.dat
"""
from setuptools import setup, find_packages
setup(
    packages = find_packages('src'),  # 包含所有src中的包
    package_dir = {'':'src'},   # 告诉distutils包都在src下
    package_data = {
        # 任何包中含有.txt文件，都包含它
        '': ['*.txt'],
        # 包含demo包data文件夹中的 *.dat文件
        'demo': ['data/*.dat'],
    }
)
#使用exclude来排除一些包
find_packages(exclude=["*.tests", "*.tests.*", "tests.*", "tests"])
```
#### 使用entry_points
一个字典，从entry point组名映射道一个表示entry point的字符串或字符串列表。Entry points是用来支持动态发现服务和插件的，也用来支持自动生成脚本。例子理解：
```
setup(
    entry_points = {
        'console_scripts': [
            'foo = demo:test',
            'bar = demo:test',
        ],
        'gui_scripts': [
            'baz = demo:test',
        ]
    }
)
# 安装后将会在bin目录下生成foo......脚本文件，如执行foo脚本，将执行test函数；
```

## 5.python包组织形式
在python中， module(也即python的模块)是一个单独的文件来实现的，要吧是py文件，或者pyc文件，甚至是C扩展的dll文件。而对于package, Python使用了文件夹来实现它，可以说，一个文件夹就是一个package,里面容纳了一些py、pyc或dll文件，这种方式就是把module聚合成一个package的具体实现。python中的package必须包含一个__init__.py的文件。

- 搜索的顺序是：当前路径 （以及从当前目录指定的**sys.path**），然后是**PYTHONPATH**，然后是python的安装设置相关的默认路径。
- 当import package时，\_\_init__.py文件中代码将执行；
- 当执行from package import \*时，package对应的**__init__.py**代码中\_\_all\_\_中包含的modules或子包将被import；

python在执行import语句时，到底进行了什么操作，按照python的文档，它执行了如下操作：
- 第1步，创建一个新的，空的module对象（它可能包含多个module）；
- 第2步，把这个module对象插入sys.module中
- 第3步，装载module的代码（如果需要，首先必须编译）
- 第4步，执行新的module中对应的代码。
