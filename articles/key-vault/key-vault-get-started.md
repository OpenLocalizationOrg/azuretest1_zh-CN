---
ms.openlocfilehash: 9d5fba949893f8900dd0f6de5c36061ad24fed00
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 密钥存储库 |Microsoft Azure"
    description="使用本教程来帮助您开始使用 Azure 密钥存储库，以加强的容器在 Azure 存储和管理加密密钥和 Azure 中的机密信息。"
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/22/2015"
    ms.author="cabailey"/>

# 开始使用 Azure 密钥存储库 #
大部分区域处于可以使用 azure 的密钥存储库。 有关详细信息，请参阅[密钥存储库定价页](../../../../pricing/details/key-vault/)。

## 简介  
使用本教程来帮助您开始使用 Azure 密钥存储库，以创建在 Azure 存储和管理加密密钥和 Azure 中的秘密加固的容器 （存储库）。 它将引导您通过使用 Windows PowerShell 创建存储库包含密钥或密码，然后使用 Azure 应用程序的过程。 它然后向您显示方式，应用程序可以使用该密钥或密码。

*估计完成时间︰** 20 分钟

>[AZURE.NOTE]  本教程不包括说明如何编写 Azure 应用程序，步骤包括，即如何授权应用程序以在密钥存储库中使用的密钥或密码。
>
>目前，您不能在 Azure 门户配置 Azure 密钥存储库。 相反，使用这些 Azure PowerShell 说明。 或者，跨平台命令行接口的说明，请参阅[本教程中等效](key-vault-manage-with-cli.md)。

关于 Azure 密钥存储库的概述信息，请参阅[Azure 密钥存储库是什么？](key-vault-whatis.md)

## 先决条件

若要完成本教程，您必须︰

