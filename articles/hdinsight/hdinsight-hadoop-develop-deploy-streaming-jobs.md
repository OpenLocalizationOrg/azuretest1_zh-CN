---
ms.openlocfilehash: 1fc98c6d6e9b32285011dbd9d59dea56bd38f3e7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
    pageTitle="开发流的 HDInsight 程序的 Hadoop C# |Microsoft Azure"
    description="了解如何开发 Hadoop 流 MapReduce 程序在 C# 中，以及如何将它们部署到 Azure HDInsight。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/08/2015"
    ms.author="jgao"/>



# 开发 C# Hadoop 流为 HDInsight 的程序

Hadoop 提供使您能够编写地图和减少非 Java 语言的函数的 MapReduce 的流式 API。 本教程将引导您完成创建一个 C# 字数统计程序，这对您提供的输入数据中的给定单词的出现次数计数。 下面的插图显示如何 MapReduce 框架进行字数统计︰

![HDI。WordCountDiagram][image-hdi-wordcountdiagram]

> [AZURE.NOTE] 这篇文章中的步骤仅适用于基于 Windows Azure HDInsight 群集。 流的基于 Linux 的 HDInsight 的示例，请参见[开发 Python HDInsight 流程序](hdinsight-hadoop-streaming-python.md)。

本教程展示如何为︰

- 开发和测试流 MapReduce 程序使用 C# HDInsight 模拟器上的 Azure Hadoop
- 在 Azure HDInsight 上运行相同的 MapReduce 作业
- 检索 MapReduce 作业的结果

##<a name="prerequisites"></a>先决条件

在开始本教程之前，您必须完成以下︰

- 安装 HDInsight 仿真程序。 有关说明，请参阅[开始使用 HDInsight 仿真程序][hdinsight-获取-启动的仿真程序]。
- 仿真程序在计算机上安装 Azure PowerShell。 有关说明，请参阅[安装和配置 Azure PowerShell][powershell 安装]。
- 获得订阅了 Azure。 有关说明，请参阅[购买选项][azure 的购买选项]，[成员提供][azure 的成员提供]或[免费试用][azure 免费试用]。


##<a name="develop"></a>开发中 C & #35; 字数统计 Hadoop 流式处理程序

字数统计解决方案包含两个控制台应用程序项目︰ 映射器和变径。 映射器应用到控制台上，流的每个单词和变径的应用程序数传输文档中的单词数。 映射器和变径从标准输入流 (stdin) 中读取字符，逐行和写入标准输出流 (stdout)。

**若要创建一个 C# 控制台应用程序**

1. 打开 Visual Studio 2013年。
2. 单击**文件**，单击**新建**，然后单击**项目**。
3. 键入或选择以下值︰


字段|值
---|---
Template|C# / Windows/控制台应用程序
名称|WordCountMapper
位置|C:\Tutorials
解决方案名称|字数统计


4. 单击**确定**以创建项目。

**若要创建映射器程序**

5. 在解决方案资源管理器中，用鼠标右键单击**Program.cs**，，然后单击**重命名**。
6. 将该文件重命名为**WordCountMapper.cs**，，然后按**enter 键**。
7. 单击**是**确认重命名的所有引用。
8. 双击**WordCountMapper.cs**以将其打开。
9. 添加下面的**using**语句︰

        using System.IO;

10. 用以下内容替换**main （）**函数︰

        static void Main(string[] args)
        {
            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            string line;
            string[] words;

            while ((line = Console.ReadLine()) != null)
            {
                words = line.Split(' ');

                foreach (string word in words)
                    Console.WriteLine(word.ToLower());
            }
        }

11. 单击**生成**，然后单击**生成解决方案**以编译映射器程序。


**若要创建变径程序**

1. 从 Visual Studio 2013，单击**文件**，单击**添加**，然后单击**新建项目**。
2. 键入或选择以下值︰

字段|值
---|---
Template|C# / Windows/控制台应用程序
名称|WordCountReducer
位置|C:\Tutorials\WordCount

3. 清除对**创建解决方案的目录**复选框，然后单击**确定**以创建项目。
4. 从解决方案资源管理器中，用鼠标右键单击**Program.cs**，，然后单击**重命名**。
5. 将该文件重命名为**WordCountReducer.cs**，，然后按**enter 键**。
7. 单击**是**确认重命名的所有引用。
8. 双击**WordCountReducer.cs**以将其打开。
9. 添加下面的**using**语句︰

        using System.IO;

