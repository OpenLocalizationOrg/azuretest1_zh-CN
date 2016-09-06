---
ms.openlocfilehash: fc0d8cefc2d47d4d2b052fac8acddc3894b62f8a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Azure PowerShell 使用 Azure 资源管理器" 
    description="使用 Azure PowerShell 为到 Azure 的资源组中部署多个资源。" 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/15/2015" 
    ms.author="tomfitz"/>

# 使用 Azure PowerShell 使用 Azure 资源管理器

> [AZURE.SELECTOR]
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)

Azure 的资源管理器引入了完全新的 Azure 资源有关的思维方式。 而不是创建和管理单个资源，首先，设想一个复杂的服务，如博客、 图片库、 SharePoint 门户或 wiki。 使用模板-该服务的资源模型--与您需要支持服务的资源创建资源组。 然后，您可以管理和部署该资源组作为一个逻辑单元。 

在本教程中，您将学习如何使用 Microsoft Azure Azure PowerShell 使用 Azure 资源管理器中。 它将引导您完成创建和部署 Azure 承载 web 应用程序与 SQL 数据库，完成与所有您需要支持它的资源的资源组的过程。

## 先决条件

若要完成本教程，您必须具有 Azure PowerShell 版本 0.8.0 或更高版本。 若要安装最新版本，并将其与您 Azure 的订阅，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)。

