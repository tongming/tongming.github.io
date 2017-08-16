---
layout: post
title: "tensorflow系列教程第一章"
subtitle:   "tensorflow基本概念"
date:       2017-08-16 12:00:00
author:     "Ming Tong"
catalog: true
tags:
    - tensorflow
---

tensorflow中最重要的几个概念分别是tensor，operation，graph，session。从tensorflow的名字可以看出其主要的计算方式是通过tensor -> flow来计算的，那么tensor在什么上面flow了？你可能已经猜到了，就是graph上面。因此tensorflow的一个基本编程模型就是通过先定义一个graph，然后创建一个session，把graph放到不同的device上进行计算。在这个章节中，通过阅读这篇文章，希望读者能够了解tensorflow的基本概念以及常用的API。

## Tensor
tensor实质上就是一个n维矩阵，tensor里的所有元素类型必须相同，tensor可以作为opertion的输入或输出，在实际编程中，用得更多的是以下几种"tensor"。
- tf.Variable
- tf.Constant
- tf.Placeholder
- tf.SparseTensor 

### Variable
tf.Variable是非常常用的一种Tensor，顾名思义，Varible就是可以改变的Tensor（严谨的说，Variable并不是Tensor的子类，而是内部有一个Tensor的handle）。Variable在程序中一般用来表示我们需要学习的参数。


下面给一个简单的例子，大家直观感受下
```
  import tensorflow as tf
  x = tf.Variable(3, 'x')
  y = tf.Variable(5, 'y')
  a = tf.add(x,y)
  sess = tf.Session()
  init = tf.global_variables_initializer()
  sess.run(init)
  print a
  print a.eval(session=sess)
  sess.close()
```
```
  >> Tensor("Add:0", shape=(), dtype=int32)
  >> 8
```
利用tensorboard生成的图（关于tensorboard，在后面会有专门的一个章节介绍，大家可以先不用去了解）
![avatar](/img/tensor_cha1.png)

首先我们看一下程序的输出，可以看到直接print Variable，不会输出结果，而是一个Tensor，只有运行a.eval，才会输出结果。通过看Variable的eval的代码，可以看出eval实际上是调用session.run函数，因此也可以用sess.run(a)调用。还有一个需要大家注意的地方是tf.global_variables_initializer()，Variable在使用之前需要先初始化，一种简单的方式初始化就是使用tf.global_variables_initializer()，初始化的过程就是把值赋给Variable。初始化也可以直接用Variable的属性initializer

再看一个复杂一点的例子，Variable是一个向量
```
  import tensorflow as tf
  x = tf.Variable([3.2, 2.2], dtype=tf.float32)
  y = tf.Variable([4.1, 6.2], dtype=tf.float32)
  a = x + y

  with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    print(sess.run(a))
```

这个例子是向量的加法，x+y是tf.add(x,y)的语法糖。在上面的例子中如果把a = x+y去掉，把sess.run(a)改为sess.run(x+y)有什么问题么？在这个简单的例子里面没有什么问题，但是偶尔在一些复杂的情况下会使得sess.run(x+y)这种做法会更加耗时。从编程模式来讲，我也推荐最开始把图构建好，之后再去执行。

###Constant
Constant和Variable相反，Constant的值在生命周期中不能修改，常见的创建Constant的函数如下：
- tf.zeros
- tf.zeros_like
- tf.ones
- tf.ones_like
- tf.fill
- tf.constant

前面5个函数，numpy中有类似的函数，我在这里不再详细解释其功能，读者朋友可以自己去查看相关API，我们主要看看tf.constant的功能。先看下面的例子：
```
  import tensorflow as tf
  x = tf.constant(2)
  y = tf.constant(3)
  a = x + y

  with tf.Session() as sess:
    writer = tf.summary.FileWriter("./graphs", sess.graph)
    print(sess.run(a))
```
tensorboard生成的图像如下：
![avatar](/img/tensor_cha1_2.png)
从代码和图中可以看出Constant和Variable的区别，一是Constant不需要initalize，另外Constant被直接保存在Graph中，如果大规模的Constant，那么程序每次load和store模型都会比较耗时，因此一般来讲Constant只用来代表primitive类型。

### Placeholder