10. 用以下内容替换**main （）**函数︰

        static void Main(string[] args)
        {
            string word, lastWord = null;
            int count = 0;

            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            while ((word = Console.ReadLine()) != null)
            {
                if (word != lastWord)
                {
                    if(lastWord != null)
                        Console.WriteLine("{0}[{1}]", lastWord, count);

                    count = 1;
                    lastWord = word;
                }
                else
                {
                    count += 1;
                }
            }
            Console.WriteLine(count);
        }

11. 单击**生成**，然后单击**生成解决方案**以变径编译程序。

映射器和变径的可执行文件位于以下位置︰

- C:\Tutorials\WordCount\WordCountMapper\bin\Debug\WordCountMapper.exe
- C:\Tutorials\WordCount\WordCountReducer\bin\Debug\WordCountReducer.exe


##<a name="test"></a>测试该程序在仿真程序

执行下列操作来测试 HDInsight 模拟器上的该程序︰

1. 将数据上载到仿真程序的文件系统
2. 上载到仿真程序的文件系统映射器和变径的应用程序
3. 字数统计 MapReduce 作业提交
4. 检查作业状态
5. 检索作业结果

默认情况下，HDInsight 仿真程序与文件系统使用 Hadoop 分布式文件系统 (HDFS)。 或者，您可以配置 HDInsight 仿真程序使用 Azure Blob 存储。 有关详细信息，请参阅[开始使用 HDInsight 仿真程序][hdinsight 的仿真程序 wasb]。 在本部分中，您将使用 HDFS **copyFromLocal**命令将文件上传。 下一节演示如何通过使用 Azure PowerShell 上载文件。 其他方法，请参阅[上载数据为 HDInsight][hdinsight 上载数据]。

本教程使用以下文件夹结构︰

Folder|注意
---|---
\WordCount|字数统计项目的根文件夹。
\WordCount\Apps|映射器和变径的可执行文件的文件夹。
\WordCount\Input|MapReduce 源文件的文件夹。
\WordCount\Output|MapReduce 输出文件文件夹中。
\WordCount\MRStatusOutput|作业输出文件夹。


本教程使用 %hadoop_home%目录中的.txt 文件。

> [AZURE.NOTE] HDFS Hadoop 命令是区分大小写的。

**将文本文件复制到仿真程序的文件系统**

1. 在 Hadoop 命令行窗口中，运行以下命令来创建一个输入文件的目录︰

        hadoop fs -mkdir /WordCount/
        hadoop fs -mkdir /WordCount/Input

    此处使用的路径是相对路径。 它等效于以下︰

        hadoop fs -mkdir hdfs://localhost:8020/WordCount/Input

2. 运行以下命令，以将一些文本文件复制到 HDFS 上输入的文件夹︰

        hadoop fs -copyFromLocal %hadoop_home%\share\doc\hadoop\common\*.txt \WordCount\Input

3. 运行以下命令以列出上载的文件︰

        hadoop fs -ls \WordCount\Input




**若要将映射器和变径部署到仿真程序的文件系统**

1. 从桌面上打开 Hadoop 命令行并在 HDFS 中创建的 /Apps 文件夹︰

        hadoop fs -mkdir /WordCount/Apps

2. 运行以下命令︰

        hadoop fs -copyFromLocal C:\Tutorials\WordCount\WordCountMapper\bin\Debug\WordCountMapper.exe /WordCount/Apps/WordCountMapper.exe
        hadoop fs -copyFromLocal C:\Tutorials\WordCount\WordCountReducer\bin\Debug\WordCountReducer.exe /WordCount/Apps/WordCountReducer.exe

3. 运行以下命令以列出上载的文件︰

        hadoop fs -ls /WordCount/Apps

    您应看到两个.exe 文件。


**若要使用 Azure PowerShell 运行 MapReduce 作业**

