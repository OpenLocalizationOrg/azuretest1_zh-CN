---
ms.openlocfilehash: 68f4392dc352dd3403ac9cf12166b74207aa8be0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="生存分析 Azure 机器学习与 |Microsoft Azure" 
    description="生存分析事件发生概率" 
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


#生存分析 

在许多情况下下下评估, 的主要结果是向感兴趣的事件的时间。 换句话说，这个问题"时，此事件会发生吗？" 系统要求。 直到 （疾病 relapse、 接收到的 PhD 学位，闸板故障） 感兴趣的事件为例，考虑其中的数据描述所用的时间 （天、 年、 里程等） 的情况下发生。 数据中的每个实例代表一个特定对象 （患者、 学生、 汽车等）。


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

此[web 服务]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis)回答"什么是感兴趣的事件将发生时间 n 个对象 x? 的可能性"的问题 通过提供生存分析模型，此 web 服务使用户能够提供数据训练该模型并对其进行测试。 实验的主要主题是感兴趣的事件发生之前的模型所用的时间长度。 

>此 web 服务可由用户 – 可能通过移动应用程序中，通过一个网站，或甚至在本地计算机上，例如消耗。 但是，web 服务的目的也是为作为示例说明如何使用 Azure 机器学习来创建 web 服务在 R 代码。 只需几行 R 代码和单击按钮在 Azure 机器学习 Studio 中，可以使用 R 代码创建并发布为 web 服务实验。 Web 服务可以进行发布到 Azure 市场和遍及世界各地的用户和设备消耗与 web 服务的作者没有基础架构设置。  

##Web 服务的消耗

下表中显示的 web 服务输入的数据架构。 六份信息所需的输入︰ 培训数据、 测试数据，感兴趣，"时间"的索引的时间维度，"事件"维度和变量类型的索引 (连续或系数)。 培训数据由使用由逗号分隔行、 列由分号分隔的字符串。 大量数据的功能非常灵活。 输入字符串中的所有元素必须都是数字。 在培训数据中，"时间"维度表示时间单位 （日、 年、 里程等） 数感兴趣 （病人返回到药品使用，学生获得博士学位，汽车无闸板故障事件直到过去研究 （接收药物治疗程序，一个学生开始博士研究，汽车开始驱动、 等患者） 的起始点等等。)发生。 "事件"维度指示是否发生末尾的研究感兴趣的事件。 值为"事件 = 1"感兴趣的事件发生在"时间"维度; 所指示的时间的方法"事件 = 0"感兴趣的事件发生时由"时间"维度的方式。

- trainingdata-一个字符的字符串。 用逗号分隔行和列由分号分隔。 每个行中包括"时间"维度、"事件"维度和预测值变量。
- testingdata-一个包含特定对象的预测值变量的数据行。
- time_of_interest-利息 n 的经过的时间。
- index_time-（从 1 开始） 的"时间"维度的列索引。
- index_event-（从 1 开始） 的"事件"维度的列索引。
- variable_types-使用分号作为分隔符的字符串。 0 表示连续变量，1 表示因子变量。


输出结果是事件的在特定时间发生的可能性。 

>这项服务，如投放在 Azure 的市场上，是 OData 服务;这些可能通过开机自检或 GET 方法被调用。 

有多种方法使用服务以自动方式 (示例应用程序是[在此处](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx))。 

###启动 web 服务消耗的 C# 代码︰
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




此测试的解释是，如下所示。 假设数据的目标就是直到与药物使用有关的病人收到两个处理程序之一返回的模型所用的时间。 Web 服务读取输出︰ 被 35 岁的患者，让上一生产的药品治疗 2 次，采用长时间居住的治疗程序，并且 heroin 和 cocaine 使用率，返回到的药物使用的概率是 95.64%的 500 天。

##创建的 web 服务

>此 web 服务是使用 Azure 机器学习来创建的。 免费试用版，以及介绍性视频创建实验和[发布 web 服务](machine-learning-publish-a-machine-learning-web-service.md)，请参阅[azure.com/ml](http://azure.com/ml)。 下面是创建 web 服务和示例代码为每个模块在实验中实验的一个屏幕快照。

从 Azure 机器学习，在新的空白实验创建和两个[执行 R 脚本][执行 r 脚本]模块被拉到工作区。 使用简单[执行 R 脚本][执行 r 脚本]，它定义了 web 服务的输入的数据架构创建了数据架构。 本模块被链接到 does 主要工作第二个[执行 R 脚本][执行 r 脚本]模块。 本模块完成数据预处理、 模型构造和预测。 在数据预处理步骤中，由一长串的输入的数据是转换而转换为数据框架。 在模型的构建步骤，外部的 R 包"survival_2.37 7.zip"第一次安装为进行生存分析。 然后在系列的数据处理任务后执行"coxph"函数。 生存分析的"coxph"函数的详细信息可以从 R 文档中读取。 在预测步骤中，测试实例提供到培训模型中使用"surfit"函数，并作为"曲线"变量生成此测试实例的生存曲线。 最后，被获得利息的时间的可能性。 

###实验流程︰

![实验流程][1]

####模块 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####第 2 单元︰

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##限制

此 web 服务可以仅数字值作为特征变量 （列）。 "事件"列可以只值 0 或 1。 "时间"列必须是一个正整数。

##常见问题
Web 服务或发布到 Azure 市场消耗的常见问题及解答，请参见[此处](machine-learning-marketplace-faq.md)。

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[执行 r 脚本]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 

测试
