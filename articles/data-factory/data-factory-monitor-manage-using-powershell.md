---
ms.openlocfilehash: 5aef48a7f5af1d5431e11fb920187b7efc337f12
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="教程︰ 使用复制活动使用 Azure PowerShell 创建管线" 
    description="在本教程中，您将使用 Azure PowerShell，复制活动与创建 Azure 数据工厂管道。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/27/2015" 
    ms.author="spelluru"/>

# 教程︰ 创建并监控使用 Azure PowerShell 的数据工厂
> [AZURE.SELECTOR]
- [教程概述](data-factory-get-started.md)
- [使用数据工厂编辑器](data-factory-get-started-using-editor.md)
- [使用 PowerShell](data-factory-monitor-manage-using-powershell.md)
- [使用 Visual Studio](data-factory-get-started-using-vs.md)


[学习如何使用 Azure 数据工厂][adf--入门]教程演示如何创建并监控使用[Azure 预览门户][azure 预览门户]Azure 数据工厂。 在本教程中，您将创建并使用 Azure PowerShell cmdlet 监控 Azure 数据工厂。 在本教程中创建的数据工厂管线从 Azure 将数据复制到 SQL Azure 数据库的 blob。       

> [AZURE.NOTE] 本文未涵盖所有数据工厂 cmdlet。 请参阅数据工厂 cmdlet[数据工厂 Cmdlet 参考][引用 cmdlet 的]全面的文档记录。 

##先决条件
除了教程概述主题中列出的前提条件，您需要在您的计算机上安装的 Azure PowerShell。 如果您没有它，下载并在计算机上安装[下载的 azure powershell]的[Azure PowerShell]。

##在本教程中
下表列出了您将作为教程和它们的说明的一部分执行的步骤。 

