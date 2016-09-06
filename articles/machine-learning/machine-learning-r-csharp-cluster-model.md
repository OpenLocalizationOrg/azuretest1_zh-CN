---
ms.openlocfilehash: b342ab53206732edf5795c85132bc9ad6949b688
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="群集模型 |Microsoft Azure" 
    description="群集模型" 
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


#群集模型    

我们如何可以以减少信用卡发行商的费用从风险中预测信用 cardholders 行为的组 我们如何可以以提高它们的性能在工作中定义组的员工的个性特点 如何医生可以成组根据其疾病的特征分类患者？ 原则上，可以通过群集分析回答所有这些问题。   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
群集分析将分成两个或多个互斥未知组根据变量的组合观测值的一组。 群集分析的目的是发现的将观察值，通常人或其特征，组织成组，系统在组成员共享属性共同。 此[服务](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model)使用的 K 表示方法，常用的群集技术，为成组的群集任意数据。 此 web 服务将数据和数 k 群集作为输入，并生成其中每个观察值所属 k 组的预测。 

>此 web 服务可由用户 – 可能通过移动应用程序中，通过一个网站，或甚至在本地计算机上，例如消耗。 但是，web 服务的目的也是为作为示例说明如何使用 Azure 机器学习来创建 web 服务在 R 代码。 只需几行 R 代码和单击按钮在 Azure 机器学习 Studio 中，可以使用 R 代码创建并发布为 web 服务实验。 Web 服务可以进行发布到 Azure 市场和遍及世界各地的用户和设备消耗与 web 服务的作者没有基础架构设置。  

##Web 服务的消耗   
此 web 服务将数据分组到 k 组一套，并输出每一行的组分配。 Web 服务需要最终用户由逗号 （，） 分隔的行、 列由分号 （;） 分隔的字符串作为输入数据。 Web 服务一次需要 1 行。 示例数据集可能如下所示︰

![示例数据][1]

假设用户想要将此数据划分为 3 个互斥组。 对于上述数据集将如下所示输入的数据︰ 值 ="10 个; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4";k ="3"。 输出结果是每个行的预测的组成员身份。

>这项服务，如投放在 Azure 的市场上，是 OData 服务;这些可能通过开机自检或 GET 方法被调用。 

有多种方法使用服务以自动方式 (示例应用程序是[在此处](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx ))。

###启动 web 服务消耗的 C# 代码︰

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

从 Azure 机器学习，在新的空白实验创建和两个[执行 R 脚本][执行 r 脚本]模块拉出到工作区。 用一个简单的[执行 R 脚本][执行 r 脚本]创建了数据架构。 然后，数据架构已链接到[执行 R 脚本][执行 r 脚本]重新创建的群集模型部分。 在[执行 R 脚本][执行 r 脚本]用于群集模型中，web 服务，然后利用"k 表示"函数，即预建到 Azure 机器学习[执行 R 脚本][执行 r 脚本]。    
   

     
![实验流程][3]

####模块 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####第 2 单元︰
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##限制
这是一个非常简单的示例 web 服务的群集。 从上面的示例代码中可看出，实现无错误捕获和服务将假定所有内容都是一个连续的变量 （没有分类功能允许），以服务仅输入数字值在此 web 服务的创建时间。 此外，服务当前处理有限的数据大小，截止日期到 web 服务调用和事实模型适合在每次调用 web 服务的请求/响应特性。 

##常见问题
Web 服务或发布到 Azure 市场消耗的常见问题及解答，请参见[此处](machine-learning-marketplace-faq.md)。

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[执行 r 脚本]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 

测试
