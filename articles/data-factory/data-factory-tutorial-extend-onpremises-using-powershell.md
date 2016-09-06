---
ms.openlocfilehash: 1e504ce9b53d84f93c9a3ca50da80937b3370ab5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将输出数据复制到一个本地 SQL Server 数据库 (Azure PowerShell)" 
    description="本演练将扩展以便管道将输出数据复制到 SQL Server 数据库使用 Azure PowerShell 教程。"
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
    ms.date="08/25/2015" 
    ms.author="spelluru"/>


# 演练︰ 将市场有效性数据复制到一个本地 SQL Server 数据库
在本演练中，您将学习如何设置环境以使管道使用的内部数据。
 
在第一个演练与分区从日志处理方案的最后一步-> Enrich-> 分析工作流程、 市场营销活动的有效性输出复制到 SQL Azure 数据库。 在您的组织内，还可以将此数据移到用于分析本地 SQL Server。
 
为了将市场营销活动的有效性数据从 Azure Blob 复制到内部部署 SQL Server，您需要创建其他现有内部链接服务，表并在第一个演练中引入使用一组相同的 cmdlet 的管道。

## 领料/收货必备项

您**必须**执行在演练[教程︰ 移动和处理日志文件使用数据工厂][datafactorytutorial]在本文中执行本演练之前。 

**（推荐）**检查和演练中创建的管道，以将数据从 SQL Server 内部移动到 Azure blob 存储[启用您的管道使用的内部数据][useonpremisesdatasources]文章中演练的做法。


在本演练中，您将执行以下步骤︰ 

