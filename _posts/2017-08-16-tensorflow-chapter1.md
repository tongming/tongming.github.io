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

  tensorflow中最重要的几个概念分别是tensor，Variable，graph，session。从tensorflow的名字可以看出其主要的计算方式是通过tensor -> flow来计算的，那么tensor在什么上面flow了？你可能已经猜到了，就是graph上面。因此tensorflow的一个基本编程模型就是通过先定义一个graph，然后创建一个session，把graph放到不同的device上进行计算。在这个章节中，通过阅读这篇文章，希望读者能够了解tensorflow的基本概念以及常用的API。

