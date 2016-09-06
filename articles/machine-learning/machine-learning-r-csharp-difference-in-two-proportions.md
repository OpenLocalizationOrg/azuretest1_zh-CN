---
ms.openlocfilehash: 381a8c9d020ecc7785361ce6d87ef5d55cffb8e0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="差额比例测试 |Microsoft Azure" 
    description="在测试中比例的差异" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
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


#在测试中比例的差异


两个比例以统计方式不同？ 假设用户想要比较两个电影来确定一个影片是否有更高比例的喜欢时与其他比较。 与较大的样本，可能是 0.50 和 0.51 比例之间存在统计上显著差异。 与一个小的示例中，可能没有足够的数据以确定是否这些比例实际上是不同的。 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

此[web 服务]( https://datamarket.azure.com/dataset/aml_labs/prop_test)进行假设检验的两个基于用户输入的成功次数和试验比较 2 组总数的比例上的差异。 一个可能的方案，在此 web 服务可以从调用中影片比较应用程序，告诉用户是否电影之一真正 '喜欢' 比其它，通常基于电影分级。

>此 web 服务可由用户 – 可能通过移动应用程序中，通过一个网站，或甚至在本地计算机上，例如消耗。 但是，web 服务的目的也是为作为示例说明如何使用 Azure 机器学习来创建 web 服务在 R 代码。 只需几行 R 代码和单击按钮在 Azure 机器学习 Studio 中，可以使用 R 代码创建并发布为 web 服务实验。 Web 服务可以进行发布到 Azure 市场和遍及世界各地的用户和设备消耗与 web 服务的作者没有基础架构设置。


##Web 服务的消耗

此服务接受 4 参数并没有假设测试的比例。

输入的参数是︰

* Successes1-的成功示例 1 中的事件数。
* Successes2-的成功示例 2 中的事件数。
* Total1-1 的示例的大小。
* Total2-2 的示例的大小。

该服务的输出是假设的结果 chi-square 统计，df，p-值，以及测试和示例 1/2 和置信区间边界中的比例。

>这项服务，如投放在 Azure 的市场上，是 OData 服务;这些可能通过开机自检或 GET 方法被调用。 

有多种方法使用服务以自动方式 (示例应用程序是[在此处](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx ))。

###启动 web 服务消耗的 C# 代码︰

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
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

从 Azure 机器学习，在新的空白试验是用创建的两个[执行 R 脚本][执行 r 脚本]模块。 在第一个模块中定义了数据架构，而第二模块使用 R 内的 prop.test 命令进行假设检验，对 2 的比例。 


###实验流程︰

![实验流程][2]


####模块 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####第 2 单元︰

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##限制 

这是一个非常简单的示例 2 比例差额的测试。 从上面的示例代码中可看出，实现无错误捕获和服务将假定所有的变量均连续。

##常见问题
Web 服务或发布到 Azure 市场消耗的常见问题及解答，请参见[此处](machine-learning-marketplace-faq.md)。

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[执行 r 脚本]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 

测试