- 对 Microsoft Azure 的订阅。 如果您没有，您可以注册[免费试用版](../../../../pricing/free-trial)。
- Azure PowerShell 版本 0.9.1 或更高版本。 若要安装最新版本，并将其与您 Azure 的订阅，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。
- 应用程序将被配置为使用键或您在本教程中创建的密码。 示例应用程序是可从[Microsoft 下载中心获取](http://www.microsoft.com/en-us/download/details.aspx?id=45343)。 有关说明，请参阅随附的自述文件。


本教程为 Windows PowerShell 初学者而设计，但它假定您了解的基本概念，如模块、 cmdlet 和会话。 有关 Windows PowerShell 的详细信息，请参阅[Windows PowerShell 入门](https://technet.microsoft.com/library/hh857337.aspx)。

要获得任何在本教程中，您看到的 cmdlet 的详细的帮助，请使用**获取帮助**cmdlet。

    Get-Help <cmdlet-name> -Detailed

例如，要**添加 AzureAccount** cmdlet 获取帮助，请键入︰

    Get-Help Add-AzureAccount -Detailed

您还可以阅读下面的教程来获取 Windows PowerShell 熟悉使用 Azure 资源管理器中︰

- [如何安装和配置 Azure PowerShell](../powershell-install-configure.md)
- [使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)


## <a id="connect"></a>连接到您的订阅 ##

Azure PowerShell 会话启动并登录到 Azure 帐户使用以下命令︰  

    Add-AzureAccount

在弹出的浏览器窗口中，输入您的 Azure 帐户的用户名和密码。 Windows PowerShell 将获得相关联，此帐户和默认情况下，将使用第一个的所有订阅。

如果多个订阅，并且想要指定一个特定使用 Azure 密钥存储库，请键入以下内容来查看您的帐户的订购︰

    Get-AzureSubscription

然后，若要指定订阅以使用，请键入︰

    Select-AzureSubscription -SubscriptionName <subscription name>

有关配置 Azure PowerShell 的详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。

## <a id="switch"></a>切换到使用 Azure 资源管理器 ##

密钥存储库 cmdlet 需要 Azure 资源管理器，因此请键入以下命令切换到 Azure 资源管理器模式︰

    Switch-AzureMode AzureResourceManager

## <a id="resource"></a>创建新的资源组 ##

使用 Azure 资源管理器时，所有相关的资源创建资源组中。 我们将创建新的资源组名**ContosoResourceGroup**为本教程︰

    New-AzureResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'

针对**-位置**参数，命令[获取 AzureLocation](https://msdn.microsoft.com/library/azure/dn654582.aspx)用于确定如何指定到该示例中的另一个位置。 如果您需要更多的信息，请键入︰ `Get-Help Get-AzureLocation`


## <a id="vault"></a>创建密钥存储库 ##

使用[New AzureKeyVault](https://msdn.microsoft.com/library/azure/dn903602.aspx) cmdlet 创建密钥存储库。 此 cmdlet 都有强制性的三个参数︰ 一个**资源组的名称**、**密钥存储库名称**和**地理位置**。

例如，如果您使用电子仓库名称**ContosoKeyVault**、 **ContosoResourceGroup**，资源组名称和**东亚**的位置，请键入︰

    New-AzureKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

此 cmdlet 的输出显示刚刚创建了密钥存储库的属性。 两个最重要的属性是︰

- **名称**︰ 在本例中，这是**ContosoKeyVault**。 您将使用此名称的其他密钥存储库的 cmdlet。
- **vaultUri**︰ 在该示例中，这是 https://contosokeyvault.vault.azure.net/。 使用的 REST API，通过存储库的应用程序必须使用此 URI。

Azure 帐户已被授权对此密钥存储库执行任何操作。 没有其他人了。

## <a id="add"></a>将密钥添加到密钥存储库 ##

如果您希望 Azure 密钥存储库，以便为您创建一个受保护的软件密钥，使用[添加 AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048.aspx) cmdlet，然后键入以下命令︰

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

但是，如果您有现有软件保护密钥。PFX 文件保存到 C:\ 驱动器命名要上载到 Azure 密钥存储库的 softkey.pfx 文件中键入以下命令以设置变量**securepfxpwd** **123**个密码。PFX 文件︰

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

然后键入以下要导入的密钥。PFX 文件中的密钥存储库服务软件保护密钥︰

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


您现在可以引用此键创建或上载到 Azure 密钥存储库，通过它的 URI。 使用**https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey**始终获取的当前版本，并使用**https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**来获取该特定版本。  

若要显示此参数的 URI，请键入︰

    $Key.key.kid

若要将密码添加到存储库，它是一个名为 SQLPassword 的密码和 Pa$ $w0rd 到 Azure 密钥存储库的值，首先将转换值的 Pa$ $w0rd 为安全字符串键入以下内容︰

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

然后，键入以下命令︰

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

现在，您可以引用通过它的 URI 添加到 Azure 密钥存储库，该密码。 使用**https://ContosoVault.vault.azure.net/secrets/SQLPassword**始终获取的当前版本，并使用**https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**来获取该特定版本。

若要显示此机密的 URI，请键入︰

    $secret.Id

让我们查看密钥或刚创建的密码︰

- 若要查看您的密钥，请键入︰ `Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- 若要查看您的密码，请键入︰ `Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

现在，您的密钥存储库和密钥或密码已准备好要使用的应用程序。 您必须授权使用它们的应用程序。  

## <a id="register"></a>Azure Active Directory 中注册应用程序 ##

此步骤通常会通过开发人员，在单独的计算机上。 它不是特定于 Azure 密钥存储库，但包括此处的完整性。


>[AZURE.IMPORTANT] 若要完成本教程后，您的帐户、 vault，并在此步骤中您将注册该应用程序必须在 Azure 位于同一个目录中。

使用密钥存储库的应用程序必须使用 Azure Active Directory 中的某个标记进行身份验证。 若要执行此操作，应用程序的所有者必须先注册该应用程序及其 Azure Active Directory 中。 在登记结束时，应用程序所有者获取下列值︰


- **应用程序 ID** (也称为客户机 ID) 和**身份验证密钥**（也称为共享密钥）。 应用程序必须提供这两个值对 Azure Active Directory 以获取令牌。 如何配置应用程序以执行此操作取决于应用程序。 密钥存储库的示例应用程序，应用程序所有者在 app.config 文件中设置这些值。

在 Azure Active Directory 中注册该应用程序︰

1. 登录到 Azure 的门户。
2. 在左边，单击**撤销**，然后选择将在其中注册应用程序的目录。 <br> <br> **注意︰**您必须选择包含 Azure 订阅创建密钥存储库使用相同的目录。 如果您不知道哪个目录这是目录的，单击**设置**标识创建密钥存储库，使用订阅，请注意最后一列中显示的名称。

3. 单击**应用程序**。 如果没有应用程序已添加到您的目录，此页将显示仅**添加应用程序**链接。 单击该链接，或者也可以单击**添加**命令栏上。
4.  在**添加应用程序**向导中，在**要做什么？**页上，单击**添加我的公司正在开发的应用程序**。
5.  **告诉我们有关您的应用程序**页上为您的应用程序指定一个名称，然后选择**WEB 应用程序和/或 WEB API** （默认）。 单击**下一步**图标。
6.  在**应用程序属性**页上为 web 应用程序指定的**符号打开 URL**和**应用程序 ID URI** 。 如果您的应用程序不具有这些值，您可以使它们为此步骤 （例如，您可以指定两个框的 http://test1.contoso.com）。 如果这些网站存在; 并不重要重要的是每个应用程序的应用程序 ID 的 URI 是不同的目录中每个应用程序。 该目录使用该字符串来标识您的应用程序。
7.  单击**完成**图标，可在向导中保存您的更改。
8.  在**快速启动**页上，单击**配置**。
9.  滚动到**密钥**部分中，选择的时间，然后单击**保存**。 页面刷新，现在显示的密钥值。 此项的值与**客户 ID**值，您必须配置您的应用程序。 （有关此配置的说明将是特定于应用程序）。
10. 复制此页，您将使用的下一步要在您的存储库上设置权限的客户端 ID 值。

## <a id="authorize"></a>授权应用程序要使用的密钥或密码 ##

若要授权访问密钥或密钥存储库中的应用程序，使用 [集 AzureKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/dn903607.aspx) cmdlet。

例如，如果电子仓库名称是**ContosoKeyVault**和您要授权的应用程序都有的客户端 ID 为 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed，并且您想要授权解密并使用您的存储库中的密钥对进行签名的应用程序，运行以下命令︰


    Set-AzureKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
    
如果您想授权读取机密信息在您的存储库中的同一个应用程序，运行以下命令︰


    Set-AzureKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>如果您想要使用硬件安全模块 (HSM) ##

为加强保证，您可以导入或在硬件安全模块 (Hsm)，永远不会离开的 HSM 边界生成密钥。 Hsm 是的 FIPS 140-2 2 级验证。 如果这一要求不适用于您，跳过此部分并转到[删除密钥存储库和相关密钥和密码](#delete)。

若要创建这些 HSM 受保护的注册表项，您必须[保险存储的订阅的支持 HSM 保护密钥](../../../pricing/free-trial)。


创建存储库时，添加**的 SKU**参数︰


    New-AzureKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



可以将软件保护的密钥 （如图所示前面） 和 HSM 保护密钥添加到此电子仓库。 若要创建一个受保护的 HSM 键，将设置**-目标**'HSM 参数︰

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

可以使用以下命令来导入的密钥。在您的计算机上的 PFX 文件。 此命令向 Hsm 密钥存储库服务中就导入密钥︰

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


下一个命令导入"携带您自己的密钥"(BYOK) 包。 这样就可以在您本地的 HSM，生成您的密钥，并将它转移到 Hsm 在密钥存储库服务，而无需离开 HSM 边界的关键︰

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

有关如何生成此 BYOK 软件包的更详细说明，请参阅[如何生成和传输 Azure 密钥存储库的 HSM 保护密钥](key-vault-hsm-protected-keys.md)。

## <a id="delete"></a>删除密钥存储库和相关的密钥和密码 ##

如果您不再需要该密钥存储库和密钥或它所包含的秘密，您可以通过[删除 AzureKeyVault](https://msdn.microsoft.com/library/azure/dn903603.aspx) cmdlet 删除密钥存储库︰

    Remove-AzureKeyVault -VaultName 'ContosoKeyVault'

或者，您可以删除整个 Azure 的资源组，其中包括密钥存储库和该组中包含的任何其他资源︰

    Remove-AzureResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>其他 Azure PowerShell Cmdlet ##

您可能会发现有用管理 Azure 密钥存储库的其他命令︰

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`︰ 此命令获取表格显示的所有项和选定的属性。
- `$Keys[0]`︰ 此命令将显示指定的键属性的完整列表
- `Get-AzureKeyVaultSecret`︰ 此命令将列出所有密钥名称和所选的属性的表格式显示。
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`︰ 示例如何删除特定项。
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`︰ 示例如何删除特定的机密。


## <a id="next"></a>下一步行动 ##

使用 Azure 密钥存储库中的 web 应用程序的后续教程，请参见[Web 应用程序中使用 Azure 密钥存储库](key-vault-use-from-web-application.md)。

有关 Windows PowerShell cmdlet Azure 密钥存储库的列表，请参阅[Azure 密钥存储库 Cmdlet](https://msdn.microsoft.com/library/azure/dn868052.aspx)。

设计程序的引用，请参阅 MSDN 上的 Microsoft Azure 文档库中的[密钥存储库](https://msdn.microsoft.com/library/azure/dn903625.aspx)。

测试
