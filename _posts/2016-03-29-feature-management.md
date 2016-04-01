---
layout: post
title: 机器学习中的特征管理
---

这篇文章和大家探讨一下在机器学习中的特征管理的问题。在大规模机器学习中，特征处理成为非常繁琐并耗时的一步。每一个模型利用到的特征往往存储在不同的存储系统中，比如一些和用户相关的特征由于规模大，往往以文本方式存储在hdfs上，而一些规模较小的特征可能是存储在数据库中。构建训练样本时，希望能够以一种可配置的方式按需获取特征。同时特征有不同的类型，有些特征需要做变换，比如离散化，另外一些特征是通过不同特征组合得到，这些处理都能通过特征管理平台利用注册回调函数等方式实现。下面详细介绍我的实现想法。

特征管理平台
----------------

特征管理平台管理整个公司挖掘的所有特征，以供下游消费特征的用户能方便的利用已有的特征。特征管理平台由数据存储平台、用户浏览界面以及元数据配置组成。特征的元数据配置文件存储在数据库中或在hdfs公共访问的位置。特征的元数据配置文件包含了特征的获取位置，特征相关联的key，特征是否有效，特征的描述，特征的类型等，一个json的例子如下：

	{ 
	  "hive_address": {
	    "user_gender_addr": {
	      "path": "hdfs:///remote/user/gender_addr",
	      "coloum": 2
	     },
	  },
	  "db_address": {
	    "ad_industry_addr": {
	      "db_name": "ad_biz_table",
	      "db_table": "ad_industry",
	      "key_name": "ad_id",
	      "value_name": "industry_id"
	     },
	  }
	  "features": [
	    {
	      "feature_name": "user_gender",
	      "key": "user_cookie_id",
	      "type": "categorical",
	      "address": "user_gender_addr",
	      "description": "predicted user gender info, 0 indicates male, 1 indicates female", 
	      "effective": "1",
	    },
	    {
	      "feature_name": "ad_industry",
	      "key": "ad_id",
	      "type": "categorical",
	      "address": "ad_industry_addr",
	      "description" "predicted advertiser's industry",
	      "effective": "1",
	    },
	  ]
	}

上面的示例用json文件描述特征，对特征进行管理，公司特征挖掘的工作人员产生新的特征需要在特征管理平台注册，在特征管理平台注册会通知所有使用特征的人员有新的可使用特征的加入并且相应的会修改json元文件。

特征处理系统
------------
特征存储在统一的存储平台，特征的使用者可以通过配置组合各种特征，从而能快速测试新特征对模型性能的帮助，以及使得新特征能快速的在生产系统中应用。
（未完待续）