步骤 | 说明
-----| -----------
[步骤 1︰ 创建 Azure 数据工厂](#CreateDataFactory) | 在此步骤中，您将创建名为**ADFTutorialDataFactoryPSH**Azure 数据工厂。 
[步骤 2︰ 创建链接的服务](#CreateLinkedServices) | 在此步骤中，您将创建两个链接的服务︰ **StorageLinkedService**和**AzureSqlLinkedService**。 StorageLinkedService 链接 Azure 存储和 AzureSqlLinkedService ADFTutorialDataFactoryPSH 的链接 SQL Azure 数据库。
[步骤 3︰ 创建输入和输出数据集](#CreateInputAndOutputDataSets) | 在此步骤中，您将定义用作输入和输出表中您将在下一步中创建的 ADFTutorialPipeline 的**复制活动**的两个数据集 （**EmpTableFromBlob**和**EmpSQLTable**）。
[步骤 4︰ 创建和运行一个管道](#CreateAndRunAPipeline) | 在此步骤中，您将创建名为**ADFTutorialPipeline**的数据工厂中的管道︰ **ADFTutorialDataFactoryPSH**。 . 管道将有一个**复制活动**，将把数据从 Azure blob 输出 Azure 数据库的表。
[步骤 5︰ 监视数据集和管道](#MonitorDataSetsAndPipeline) | 在此步骤中，您将监视数据集，并在此步骤中使用 Azure PowerShell 的管道。

## <a name="CreateDataFactory"></a>步骤 1︰ 创建 Azure 数据工厂
在此步骤中，您可以使用 Azure PowerShell 创建名为**ADFTutorialDataFactoryPSH**Azure 数据工厂。

1. 启动**Azure PowerShell** ，然后执行以下命令。 本教程结束时才使 Azure PowerShell 保持打开状态。 如果您关闭并重新打开时，您需要再次运行这些命令。
    - 运行**添加 AzureAccount** ，输入的用户名称和密码用于登录到 Azure 预览门户。  
    - 运行**Get AzureSubscription**来查看该帐户的所有订阅。
    - 运行**选择 AzureSubscription**来选择您想要使用的订阅。 此订阅应在 Azure 预览门户使用相同。 
2. 随着 Azure 数据工厂 cmdlet 都可在此模式下，请切换到**AzureResourceManager**模式。

        Switch-AzureMode AzureResourceManager 
3. 创建名为 Azure 的资源组︰ **ADFTutorialResourceGroup**通过运行下面的命令。
   
        New-AzureResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    一些在本教程中的步骤假定您使用名为**ADFTutorialResourceGroup**的资源组。 如果您使用不同的资源组，您将需要在本教程中使用它来代替 ADFTutorialResourceGroup。 
4. 运行用于创建名为的数据工厂**新建 AzureDataFactory** : **ADFTutorialDataFactoryPSH**。  

        New-AzureDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"


    Azure 数据工厂的名称必须是全局唯一的。 如果您收到错误︰**数据工厂名称"ADFTutorialDataFactoryPSH"是不可用**，将更改的名称 (例如，yournameADFTutorialDataFactoryPSH)。 执行在本教程中的步骤时，请使用此名称来代替 ADFTutorialFactoryPSH。

## <a name="CreateLinkedServices"></a>步骤 2︰ 创建链接的服务
链接的服务链接的数据存储区或计算到 Azure 数据工厂服务。 Azure 存储、 SQL Azure 数据库或本地 SQL Server 数据库包含输入的数据或存储数据工厂管道的输出数据，可以是一个数据存储区。 计算服务是服务，处理输入的数据并产生输出数据。 

在此步骤中，您将创建两个链接的服务︰ **StorageLinkedService**和**AzureSqlLinkedService**。 StorageLinkedService 链接服务链接 Azure 存储帐户和 AzureSqlLinkedService 链接到数据工厂的 SQL Azure 数据库︰ **ADFTutorialDataFactoryPSH**。 以后在本教程中，将数据复制到 AzureSqlLinkedService 中的 SQL 表 StorageLinkedService 中的 blob 容器，您将创建一个管道。

### 创建链接的 Azure 存储帐户服务
1.  创建一个名为**StorageLinkedService.json** **C:\ADFGetStartedPSH**使用以下内容中的 JSON 文件。 如果不存在，请创建该文件夹 ADFGetStartedPSH。

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
          }
        }

    **帐户名**与您的存储帐户和**accountkey**的名称替换项 Azure 存储帐户。
2.  在**Azure PowerShell**，切换到**ADFGetStartedPSH**文件夹。 
3.  **新 AzureDataFactoryLinkedService** cmdlet 可以用于创建链接的服务。 此 cmdlet，您在本教程中使用其他数据工厂 cmdlet 需要传递的**ResourceGroupName**和**DataFactoryName**参数的值。 或者，您可以使用**Get AzureDataFactory**获取 DataFactory 对象并将对象传递而无需键入 ResourceGroupName，DataFactoryName 每次运行 cmdlet。 运行以下命令来**获取 AzureDataFactory** cmdlet 的输出分配给一个变量︰ **$df**。 

        $df=Get-AzureDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH

4.  现在，运行**新建 AzureDataFactoryLinkedService**用于创建链接的服务︰ **StorageLinkedService**。 

        New-AzureDataFactoryLinkedService $df -File .\StorageLinkedService.json

    如果您尚未运行**Get AzureDataFactory** cmdlet 和分配给变量**$df**的输出，您将不得不按如下方式指定的 ResourceGroupName 和 DataFactoryName 参数的值。   
        
        New-AzureDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactoryPSH -File .\StorageLinkedService.json

    如果您关闭 Azure PowerShell 中间教程，您将具有运行 Get AzureDataFactory cmdlet 启动 Azure PowerShell 若要完成本教程的下一次。

### 创建 SQL Azure 数据库链接的服务
1.  创建名为 AzureSqlLinkedService.json，使用以下内容的 JSON 文件。

        {
          "name": "AzureSqlLinkedService",
          "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
              "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=user@server;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
          }
        }

    **服务器名称**、**数据库名称**、 **username@servername**和**密码**替换为名称的 SQL Azure 服务器、 数据库、 用户帐户和密码。

2.  运行以下命令以创建链接的服务。 
    
        New-AzureDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json

    确认打开 SQL Azure 服务器**允许访问 Azure 服务**设置。 若要验证并将其打开，请执行以下操作︰

    1. 单击**浏览**左侧的中心并单击**SQL 服务器**。
    2. 选择您的服务器，然后单击 SQL 服务器刀片式服务器上的**设置**。
    3. 在**设置**刀片式服务器，单击**防火墙**。
    4. **在**Firewalll 设置**刀片式服务器，单击**Azure 服务允许**访问。**
    5. 请单击左边要切换到**数据工厂**刀片式服务器先前的**活动**中心。
    

## <a name="CreateInputAndOutputDataSets"></a>步骤 3︰ 创建输入和输出表

在前面的步骤中，您创建了链接的服务**StorageLinkedService**和**AzureSqlLinkedService**来链接到数据工厂的 Azure 存储帐户和 SQL Azure 数据库︰ **ADFTutorialDataFactoryPSH**。 在此步骤中，您将创建表的表示输入和输出的数据，为下一步中，您将创建的管道中的复制活动。 

表是一个矩形的数据集，它是唯一的这次支持的数据集类型。 在本教程中的输入的表引用 Azure 存储该 StorageLinkedService 中的 blob 容器点，指 SQL Azure 数据库中的 SQL 表 AzureSqlLinkedService 指向输出表。  

### Azure Blob 存储和 SQL Azure 数据库准备教程
如果您已经通过了本教程从[Azure 数据工厂开始][adf--入门]文章，请跳过此步骤。 

您需要执行以下步骤来准备本教程的 Azure blob 存储和 SQL Azure 数据库。 
 
* 创建名为**adftutorial** **StorageLinkedService**点到 Azure 的 blob 存储在一个 blob 容器。 
* 创建并上载文本文件， **emp.txt**，作为**adftutorial**容器的 blob。 
* 创建名为**emp** **AzureSqlLinkedService**指向 SQL Azure 数据库 SQL Azure 数据库中的表。


1. 启动记事本、 粘贴以下文本，并将其保存为**emp.txt**到**C:\ADFGetStartedPSH**文件夹在您的硬盘上。 

        John, Doe
        Jane, Doe
                
2. 若要创建**adftutorial**容器，将**emp.txt**文件上载到容器使用[Azure 存储浏览器](https://azurestorageexplorer.codeplex.com/)这样的工具。

    ![Azure 存储资源管理器][image-data-factory-get-started-storage-explorer]
3. 使用以下 SQL 脚本在 SQL Azure 数据库中创建了**emp**表。  


        CREATE TABLE dbo.emp 
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID); 

    如果您有在您的计算机上安装 SQL Server 2014年︰ 按照说明从[第 2 步︰ 连接到 SQL 数据库的管理 Azure SQL 数据库使用 SQL Server 管理 Studio][sql-管理 studio]项目连接到 SQL Azure 服务器并运行 SQL 脚本。

    如果您有在您的计算机上安装的 Visual Studio 2013年︰ 在 Azure 预览门户 ([http://portal.azure.com](http://portal.sazure.com)) 中，单击**浏览**左侧的中心，单击**SQL 服务器**、 选择您的数据库，单击**在 Visual Studio 中打开**工具栏上的按钮来连接到 SQL Azure 服务器，然后运行该脚本。 如果您的客户端不允许访问该 SQL Azure 服务器，您需要配置防火墙，SQL Azure 服务器以允许从您的计算机 （IP 地址） 的访问。 请参见上面的步骤来配置您的 SQL Azure 服务器防火墙文章。
        
### 创建输入的表 
表是一个矩形的数据集和架构。 在此步骤中，您将创建一个名为**EmpBlobTable**指向 blob 容器由**StorageLinkedService**链接服务的 Azure 存储区中的表。 该 blob 容器 (**adftutorial**) 包含在该文件中的输入的数据︰ **emp.txt**。 

1.  创建一个名为**EmpBlobTable.json** **C:\ADFGetStartedPSH**文件夹使用以下内容中的 JSON 文件︰

        {
          "name": "EmpTableFromBlob",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
    
    请注意以下事项︰ 
    
    - 数据集**类型**设置为**AzureBlob**。
    - **linkedServiceName**设置为**StorageLinkedService**。 
    - **采用文件夹路径**设置为**adftutorial**容器。 您还可以指定文件夹中的 blob 名称。 由于并不指定的 blob 名称，从该容器中的所有 blob 数据被认为是作为输入的数据。  
    - 格式**类型**设置**格式**为
    - 在用逗号字符 (**columnDelimiter**) 分隔的文本文件 –**名字字段**和**姓氏**--有两个字段 
    - **可用性**设置为**每小时**（**频率**设置为**小时**和**间隔**设置为**1** ），以便数据工厂服务将查找输入数据每隔一小时在您指定的 blob 容器 (**adftutorial**) 的根文件夹中。

    如果没有指定**文件名**用于**输入****表**，从输入的文件夹 (**采用文件夹路径**) 的所有文件/blob 被都视为输入。 如果在 JSON 中指定一个文件名，只指定的文件/blob 被认为是 asn 输入。 请参阅示例[教程][adf 教程]中的示例文件。
 
    如果不指定**文件名**的**输出表**，**采用文件夹路径**中生成的文件被命名为以下格式︰ 数据。 < Guid\>.txt (示例︰ Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt。)。

    要设置**采用文件夹路径**和**文件名** **SliceStart**时间动态变化，请使用**partitionedBy**属性。 在以下示例中，采用文件夹路径使用的年、 月和 SliceStart （正在处理切片的开始时间） 从天和文件名使用来自 SliceStart 小时。 例如，如果一块被生成为 2014年-10-20T08:00:00，文件夹名称设置为 wikidatagateway/wikisampledataout/2014年/10/20 和文件名设置为 08.csv。 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

    有关 JSON 属性的详细信息，请参阅[JSON 脚本参考](http://go.microsoft.com/fwlink/?LinkId=516971)。

2.  运行以下命令来创建数据工厂表。

        New-AzureDataFactoryTable $df -File .\EmpBlobTable.json

### 创建输出表
在此部分中的步骤，您将创建一个名为指向 SQL 表 (**emp**) 的**EmpSQLTable**的输出表由**AzureSqlLinkedService**链接服务的 SQL Azure 数据库中。 该管道将数据复制到了**emp**表输入的 blob。 

1.  创建一个名为**EmpSQLTable.json** **C:\ADFGetStartedPSH**文件夹使用以下内容中的 JSON 文件。
        
        {
          "name": "EmpSQLTable",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService1",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     请注意以下事项︰ 
    
    * 数据集**类型**设置为**AzureSqlTable**。
    * **linkedServiceName**设置为**AzureSqlLinkedService**。
    * **表名**设置为**emp**。
    * 有三个列- **ID**、**名字**和**姓氏**– emp 表在数据库中，但 ID 是标识列，因此您需要在此处指定只有**名字字段**和**姓氏**。
    * **可用性**设置为**每小时**（设置为**小时**的**频率**和**时间间隔**设置为**1**）。  数据工厂服务将输出数据切片每隔 1 小时在生成 SQL Azure 数据库中了**emp**表。

2.  运行以下命令来创建数据工厂表。 
    
        New-AzureDataFactoryTable $df -File .\EmpSQLTable.json


## <a name="CreateAndRunAPipeline"></a>步骤 4︰ 创建和运行一个管道
在此步骤中，您创建管线与一个**复制活动**，将**EmpTableFromBlob**用作输入和输出为**EmpSQLTable** 。

1.  创建一个名为**ADFTutorialPipeline.json** **C:\ADFGetStartedPSH**文件夹使用以下内容中的 JSON 文件︰ 
    
         {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "EmpTableFromBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "EmpSQLTable"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "00:60:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
          }
        }

    请注意以下事项︰

    - 在活动部分是其**类型**设置为**复制**只能有一个活动。
    - 活动的输入设置为**EmpTableFromBlob** ，输出为活动设置为**EmpSQLTable**。
    - 在**转换**部分中， **BlobSource**指定为源类型和**SqlSink**指定为接收器类型。

    **启动**属性的值替换为当前的日期和**最终**值与第二天。 同时开始和结束 datetimes 必须为[ISO 格式](http://en.wikipedia.org/wiki/ISO_8601)。 例如︰ 2014年-10-14T16:32:41Z。 **结束**时间是可选的但我们将在本教程中使用它。 
    
    如果不指定**end**属性的值，它将计算为"**开始 + 48 小时**"。 要无限期地运行管道， **end**属性的值指定**9/9/9999** 。
    
    在上面的示例中，将有 24 数据切片是每小时生成每个数据片。
    
    有关 JSON 属性的详细信息，请参阅[JSON 脚本参考](http://go.microsoft.com/fwlink/?LinkId=516971)。
2.  运行以下命令来创建数据工厂表。 
        
        New-AzureDataFactoryPipeline $df -File .\ADFTutorialPipeline.json

**祝贺您 ！** 已成功创建 Azure 数据工厂、 链接的服务、 表和管道并安排管道。

## <a name="MonitorDataSetsAndPipeline"></a>步骤 5︰ 监视的数据集和管道
在此步骤中，您将使用 Azure PowerShell 监视在 Azure 数据工厂中正在运行的内容。

1.  运行**Get AzureDataFactory** ，并将输出赋给变量 $df。

        $df=Get-AzureDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH
 
2.  运行**Get AzureDataFactorySlice**以获取有关所有扇区的**EmpSQLTable**，这是管道的输出表的详细信息。  

        Get-AzureDataFactorySlice $df -TableName EmpSQLTable -StartDateTime 2015-03-03T00:00:00

    替换当前的年、 月和日期年份、 月份和日期的**开始日期时间**参数的一部分。 这应与管道 JSON 中的**起始**值相匹配。 

    您应该看到 24 段，到第二天上午 12 每小时从当天的上午 12 个。 
    
    **第一个扇区︰**

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : ADFTutorialDataFactoryPSH
        TableName         : EmpSQLTable
        Start             : 3/3/2015 12:00:00 AM
        End               : 3/3/2015 1:00:00 AM
        RetryCount        : 0
        Status            : PendingExecution
        LatencyStatus     :
        LongRetryCount    : 0

    **最后一个扇区︰**

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : ADFTutorialDataFactoryPSH
        TableName         : EmpSQLTable
        Start             : 3/4/2015 11:00:00 PM
        End               : 3/4/2015 12:00:00 AM
        RetryCount        : 0
        Status            : PendingExecution
        LatencyStatus     : 
        LongRetryCount    : 0

3.  运行**Get AzureDataFactoryRun**以获取**特定**片的活动运行的详细信息。 更改**开始日期时间**参数，以匹配该扇区从上面的输出的**开始**时间的值。 **开始日期时间**值必须为[ISO](http://en.wikipedia.org/wiki/ISO_8601)格式。 例如︰ 2014年-03-03T22:00:00Z。

        Get-AzureDataFactoryRun $df -TableName EmpSQLTable -StartDateTime 2015-03-03T22:00:00

    您应该看到类似于下面的输出︰

        Id                  : 3404c187-c889-4f88-933b-2a2f5cd84e90_635614488000000000_635614524000000000_EmpSQLTable
        ResourceGroupName   : ADFTutorialResourceGroup
        DataFactoryName     : ADFTutorialDataFactoryPSH
        TableName           : EmpSQLTable
        ProcessingStartTime : 3/3/2015 11:03:28 PM
        ProcessingEndTime   : 3/3/2015 11:04:36 PM
        PercentComplete     : 100
        DataSliceStart      : 3/8/2015 10:00:00 PM
        DataSliceEnd        : 3/8/2015 11:00:00 PM
        Status              : Succeeded
        Timestamp           : 3/8/2015 11:03:28 PM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        :
        ActivityName        : CopyFromBlobToSQL
        PipelineName        : ADFTutorialPipeline
        Type                : Copy

请参阅数据工厂 cmdlet[数据工厂 Cmdlet 参考][引用 cmdlet 的]全面的文档记录。 

## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-monitor-manage-using-powershell.md)。 


[adf 教程]: data-factory-tutorial.md
[使用自定义活动]: data-factory-use-custom-activities.md
[疑难解答]: data-factory-troubleshoot.md
[开发人员参考]: http://go.microsoft.com/fwlink/?LinkId=516908

[cmdlet 引用]: https://msdn.microsoft.com/library/dn820234.aspx
[azure 释放试验]: http://azure.microsoft.com/pricing/free-trial/
[数据工厂-创建的存储]: ../storage-create-storage-account.md

[adf 获取启动]: data-factory-get-started.md
[azure 预览门户]: http://portal.azure.com
[下载-azure powershell]: ../powershell-install-configure.md
[数据-工厂简介]: data-factory-introduction.md

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-monitor-manage-using-powershell/getstarted-storage-explorer.png

[sql 的管理-studio]: ../sql-database-manage-azure-ssms.md#Step2
 
测试
