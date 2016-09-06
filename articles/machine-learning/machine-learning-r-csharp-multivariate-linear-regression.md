---
ms.openlocfilehash: 740938aeec281a1f803c6dbf325ca86943eb0622
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="多变量线性回归 |Microsoft Azure" 
    description="多变量线性回归" 
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


#多变量线性回归   
 

 
假设您有一个数据集并且想要快速预测因变量 (y) 的每个人 (i) 根据自变量。 线性回归被用于此类预测的常用统计技术。 此处假定因变量 y 是一个连续的值。  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

一个简单的方案可能是研究人员正试图预测个人 (y) 根据其高度 (x) 的重量。 其中研究人员有附加信息 （如重量、 性别、 种族等） 的个人，尝试预测个人的重量，可能是更高级的方案。 此[web 服务]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression)适合于数据的线性回归模型，并为每个数据的观测值输出预测的值 (y)。

>此 web 服务可由用户 – 可能通过移动应用程序中，通过一个网站，或甚至在本地计算机上，例如消耗。 但是，web 服务的目的也是为作为示例说明如何使用 Azure 机器学习来创建 web 服务在 R 代码。 只需几行 R 代码和单击按钮在 Azure 机器学习 Studio 中，可以使用 R 代码创建并发布为 web 服务实验。 Web 服务可以进行发布到 Azure 市场和遍及世界各地的用户和设备消耗与 web 服务的作者没有基础架构设置。  

##Web 服务的消耗  
此 web 服务提供了根据所有观察值的独立变量因变量的预测的值。 Web 服务需要最终用户由逗号 （，） 分隔的行、 列由分号 （;） 分隔的字符串作为输入数据。 Web 服务一次需要 1 行，期望为因变量的第一列。 示例数据集可能如下所示︰

![示例数据][1]

而因变量的观测值应为"NA"输入 y。 数据输入上面的数据集很下面的字符串:"10 个; 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1，NA; 3; 4"。 输出是根据自变量的行的每个预测的值。 

>这项服务，如投放在 Azure 的市场上，是 OData 服务;这些可能通过开机自检或 GET 方法被调用。 

有多种方法使用服务以自动方式 (示例应用程序是[在此处](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx ))。

###启动 web 服务消耗的 C# 代码︰

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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


从 Azure 机器学习，在新的空白实验创建和两个[执行 R 脚本][执行 r 脚本]模块被拉到工作区。 此 web 服务将运行基础的 R 脚本，Azure 机器学习实验。 有此试验 2 部分︰ 架构定义和培训模型 + 评分。 第一个模块定义输入数据集，其中的第一个变量为因变量，则将其余变量无关的预期的的结构。 第二个模块适合一般线性回归模型的输入数据。  
  
![实验流程][3]

####模块 1:
 
####架构定义  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####第 2 单元︰
####LM 建模   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##限制
这是一个非常简单线性回归分析 web 服务的多个示例。 从上面的示例代码中可看出，实现无错误捕获和服务将假定所有内容都是一个连续的变量 （没有分类功能允许），以服务仅输入数字值在此 web 服务的创建时间。 此外，服务当前处理有限的数据大小，截止日期到 web 服务调用和事实模型适合在每次调用 web 服务的请求/响应特性。 

##常见问题
Web 服务或发布到 Azure 市场消耗的常见问题及解答，请参见[此处](machine-learning-marketplace-faq.md)。

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[执行 r 脚本]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 

测试
