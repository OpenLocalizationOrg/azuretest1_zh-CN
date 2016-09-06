---
ms.openlocfilehash: d248d7f1d7813e49d89689016071ed6af244542d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将 DocumentDB 和 Azure 应用程序服务 Web 应用程序使用 Azure 资源管理器模板部署 |Microsoft Azure" 
    description="了解如何将 DocumentDB 帐户，Azure 应用程序服务 Web 应用程序和使用 Azure 资源管理器模板示例 web 应用程序部署。" 
    services="documentdb, app-service\web" 
    authors="stephbaron" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/28/2015" 
    ms.author="stbaro"/>

# 将 DocumentDB 和 Azure 应用程序服务 Web 应用程序使用 Azure 资源管理器模板部署 #

本教程展示如何使用 Azure 资源管理器模板部署和集成[Microsoft Azure DocumentDB](http://azure.microsoft.com/services/documentdb/)、 [Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)web 应用程序中和一个示例 web 应用程序。

后学完本教程，您将能够回答以下问题︰  

-   如何使用 Azure 资源管理器模板部署和集成 DocumentDB 帐户，并在 Azure 应用程序服务 web 应用程序？
-   如何使用 Azure 资源管理器模板部署和集成 DocumentDB 帐户、 在应用程序服务 Web 应用程序，web 应用程序和 Webdeploy 应用程序？

<a id="Prerequisites"></a>
## 先决条件 ##
> [AZURE.TIP] 虽然本教程并不假定您要修改的模板的引用或部署选项的 Azure 资源管理器模板、 JSON，或 Azure PowerShell 的经验，这些方面的知识将要求。

在本教程中按照说明操作之前，确保您具备以下︰

- Azure 的订阅。 Azure 是一个基于订阅的平台。  有关获取订阅的详细信息，请参阅[购买选项](http://azure.microsoft.com/pricing/purchase-options/)，[成员提供](http://azure.microsoft.com/pricing/member-offers/)，或[免费试用版](http://azure.microsoft.com/pricing/free-trial/)。
- Azure 存储帐户。 有关说明，请参阅[关于 Azure 存储帐户](../storage-whatis-account.md)。
- 带有 Azure PowerShell 的工作站。 有关说明，请参阅[安装和配置 Azure PowerShell](../install-configure-powershell.md)。

##<a id="CreateDB"></a>步骤 1︰ 下载并解压缩示例文件 ##
让我们先下载本教程中，我们将使用的示例文件。

1. 下载到本地文件夹中 (例如 C:\DocumentDBTemplates) 的[DocumentDB 帐户，Web 应用程序中，创建和部署演示应用程序示例](https://portalcontent.blob.core.windows.net/samples/CreateDocDBWebsiteTodo.zip)，并提取文件。  此示例将 DocumentDB 帐户和应用程序服务 web 应用程序中，web 应用程序部署。  它还会自动将配置 web 应用程序连接到 DocumentDB 帐户。

2. 下载到本地文件夹中 (例如 C:\DocumentDBTemplates) 的[DocumentDB 帐户创建和 Web 应用程序的示例](https://portalcontent.blob.core.windows.net/samples/CreateDocDBWebSite.zip)，并提取文件。  此示例将部署 DocumentDB 帐户，应用程序服务 web 应用程序，并将修改 web 应用程序的配置，以便轻松地表面 DocumentDB 连接信息，但不包含 web 应用程序。  

> [AZURE.TIP] 请注意，根据您的计算机的安全设置，您可能需要通过用鼠标右键单击，单击**属性**，单击**取消阻止**取消解压缩的文件。

![与取消阻止按钮突出显示在属性窗口的屏幕抓图](./media/documentdb-create-documentdb-website/image1.png)

<a id="Build"></a>
##步骤 2︰ 将部署文档帐户、 应用程序服务 web 应用程序和演示应用程序示例 ##

现在让我们将部署我们的第一个模板。

> [AZURE.TIP] 该模板不会验证的 web 应用程序名称和输入下面的 DocumentDB 帐户名称是 a） 有效和 b） 可用。  强烈建议您验证计划运行 PowerShell 部署脚本之前提供的名称的可用性。

1. 打开 Microsoft Azure PowerShell 并导航到的文件夹，下载并解压的[DocumentDB 帐户，应用程序服务 web 应用程序中，创建和部署演示应用程序示例](https://portalcontent.blob.core.windows.net/samples/CreateDocDBWebsiteTodo.zip)(例如 C:\DocumentDBTemplates\CreateDocDBWebsiteTodo)。


2. 我们要运行 CreateDocDBWebsiteTodo.ps1 PowerShell 脚本。  该脚本将采取下列必需的参数︰
    - 网站名称︰ 指定的应用程序服务 web 应用程序名称和用于构造 URL 将用来访问 web 应用程序 (例如如果您指定"mydemodocdbwebapp"，然后将所访问的 web 应用程序的 URL 将是 mydemodocdbwebapp.azurewebsites.net)。

    - ResourceGroupName︰ 指定要部署的 Azure 资源组的名称。 如果指定的资源组不存在，将创建它。

    - docDBAccountName︰ 指定要创建的 DocumentDB 帐户的名称。

    - 位置︰ 指定要在其中创建的 DocumentDB 和 web 应用程序资源的 Azure 位置。  有效值为东亚、 东南亚、 美国东、 西美国、 北欧，西欧 （请注意提供的位置值是区分大小写）。


3. 下面是运行该脚本的示例命令︰

        PS C:\DocumentDBTemplates\CreateDocDBWebAppTodo> .\CreateDocDBWebsiteTodo.ps1 -WebSiteName "mydemodocdbwebapp" -ResourceGroupName "myDemoResourceGroup" -docDBAccountName "mydemodocdbaccount" -location "West US"

    > [AZURE.TIP] 请注意，您将会提示您输入的 Azure 帐户用户名和密码作为运行脚本的一部分。  完全部署将需要 10 到 15 分钟才能完成。     

4. 而以下是所生成的输出的示例︰ 

        VERBOSE: 1:06:00 PM - Created resource group 'myDemoResourceGroup' in location westus'
        VERBOSE: 1:06:01 PM - Template is valid.
        VERBOSE: 1:06:01 PM - Create template deployment 'Microsoft.DocumentDBWebSiteTodo'.
        VERBOSE: 1:06:08 PM - Resource Microsoft.DocumentDb/databaseAccounts 'mydemodocdbaccount' provisioning status is running
        VERBOSE: 1:06:10 PM - Resource Microsoft.Web/serverFarms 'mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:14 PM - Resource microsoft.insights/alertrules 'CPUHigh mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:16 PM - Resource microsoft.insights/autoscalesettings 'mydemodocdbwebapp-myDemoResourceGroup' provisioning status is succeeded
        VERBOSE: 1:06:16 PM - Resource microsoft.insights/alertrules 'LongHttpQueue mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:21 PM - Resource Microsoft.Web/Sites 'mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:23 PM - Resource microsoft.insights/alertrules 'ForbiddenRequests mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:25 PM - Resource microsoft.insights/alertrules 'ServerErrors mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:25 PM - Resource microsoft.insights/components 'mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:16:22 PM - Resource Microsoft.DocumentDb/databaseAccounts 'mydemodocdbaccount' provisioning status is succeeded
        VERBOSE: 1:16:22 PM - Resource Microsoft.DocumentDb/databaseAccounts 'mydemodocdbaccount' provisioning status is succeeded
        VERBOSE: 1:16:24 PM - Resource Microsoft.Web/Sites/config 'mydemodocdbwebapp/web' provisioning status is succeeded
        VERBOSE: 1:16:27 PM - Resource Microsoft.Web/Sites/Extensions 'mydemodocdbwebapp/MSDeploy' provisioning status is running
        VERBOSE: 1:16:35 PM - Resource Microsoft.Web/Sites/Extensions 'mydemodocdbwebapp/MSDeploy' provisioning status is succeeded

        ResourceGroupName : myDemoResourceGroup
        Location          : westus
        Resources         : {mydemodocdbaccount, CPUHigh mydemodocdbwebapp, ForbiddenRequests mydemodocdbwebapp, LongHttpQueue mydemodocdbwebapp...}
        ResourcesTable    :
                    Name                                    Type                                   Location
                    ======================================  =====================================  =========
                    mydemodocdbaccount                      Microsoft.DocumentDb/databaseAccounts  westus
                    CPUHigh mydemodocdbwebapp              microsoft.insights/alertrules          eastus
                    ForbiddenRequests mydemodocdbwebapp    microsoft.insights/alertrules          eastus
                    LongHttpQueue mydemodocdbwebapp        microsoft.insights/alertrules          eastus
                    ServerErrors mydemodocdbwebapp         microsoft.insights/alertrules          eastus
                    mydemodocdbwebapp-myDemoResourceGroup  microsoft.insights/autoscalesettings   eastus
                    mydemodocdbwebapp                      microsoft.insights/components          centralus
                    mydemodocdbwebapp                      Microsoft.Web/serverFarms              westus
                    mydemodocdbwebapp                      Microsoft.Web/sites                    westus

        ProvisioningState : Succeeded


5. 我们看我们的示例应用程序之前，请让我们了解什么模板部署来实现的︰

    - 已创建应用程序服务 web 应用程序。

    - DocumentDB 帐户已创建。

    - Web 部署包部署到应用程序服务 web 应用程序

    - Web 应用程序配置进行了修改，这样的 DocumentDB 终结点和主主密钥浮现为应用程序设置。

    - 创建了一系列的默认的监视规则。

    
6. 若要使用该应用程序，只需导航到 web 应用程序 URL （在上面的示例中，URL 是 http://mydemodocdbwebapp.azurewebsites.net）。  您将看到下面的 web 应用程序︰

    ![托多应用程序的示例](./media/documentdb-create-documentdb-website/image2.png)

7. 继续操作并创建几个任务，然后让我们打开[Microsoft Azure 预览门户](https://portal.azure.com)。

8. 选择浏览资源组，然后选择我们 （在上方，myDemoResourceGroup 示例） 部署期间创建的资源组。

    ![突出显示 myDemoResourceGroup Azure 门户网站的屏幕截图](./media/documentdb-create-documentdb-website/image3.png)
9.  请注意摘要镜头中的资源映射方式显示所有相关资源 （DocumentDB 帐户应用程序服务 web 应用程序、 监视）。

    ![摘要的镜头的屏幕抓图](./media/documentdb-create-documentdb-website/image4.png)
10.  单击您的 DocumentDB 帐户，并启动查询浏览器 （帐户刀片底部）。

    ![具有突出显示的查询资源管理器中平铺的资源组和帐户薄片的屏幕抓图](./media/documentdb-create-documentdb-website/image8.png)

11. 运行默认查询、"选择*FROM c"，然后检查结果。 请注意查询检索到的 JSON 表示 todo 项 7 以上步骤中创建。 随意的查询;例如，尝试运行选择*从 c WHERE c.isComplete = true 返回所有已标记为已完成的 todo 项目。


    ![查询资源管理器和结果刀片式服务器显示查询结果的屏幕快照](./media/documentdb-create-documentdb-website/image5.png)
12. 随意浏览 DocumentDB 门户体验或修改示例 Todo 应用程序。  当您准备好时，让我们部署另一个模板。
    
<a id="Build"></a> 
## 步骤 3︰ 将部署文档帐户和 web 应用程序示例 ##

现在让我们将部署我们的第二个模板。

> [AZURE.TIP] 该模板不会验证的 web 应用程序名称和输入下面的 DocumentDB 帐户名称是 a） 有效和 b） 可用。  强烈建议您验证计划运行 PowerShell 部署脚本之前提供的名称的可用性。

1. 打开 Microsoft Azure PowerShell 并导航到的文件夹，下载并[创建一个 DocumentDB 帐户和 web 应用程序示例](https://portalcontent.blob.core.windows.net/samples/CreateDocDBWebSite.zip)(例如 C:\DocumentDBTemplates\CreateDocDBWebsite) 中提取。


2. 我们要运行 CreateDocDBWebsite.ps1 PowerShell 脚本。  该脚本的参数为我们部署，即第一个模板︰
    - 网站名称︰ 指定的应用程序服务 web 应用程序名称和用于构造 URL 将用来访问 web 应用程序 (例如如果您指定"myotherdocumentdbwebapp"，然后将所访问的 web 应用程序的 URL 将是 myotherdocumentdbwebapp.azurewebsites.net)。

    - ResourceGroupName︰ 指定要部署的 Azure 资源组的名称。  如果指定的资源组不存在，将创建它。

    - docDBAccountName︰ 指定要创建的 DocumentDB 帐户的名称。

    -   位置︰ 指定要在其中创建的 DocumentDB 和 web 应用程序资源的 Azure 位置。  有效值为东亚、 东南亚、 美国东、 西美国、 北欧，西欧 （请注意提供的位置值是区分大小写）。

3. 下面是运行该脚本的示例命令︰

        PS C:\DocumentDBTemplates\CreateDocDBWebSite> .\CreateDocDBWebSite.ps1 -WebSiteName "myotherdocumentdbwebapp" -ResourceGroupName "myOtherDemoResourceGroup" -docDBAccountName "myotherdocumentdbdemoaccount" -location "East US"

    > [AZURE.TIP] 请注意，您将会提示您输入的 Azure 帐户用户名和密码作为运行脚本的一部分。  完全部署将需要 10 到 15 分钟才能完成。     

4. 部署输出将第一个模板示例非常相似。 
5. 我们打开 Azure 预览门户之前，我们来了解一下什么这一模板部署来实现的︰

    - 已创建应用程序服务 web 应用程序。

    - DocumentDB 帐户已创建。

    -   Web 应用程序配置进行了修改，这样的 Azure DocumentDB 终结点、 主主键和辅助主密钥浮现为应用程序设置。

    -   创建了一系列的默认的监视规则。

6. 让我们打开[门户 Azure 预览](https://portal.azure.com)，选择浏览资源组，然后选择我们 （在上方，myOtherDemoResourceGroup 示例） 部署期间创建的资源组。
7. 在摘要的镜头，只是已部署的 web 应用程序。

    ![突出显示 myotherdocumentdbwebapp web 应用程序与镜头的摘要屏幕抓图](./media/documentdb-create-documentdb-website/image6.png)
8. 刀片式服务器的 web 应用程序，单击**所有设置**，然后**应用程序设置**并记下如何有存在 DocumentDB 端点和 DocumentDB 主要密钥的每个应用程序设置。

    ![Web 应用程序、 设置和应用程序设置刀片的屏幕抓图](./media/documentdb-create-documentdb-website/image7.png)
9. 感觉可以自由地继续探索 Azure 预览门户，或者按照我们 DocumentDB[样本](http://go.microsoft.com/fwlink/?LinkID=402386)来创建您自己的 DocumentDB 应用程序之一。

    
    
<a name="NextSteps"></a>
## 下一步行动

祝贺您 ！ 您已经部署了 DocumentDB，应用程序服务 web 应用程序和使用 Azure 资源管理器模板示例 web 应用程序。

- 若要了解有关 DocumentDB 的详细信息，请单击[此处](http://azure.com/docdb)。
- 若要了解有关 Azure 应用程序服务 Web 应用程序的详细信息，请单击[此处](http://go.microsoft.com/fwlink/?LinkId=325362)。
- 若要了解有关 Azure 资源管理器模板的详细信息，请单击[此处](https://msdn.microsoft.com/library/azure/dn790549.aspx)。


## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 旧的门户与新门户的更改的指南，请参阅︰[用于导航的 Azure 门户的引用](http://go.microsoft.com/fwlink/?LinkId=529715)

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。
 
测试
