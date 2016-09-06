---
ms.openlocfilehash: e5909a2fb0d85b8530edd2be3b527918a55d4b96
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="了解如何使用流分析管理.NET SDK |Microsoft Azure" 
    description="开始使用流分析管理.NET SDK。 了解如何设置和运行分析作业︰ 创建项目、 输入、 输出和转换。" 
    keywords=".net skd、 分析工作、 事件中心"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="08/19/2015" 
    ms.author="jeffstok"/>


# 设置和运行分析作业使用 Azure 流分析管理.NET SDK

了解如何设置使用流分析管理.NET SDK 运行的分析作业。 设置项目、 创建输入和输出源、 转换和开始和停止作业。 为分析作业，可以从 Blob 存储或事件中心的数据传输。

Azure 流分析是一种完全托管的服务，通过流式处理数据在云中提供低延迟、 高可用性、 可扩展、 复杂事件处理。 流分析使客户能够设置流作业，以分析数据流，并使他们能够接近实时分析。  

有关.NET API 参考，请参阅[流分析管理.NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx)。


## 先决条件
在开始这篇文章之前，您必须具有以下︰

- 安装 Visual Studio 2012 或 2013年。
- 下载并安装[Azure.NET SDK](http://azure.microsoft.com/downloads/)。 
- 在您的订阅创建 Azure 资源组。 下面是一个示例脚本，Azure PowerShell。 Azure PowerShell 的信息，请参阅[安装和配置 Azure PowerShell](../install-configure-powershell.md);  


        # Configure the Azure PowerShell session to access Azure Resource Manager
        Switch-AzureMode AzureResourceManager

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

        # Create an Azure resource group    
        New-AzureResourceGroup -Name <YOUR RESORUCE GROUP NAME> -Location <LOCATION>


-   输入的源和输出目标设置为使用。 请参阅[获取启动使用 Azure 流分析](stream-analytics-get-started.md)设置一个示例输入和/或输出使用的这篇文章。


## 设置项目

若要创建一个分析作业，首先设置您的项目。

1. 创建一个 Visual Studio C#.NET 控制台应用程序。
2. 在程序包管理器控制台上，运行以下命令以安装 NuGet 程序包。 第一个是 Azure 流分析管理.NET SDK。 第二个是 Azure Active Directory 客户端将使用的身份验证。

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. 在 App.config 文件中添加以下**appSettings**部分︰

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    **SubscriptionId**和**ActiveDirectoryTenantId**的值替换 Azure 订购，租户 Id。 您可以通过运行下面的 Azure PowerShell cmdlet 来获取这些值︰

        Get-AzureAccount

5. 将以下**using**语句添加到项目中的源文件 (Program.cs):

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  添加身份验证帮助程序方法︰

        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(
                        ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                        ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
        
                    result = context.AcquireToken(
                        resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                        clientId: ConfigurationManager.AppSettings["AsaClientId"],
                        redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                        promptBehavior: PromptBehavior.Always);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });
        
            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();
        
            if (result != null)
            {
                return result.AccessToken;
            }
        
            throw new InvalidOperationException("Failed to acquire token");
        }  


## 创建一个流分析管理客户端

可以使用**StreamAnalyticsManagementClient**对象来管理作业和作业组件，例如，输入、 输出和转换。 

将以下代码添加到**Main**方法的开头︰ 

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";
    
    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());
    
    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

**ResourceGroupName**变量的值应为资源组创建或选取的必备步骤中的名称相同。

这篇文章的其余部分假定此代码运行到**Main**方法的开始处。

## 创建一个流分析作业

下面的代码创建您已定义的资源组下的流分析作业。 您将输入、 输出和转换作业以后。

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };
    
    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## 创建流分析的输入的源

下面的代码创建带有斑点的输入的源类型和 CSV 序列化流分析的输入的源。 若要创建事件中心输入的源，而不是**BlobStreamInputDataSource**中使用**EventHubStreamInputDataSource** 。 同样，您可以自定义的序列化类型的输入源。

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };
    
    InputCreateOrUpdateResponse inputCreateResponse = 
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

输入的源，从 Blob 存储或事件中心，与一个特定的作业。 若要使用相同的输入的源不同的作业，必须再次调用方法并指定另一个作业名称。


## 测试流分析的输入的源

**TestConnection**方法测试流分析作业是否能够连接到输入的源，以及特定的输入的源类型到其他方面。 例如，在您在先前步骤中创建的 blob 输入源，方法将检查该存储帐户名称和密钥对可用于连接到存储帐户，以及检查存在指定的容器。

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse = 
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## 创建流分析的输出目标

创建输出目标是非常类似于创建流分析的输入的源。 输入源，如输出的目标与一个特定的作业。 若要使用相同的输出目标不同的作业，必须再次调用该方法，并指定另一个作业名称。

下面的代码创建一个输出目标 （SQL Azure 数据库）。 您可以自定义输出目标的数据类型和/或序列化类型。

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };
    
    OutputCreateOrUpdateResponse outputCreateResponse = 
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## 测试流分析输出目标

还有用于测试连接的**TestConnection**方法流分析的输出目标。

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse = 
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## 创建流分析转换

下面的代码创建一个流分析转换与查询"选择 * 从输入"并指定为流分析作业分配一个流式处理单位。 调整流设备的详细信息，请参阅[缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)。


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };
    
    var transformationCreateResp = 
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

输入和输出，如转换还被绑定到特定的流分析作业下创建它。

## 开始流分析作业
创建流分析作业及其输入、 输出和转换后，您可以通过调用**Start**方法开始作业。 

下面的示例代码开始流分析作业与自定义输出的开始时间设置为 2012 年 12 月 12 日，12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };
    
    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## 停止流分析作业
您可以通过调用**Stop**方法来停止正在运行的流分析作业。

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## 删除流分析作业
**Delete**方法将删除作业以及底层的子资源，包括输入、 输出，以及作业的转换。

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## 获得支持
进一步的帮助，请尝试我们的[Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)。 


## 下一步行动

您已经学习使用.NET SDK 来创建和运行分析作业的基本知识。 若要了解详细信息，请参阅以下︰

- [Azure 流分析简介](stream-analytics-introduction.md)
- [开始使用 Azure 流分析](stream-analytics-get-started.md)
- [小数位数 Azure 流分析作业](stream-analytics-scale-jobs.md)
- [Azure 流分析管理.NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx)。
- [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 