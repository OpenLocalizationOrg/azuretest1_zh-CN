---
ms.openlocfilehash: fa797bfe67adc45e579ce3cd6c1806ebcee67dab
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="设置临时在 Azure 应用程序服务 web 应用程序的环境"
    description="了解如何使用分阶段的发布 Azure 应用程序服务中的 web 应用程序。"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2015"
    ms.author="cephalin"/>

# 设置临时在 Azure 应用程序服务 web 应用程序的环境
<a name="Overview"></a>

当您的 web 应用程序部署到[应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)时，可以部署至单独的部署而不是默认生产插槽的插槽在**标准**或**高级**应用程序服务计划模式中运行时。 部署插槽均为实际的 web 应用程序使用它们自己的主机名。 Web 应用程序的内容和配置元素可以部署两个插槽，包括生产插槽之间交换。 部署应用程序的部署槽具有以下优点︰

- 您可以换生产插槽前验证临时部署插槽中的 web 应用程序更改。

- 首次部署 web 应用程序到插槽和换成生产以确保插槽的所有实例都取暖被交换到生产环境之前。 在部署 web 应用程序时，这消除了停机时间。 通信流重定向是紧密相连的并没有请求丢弃交换操作的结果。 这整个工作流可以通过配置[自动交换](#configure-auto-swap-for-your-web-app)不需要预先交换验证时自动执行。

- 换用后，用以前分阶段的 web 应用程序现在有以前的生产 web 应用程序。 如果换到生产插槽的更改未按预期的那样，您可以执行相同的交换，立即以获取"最后一个已知良好站点"回。

每个应用程序服务计划模式支持不同的部署的插槽数。 若要找出数槽，web 应用程序的模式支持信息，请参阅[应用程序服务的定价](/pricing/details/app-service/)。

- 当您的 web 应用程序具有多个插槽时，您不能更改模式。

- 缩放比例不可用于非生产插槽。

- 对于非生产插槽不支持链接的资源管理。

    > [AZURE.NOTE] 在[Azure 预览门户](http://go.microsoft.com/fwlink/?LinkId=529715)，可以暂时将非生产插槽移动到其他应用程序服务计划模式来避免这种潜在影响生产插槽上。 注意非生产插槽生产插槽前您必须再一次共享相同的模式可以交换两个插槽。

<a name="Add"></a>
## 添加到 web 应用程序部署插槽 ##

必须在要启用多个部署插槽顺序**标准**或**特优**模式下运行 web 应用程序。

1. 在[Azure 预览门户](https://portal.azure.com/)中，打开您的 web 应用程序的刀片式服务器。
2. 单击**部署插槽**。 然后，在**部署插槽**刀片式服务器，请单击**添加插槽**。

    ![添加一个新的部署槽][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > 如果 web 应用程序不在**标准**或**特优**模式中，您将收到一条消息指出实现分阶段的发布支持的模式。 此时，您可以选择**升级**并在继续操作之前定位到您的 web 应用程序的**刻度**选项卡。

2. 在**添加一个插槽**刀片式服务器，指定插槽的名称，并选择是否复制另一个现有部署插槽中的 web 应用程序配置。 单击复选标记以继续。

    ![配置源][ConfigurationSource1]

    第一次添加一个插槽，将只有两个选择︰ 克隆配置缺省插槽进行生产，或根本不从。

    创建多个插槽之后，您将能够克隆配置从插槽，但不在生产中的一个︰

    ![配置源][MultipleConfigurationSources]

5. 在**部署插槽**刀片式服务器，请单击打开刀片式服务器插槽，使用一套标准和配置，就像任何其他 web 应用程序的部署槽。 **your-web-app-name-deployment-slot-name**将显示在顶部的刀片来提醒您，您正在查看部署插槽。

    ![部署的插槽标题][StagingTitle]

5. 单击应用程序中的插槽刀片式服务器 URL。 通知部署插槽都有它自己的主机名，也是一个实时的应用程序。 若要限制公众对于部署插槽，请参阅[应用程序服务 Web 应用程序 – 阻止 web 访问到非生产部署的插槽](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)。

后部署插槽创建没有内容。 从不同的资料库分支或一个完全不同的存储库，您可以部署到插槽。 您还可以更改插槽配置。 使用部署插槽有内容更新相关联的发布配置文件或部署凭据。  例如，您可以[将发布到该插槽使用 git](web-sites-publish-source-control.md)。

<a name="AboutConfiguration"></a>
## 配置用于部署的插槽 ##
克隆配置部署的另一个插槽中时，克隆的配置时，可编辑。 此外，某些配置元素将其他配置元素将保留在相同的插槽 （插槽） 换用后，虽然换用 （不是插槽特定） 跨中遵循内容。 下面的列表显示了将更改时换用插槽配置。

**设置的交换**︰

- 常规设置的框架版本，32/64 位，如 Web 套接字
- 应用程序设置 （可配置坚持一个插槽）
- 连接字符串 （可以将配置为坚持一个插槽）
- 处理程序映射
- 监视和诊断设置
- WebJobs 内容

**不换的设置**︰

- 发布的终结点
- 自定义域名
- SSL 证书和绑定
- 缩放设置
- WebJobs 计划程序

要配置应用程序设置或连接字符串粘贴到插槽 （不替换），访问**应用程序设置**刀片式服务器的特定的插槽，然后选择应坚持插槽配置元素的**槽设置**框。 注意根据特定的插槽的效果与 web 应用程序的所有部署插槽在不更换为建立该元素的该标记的配置元素。

![设置插槽][SlotSettings]

<a name="Swap"></a>
## 可供部署换用插槽 ##

>[AZURE.IMPORTANT] 换一个 web 应用程序部署插槽中的用到生产环境之前，请确保完全按照您想要它交换目标中配置了所有非插槽的特定设置。

1. 要交换部署插槽，请单击**交换**按钮在 web 应用程序的命令栏或部署插槽的命令栏中。 请确保正确设置交换源和交换目标。 通常情况下，交换目标是生产插槽。  

    ![交换按钮][SwapButtonBar]

3. 单击**确定**以完成此操作。 当操作完成后时，已交换部署插槽。

## 为您的 web 应用程序中配置自动交换 ##

自动交换优化了 DevOps 方案想要不断地为 web 应用程序的最终客户部署 web 应用程序与零冷启动和停机时间为零。 部署插槽为配置时自动交换到生产中，每次您将您的代码更新推送到该插槽时，应用程序服务会自动交换使 web 应用程序投入生产后它具有已预热的插槽中。

>[AZURE.IMPORTANT] 当启用自动交换插槽时，请确保插槽配置是完全适用于目标插槽 （通常生产插槽） 的配置。

一个插槽配置自动交换很容易。 请按照下列步骤操作︰

1. 在**部署插槽**刀片式服务器，选择非生产插槽，单击该插槽刀片式服务器的**所有设置**。  

    ![][Autoswap1]

2. 单击**应用程序设置**。 选择**在****自动交换**、**自动换用插槽**，选择所需的目标插槽和命令栏中单击**保存**。 请确保该插槽配置是完全适用于目标插槽配置。

    操作完成后，**通知**选项卡会闪烁绿色的**成功**。

    ![][Autoswap2]

    >[AZURE.NOTE] 要测试您的 web 应用程序的自动交换，可以首先要熟悉功能**自动换用插槽**中选择非生产目标插槽。  

3. 执行代码推到该部署插槽。 在短时间后会发生自动交换和更新将会反映在目标插槽的 URL。

<a name="Rollback"></a>
## 回滚后交换一个生产应用程序 ##
如果插槽交换后的生产中发现的任何错误，回滚槽为预交换状态通过立即更换相同两个插槽。

<a name="Delete"></a>
## 若要删除部署插槽##

在部署插槽刀片式服务器，在命令栏中单击**删除**。  

![删除部署插槽][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
## Azure PowerShell cmdlet 的部署插槽

Azure PowerShell 是提供 cmdlet 来管理 Windows PowerShell，包括用于管理 web 应用程序部署 Azure 应用程序服务中的插槽支持通过 Azure 的模块。

- 在安装和配置 Azure PowerShell 和 Azure 订阅验证 Azure PowerShell 的信息，请参阅[如何安装和配置 Microsoft Azure PowerShell](../install-configure-powershell.md)。  

- 若要列出可用的 cmdlet 中 PowerShell 的 Azure 应用程序服务，请调用`help AzureWebsite`。

----------

### 获得 AzureWebsite
**获得 AzureWebsite** cmdlet 提供有关当前订阅，如下面的示例中所示的 Azure 的 web 应用程序的信息。

`Get-AzureWebsite webappslotstest`

----------

### 新 AzureWebsite
您可以通过使用**New AzureWebsite** cmdlet 并指定 web 应用程序和插槽的名称创建部署插槽。 此外指示作为 web 应用程序部署插槽创建，如下面的示例中所示的同一个地区。

`New-AzureWebsite webappslotstest -Slot staging -Location "West US"`

----------

### 将发布 AzureWebsiteProject
内容部署中，如以下示例所示，可以使用**发布 AzureWebsiteProject** cmdlet。

`Publish-AzureWebsiteProject -Name webappslotstest -Slot staging -Package [path].zip`

----------

### 显示 AzureWebsite
内容和配置更新已应用到新插槽之后，您可以通过浏览到使用**显示 AzureWebsite** cmdlet，如下面的示例中所示的插槽验证更新。

`Show-AzureWebsite -Name webappslotstest -Slot staging`

----------

### 开关 AzureWebsiteSlot
**开关 AzureWebsiteSlot** cmdlet 可以执行替换操作，以更新的部署槽生产站点，如以下示例所示。 生产应用程序将不会遇到任何的停机时间，也会经过冷启动。

`Switch-AzureWebsiteSlot -Name webappslotstest`

----------

### 删除 AzureWebsite
如果不再需要部署槽，它可以使用删除**删除 AzureWebsite** cmdlet，如以下示例所示。

`Remove-AzureWebsite -Name webappslotstest -Slot staging`

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
## Azure 的命令行界面 (Azure CLI) 命令部署插槽

Azure CLI 提供跨平台命令使用 Azure，包括用于管理 Web 应用程序部署插槽的支持。

- 有关安装和配置 Azure CLI 中，其中包括有关如何将 Azure CLI 连接到 Azure 的订阅，请参阅[安装和配置 Azure CLI](../xplat-cli.md)。

-  若要列出可用的命令在 Azure CLI 的 Azure 应用程序服务，请调用`azure site -h`。

----------
### azure 站点列表
有关 web 应用程序中当前的订阅信息，调用**azure 站点列表**，如以下示例所示。

`azure site list webappslotstest`

----------
### azure 站点创建
若要创建部署插槽，调用**azure 站点创建**并指定一个现有的 web 应用程序的名称和创建，如下面的示例中所示的槽的名称。

`azure site create webappslotstest --slot staging`

若要启用源代码管理新槽，使用**git-**选项，如以下示例所示。

`azure site create --git webappslotstest --slot staging`

----------
### azure 站点交换
要将更新的部署槽生产应用程序，请使用**azure 站点交换**命令执行替换操作，如以下示例所示。 生产应用程序将不会遇到任何的停机时间，也会经过冷启动。

`azure site swap webappslotstest`

----------
### azure 网站删除
若要删除不再需要的部署插槽，使用**azure 站点删除**命令，如以下示例所示。

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## 下一步行动 ##
[Azure 应用程序服务 Web 应用程序 – 阻止 web 访问非生产部署插槽](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Microsoft Azure 免费试用版](/pricing/free-trial/)

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 旧的门户与新门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 

测试