1. 打开 Azure PowerShell。 有关说明，请参阅[安装和配置 Azure PowerShell][powershell 安装]。
3. 运行以下命令以设置变量︰

        $clusterName = "http://localhost:50111"

        $mrMapper = "WordCountMapper.exe"
        $mrReducer = "WordCountReducer.exe"
        $mrMapperFile = "/WordCount/Apps/WordCountMapper.exe"
        $mrReducerFile = "/WordCount/Apps/WordCountReducer.exe"
        $mrInput = "/WordCount/Input/"
        $mrOutput = "/WordCount/Output"
        $mrStatusOutput = "/WordCount/MRStatusOutput"

    HDInsight 仿真程序群集名称是"http://localhost:50111"。  

4. 运行以下命令以定义工作流︰

        $mrJobDef = New-AzureHDInsightStreamingMapReduceJobDefinition -JobName mrWordCountStreamingJob -StatusFolder $mrStatusOutput -Mapper $mrMapper -Reducer $mrReducer -InputPath $mrInput -OutputPath $mrOutput
        $mrJobDef.Files.Add($mrMapperFile)
        $mrJobDef.Files.Add($mrReducerFile)

5. 运行以下命令来创建一个凭据︰

        $creds = Get-Credential -Message "Enter password" -UserName "hadoop"

    将出现输入密码的提示。 密码可以是任何字符串。 用户名必须是"hadoop"。

6. 运行下列命令以提交 MapReduce 作业并等待作业完成︰

        $mrJob = Start-AzureHDInsightJob -Cluster $clusterName -Credential $creds -JobDefinition $mrJobDef
        Wait-AzureHDInsightJob -Credential $creds -job $mrJob -WaitTimeoutInSeconds 3600

    在该作业完成后，您将收到输出类似如下︰

        StatusDirectory : /WordCount/MRStatusOutput
        ExitCode        :
        Name            : mrWordCountStreamingJob
        Query           :
        State           : Completed
        SubmissionTime  : 11/15/2013 7:18:16 PM
        Cluster         : http://localhost:50111
        PercentComplete : map 100%  reduce 100%
        JobId           : job_201311132317_0034

    您可以看到在输出中，例如，*作业-201311132317-0034*作业 ID。

**要检查作业状态**

1. 在桌面上，单击**Hadoop YARN 状态**，或浏览到**http://localhost:50030/jobtracker.jsp**。
2. 通过使用**运行**或**已完成**类别下的作业 ID 找到作业。
3. 如果作业失败，您可以发现**故障**类别下。 您还可以打开作业详细信息和查找一些有用的信息以进行调试。


**若要显示 HDFS 的输出**

1. 打开 Hadoop 命令行。
2. 运行以下命令以显示输出︰

        hadoop fs -ls /WordCount/Output/
        hadoop fs -cat /WordCount/Output/part-00000

    您可以附加"| 更多"结尾的命令，以获得网页视图。

##<a id="upload"></a>将数据上载到 Azure Blob 存储
Azure HDInsight 使用 Azure Blob 存储作为默认的文件系统。 您可以配置 HDInsight 群集使用附加 Blob 存储空间的数据文件。 在此部分中，您将创建 Azure 存储帐户，并到 Blob 存储上载的数据文件。 数据文件是在 %hadoop_home%\share\doc\hadoop\common 目录中的.txt 文件。


**若要创建存储帐户和容器**

1. 打开 Azure PowerShell。
2. 设置变量，然后再运行命令︰

        $subscriptionName = "<AzureSubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"  
        $containerName = "<ContainerName>"
        $location = "<MicrosoftDataCenter>"  # For example, "East US"

3. 运行以下命令以创建存储帐户和 Blob 存储容器的帐户︰

        # Select an Azure subscription
        Select-AzureSubscription $subscriptionName

        # Create a Storage account
        New-AzureStorageAccount -StorageAccountName $storageAccountName -location $location

        # Create a Blob storage container
        $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
        $destContext = New-AzureStorageContext –StorageAccountName $storageAccountName –StorageAccountKey $storageAccountKey  
        New-AzureStorageContainer -Name $containerName -Context $destContext

4. 运行以下命令以验证存储帐户和容器︰

        Get-AzureStorageAccount -StorageAccountName $storageAccountName
        Get-AzureStorageContainer -Context $destContext

**若要上载的数据文件**

