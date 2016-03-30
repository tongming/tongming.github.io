---
layout: post
title: 机器学习中的特征管理
---

这篇文章和大家探讨一下在机器学习中的特征管理的问题。在大规模机器学习中，特征处理成为非常繁琐并耗时的一步。每一个模型利用到的特征往往存储在不同的存储系统中，比如一些和用户相关的特征由于规模大，往往以文本方式存储在hdfs上，而一些规模较小的特征可能是存储在数据库中。构建训练样本时，希望能够以一种可配置的方式按需获取特征。同时特征有不同的类型，有些特征需要做变换，比如离散化，另外一些特征是通过不同特征组合得到，这些处理都能通过特征管理平台利用注册回调函数等方式实现。下面详细介绍我的实现想法。