本教程为 PowerShell 初学者设计，但它假定您了解的基本概念，如模块、 cmdlet 和会话。 有关 Windows PowerShell 的详细信息，请参阅[Windows PowerShell 入门知识](http://technet.microsoft.com/library/hh857337.aspx)。

要获得任何在本教程中，您看到的 cmdlet 的详细的帮助，请使用获取帮助 cmdlet。 

    Get-Help <cmdlet-name> -Detailed

例如，要添加 AzureAccount cmdlet 获取帮助，请键入︰

    Get-Help Add-AzureAccount -Detailed

## 关于 Azure PowerShell 模块
Azure PowerShell 安装 0.8.0 版中的新的开始，包括多个 PowerShell 模块。 您必须明确决定是否使用 Azure 的模块或模块 Azure 资源管理器中可用的命令。 为了更容易地在它们之间切换，我们具有模块中添加新的 cmdlet，**开关 AzureMode**，Azure 的配置文件。

当您使用 Azure PowerShell 时，Azure 的模块中的 cmdlet，默认情况下导入。 若要切换到 Azure 资源管理器模块，请使用交换机 AzureMode cmdlet。 它将 Azure 模块删除从您的会话并将 Azure 资源管理器和 Azure 配置文件模块导入。

若要切换到的 AzureResoureManager 模块，请键入︰

    PS C:\> Switch-AzureMode -Name AzureResourceManager

若要切换回 Azure 的模块，请键入︰

    PS C:\> Switch-AzureMode -Name AzureServiceManagement

默认情况下，开关 AzureMode 会影响只对当前会话。 若要进行切换所有 PowerShell 会话中有效，使用交换机 AzureMode 的**全局**参数。

与交换机 AzureMode cmdlet 的帮助，请键入︰ `Get-Help Switch-AzureMode` ，或者查看[交换机 AzureMode](http://go.microsoft.com/fwlink/?LinkID=394398)。
  
若要帮助概述了 AzureResourceManager 模块中获取的 cmdlet 的列表，请键入︰ 

    PS C:\> Get-Command -Module AzureResourceManager | Get-Help | Format-Table Name, Synopsis

输出将类似于下面的摘录︰

    Name                                   Synopsis
    ----                                   --------
    Add-AlertRule                          Adds or updates an alert rule of either metric, event, o...
    Add-AzureAccount                       Adds the Azure account to Windows PowerShell
    Add-AzureEnvironment                   Creates an Azure environment
    Add-AzureKeyVaultKey                   Creates a key in a vault or imports a key into a vault.
        ...

若要获取完整的帮助 cmdlet，键入具有该格式的命令︰

    Get-Help <cmdlet-name> -Full

例如， 

    Get-Help Get-AzureLocation -Full

有关完整的 Azure 资源管理器的命令集，请参阅[Azure 资源管理器的 Cmdlet](http://go.microsoft.com/fwlink/?LinkID=394765)。 
  
## 创建资源组

本教程的这一节将指导您完成创建和部署 web 应用程序与 SQL 数据库的资源组的过程。 

您不必是专家 Azure、 SQL、 web 应用程序或资源管理来执行该任务。 模板提供了所有你可能需要的资源的资源组的模型。 然后，我们使用 Windows PowerShell 来自动执行任务，因为您可以使用这些过程作为模型大规模任务编写脚本。

### 步骤 1︰ 切换到 Azure 资源管理器 
1. 启动 PowerShell。 您可以使用任何主机程序都喜欢，如 Azure PowerShell 控制台或 Windows PowerShell ISE。

2. 使用**交换机 AzureMode** cmdlet 的 cmdlet 的 AzureResourceManager 和 AzureProfile 模块中导入。 

        PS C:\> Switch-AzureMode AzureResourceManager

3. 若要添加您的 Azure 帐户使用 Windows PowerShell 会话时，**添加 AzureAccount** cmdlet。 

        PS C:\> Add-AzureAccount

该 cmdlet 将提示您输入您的 Azure 帐户的登录凭据。 登录后，它将下载您的帐户设置以便它们可提供给 Windows PowerShell。 

帐户设置过期，因此需要偶尔刷新它们。 若要刷新的帐户设置，请再次运行**添加 AzureAccount** 。 

>[AZURE.NOTE] AzureResourceManager 模块需要添加 AzureAccount。 发布设置文件是不够的。     

### 步骤 2︰ 选择库模板

有多种方法可以创建资源组和资源，但最简单的方法是使用资源组模板。 *资源组模板*是定义资源的资源组中的 JSON 字符串。 该字符串包括用户定义的值，如大小和名称称为"参数"的占位符。

Azure 承载的资源组模板库并从头或编辑库模板，可以创建您自己的模板。 在本教程中，我们将使用一个库模板。 

若要查看所有 Azure 资源组模板库中的模板，使用**Get AzureResourceGroupGalleryTemplate** cmdlet;但是，此命令会返回大量的模板。 若要查看模板更易于管理的数目，指定发布服务器参数。 

在 Powershell 提示符下，键入︰
    
    PS C:\> Get-AzureResourceGroupGalleryTemplate -Publisher Microsoft

该 cmdlet 返回作为发布与 Microsoft 库模板的列表。 使用**标识**属性来标识该模板在命令。

Microsoft.WebSiteSQLDatabase.0.2.6 预览模板看起来有趣。 当您运行命令时，模板的版本可能是略有不同，因为已发布一个新版本。 使用最新版本的模板。 若要获取有关库模板的详细信息，请使用**标识**参数。 标识参数的值是模板的标识。

    PS C:\> Get-AzureResourceGroupGalleryTemplate -Identity Microsoft.WebSiteSQLDatabase.0.2.6-preview

该 cmdlet 返回关于该模板，包括摘要和说明的详细信息的对象。

该模板看起来它将满足我们的需要。 让我们将它保存到磁盘，并更仔细地看一下。

### 步骤 3︰ 查看模板

让我们将模板保存到磁盘上的 JSON 文件。 此步骤不是必需的但还可以更容易地查看模板。 若要保存该模板，请使用**Save AzureResourceGroupGalleryTemplate** cmdlet。 其**标识**参数用于指定模板和**Path**参数指定磁盘上的路径。

保存 AzureResourceGroupGalleryTemplate 保存模板并返回 JSON 模板文件的文件名的路径。 

    PS C:\> Save-AzureResourceGroupGalleryTemplate -Identity Microsoft.WebSiteSQLDatabase.0.2.6-preview -Path C:\Azure\Templates\New_WebSite_And_Database.json

    Path
    ----
    C:\Azure\Templates\New_WebSite_And_Database.json


您可以在记事本之类的文本编辑器中查看模板文件。 每个模板都有**参数**部分和**资源**部分。

模板的**参数**部分是在所有的资源中定义的参数的集合。 它包括可以提供时设置资源组的属性值。

    "parameters": {
      "siteName": {
        "type": "string"
      },
      "hostingPlanName": {
        "type": "string"
      },
      "siteLocation": {
        "type": "string"
      },
      ...
    }

一些参数有默认值。 当您使用该模板时，不需要为这些参数提供值。 如果不指定值，则使用默认值。 

    "collation": {
      "type": "string",
      "defaultValue": "SQL_Latin1_General_CP1_CI_AS"
    },

当参数具有枚举的值时，参数列出有效的值。 例如， **sku**参数可以采用的自由、 共享、 基本和标准的值。 如果不指定**sku**参数的值，它免费使用默认值。

    "sku": {
      "type": "string",
      "allowedValues": [
        "Free",
        "Shared",
        "Basic",
        "Standard"
      ],
      "defaultValue": "Free"
    },


注意， **administratorLoginPassword**参数使用安全的字符串，而非纯文字。 当您提供安全字符串的值时，该值是被遮住。 

    "administratorLoginPassword": {
      "type": "securestring"
    },

模板的**资源**部分列出的模板创建的资源。 该模板创建 SQL 数据库服务器和 SQL 数据库、 服务器场和网站上和几个管理设置。
  
每个资源的定义包括其属性，如名称、 类型和位置，以及用户定义的值的参数。 例如，此部分中的模板定义 SQL 数据库。 它包括数据库名称参数 ([parameters('databaseName')])，数据库服务器位置 [parameters('serverLocation')] 和排序规则属性 [parameters('collation')]。

    {
        "name": "[parameters('databaseName')]",
        "type": "databases",
        "location": "[parameters('serverLocation')]",
        "apiVersion": "2.0",
        "dependsOn": [
          "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
        ],
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveId": "[parameters('requestedServiceObjectiveId')]"
        }
    },


我们几乎可以使用该模板，但我们做之前，我们需要查找的每个资源的位置。

### 第 4 步︰ 获取资源类型位置

大多数模板要求您为每个资源的资源组中指定的位置。 每个资源位于 Azure 数据中心，但不是每个 Azure 数据中心支持每种资源类型。 

选择支持的资源类型的任意位置。 不需要的所有资源创建资源组中的同一位置;但是，只要有可能，您需要创建资源，在相同的位置，以优化性能。 特别是，要确保您的数据库是在访问它的应用程序相同的位置。 

若要获得支持每种资源类型的位置，使用**Get AzureLocation** cmdlet。 要将限制为特定类型的资源，如 ResourceGroup，输出使用︰

    Get-AzureLocation | Where-Object Name -eq "ResourceGroup" | Format-Table Name, LocationsString -Wrap

输出将类似于︰

    Name                                 LocationsString
    ----                                 ---------------
    ResourceGroup                        East Asia, South East Asia, East US, West US, North
                                         Central US, South Central US, Central US, North Europe,
                                         West Europe

现在，我们有我们需要创建的资源组的信息。

### 步骤 5︰ 创建资源组
 
在此步骤中，我们将使用资源组模板以创建资源组。 为了便于参考，打开磁盘上的 New_WebSite_And_Database.json 文件，跟着移动。 模板文件可以用于确定要传递的信息，如资源正确的 ApiVersion 参数值非常有用。

若要创建资源组，请使用**New AzureResourceGroup** cmdlet。

该命令使用**Name**参数指定资源组，使用**位置**参数来指定其位置的名称。 使用**Get AzureLocation**的输出为资源组选择一个位置。 它使用**GalleryTemplateIdentity**参数指定的库模板。

    PS C:\> New-AzureResourceGroup -Name TestRG1 -Location "East Asia" -GalleryTemplateIdentity Microsoft.WebSiteSQLDatabase.0.2.6-preview
            ....

只要键入模板名称，新建 AzureResourceGroup 提取模板、 分析，并动态地向该命令添加模板参数。 这使得它很容易地指定模板参数值。 并且，如果您忘记了所需的参数值，Windows PowerShell 的提示值。

**动态模板参数**

若要获取的参数，请键入减号 （-） 来指示参数名称，然后按 TAB 键。 或者，键入参数名称，例如，网站名称的开头几个字母，然后按 TAB 键。 

    PS C:\> New-AzureResourceGroup -Name TestRG1 -Location "East Asia" -GalleryTemplateIdentity Microsoft.WebSiteSQLDatabase.0.2.6-preview -si<TAB>

PowerShell 完成参数名称。 要循环参数名称，请重复按 TAB。

    PS C:\> New-AzureResourceGroup -Name TestRG1 -Location "East Asia" -GalleryTemplateIdentity Microsoft.WebSiteSQLDatabase.0.2.6-preview -siteName 

输入网站的名称，然后对每个参数重复选项卡过程。 使用默认值的参数是可选的。 若要接受默认值，忽略命令参数。 

当模板参数已枚举值，例如 sku 参数在此模板中，可循环使用的参数值，按 TAB 键。

    PS C:\> New-AzureResourceGroup -Name TestRG1 -Location "East Asia" -GalleryTemplateIdentity Microsoft.WebSiteSQLDatabase.0.2.6-preview -siteName TestSite -sku <TAB>

    PS C:\> New-AzureResourceGroup -Name TestRG1 -Location "East Asia" -GalleryTemplateIdentity Microsoft.WebSiteSQLDatabase.0.2.6-preview -siteName TestSite -sku Basic<TAB>

    PS C:\> New-AzureResourceGroup -Name TestRG1 -Location "East Asia" -GalleryTemplateIdentity Microsoft.WebSiteSQLDatabase.0.2.6-preview -siteName TestSite -sku Free<TAB>

这里是新建 AzureResourceGroup 命令指定仅需的模板参数和**详细**的常见参数的一个示例。 请注意**administratorLoginPassword**被省略。

    PS C:\> New-AzureResourceGroup -Name TestRG -Location "East Asia" -GalleryTemplateIdentity Microsoft.WebSiteSQLDatabase.0.2.6-preview -siteName TestSite -hostingPlanName TestPlan -siteLocation "East Asia" -serverName testserver -serverLocation "East Asia" -administratorLogin Admin01 -databaseName TestDB -Verbose

当您输入命令，提示您输入缺少的必需参数， **administratorLoginPassword**。 而且，当您键入的密码，安全字符串值被遮盖。 这种策略消除了风险提供密码以纯文本格式。

    cmdlet New-AzureResourceGroup at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: **********

**新 AzureResourceGroup**返回的资源组，它创建并部署。 

在几个步骤中，我们创建并部署一个复杂的网站所需的资源。 库模板提供了几乎所有我们需要执行此任务的信息。
而且，很容易自动化任务。 

## 获取有关资源组的信息

创建资源组之后, 可以使用 AzureResourceManager 模块中的 cmdlet 来管理您的资源组。

- 若要获取资源组的所有您的订阅中，使用**Get AzureResourceGroup** cmdlet:

        PS C:>Get-AzureResourceGroup

        ResourceGroupName : TestRG
        Location          : eastasia
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        
        ...

- 若要获取资源的资源组中，使用**Get AzureResource** cmdlet 和 ResourceGroupName 参数。 不带参数，获取 AzureResource Azure 订阅中获取的所有资源。

        PS C:\> Get-AzureResource -ResourceGroupName TestRG
        
        ResourceGroupName : TestRG
        Location          : eastasia
        ProvisioningState : Succeeded
        Tags              :
        
        Resources         :
                Name                   Type                          Location
                ----                   ------------                  --------
                ServerErrors-TestSite  microsoft.insights/alertrules         eastasia
                TestPlan-TestRG        microsoft.insights/autoscalesettings  eastus
                TestSite               microsoft.insights/components         centralus
                testserver             Microsoft.Sql/servers                 eastasia
                TestDB                 Microsoft.Sql/servers/databases       eastasia
                TestPlan               Microsoft.Web/serverFarms             eastasia
                TestSite               Microsoft.Web/sites                   eastasia
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG

## 添加到资源组

- 若要将资源添加到资源组，请使用**New AzureResource** cmdlet。 此命令将添加到 TestRG 的资源组的新网站。 此命令是有点复杂，，因为它不使用模板。 

        PS C:\>New-AzureResource -Name TestSite2 -Location "North Europe" -ResourceGroupName TestRG -ResourceType "Microsoft.Web/sites" -ApiVersion 2014-06-01 -PropertyObject @{"name" = "TestSite2"; "siteMode"= "Limited"; "computeMode" = "Shared"}

- 若要向资源组中添加新的基于模板的部署，使用**New AzureResourceGroupDeployment**命令。 

        PS C:\>New-AzureResourceGroupDeployment ` 
        -ResourceGroupName TestRG `
        -GalleryTemplateIdentity Microsoft.WebSite.0.2.6-preview `
        -siteName TestWeb2 `
        -hostingPlanName TestDeploy2 `
        -siteLocation "North Europe" 

## 移动资源

可以将现有资源移到新的资源组。 有关示例，请参阅[将资源移动到新的资源组或订阅](resource-group-move-resources.md)。

## 删除某一资源组

- 若要从资源组中删除资源时，使用**删除 AzureResource** cmdlet。 此 cmdlet 删除该资源，但不会删除该资源组。

    此命令从 TestRG 资源组中移除 TestSite2 网站。

        Remove-AzureResource -Name TestSite2 -ResourceGroupName TestRG -ResourceType "Microsoft.Web/sites" -ApiVersion 2014-06-01

- 要删除某一资源组，请使用**删除 AzureResourceGroup** cmdlet。 此 cmdlet 删除该资源组和资源。

        PS C:\ps-test> Remove-AzureResourceGroup -Name TestRG
        
        Confirm
        Are you sure you want to remove resource group 'TestRG'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## 资源组的疑难解答
当您尝试使用 AzureResourceManager 模块中的 cmdlet，您很可能会遇到错误。 使用本部分中的技巧来解决这些问题。

### 防止错误

AzureResourceManager 模块包含可帮助您避免错误的 cmdlet。


- **获得 AzureLocation**︰ 此 cmdlet 获取支持每种类型的资源的位置。 输入资源的位置之前，请使用此 cmdlet 验证位置支持的资源类型。


- **测试 AzureResourceGroupTemplate**︰ 在使用它们之前测试模板和模板参数。 输入一个自定义或库模板和要使用的模板参数值。 此 cmdlet 测试是否是内部一致的模板参数值是否设置与模板匹配。 



### 修复错误

- **获得 AzureResourceGroupLog**︰ 此 cmdlet 的资源组中的每个部署日志中获取项。 如果出现错误，首先检查部署日志。 

- **冗余和调试**︰ AzureResourceManager 模块中的 cmdlet 调用执行的实际工作的 REST Api。 若要查看 Api 返回的消息，设置 $DebugPreference 变量对"继续"，在您的命令中使用详细的公共参数。 消息通常提供重要线索的任何故障的原因。

- **您的 Azure 凭据未设置，或已过期**︰ 若要刷新 Windows PowerShell 会话中的凭据，请使用添加 AzureAccount cmdlet。 发布设置文件中的凭据没有足够的 AzureResourceManager 模块中的 cmdlet。


## 下一步行动

- 若要了解有关创建资源管理器模板，请参阅[创作 Azure 资源管理器模板](./resource-group-authoring-templates.md)。
- 若要了解有关部署模板，请参阅[部署了 Azure 资源管理器模板的应用程序](./resource-group-template-deploy.md)。
- 部署项目的详细示例，请参见[部署 microservices Azure 中预知](app-service-web/app-service-deploy-complex-application-predictably.md)。


测试
