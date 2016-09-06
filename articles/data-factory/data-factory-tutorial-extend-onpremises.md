---
ms.openlocfilehash: b6336c986e1f3c182aba3fd6a250316dcfad0e13
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将输出数据复制到一个本地 SQL Server 数据库 （Azure 门户）" 
    description="本演练将扩展以便管道拷贝到 SQL Server 数据库的数据输出使用 Azure 门户中的数据工厂编辑器教程。"
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

1.  创建逻辑数据网关。 在**Azure 预览门户网站**中，请单击数据工厂**数据工厂**刀片式服务器上的**链接服务**。
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

1.  在**Azure 预览门户**中，单击**LogProcessingFactory****作者和部署****数据工厂**刀片式服务器上的平铺。
2.  在**数据工厂编辑器**中，在工具栏上，单击**新的数据存储**，并选择**本地 SQL Server 数据库**。
3.  在 JSON 脚本中，执行下列操作︰ 
    1.  更换**<servername>**与承载您的 SQL Server 数据库的服务器的名称。
    2.  更换**<databasename>**与**MarketingCampaigns**。
    3.  如果使用**SQL 身份验证**
        1.  指定**<username>**，**<password>**在**连接字符串**中。
        2.  删除最后两行 （**用户名**和**密码**的 JSON 属性使用 Windows 身份验证时才需要）。 
        3.  **，（逗号） ** **gatewayName**行的结尾处删除。
        
        **如果您使用的 Windows 身份验证︰**
        1. 在**连接字符串**中，**集成安全性**的值设置为**True** 。 删除"**用户 ID =<username>;密码 =<password>;**"从连接字符串中。 
        2. 指定有权访问数据库的**用户名**属性的用户的名称。 
        3. 指定的用户帐户的**密码**。   
    4. 指定的网关 (**MyGateway**) 的 gatewayName 属性的名称。             
3.  将链接的服务部署工具栏上，单击**部署**。 

## <a name="OnPremStep3"></a> 步骤 3︰ 创建表和管道

### 创建内部逻辑表

1.  在**数据工厂编辑器**中，从工具栏中，单击**新的数据集**，并选择**内部 SQL**。 
2. 替换为 JSON 在右窗格中**C:\ADFWalkthrough\OnPremises**文件夹中的**MarketingCampaignEffectivenessOnPremSQLTable.json**文件中的 JSON 脚本。
3. 从**OnPremSqlServerLinkedService**的链接服务 （**linkedServiceName**属性） 的名称更改为**SqlServerLinkedService**。
4. 若要部署表工具栏上，单击**部署**。 
     
#### 创建要将 Azure Blob 数据复制到 SQL Server 的管线

1.  1. 在**数据工厂编辑器**中，单击工具栏上的**新管道**按钮。 单击**按钮。.（省略号）**如果您看不到按钮，工具栏。 或者，您可以在树视图中右击**管线**并单击**新管道**。
2. 替换为 JSON 在右窗格中**C:\ADFWalkthrough\OnPremises**文件夹中的**EgressDataToOnPremPipeline.json**文件中的 JSON 脚本。
3. 添加**逗号 （，）**结尾的**右方括号 (])** json 格式，然后在右方括号之后添加以下三行。 

        "start": "2014-05-01T00:00:00Z",
        "end": "2014-05-05T00:00:00Z",
        "isPaused": false

    注意︰ 因为本演练中的示例数据距离 05/01/2014 05/05/2014年到 05/05/2014 05/01/2014年与设置**开始**和**结束**时间。 
 
3. 来创建和部署管线工具栏上，单击**部署**。 请确认您在编辑器的标题栏上看到**管道创建成功**的消息。
    
## <a name="OnPremStep4"></a> 步骤 4︰ 监视管道并查看结果

现在可以使用引入[主教程][datafactorytutorial] **显示器管线和数据切片**一节中相同的步骤来监视的新管道和新的内部 ADF 表的数据片。
 
当看到一块表**MarketingCampaignEffectivenessOnPremSQLTable**变为已准备好的状态时，它意味着，该管道的扇区执行完成。 若要查看结果，查询在 SQL Server 中的**MarketingCampaigns**数据库中的**MarketingCampaignEffectiveness**表。
 
祝贺您 ！ 成功地仔细演练中使用您的本地数据源。 
 

[监视器管理-使用 powershell]: data-factory-monitor-manage-using-powershell.md
[使用自定义活动]: data-factory-use-custom-activities.md
[疑难解答]: data-factory-troubleshoot.md
[cmdlet 引用]: http://go.microsoft.com/fwlink/?LinkId=517456

[datafactorytutorial]: data-factory-tutorial.md
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

[image-data-factory-datamanagementgateway-configuration-manager]: ./media/data-factory-tutorial-extend-onpremises/DataManagementGatewayConfigurationManager.png

 
测试
