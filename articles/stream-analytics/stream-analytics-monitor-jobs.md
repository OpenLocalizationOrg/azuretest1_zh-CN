---
ms.openlocfilehash: 8d9b173405c21df3ee88b11fe595c8c40724a64f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="以编程方式监视作业流分析 |Microsoft Azure" 
    description="了解如何通过编程方式来监视通过 REST Api，Azure SDK 或 Powershell 创建流分析作业。" 
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


# 以编程方式监视流分析作业 
本文演示如何启用监视流分析作业。 通过 REST Api，Azure SDK 或 Powershell 创建作业执行流分析不具有监视默认启用的。  您可以通过手动启用此 Azure 门户中导航到该作业的监视器页面并单击启用按钮，或您可以按照本文中的步骤自动执行此过程。 监视数据将显示在流分析作业 Azure 门户中的"显示器"选项卡。

![监视作业选项卡](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## 先决条件
在开始这篇文章之前，您必须具有以下︰

- Visual Studio 2012 或 2013年。
- 下载并安装[Azure.NET SDK](http://azure.microsoft.com/downloads/)。
- 需要启用监控功能的流分析现有作业。

## 安装项目

1.  创建一个 Visual Studio C#.Net 控制台应用程序。
2.  在程序包管理器控制台上，运行以下命令以安装 NuGet 程序包。 第一个是 Azure 流分析管理.NET SDK。 第二个是 Azure 的见解 SDK 将用于启用监视。 最后一个是 Azure Active Directory 客户端将使用的身份验证。

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  在 App.config 文件中添加以下 appSettings 部分。

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
*SubscriptionId*和*ActiveDirectoryTenantId*的值替换 Azure 订购，租户 Id。 您可以通过运行下面的 PowerShell cmdlet 获取这些值︰

    ```
    Get-AzureAccount
    ```
4.  将以下内容添加到项目中的源文件 (Program.cs) 使用语句。 

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  添加身份验证帮助程序方法。

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

## 创建管理客户端
下面的代码将设置必需的变量和管理客户端。

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## 启用监视现有流分析作业

下面的代码将使**现有**的流分析作业的监控。 代码的第一部分执行针对流分析服务的 GET 请求来检索有关特定流分析作业的信息。 它使用 （GET 请求中检索） 的"Id"属性作为参数传方法中第二个一半的代码的发送 PUT 请求到见解服务来启用监视流分析作业。

> [AZURE.WARNING]
> 如果先前已启用不同的流分析作业，通过 Azure 门户或通过编程方式监视代码下，**建议您提供相同的存储帐户名时先前已启用监视。**
> 
> 存储帐户链接到创建流分析工作中的区域，没有特别的作业本身。 
> 
> 所有流分析作业 （和所有其他 Azure 的资源），在同一地区共享此存储帐户存储监视数据。 如果您提供一个不同的存储帐户，它可能会导致意外的副作用的其他流分析作业和/或其他 Azure 资源的监视。
> 
> 用来替换的存储帐户名称```“<YOUR STORAGE ACCOUNT NAME>”```下面应该是在相同的订阅，因为流分析作业的存储帐户启用监视。

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## 获得支持
进一步的帮助，请尝试我们的[Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)。 


## 下一步行动

- [Azure 流分析简介](stream-analytics-introduction.md)
- [开始使用 Azure 流分析](stream-analytics-get-started.md)
- [小数位数 Azure 流分析作业](stream-analytics-scale-jobs.md)
- [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 