---
ms.openlocfilehash: fb03faf48bebaf4385421e5c7d52d99fe59d3d14
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理密钥存储库使用 CLI |Microsoft Azure"
    description="使用本教程使用 CLI 自动密钥存储库中的常见任务"
    services="key-vault"
    documentationCenter=""
    authors="msmbaldwin"
    manager="mbaldwin"tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/25/2015"
    ms.author="bruceper"/>

# 管理密钥存储库使用 CLI #
大部分区域处于可以使用 azure 的密钥存储库。 有关详细信息，请参阅[密钥存储库定价页](../../../../pricing/details/key-vault/)。

## 简介  
使用本教程来帮助您开始使用 Azure 密钥存储库，以创建在 Azure 存储和管理加密密钥和 Azure 中的秘密加固的容器 （存储库）。 它将引导您完成使用 Azure 跨平台命令行界面来创建存储库包含密钥或密码，然后使用 Azure 应用程序的过程。 随后，还介绍如何应用程序然后使用该密钥或密码。

**估计完成时间︰** 20 分钟

>[AZURE.NOTE]  本教程不包括有关如何编写 Azure 应用程序，步骤包括，它演示如何授权应用程序以在密钥存储库中使用的密钥或密码的说明。
>
>目前，您不能在 Azure 门户配置 Azure 密钥存储库。 相反，使用下列命令行界面跨平台说明。 或者，Azure PowerShell 的说明，请参阅[本教程中等效](key-vault-get-started.md)。

关于 Azure 密钥存储库的概述信息，请参阅[Azure 密钥存储库是什么？](key-vault-whatis.md)

## 先决条件

若要完成本教程，您必须︰

