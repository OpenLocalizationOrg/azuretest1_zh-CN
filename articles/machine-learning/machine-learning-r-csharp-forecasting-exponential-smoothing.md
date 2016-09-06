---
ms.openlocfilehash: ab544c9509ef1890bb65544c343795b02e337ed1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="预测指数平滑 |Microsoft Azure" 
    description="Web 服务︰ 预测指数平滑" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2015" 
    ms.author="jaymathe"/> 


#预测-指数平滑 

此[web 服务]( https://datamarket.azure.com/dataset/aml_labs/ets)实现的指数平滑模型 (ETS) 来生成基于用户提供的历史数据的预测。 对特定产品的需求量会增加今年吗？ 可以我预测产品销售的圣诞节期间，以便有效地计划我的库存？ 预测模型是易于解决的问题。 由于过去的数据，这些模型检查隐藏的趋势和季节性预测未来的趋势。  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 
>此 web 服务可由用户 – 可能通过移动应用程序中，通过一个网站，或甚至在本地计算机上，例如消耗。 但是，web 服务的目的也是为作为示例说明如何使用 Azure 机器学习来创建 web 服务在 R 代码。 只需几行 R 代码和单击按钮在 Azure 机器学习 Studio 中，可以使用 R 代码创建并发布为 web 服务实验。 Web 服务可以进行发布到 Azure 市场和遍及世界各地的用户和设备消耗与 web 服务的作者没有基础架构设置。
 
##Web 服务的消耗 
 
此服务接受 4 参数并计算对 ETS 的预测。
输入的参数是︰

* 频率-指示原始数据 （每日/每周/每月/季度/年度） 的频率。
* 地平线-未来预测的时间段内。
* 日期-时间在新的时间序列数据中添加。
* 值-添加新的时间序列数据值中。

该服务的输出是计算的预测的值。

示例输入可能包括︰ 

* 频率-12
* 地平线-12
* 日期-1/15/2012; 2/15/2012年; 3/15/2012; 4/15/2012; 5/15/2012; 6/15/2012; 7/15/2012年; 8 /15/2012年; 9/15/2012; 10/15/2012年; 11/15/2012年; 12/15/2012;1/15/2013; 2/15/2013年; 3/15/2013; 4/15/2013; 5/15/2013; 6/15/2013; 7/15/2013年; 8 /15/2013年; 9/15/2013; 10/15/2013年; 11/15/2013年; 12/15/2013;1/15/2014; 2/15/2014; 3/15/2014; 4/15/2014; 5/15/2014; 6/15/2014; 7/15/2014; 8 /15/2014年; 9/15/2014
* 值-3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511;3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571 3.509;
 
>这项服务，如投放在 Azure 的市场上，是 OData 服务;这些可能通过开机自检或 GET 方法被调用。 

有多种方法使用服务以自动方式 (示例应用程序是[在此处](http://microsoftazuremachinelearning.azurewebsites.net/etsForecasting.aspx))。

###启动 web 服务消耗的 C# 代码︰
    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }



##创建的 web 服务 

>此 web 服务是使用 Azure 机器学习来创建的。 免费试用版，以及介绍性视频创建实验和[发布 web 服务](machine-learning-publish-a-machine-learning-web-service.md)，请参阅[azure.com/ml](http://azure.com/ml)。 下面是创建 web 服务和示例代码为每个模块在实验中实验的一个屏幕快照。

从 Azure 机器学习，在新的空白实验已创建。 上载示例输入的数据与预定义的数据架构。 链接到数据架构是生成预测模型，通过使用 ets 和预测的功能从。 ETS[执行 R 脚本][执行 r 脚本]模块 


![实验流程][2]

####模块 1:
 
    # Add in the CSV file with the data in the format shown below 
![数据示例][3]   

####第 2 单元︰
    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # Fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- ets(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)
    
    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # Data output
    maml.mapOutputPort("data.forecast");

 
##限制 

这是一个非常简单的示例，ETS 预测。 从上面的示例代码中可看出，已实现无错误捕获，和服务假定所有的变量均连续/正数的值，并且频率应大于 1 的整数。 日期和值向量的长度应该是相同的。 Date 变量应遵循 mm/dd/yyyy 的格式。

##常见问题
Web 服务或发布到 Azure 市场消耗的常见问题及解答，请参见[此处](machine-learning-marketplace-faq.md)。

[1]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img1.png
[2]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img2.png
[3]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img3.png


<!-- Module References -->
[执行 r 脚本]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 

测试