1. 在 Azure PowerShell 窗口的值设置为本地和目标文件夹︰

        $localFolder = "C:\hdp\hadoop-2.4.0.2.1.3.0-1981\share\doc\hadoop\common"
        $destFolder = "WordCount/Input"

    请注意本地源文件的文件夹是**C:\hdp\hadoop-2.4.0.2.1.3.0-1981\share\doc\hadoop\common**，和目标文件夹是**字数统计/输入**。 源位置是 HDInsight 模拟器上的.txt 文件的位置。 目标是将反映在 Azure Blob 容器下的文件夹结构。

3. 运行以下命令来获取源文件的文件夹中的.txt 文件的列表︰

        # Get a list of the .txt files
        $filesAll = Get-ChildItem $localFolder
        $filesTxt = $filesAll | where {$_.Extension -eq ".txt"}

5. 运行下面的代码段以复制这些文件︰

        # Copy the files from the local workstation to the Blob container
        foreach ($file in $filesTxt){

            $fileName = "$localFolder\$file"
            $blobName = "$destFolder/$file"

            write-host "Copying $fileName to $blobName"

            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -Context $destContext
        }

6. 运行以下命令以列出上载的文件︰

        # List the uploaded files in the Blob storage container
        Get-AzureStorageBlob -Container $containerName  -Context $destContext -Prefix $destFolder


**若要上载的字数统计应用程序**

1. 在 Azure PowerShell 窗口中，设置以下变量︰

        $mapperFile = "C:\Tutorials\WordCount\WordCountMapper\bin\Debug\WordCountMapper.exe"
        $reducerFile = "C:\Tutorials\WordCount\WordCountReducer\bin\Debug\WordCountReducer.exe"
        $blobFolder = "WordCount/Apps"

    通知的目标文件夹是**字数统计/应用程序**，这将会反映在 Azure Blob 容器的结构。

2. 运行以下命令复制应用程序︰

        Set-AzureStorageBlobContent -File $mapperFile -Container $containerName -Blob "$blobFolder/WordCountMapper.exe" -Context $destContext
        Set-AzureStorageBlobContent -File $reducerFile -Container $containerName -Blob "$blobFolder/WordCountReducer.exe" -Context $destContext

3. 运行以下命令以列出上载的文件︰

        # List the uploaded files in the Blob storage container
        Get-AzureStorageBlob -Container $containerName  -Context $destContext -Prefix $blobFolder

    您应看到列出这两个应用程序文件。


##<a name="run"></a>在 Azure HDInsight 上运行 MapReduce 作业

本部分提供了 Azure PowerShell 脚本来执行与运行 MapReduce 作业相关的所有任务。 列表中的任务包括︰

1. 条款 HDInsight 群集

    1. 创建一个将用作默认 HDInsight 群集文件系统的存储帐户
    2. 创建 Blob 存储容器
    3. 创建一个 HDInsight 群集

2. MapReduce 作业提交

    1. 创建流的 MapReduce 作业定义
    2. MapReduce 作业提交
    3. 等待要完成的作业
    4. 显示标准错误
    5. 显示标准输出

3. 删除群集

    1. 删除 HDInsight 群集
    2. 删除存储帐户用作默认 HDInsight 群集文件系统


**若要运行 Azure PowerShell 脚本**

