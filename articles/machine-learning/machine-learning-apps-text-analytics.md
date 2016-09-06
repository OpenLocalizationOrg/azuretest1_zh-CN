---
ms.openlocfilehash: 75f1c6807458e44e162c0cc6ba9d33364dd44090
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="机器学习应用程序︰ 观点分析 |Microsoft Azure"
    description="文本分析 API 是一套内蕴 Azure 机器学习的文本分析。 API 可以用于分析非结构化的文本为主观意图分析和关键短语抽取等任务。"
    services="machine-learning"
    documentationCenter=""
    authors="LuisCabrer"
    manager="paulettm"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2015"
    ms.author="luisca"/>


# 机器学习应用程序︰ 用于分析主观意图的文本分析服务#
##概述
文本分析 API 是一套内蕴 Azure 机器学习的文本分析[web 服务]( https://datamarket.azure.com/dataset/amla/text-analytics)。 API 可以用于分析非结构化的文本为主观意图分析和关键短语抽取等任务。 没有训练数据时需要使用此 API，只需将文本数据。 我们现在只支持英语。 此 API 使用高级的自然语言处理技术，在汽车发动机罩下。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
 
## 观点分析##
该 API 返回 0 和 1 之间的数字得分。 接近于 1 的成绩表示肯定的观点，尽管接近 0 的分数表示否定的观点。 使用分类技术生成主观意图分数。 对分类器的输入的功能包括 n 元语法词，从词性的标记和 word 嵌入生成的功能。
 
## 关键短语抽取##
该 API 返回表示谈话要点中输入的文本的字符串的列表。 我们将使用 Microsoft Office 的复杂的自然语言处理工具包中的技术。

## API 定义##

###GetSentiment###

**URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**示例请求**

在下面 GET 调用中，我们请求的主观意图*Hello World*的短语

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

标头：

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

获取帐户密钥，[此处]( https://datamarket.azure.com/account/keys)。 

**示例响应**

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

###GetKeyPhrases###

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**示例请求**

在下面 GET 调用中，我们请求的主观意图的文本*很奇妙旅馆停留处，具有独特的装饰和友好的员工*中关键的词组

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

标头：

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey)

获取帐户密钥，[此处]( https://datamarket.azure.com/account/keys)。 


**示例响应**

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata","KeyPhrases":[
        "wonderful hotel","unique decor","friendly staff"]
    }
 
---

###GetSentimentBatch###

**URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**示例请求**

在开机自检气氛下，我们正在请求为下面的短语的心声︰ Hello World，Hello Foo 的世界中，你好我世界中请求的正文

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

正文：

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}


标头：

    Authorization: Basic <creds>
    Accept: application/json

    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  


获取帐户密钥，[此处]( https://datamarket.azure.com/account/keys)。 

**示例响应**

在下面响应中，您获得的分数与文本 Id 相关联的列表︰

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata", "SentimentBatch":
        [{"Score":0.9549767,"Id":"1"},
         {"Score":0.7767222,"Id":"2"},
         {"Score":0.8988889,"Id":"3"}
        ],  
        "Errors":[] 
    }


---

###GetKeyPhrasesBatch###

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**示例请求**

在开机自检气氛下，我们正在请求列表的以下文本中关键的词组的心声︰ 

*它是奇妙旅馆停留处，具有独特的装饰和友好的工作人员*
 
*它是非常有趣的讨论与令人惊叹的生成会议*

*通信是可怕的我花了三个小时到机场*



    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

正文：

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

标头：

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey)

获取帐户密钥，[此处]( https://datamarket.azure.com/account/keys)。 


**示例响应**

在下面响应中，您将获得关键的词组与文本 Id 相关联的列表︰

    { "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[ ]
    }

---

**与批处理相关的说明**

输入系统的 Id 是由系统返回的 Id。 Web 服务不检查 Id 都是唯一的。 它负责的调用方可以验证的唯一性。 
 

测试
