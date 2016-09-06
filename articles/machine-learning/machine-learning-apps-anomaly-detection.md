---
ms.openlocfilehash: 75249b25491bfa1b73259b154091482668cd8563
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="机器学习应用程序︰ 异常检测服务 |Microsoft Azure" 
    description="异常检测 API 是使用 Microsoft Azure 机器学习在时间均匀的数值与时间序列数据中检测到异常生成一个示例。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="paulettm"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="reference" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="07/28/2015" 
    ms.author="luisca"/>


# 机器学习异常检测服务#

##概述

异常检测 API 是内蕴 Azure 机器学习在时间均匀的数值与时间序列数据中检测到异常情况的一个示例。 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

此异常检测服务可以检测到以下不同类型的时间序列数据的异常︰

1. 积极的和消极的趋势︰ 在监视内存使用情况，在计算时，例如，上升趋势是表明存在内存泄漏，

2. 提高动态范围的值中︰ 例如，当监视服务引发异常时，动态范围的值中的任何增加可能表示的服务，健康状况不稳定，

3. 高峰和低谷︰ 例如，在监视服务的登录失败次数或电子商务站点中签出的数目时，峰值或 dip 可能表示异常行为。


这些检测器跟踪值随时间的变化，并报告它们的值中进行的更改。 它们不需要临时调整阈值，其分值可以用于控制误报率之所以。 异常检测 API 是非常有用的几个像随着时间的推移，服务监视通过跟踪文件中读取段时间后，使用指标，如搜索、 点击数、 内存、 cpu、 类似的性能计数器的 Kpi，等等。 

##API 定义

该服务提供其余部分基于 API 通过 HTTPS，可以以不同的方式，包括 web 或移动应用程序，R，Python，Excel，消耗等。我们拥有一个[Azure 的 web 应用程序](http://anomalydetection-aml.azurewebsites.net/)，可帮助对数据进行异常检测 web 服务并显示结果。 

您还可以通过 REST API 调用，此服务发送时间系列数据，并且它运行上面介绍的三种异常类型的组合。 此服务运行在 AzureML 机器学习平台，可扩展到您的业务需要无缝地，并提供了 99.95%的 Sla。

下图显示检测到的异常的示例时间系列使用以上的框架。 时间序列具有 2 不同级别更改和 3 高峰。 红点显示的级别检测到更改，而红色的向上箭头显示检测到的高峰时间。


![][1]

##输入

该 API 采用 2 输入的参数 

1. "数据"表示的格式输入的时间系列︰ t1 = v1; t2 = v2;... 

 
    示例数据︰ 
        
        "9/21/2014 11:05:00 AM=3;9/21/2014 11:10:00 AM=9.09;9/21/2014 11:15:00 AM=0;"

2. "参数"设置为"SpikeDetector.TukeyThresh=3;SpikeDetector.ZscoreThresh=3"哪些控件峰值检测器的灵敏度。 较高的值会捕捉更高的峰值，反之亦然。 

    示例使用上面提到的输入参数的 URL:

        https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v1/Score?data=%279%2F21%2F2014%2011%3A05%3A00%20AM%3D3%3B9%2F21%2F2014%2011%3A10%3A00%20AM%3D9.09%3B9%2F21%2F2014%2011%3A15%3A00%20AM%3D0%3B%27&params=%27SpikeDetector.TukeyThresh%3D3%3B%20SpikeDetector.ZscoreThresh%3D3%27



###Output###

API 在时间系列数据上运行这些检测器，并及时返回的每个点的异常分数。 可以使用[https://adresultparser.codeplex.com/](https://adresultparser.codeplex.com/)中所示的简单分析器分析此输出。 这样的示例代码，演示如何连接到 API 和分析输出。 生成的警报可以在仪表板上可视化和/或传递给人类的专家可以采取纠正措施。

上面的示例输入输出的示例︰ 

    "Time,Data,TSpike,ZSpike,Martingale values,Alert indicator,Martingale values(2),Alert indicator(2),;9/21/2014 11:05:00 AM,3,0,0,-0.687952590518378,0,-0.687952590518378,0,;9/21/2014 11:10:00 AM,9.09,0,0,-1.07030497733224,0,-0.884548154298423,0,;9/21/2014 11:15:00 AM,0,0,0,-1.05186237440962,0,-1.173800281031,0,;"

它是表示形式如下表︰

时间|Data|Tspike|Zspike|鞅值|警报指示符|鞅值 (2)|警报指示符 (2)
---|---|---|---|---|---|---|---
9/21/2014 11:05|3|0|0|-0.687952591|0|-0.687952591|0|   
9/21/2014 11:10|9.09|0|0|-1.070304977|0|-0.884548154|0|    
9/21/2014 11:15|0|0|0|-1.051862374|0|-1.1738002814|0|   
   

[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection.jpg

 

 

测试
