---
ms.openlocfilehash: 1b95b8f7ac1052c5e1597ff707e00b08ceac3bb0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="词典基于主观意图分析 |Microsoft Azure" 
    description="词典基于主观意图分析" 
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



#词典基于主观意图分析 

如何可以测量用户的观点和态度的品牌或在线社交网络中的主题如 Facebook 发，tweets、 评论等。？ 观点分析提供了分析此类问题的方法。


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

通常有两种观点分析的方法。 人使用一种监察的学习算法，并另可视为监督学习。 在大型的批注 corpus 监察的学习算法通常基础上分类模型。 其精度主要取决于质量的注释，并通常培训过程将需要很长时间。 此外，当我们将该算法应用到另一个域，结果通常是不好。 相比以管理学习的、 基于词典的监督的学习使用不需要存储大数据集和培训使用主观意图字典-这使得整个过程要快得多。 

我们的[服务](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis)是基于 MPQA 主观性词典 (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/)，这是最常用的主观性词典之一。 在 MPQA 中有 5097 负和 2533年正词。 和所有这些词都注释为强或弱极性。 整个集是根据 GNU 通用公共许可证。 Web 服务可以应用于任何短句子，tweets 和 Facebook 帖子等。 

>无法由可能通过移动应用程序中，通过一个网站，或即使在本地计算机上的用户 – 例如使用此 web 服务。 但是，web 服务的目的也是为作为示例说明如何使用 Azure 机器学习来创建 web 服务在 R 代码。 只需几行 R 代码和单击按钮在 Azure 机器学习 Studio 中，可以使用 R 代码创建并发布为 web 服务实验。 Web 服务可以进行发布到 Azure 市场和遍及世界各地的用户和设备消耗与 web 服务的作者没有基础架构设置。

##Web 服务的消耗

输入的数据可以是任何文本，但是 web 服务更好地配合短句子。 输出是介于-1 和 1 之间的数值。 小于 0 的任何值表示文本的主观意图为负;如果 0 以上正。 结果的绝对值表示关联的主观意图的强度。 

>这项服务，如投放在 Azure 的市场上，是 OData 服务;这些可能通过开机自检或 GET 方法被调用。 

有多种方法使用服务以自动方式 (示例应用程序是[在此处](http://microsoftazuremachinelearning.azurewebsites.net/))。

###启动 web 服务消耗的 C# 代码︰

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



输入是"今天是很好的一天"。 输出是"1"，指示关联的输入句积极观点。 

##创建的 web 服务
>此 web 服务是使用 Azure 机器学习来创建的。 免费试用版，以及介绍性视频创建实验和[发布 web 服务](machine-learning-publish-a-machine-learning-web-service.md)，请参阅[azure.com/ml](http://azure.com/ml)。 下面是创建 web 服务和示例代码为每个模块在实验中实验的一个屏幕快照。


从 Azure 机器学习，在新的空白实验已创建。 下图显示了基于词典的观点分析的实验流程。 "Sent_dict.csv"文件 MPQA 主观性词典，并作为一个[执行 R 脚本][执行 r 脚本]的输入设置。 另一个输入源是抽样的审查从 Amazon 评审数据集进行测试，请我们执行选择、 列名称的修改，并剥离操作。 我们使用哈希包存储在内存中的主观性词典并加快分数计算流程。 整个文本将按"tm"包标记并与主观意图词典中的单词进行比较。 最后，将通过在文本中添加主观的每个单词的权重计算分数。 

###实验流程︰

![实验流程][2]


####模块 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # 安装程序包哈希
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##限制

从算法的角度看，基于词典的观点分析是可能无法执行特定字段的分类方法优于常规的观点分析工具。 求反问题没有很好地处理。 我们进行硬编码几个否定词在我们的计划，但更好的方法是使用否定字典，生成一些规则。 Web 服务执行更好地在简短的句子，如 tweets，Facebook 帖子上比长而复杂的句子，如 Amazon 评论。 

##常见问题
Web 服务或发布到 Azure 市场消耗的常见问题及解答，请参见[此处](machine-learning-marketplace-faq.md)。

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[执行 r 脚本]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 

测试