1. 打开记事本。
2. 复制并粘贴以下代码︰

        # ====== STORAGE ACCOUNT AND HDINSIGHT CLUSTER VARIABLES ======
        $subscriptionName = "<AzureSubscriptionName>"
        $stringPrefix = "<StringForPrefix>"     ### Prefix to cluster, Storage account, and container names
        $storageAccountName_Data = "<TheDataStorageAccountName>"
        $containerName_Data = "<TheDataBlobStorageContainerName>"
        $location = "<MicrosoftDataCenter>"     ### Must match the data storage account location
        $clusterNodes = 1

        $clusterName = $stringPrefix + "hdicluster"

        $storageAccountName_Default = $stringPrefix + "hdistore"
        $containerName_Default =  $stringPrefix + "hdicluster"

        # ====== THE STREAMING MAPREDUCE JOB VARIABLES ======
        $mrMapper = "WordCountMapper.exe"
        $mrReducer = "WordCountReducer.exe"
        $mrMapperFile = "wasb://$containerName_Data@$storageAccountName_Data.blob.core.windows.net/WordCount/Apps/WordCountMapper.exe"
        $mrReducerFile = "wasb://$containerName_Data@$storageAccountName_Data.blob.core.windows.net/WordCount/Apps/WordCountReducer.exe"
        $mrInput = "wasb://$containerName_Data@$storageAccountName_Data.blob.core.windows.net/WordCount/Input/"
        $mrOutput = "wasb://$containerName_Data@$storageAccountName_Data.blob.core.windows.net/WordCount/Output/"
        $mrStatusOutput = "wasb://$containerName_Data@$storageAccountName_Data.blob.core.windows.net/WordCount/MRStatusOutput/"

        Select-AzureSubscription $subscriptionName

        #====== CREATE A STORAGE ACCOUNT ======
        Write-Host "Create a storage account" -ForegroundColor Green
        New-AzureStorageAccount -StorageAccountName $storageAccountName_Default -location $location

        #====== CREATE A BLOB STORAGE CONTAINER ======
        Write-Host "Create a Blob storage container" -ForegroundColor Green
        $storageAccountKey_Default = Get-AzureStorageKey $storageAccountName_Default | %{ $_.Primary }
        $destContext = New-AzureStorageContext –StorageAccountName $storageAccountName_Default –StorageAccountKey $storageAccountKey_Default

        New-AzureStorageContainer -Name $containerName_Default -Context $destContext

        #====== CREATE AN HDINSIGHT CLUSTER ======
        Write-Host "Create an HDInsight cluster" -ForegroundColor Green
        $storageAccountKey_Data = Get-AzureStorageKey $storageAccountName_Data | %{ $_.Primary }

        $config = New-AzureHDInsightClusterConfig -ClusterSizeInNodes $clusterNodes |
            Set-AzureHDInsightDefaultStorage -StorageAccountName "$storageAccountName_Default.blob.core.windows.net" -StorageAccountKey $storageAccountKey_Default -StorageContainerName $containerName_Default |
            Add-AzureHDInsightStorage -StorageAccountName "$storageAccountName_Data.blob.core.windows.net" -StorageAccountKey $storageAccountKey_Data

        Select-AzureSubscription $subscriptionName
        New-AzureHDInsightCluster -Name $clusterName -Location $location -Config $config

        #====== CREATE A STREAMING MAPREDUCE JOB DEFINITION ======
        Write-Host "Create a streaming MapReduce job definition" -ForegroundColor Green

        $mrJobDef = New-AzureHDInsightStreamingMapReduceJobDefinition -JobName mrWordCountStreamingJob -StatusFolder $mrStatusOutput -Mapper $mrMapper -Reducer $mrReducer -InputPath $mrInput -OutputPath $mrOutput
        $mrJobDef.Files.Add($mrMapperFile)
        $mrJobDef.Files.Add($mrReducerFile)

        #====== RUN A STREAMING MAPREDUCE JOB ======
        Write-Host "Run a streaming MapReduce job" -ForegroundColor Green
        Select-AzureSubscription $subscriptionName
        $mrJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $mrJobDef
        Wait-AzureHDInsightJob -Job $mrJob -WaitTimeoutInSeconds 3600

        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $mrJob.JobId -StandardError
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $mrJob.JobId -StandardOutput

        #====== DELETE THE HDINSIGHT CLUSTER ======
        Write-Host "Delete the HDInsight cluster" -ForegroundColor Green
        Select-AzureSubscription $subscriptionName
        Remove-AzureHDInsightCluster -Name $clusterName

        #====== DELETE THE STORAGE ACCOUNT ======
        Write-Host "Delete the storage account" -ForegroundColor Green
        Remove-AzureStorageAccount -StorageAccountName $storageAccountName_Default

3. 在脚本中设置的前四个变量。 **$StringPrefix**变量用于 HDInsight 群集名称、 存储帐户名称和 Blob 存储容器名称为指定的字符串前添加前缀。 因为这些名称必须是 3 到 24 个字符，请确保您指定的字符串并在一起，此脚本使用的名称不能超过该名称的字符限制。 对于**$stringPrefix**，则必须使用全部小写。

    在变量**$storageAccountName_Data**和**$containerName_Data**变量是存储帐户和已经在前面的步骤中创建的容器。 因此，您必须提供那些的名称。 这些分组可用于存储数据文件和应用程序。 **$Location**变量必须匹配的数据存储帐户位置。

