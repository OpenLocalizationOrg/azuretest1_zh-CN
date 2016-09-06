---
ms.openlocfilehash: b6888026934622c99e3da8d658d955ab24be0d25
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="猪的 Hadoop 使用.NET 中 HDInsight |Microsoft Azure"
   description="了解如何使用.NET SDK 的 Hadoop 猪将作业提交到在 HDInsight 上的 Hadoop。"
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/24/2015"
   ms.author="larryfr"/>

#运行 Hadoop 在 HDInsight 中使用.NET SDK 的猪的作业

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

本文档提供使用 Hadoop 的.NET SDK 豚将作业提交到 HDInsight 群集上 Hadoop 的示例。

HDInsight.NET SDK 提供了便于更方便地使用 HDInsight 群集从.NET 的.NET 客户端库。 猪可以通过 MapReduce 操作建模的一系列数据转换。 您将学习如何使用基本的 C# 应用程序提交到 HDInsight 群集猪的作业。

[AZURE.INCLUDE [azure-preview-portal](../../includes/hdinsight-azure-portal.md)]

* [运行 Hadoop 在 HDInsight 中使用.NET SDK 的猪的作业](hdinsight-hadoop-use-pig-dotnet-sdk.md)

##<a id="prereq"></a>先决条件

若要完成本文中的步骤操作，您需要。

* Azure HDInsight (在 HDInsight 上的 Hadoop) 群集 (Windows 或基于 Linux 的)

* Visual Studio 2012 或 2013

##<a id="certificate"></a>创建管理证书

要验证到 Azure HDInsight 应用程序，必须创建自行签署式证书、 将其安装在您的开发工作站，并还将其上传到 Azure 订购。

