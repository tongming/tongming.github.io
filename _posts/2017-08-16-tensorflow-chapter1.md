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

tensor实质上就是一个n维矩阵，tensor里的所有元素类型必须相同，tensor可以作为opertion的输入或输出，在实际编程中，用得更多的是以下几种"tensor"。
- tf.Variable
- tf.Constant
- tf.Placeholder
- tf.SparseTensor 

下面给一个简单的例子，大家直观感受下
```
  import tensorflow as tf
  a = tf.add(3,5)
  sess = tf.Session()
  print a
  print a.eval()
```
利用tensorboard生成的图（关于tensorboard，在后面会有专门的一个章节介绍，大家可以先不用去了解）
![avatar](/img/tensor_cha1.png)