4. 检查变量的其余部分。
5. 保存该脚本文件。
6. 打开 Azure PowerShell。
7. 运行以下命令以设置 RemoteSigned 执行策略︰

        PowerShell -File <FileName> -ExecutionPolicy RemoteSigned

8. 出现提示时，输入的用户名称和密码 HDInsight 群集。 请确保密码至少 10 个字符，包含一个大写字母、 一个小写字母、 数字和特殊字符。 如果您不想收到凭据提示，请参阅[使用密码、 安全字符串和凭据在 Windows PowerShell][powershell PSCredential]。

在提交作业流的 Hadoop HDInsight.NET SDK 示例，请参阅[提交 Hadoop 作业以编程方式][hdinsight 提交作业]。


##<a name="retrieve"></a>检索的 MapReduce 作业输出
本节说明了如何下载并显示输出。 有关在 Excel 中显示结果的信息，请参阅[将 Excel 连接到 Microsoft 配置单元 ODBC 驱动程序的 HDInsight][hdinsight ODBC]和[将 Excel 连接到电源查询 HDInsight][hdinsight 电源查询]。


**若要检索输出**

1. 打开 Azure PowerShell 窗口。
2. 设置的值，然后再运行命令︰

        $subscriptionName = "<AzureSubscriptionName>"
        $storageAccountName = "<TheDataStorageAccountName>"
        $containerName = "<TheDataBlobStorageContainerName>"
        $blobName = "WordCount/Output/part-00000"

3. 运行以下命令以创建 Azure 存储上下文对象︰

        Select-AzureSubscription $subscriptionName
        $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
        $storageContext = New-AzureStorageContext –StorageAccountName $storageAccountName –StorageAccountKey $storageAccountKey  

4. 运行以下命令以下载并显示输出︰

        Get-AzureStorageBlobContent -Container $ContainerName -Blob $blobName -Context $storageContext -Force
        cat "./$blobName" | findstr "there"



##<a id="nextsteps"></a>下一步行动
在本教程中，您学习了如何开发流 MapReduce Hadoop 作业、 如何测试 HDInsight 模拟器上的应用程序和如何撰写 Azure PowerShell 脚本设置 HDInsight 群集和群集上运行 MapReduce 作业。 若要了解详细信息，请参阅下列文章︰

- [开始使用 Azure HDInsight](../hdinsight-get-started.md)
- [HDInsight 仿真程序入门][hdinsight-获取-开始-仿真程序]
- [HDInsight 的开发 Java MapReduce 程序][hdinsight 开发 mapreduce]
- [HDInsight 使用 Azure Blob 存储][hdinsight 存储]
- [使用 Azure PowerShell 的管理 HDInsight][hdinsight-管理 powershell]
- [上载到 HDInsight 的数据][hdinsight 上载数据]
- [使用 HDInsight 蜂巢][配置单元使用 hdinsight-]
- [使用与 HDInsight 的小猪][使用猪的 hdinsight]

[azure 的购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[azure 的成员提供]: http://azure.microsoft.com/pricing/member-offers/
[azure 释放试验]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight 开发 mapreduce]: hdinsight-develop-deploy-java-mapreduce.md
[hdinsight 提交作业]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-获取-开始-仿真程序]: ../hdinsight-get-started-emulator.md
[hdinsight 的仿真程序 wasb]: ../hdinsight-get-started-emulator.md#blobstorage
[hdinsight 上载数据]: hdinsight-upload-data.md
[hdinsight 存储]: ../hdinsight-use-blob-storage.md
[hdinsight-管理 powershell]: hdinsight-administer-use-powershell.md

[配置-单元使用 hdinsight]: hdinsight-use-hive.md
[使用猪的 hdinsight]: hdinsight-use-pig.md
[hdinsight ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-电源-查询]: hdinsight-connect-excel-power-query.md

[powershell PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx
[powershell 安装]: ../powershell-install-configure.md

[图像的 hdi-wordcountdiagram]: ./media/hdinsight-hadoop-develop-deploy-streaming-jobs/HDI.WordCountDiagram.gif "MapReduce 字数统计应用程序流"

测试