有关如何执行此操作的说明，请参阅[创建自签名证书](http://go.microsoft.com/fwlink/?LinkId=511138)。

> [AZURE.NOTE] 在创建证书时，一定要注意好记的名称使用，因为将稍后使用。

##<a id="subscriptionid"></a>查找您的订阅 ID

每个 Azure 订阅标识的 GUID 值，称为订阅 id。 使用以下步骤查找该值。

1. 请访问[Azure 的管理控制台](https://manage.windowsazure.com/)。

2. 在入口的左侧栏中，选择**设置**。

3. 在页面右侧显示的信息，找到您想要使用并记下**预订 ID**列中的值的订阅。

保存订阅 ID，如稍后将使用它。

##<a id="create"></a>创建应用程序

1. 打开 Visual Studio 2012 或 2013

2. 从**文件**菜单中，选择**新建**，然后选择**项目**。

3. 新的项目中，键入或选择以下值。

    <table>
    <tr>
    <th>Propety</th>
    <th>值</th>
    </tr>
    <tr>
    <th>Category</th>
    <th>模板/视频 C# / 窗口</th>
    </tr>
    <tr>
    <th>Template</th>
    <th>控制台应用程序</th>
    </tr>
    <tr>
    <th>名称</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>

4. 单击**确定**以创建项目。

5. 从**工具**菜单中，选择**库程序包管理器**或**Nuget 程序包管理器**，然后选择**程序包管理器控制台**。

6. 控制台可以安装.NET SDK 包中运行以下命令。

        Install-Package Microsoft.Windowsazure.Management.HDInsight

7. 从解决方案资源管理器中，双击**Program.cs**以将其打开。 现有代码替换为以下。

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;

        using System.IO;
        using System.Threading;
        using System.Security.Cryptography.X509Certificates;

        using Microsoft.WindowsAzure.Management.HDInsight;
        using Microsoft.Hadoop.Client;

        namespace SubmitPigJob
        {
            class Program
            {
                static void Main(string[] args)
                {
                    // Get the subscription ID
                    string subscriptionID = PromptForInput("Enter your Azure Subscription ID:");

                    // Get the certificate name
                    string certFriendlyName = PromptForInput("Enter the management certificate name:");

                    // Get the cluster name
                    string clusterName = PromptForInput("Enter the HDInsight cluster name:");

                    // Set the folder that job status is written to
                    string statusFolderName = @"/tutorials/usepig/status";

                    // The Pig Latin statements to run
                    string queryString = "LOGS = LOAD 'wasb:///example/data/sample.log';" +
                        "LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
                        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
                        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
                        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
                        "RESULT = order FREQUENCIES by COUNT desc;" +
                        "DUMP RESULT;";

                    // Define the Pig job
                    PigJobCreateParameters myJobDefinition = new PigJobCreateParameters()
                    {
                        Query = queryString,
                        StatusFolder = statusFolderName
                    };

                    // Get the certificate object from certificate store using the friendly name to identify it
                    X509Store store = new X509Store();
                    store.Open(OpenFlags.ReadOnly);
                    X509Certificate2 cert = store.Certificates.Cast<X509Certificate2>().First(item => item.FriendlyName == certFriendlyName);

                    JobSubmissionCertificateCredential creds = new JobSubmissionCertificateCredential(new Guid(subscriptionID), cert, clusterName);

                    // Create a hadoop client to connect to HDInsight
                    var jobClient = JobSubmissionClientFactory.Connect(creds);

                    // Run the MapReduce job
                    Console.WriteLine("----- Submit the Pig job ...");
                    JobCreationResults mrJobResults = jobClient.CreatePigJob        (myJobDefinition);

                    // Wait for the job to complete
                    Console.WriteLine("----- Wait for the Pig job to complete ...");
                    WaitForJobCompletion(mrJobResults, jobClient);

                    // Display the error log
                    Console.WriteLine("----- The Pig job error log.");
                    using (Stream stream = jobClient.GetJobErrorLogs(mrJobResults.JobId))
                    {
                        var reader = new StreamReader(stream);
                        Console.WriteLine(reader.ReadToEnd());
                    }

                    // Display the output log
                    Console.WriteLine("----- The Pig job output log.");
                    using (Stream stream = jobClient.GetJobOutput(mrJobResults.JobId))
                    {
                        var reader = new StreamReader(stream);
                        Console.WriteLine(reader.ReadToEnd());
                    }

                    Console.WriteLine("----- Press ENTER to continue.");
                    Console.ReadLine();
                }

                private static void WaitForJobCompletion(JobCreationResults jobResults, IJobSubmissionClient client)
                {
                    JobDetails jobInProgress = client.GetJob(jobResults.JobId);
                    while (jobInProgress.StatusCode != JobStatusCode.Completed && jobInProgress.StatusCode != JobStatusCode.Failed)
                    {
                        jobInProgress = client.GetJob(jobInProgress.JobId);
                        Thread.Sleep(TimeSpan.FromSeconds(10));
                    }
                }

                private static string PromptForInput(string message)
                {
                    Console.WriteLine(message);
                    return Console.ReadLine();
                }
            }
        }


7. 保存该文件。

##<a id="run"></a>运行应用程序

使用**F5**启动应用程序。 出现提示时，输入**订阅 ID**、**证书好记的名称**和**HDInsight 群集名称**。 当它运行时，结束使用类似于以下内容，应用程序将产生多的行信息。

```
----- The Pig job output log.
(TRACE,816)
(DEBUG,434)
(INFO,96)
(WARN,11)
(ERROR,6)
(FATAL,2)

----- Press ENTER to continue.
```

按**enter 键**，退出应用程序。

##<a id="summary"></a>摘要

正如您所看到的.NET SDK 的 Hadoop 使您得以创建.NET 应用程序提交到 HDInsight 的群集，猪的作业监视作业状态，并检索输出。

##<a id="nextsteps"></a>下一步行动

猪的 HDInsight 中的一般信息。

* [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

有关其他方法的信息可以使用 Hadoop HDInsight 上。

* [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

* [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

测试
