---
layout: post
title: tensorflow中变量赋值
categories: tensorflow
description: 给tf.variable赋值
keywords: tensorflow
---

Tensorflow是AI从业人员必须掌握的深度学习框架，其功能强大，越使用，越发现自己懂的仅仅是皮毛。

## 问题描述
最近使用C++写一些基本的神经网络层，如CNN、LSTM等，为了验证这些层计算结果是否正确，需要与tensorfow中的结果进行比较。在验证LSTM时遇到一个问题，tensorflow中LSTM中权重变量并没有提供外部接口，封装死了。像CNN，**tf.nn.conv2d(input, filter, strides, padding)**，输入和权重都可以直接通过接口参数赋值。这就麻烦了，如何给LSTM赋值权重成了一个大问题，网上搜索了很多资料，最终找到了解决方案：通过变量的name定位，使用assign或load赋值。

## 解决方案
###  1. tensorflow中变量定义
```python
# 通过初始化定义变量
tf.Variable(initial_value, trainable=True, collections=None, validate_shape=True, name=None)
# 定义变量，名字有冲突会报错，可通过tf.variable_scope("xx")来定义新的命名空间
tf.get_variable(name,shape=None,dtype=tf.float32,initializer=None, trainable=True, collections=None)
```
### 2. “隔空”定位variable

何谓“隔空”，我定义为神经网络图定义独立的情况，获取变量的方式。可以联想到tensorfow中的tf.Saver，在不给var_list的情况下，是如何保存和恢复变量的？

在tensorflow构建图机制中，所有的变量都会默认加入一个collection（一个变量集合），即tf.GraphKeys.GLOBAL_VARIABLES，而在定义变量中设置trainable=True的变量会加入tf.GraphKeys.TRAINABLE-VARIABLES集合，也可以自己定义集合。tensorfow提供了tf.get-collection()方法来获得变量集合，因此，只要知道变量的完整name，即可从collection中定位到variable。

第二种方案通过共享variable找到变量，在同样命名的scope内，设置reuse=True，则使用tf.get_variable(“xx”)时，会共享同样命名的变量。
```python
# 返回variables列表
var_list = tf.get_collection(key=tf.GraphKeys.GLOBAL_VARIABLES, scope="top")

# 通过共享variable定位
with tf.variable_scope("top", reuse = True):
    x_input_ = tf.get_variable("x_input")
```
### 3. 给variable赋新值
定位到variable后，如何给变量赋值呢？tf.Variable提供assign方法和load方法进行赋值数据，两者有较大差异。

tf.Variable.assign(value, use_locking=False)中value必须是tensor类型，这里可以使用tf.constant(np.arange(6).reshape(2,3), dtype=tf.float32)或tf.convert-to-tensor来将numpy矩阵数据转为tensor类型。
```python
x = tf.Variable(tf.constant(1.0))
x_n = x.assign(tf.constant(2.0))
# 变量使用前一定初始化
sess = tf.Session()
sess.run(tf.global_variables_initializer())
print(sess.run(x)) # 打印1.0
print(sess.run(x_n )) # 打印2.0
print(sess.run(x)) # 打印2.0，只有run(x_n)后x才会赋值生效
```

第二种方法采用更简单的load方法，直接从numpy矩阵中导入数据，立即生效。
```python
x = tf.Variable(tf.constant(1.0))
# 变量使用前一定初始化
sess = tf.Session()
sess.run(tf.global_variables_initializer())
print(sess.run(x)) # 打印1.0
x.load(2.0)
print(sess.run(x)) # 打印2.0，load后不能在执行初始化，否则初始化的值覆盖新值。
```


## 后话
每天掌握一点新知识，加油啦！

## 参考
* <http://www.yolinux.com/TUTORIALS/GDB-Commands.html>