1. [第 1 步︰ 创建一个数据管理网关](#OnPremStep1)。 数据管理网关是从云到您组织中的内部数据源提供访问的客户端代理。 网关实现内部 SQL Server 和 Azure 数据存储之间的数据传输。  

    您必须具有至少一个网关安装在您的企业环境以及与 Azure 数据工厂注册到 Azure 数据工厂作为链接服务添加本地 SQL Server 数据库之前。

2. [步骤 2︰ 创建链接的本地 SQL Server 服务](#OnPremStep2)。 在此步骤中，您首先在内部部署 SQL Server 计算机上创建数据库和表，然后创建链接的服务︰ **OnPremSqlLinkedService**。  
3. [步骤 3︰ 创建表和管线](#OnPremStep3)。 在此步骤中，将创建一个表**MarketingCampaignEffectivenessOnPremSQLTable**和**EgressDataToOnPremPipeline**的管道。 

4. [第 4 步︰ 监视管道和查看结果](#OnPremStep4)。 在此步骤中，您将使用 Azure 门户监视管线、 表和数据切片。


## <a name="OnPremStep1"></a> 步骤 1︰ 创建一个数据管理网关

数据管理网关是从云到您组织中的内部数据源提供访问的客户端代理。 网关实现内部 SQL Server 和 Azure 数据存储之间的数据传输。
  
您必须具有至少一个网关安装在您的企业环境以及与 Azure 数据工厂注册到 Azure 数据工厂作为链接服务添加本地 SQL Server 数据库之前。

如果您有现有的数据网关，您可以使用，请跳过此步骤。

1.  创建逻辑数据网关。 在**Azure 预览门户网站**中，请单击**数据工厂**刀片式服务器上的**链接服务**。
2.  命令栏上，单击**添加 （+） 数据网关**。  
3.  在**新的数据网关**刀片式服务器，请单击**创建**。
4.  在**创建**刀片式服务器，输入数据网关**名称** **MyGateway** 。
5.  单击**选择区域**，并根据需要进行更改。 
6.  在**创建**刀片式服务器，请单击**确定**。 
7.  您应该会看到**配置**刀片式服务器。 
8.  在**配置**刀片式服务器，请单击**直接在此计算机上安装**。 这将下载、 安装和配置网关，您的计算机上并将注册该服务网关。 如果您有现有网关安装在您要链接到此门户上的逻辑网关的计算机上，使用在此刀片上键重新注册您的网关使用数据管理网关配置管理器 （预览） 工具。

    ![数据管理网关配置管理器][image-data-factory-datamanagementgateway-configuration-manager]

9. 单击**确定**以关闭**配置**刀片和**确定**以关闭**创建**刀片式服务器。 等待，直到**MyGateway** **链接服务**刀片式服务器中的状态将更改为**好**。 您还可以启动**数据管理网关配置管理器 （预览）**工具来确认网关的名称与在门户中的名称匹配，并且**状态**为**已注册**。 您可能需要关闭并重新打开链接服务刀片式服务器以查看最新的状态。 它可能需要几分钟之前屏幕刷新为最新状态。 

## <a name="OnPremStep2"></a> 步骤 2︰ 创建链接的本地 SQL Server 服务

在此步骤中，您第一次在本地 SQL Server 计算机上创建必需的数据库和表，然后创建链接的服务。

### 准备部署数据库和表

启动时，您需要创建的 SQL Server 数据库、 表、 用户定义的类型和存储的过程。 这将从 Azure 移动**MarketingCampaignEffectiveness**结果用到 SQL Server 数据库的 blob。

1.  在**Windows 资源管理器**中定位到**C:\ADFWalkthrough** （或解压缩示例的位置） 中**OnPremises**的子文件夹。
2.  在您最喜爱的编辑器中打开**prepareOnPremDatabase 和 Table.ps1** ，替换您的 SQL Server 信息突出显示并保存该文件 （请提供**SQL 身份验证**的详细信息）。 教程，以便为您的数据库中启用 SQL 身份验证。 
            
        $dbServerName = "<servername>"
        $dbUserName = "<username>"
        $dbPassword = "<password>"

3. 在**Azure PowerShell**，定位到**C:\ADFWalkthrough\OnPremises**文件夹。
4.  运行**prepareOnPremDatabase 和 Table.ps1** **(无论是和用双引号括起来，或如下所示)**。
            
        & '.\prepareOnPremDatabase&Table.ps1'

5. 一旦成功地执行该脚本，您将看到以下项目︰   
            
        PS E:\ADF\ADFWalkthrough\OnPremises> & '.\prepareOnPremDatabase&Table.ps1'
        6/10/2014 10:12:33 PM Script to create sample on-premises SQL Server Database and Table
        6/10/2014 10:12:33 PM Creating the database [MarketingCampaigns], table and stored procedure on [.]...
        6/10/2014 10:12:33 PM Connecting as user [sa]
        6/10/2014 10:12:33 PM Summary:
        6/10/2014 10:12:33 PM 1. Database 'MarketingCampaigns' created.
        6/10/2014 10:12:33 PM 2. 'MarketingCampaignEffectiveness' table and stored procedure 


### 创建链接的服务

1.  在**Azure 预览门户**，单击**LogProcessingFactory****链接服务****数据工厂**刀片式服务器上的平铺。
2.  在**链接服务**刀片式服务器，请单击**加号 （+） 数据存储区**。
3.  在**新的数据存储**刀片式服务器，输入**名称** **OnPremSqlLinkedService** 。 
4.  单击**键入 （需要设置）**并选择**SQL Server**。 现在应该看到**数据网关**、**服务器**、**数据库**和**存储新数据**刀片式服务器中的**凭据**设置。 
5.  单击**数据网关 （配置所需的设置）** ，选择前面创建的**MyGateway** 。 
6.  输入数据库服务器承载**MarketingCampaigns**数据库的**名称**。 
7.  数据库中输入**MarketingCampaigns** 。 
8.  单击**凭据**。 
9.  在**凭据**刀片式服务器，单击**单击此处以安全地设置凭据**。
10. 它将安装在第一次点击式应用程序，启动**设置凭据**对话框。 
11. 在**设置凭据**对话框中，输入**用户名**和**密码**，并单击**确定**。 等待，直到对话框关闭。 
12. **新的数据存储**刀片式服务器中，请单击**确定**。 
13. 在**链接服务**刀片式服务器，确认**OnPremSqlLinkedService**在列表中显示链接的服务的**状态**是**好的**。

## <a name="OnPremStep3"></a> 步骤 3︰ 创建表和管道

### 创建内部逻辑表

1.  在**Azure PowerShell**，切换到**C:\ADFWalkthrough\OnPremises**文件夹。 
2.  使用 cmdlet**新建 AzureDataFactoryTable**为**MarketingCampaignEffectivenessOnPremSQLTable.json**，如下所示创建这些表。

            
        New-AzureDataFactoryTable -ResourceGroupName ADF -DataFactoryName $df –File .\MarketingCampaignEffectivenessOnPremSQLTable.json
     
#### 创建要将 Azure Blob 数据复制到 SQL Server 的管线

1.  使用 cmdlet**新建 AzureDataFactoryPipeline**为**EgressDataToOnPremPipeline.json**，如下所示创建管线。

            
        New-AzureDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName $df –File .\EgressDataToOnPremPipeline.json
     
2. 使用 cmdlet**集 AzureDataFactoryPipelineActivePeriod** **EgressDataToOnPremPipeline**为指定的有效期间。

            
        Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADF -DataFactoryName $df -StartDateTime 2014-05-01Z -EndDateTime 2014-05-05Z –Name EgressDataToOnPremPipeline

    请按**Y**以继续。
    
## <a name="OnPremStep4"></a> 步骤 4︰ 监视管道并查看结果

现在，您可以使用相同的步骤中引入[第 6 步︰ 监视表和管线](#MainStep6)来监视的新管道和新的内部 ADF 表的数据片。
 
当看到一块表**MarketingCampaignEffectivenessOnPremSQLTable**变为已准备好的状态时，它意味着，该管道的扇区执行完成。 若要查看结果，查询在 SQL Server 中的**MarketingCampaigns**数据库中的**MarketingCampaignEffectiveness**表。
 
祝贺您 ！ 成功地仔细演练中使用您的本地数据源。 
 

[监视器管理-使用 powershell]: data-factory-monitor-manage-using-powershell.md
[使用自定义活动]: data-factory-use-custom-activities.md
[疑难解答]: data-factory-troubleshoot.md
[cmdlet 引用]: http://go.microsoft.com/fwlink/?LinkId=517456

[datafactorytutorial]: data-factory-tutorial-using-powershell.md
[adfgetstarted]: data-factory-get-started.md
[adfintroduction]: data-factory-introduction.md
[useonpremisesdatasources]: data-factory-move-data-between-onprem-and-cloud.md

[azure 预览门户]: http://portal.azure.com
[azure 的购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[azure 的成员提供]: http://azure.microsoft.com/pricing/member-offers/
[azure 释放试验]: http://azure.microsoft.com/pricing/free-trial/

[sqlcmd 安装]: http://www.microsoft.com/download/details.aspx?id=35580
[azure sql 防火墙]: http://msdn.microsoft.com/library/azure/jj553530.aspx

[下载-azure powershell]: http://azure.microsoft.com/documentation/articles/install-configure-powershell
[adfwalkthrough 下载]: http://go.microsoft.com/fwlink/?LinkId=517495
[开发人员参考]: http://go.microsoft.com/fwlink/?LinkId=516908

[image-data-factory-datamanagementgateway-configuration-manager]: ./media/data-factory-tutorial-extend-onpremises-using-powershell/DataManagementGatewayConfigurationManager.png

 
测试