- 对 Microsoft Azure 的订阅。 如果您没有，您可以注册[免费试用版](../../../pricing/free-trial)。
- 命令行界面版本 0.9.1 或更高版本。 若要安装最新版本并连接到 Azure 订阅，请参见[安装和配置 Azure 跨平台命令行界面](xplat-cli.md)。
- 应用程序将被配置为使用键或您在本教程中创建的密码。 示例应用程序是可从[Microsoft 下载中心获取](http://www.microsoft.com/download/details.aspx?id=45343)。 有关说明，请参阅随附的自述文件。

## 获得帮助使用 Azure 跨平台命令行界面

本教程假定您熟悉命令行界面 （大扫除，终端，通过命令提示符）

--帮助或-h 参数可以用于查看有关特定命令的帮助。 另外，azure 帮助 [命令] [选项] 格式还可用于返回相同的信息。 例如，以下命令都返回相同的信息︰

    azure account set --help

    azure account set -h

    azure help account set

当有疑问时有关参数所需的命令，请参阅的帮助使用 — 帮助、-h 或 azure 帮助 [命令]。

您还可以阅读下面的教程以熟悉使用 Azure 资源管理器在 Azure 跨平台命令行界面︰

- [如何安装和配置 Azure 跨平台命令行界面](xplat-cli.md)
- [使用 Azure 的跨平台命令行界面使用 Azure 资源管理器](xplat-cli-azure-resource-manager.md)


## 连接到您的订阅

若要记录中使用的组织的帐户，请使用下面的命令︰

    azure login -u username -p password

或如果您想要通过键入以交互方式登录

    azure login

>[AZURE.NOTE]  Login 方法只适用于组织的帐户中。 组织的帐户是由您的组织中，管理和组织的 Azure Active Directory 租户中定义的用户。


如果当前没有一个组织的帐户，并且使用的是 Microsoft 帐户登录到 Azure 订购，您可以轻松创建一个使用下列步骤。

1.  登录到[Azure 管理门户](https://manage.windowsazure.com/)，Active Directory 上的单击登录。
2.  如果不存在目录，选择创建您的目录，并提供所需的信息。
3.  选择您的目录，并添加新的用户。 该新用户是一个组织的帐户。 在用户创建后，您将提供用户和临时密码的电子邮件地址。 保存此信息，因为它用在另一个步骤。
4.  从门户中，选择设置，然后选择管理员。 选择添加，并作为共同管理员添加新的用户。 这允许组织帐户管理 Azure 的订阅。
5.  最后，从 Azure 门户注销，然后登录回中使用新的组织帐户。 如果是第一次登录此帐户，系统将提示您更改密码。

有关使用 Microsoft Azure 的组织帐户的详细信息，请参阅[Microsoft Azure 作为一个组织注册](sign-up-organization.md)。

如果多个订阅，并且想要指定一个特定使用 Azure 密钥存储库，请键入以下内容来查看您的帐户的订购︰

    azure account list

然后，若要指定订阅以使用，请键入︰

    azure account set <subscription name>

有关配置跨平台 Azure 的命令行界面的详细信息，请参阅[如何安装和配置 Azure 跨平台命令行界面](xplat-cli.md)。


## 切换到使用 Azure 资源管理器

密钥存储库需要 Azure 资源管理器，因此请键入以下命令切换到 Azure 资源管理器模式︰

    azure config mode arm

## 创建新的资源组

在使用 Azure 资源管理器时，所有相关的资源创建资源组中。 对于本教程，我们将创建新的资源组 ContosoResourceGroup。

    azure group create 'ContosoResourceGroup' 'East Asia'

第一个参数是资源组的名称，第二个参数是的位置。 对于位置，使用命令`azure location list`来确定如何指定到该示例中的另一个位置。 如果您需要更多的信息，请键入︰ `azure help location`



## 创建密钥存储库

使用`azure keyvault create`命令以创建密钥存储库。 此脚本有三个必需的参数︰ 资源组名、 密钥存储库名称和地理位置。

例如，如果您使用电子仓库名称 ContosoKeyVault、 ContosoResourceGroup，资源组名称和东亚的位置，请键入︰

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

此命令的输出显示了您刚才创建的密钥存储库的属性。 两个最重要的属性是︰

- **名称**︰ 在本例中，这是 ContosoKeyVault。 您将使用此名称的其他密钥存储库的 cmdlet。
- **vaultUri**︰ 此示例中是 https://contosokeyvault.vault.azure.net。 使用的 REST API，通过存储库的应用程序必须使用此 URI。

Azure 帐户已被授权对此密钥存储库执行任何操作。 没有其他人了。


## 将密钥添加到密钥存储库

如果您希望 Azure 密钥存储库，以便为您创建一个受保护的软件密钥，使用`azure key create`命令，并键入以下命令︰

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

但是，如果您有一个现有密钥.pem 文件另存为文件命名为 softkey.pem，您要上载到 Azure 密钥存储库中的本地文件中，键入以下命令以导入的密钥。PEM 文件，该文件中的密钥存储库服务软件保护密钥︰

    azure keyvault key import --vaultName 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' –-password 'PaSSWORD' --destination software

现在，您可以引用创建或上载到 Azure 密钥存储库，通过它的 URI 的键。 使用**https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey**始终获取的当前版本，并使用**https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**来获取该特定版本。

若要将一个秘密添加到存储库，这是一个名为 SQLPassword 的密码并具有值的 Pa$ $w0rd 到 Azure 密钥存储库，请键入以下命令︰

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

现在，您可以引用通过它的 URI 添加到 Azure 密钥存储库，该密码。 使用**https://ContosoVault.vault.azure.net/secrets/SQLPassword**始终获取的当前版本，并使用**https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**来获取该特定版本。

让我们查看密钥或刚创建的密码︰

- 若要查看您的密钥，请键入︰ `azure keyvault key list --vault-name 'ContosoKeyVault'`
- 若要查看您的密码，请键入︰ `azure keyvault secret list -–vault-name 'ContosoKeyVault'`


## Azure Active Directory 中注册应用程序

此步骤通常会通过开发人员，在单独的计算机上。 它不是特定于 Azure 密钥存储库，但出于完整性的考虑，将包括。


>[AZURE.IMPORTANT] 若要完成本教程后，您的帐户、 vault，并在此步骤中您将注册该应用程序必须在 Azure 位于同一个目录中。

使用密钥存储库的应用程序必须使用 Azure Active Directory 中的某个标记进行身份验证。 若要执行此操作，应用程序的所有者必须先注册该应用程序及其 Azure Active Directory 中。 在登记结束时，应用程序所有者获取下列值︰


- **应用程序 ID** (也称为客户机 ID) 和**身份验证密钥**（也称为共享密钥）。 应用程序必须提供这两种值对 Azure Active Directory 以获取令牌。 如何配置应用程序以执行此操作取决于应用程序。 密钥存储库的示例应用程序，应用程序所有者在 app.config 文件中设置这些值。



在 Azure Active Directory 中注册该应用程序︰

1. 登录到 Azure 的门户。
2. 在左边，单击**撤销**，然后选择将在其中注册应用程序的目录。 <br> <br> 注意︰ 您必须选择包含 Azure 订阅创建密钥存储库使用相同的目录。 如果您不知道哪个目录这是目录的，单击**设置**标识创建密钥存储库，使用订阅，请注意最后一列中显示的名称。

3. 单击**应用程序**。 如果没有应用程序已添加到您的目录，此页将显示仅**添加应用程序**链接。 单击该链接，或者也可以单击**添加**命令栏上。
4.  在**添加应用程序**向导中，在**要做什么？**页上，单击**添加我的公司正在开发的应用程序**。
5.  **告诉我们有关您的应用程序**页上为您的应用程序指定一个名称，选择**WEB 应用程序和/或 WEB API** （默认值）。 单击下一步图标。
6.  在**应用程序属性**页上为 web 应用程序指定的**符号打开 URL**和**应用程序 ID URI** 。 如果您的应用程序不具有这些值，您可以使它们为此步骤 （例如，您可以指定两个框的 http://test1.contoso.com）。 如果这些网站存在; 并不重要重要的是每个应用程序的应用程序 ID 的 URI 是不同的目录中每个应用程序。 该目录使用该字符串来标识您的应用程序。
7.  单击完成图标以在向导中保存您的更改。
8.  在快速启动页上，单击**配置**。
9.  滚动到**密钥**部分中，选择的时间，然后单击**保存**。 页面刷新，现在显示的密钥值。 此项的值与**客户 ID**值，您必须配置您的应用程序。 （有关此配置的说明将是特定于应用程序）。
10. 复制此页，您将使用的下一步要在您的存储库上设置权限的客户端 ID 值。




## 授权应用程序要使用的密钥或密码

若要授权访问密钥或密钥存储库中的应用程序，请使用`azure keyvault set-policy`命令。

例如，如果电子仓库名称是 ContosoKeyVault 和您要授权的应用程序的客户端 ID 为 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed，并且您要授权应用程序解密并使用您的存储库中的密钥对进行签名，然后运行以下命令︰

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perm-to-keys '[“decrypt”,”sign”]'
    
如果您想授权读取机密信息在您的存储库中的同一个应用程序，运行以下命令︰

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perm-to-secrets '["Get"]'

## 如果您想要使用硬件安全模块 (HSM) ##

为加强保证，您可以导入或在硬件安全模块 (Hsm)，永远不会离开的 HSM 边界生成密钥。 Hsm 是的 FIPS 140-2 2 级验证。 如果这一要求不适用于您，跳过此部分并转到[删除密钥存储库和相关密钥和密码](#delete-the-key-vault-and-associated-keys-and-secrets)。

若要创建这些 HSM 受保护的注册表项，您必须支持 HSM 保护的密钥存储库订阅。

当您创建 keyvault 时，添加 sku 参数︰

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

可以将软件保护的密钥 （如图所示前面） 和 HSM 保护密钥添加到此电子仓库。 若要创建一个受保护的 HSM 键，设置 HSM' 的目标参数︰

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

您可以使用下面的命令从您的计算机上的.pem 文件导入密钥。 此命令向 Hsm 密钥存储库服务中就导入密钥︰

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' –-password 'PaSSWORD'

下一个命令导入"携带您自己的密钥"(BYOK) 包。 这样就可以在您本地的 HSM，生成您的密钥，并将它转移到 Hsm 在密钥存储库服务，而无需离开 HSM 边界的关键︰

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

有关如何生成此 BYOK 软件包的更详细说明，请参阅[如何使用 HSM-Protected 键与 Azure 密钥存储库](https://msdn.microsoft.com/library/azure/dn903624.aspx)。


## 删除密钥存储库和相关的密钥和密码

如果您不再需要该密钥存储库和密钥或它所包含的秘密，您可以通过使用 azure keyvault 的删除命令删除密钥存储库︰

    azure keyvault delete --vault-name 'ContosoKeyVault'

或者，您可以删除整个 Azure 的资源组，其中包括密钥存储库和该组中包含的任何其他资源︰

    azure group delete --name 'ContosoResourceGroup'


## 其他 Azure 的跨平台命令行界面命令

您可能会用于管理 Azure 密钥存储库的其他命令。

此命令将列出表格显示的所有项和选定的属性︰

    azure keyvault key list --vault-name 'ContosoKeyVault'

此命令将显示指定的键属性的完整列表︰

    azure keyvault key show --vault-name 'ContosoKeyVault' –-key-name 'ContosoFirstKey'

此命令将列出表格显示的所有密钥名称和选定的属性︰

    azure keyvault secret list --vault-name 'ContosoKeyVault'

这里是如何删除特定项的示例︰

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

下面是如何删除特定密钥的一个示例︰

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## 下一步行动

编程参考信息，请参阅[Azure 密钥存储库 REST API 参考](https://msdn.microsoft.com/library/azure/dn903609.aspx)和[Azure 密钥存储库 C# 客户端 API 参考](https://msdn.microsoft.com/library/azure/dn903628.aspx)。

测试
