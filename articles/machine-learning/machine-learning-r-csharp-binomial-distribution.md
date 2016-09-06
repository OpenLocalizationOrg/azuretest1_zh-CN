---
ms.openlocfilehash: 4ea42c37d009bf6c03c00492c52db075ed72b004
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="二项式分布套件 |Microsoft Azure" 
    description="二项式分布套件" 
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


#二项式分布套件




二项式分布套件是帮助生成和处理的二项式分布的示例 web 服务 （[二项式生成器](https://datamarket.azure.com/dataset/aml_labs/bdg5)，[概率计算器]( https://datamarket.azure.com/dataset/aml_labs/bdp4)， [Quantile 计算器]( https://datamarket.azure.com/dataset/aml_labs/bdq5)） 一套。 这些服务允许生成任意长度，计算 quantiles 的二项式分布的函数序列的给定概率和概率计算从给定 quantile。 每个服务发出基于所选服务的不同输出 （请参见下面的说明）。 二项式分布套件基于 R 的函数 qbinom、 rbinom 和 pbinom，R 统计软件包中包括的。 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>这些 web 服务可以被用户 – 可能直接在市场上，通过移动应用程序中，通过一个网站，或甚至在本地计算机上，例如。 但是，web 服务的目的也是为作为示例说明如何使用 Azure 机器学习来创建 web 服务在 R 代码。 只需几行 R 代码和单击按钮在 Azure 机器学习 Studio 中，可以使用 R 代码创建并发布为 web 服务实验。 Web 服务可以然后发布到 Azure 市场并由世界各地的用户和设备 — 通过 web 服务的作者没有基础结构设置是必需的。

##Web 服务的消耗
二项式分布套件包括以下 3 服务。

###二项式分布的函数 Quantile 计算器
此服务接受 4 正态分布的参数，并计算相关联的 quantile。
输入的参数是︰

- p-单个聚合多个试验的可能性。  
- 大小的试验次数。
- prob-在试验中成功的概率。
- 一侧的底边的分布，分布的顶边 U L。 

该服务的输出是程序与给定的概率计算的 quantile。

###二项式分布的概率计算器
此服务接受 4 的二项式分布参数，并计算概率。
输入的参数是︰

- q-事件与二项式分布的单个 quantile。 
- 大小的试验次数。
- prob-在试验中成功的概率。
- 一侧的底边的分布，顶边的分发或等于单个成功次数的 E U L。

该服务的输出是与给定 quantile 相关联的计算的概率。

###二项式分布的函数生成器
此服务接受二项式分布的 3 个参数，并生成编号 binomially 分布的随机序列。 应在请求中给它提供以下参数︰

- n-观察值个数。 
- 大小的试验次数。
- prob-成功的概率。

该服务的输出是通过基于大小和 prob 参数二项式分布长度 n 的序列。

>这项服务，如投放在 Azure 的市场上，是 OData 服务;这些可能通过开机自检或 GET 方法被调用。 

有多种方法使用服务以自动方式 (在此处的示例应用程序︰[生成器](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx)，[概率计算器](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx)， [Quantile 计算器](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator))。 

###启动 web 服务消耗的 C# 代码︰

###二项式分布的函数 Quantile 计算器
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###二项式分布的概率计算器
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###二项式分布的函数生成器
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

###二项式分布的函数 Quantile 计算器

![创建工作区][4]

####模块 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####第 2 单元︰

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###二项式分布的概率计算器

![创建工作区][5]

####模块 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####第 2 单元︰
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###二项式分布的函数生成器

![创建工作区][6]

####模块 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####第 2 单元︰
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##限制 
这是非常简单的示例周围二项式分布。 从上面的示例代码中可看出，被实现小错误捕获。

##常见问题
Web 服务或发布到 Azure 市场消耗的常见问题及解答，请参见[此处](machine-learning-marketplace-faq.md)。


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 

测试
