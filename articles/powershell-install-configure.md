---
ms.openlocfilehash: 19c0e5ea551b47ac1c0b6f1b239dd460bef59d6b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何安装和配置 Azure PowerShell"
    description="了解如何安装和配置 Azure PowerShell。"
    editor="tysonn"
    manager="stevenka"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/20/2015"
    ms.author="coreyp"/>

# 如何安装和配置 Azure PowerShell#

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure CLI</a></div>

可以使用 Windows PowerShell 若要在命令提示符下以交互方式或通过脚本自动在 Azure，执行各种任务。 Azure PowerShell 是提供 cmdlet 来管理通过 Windows PowerShell Azure 的模块。 可以使用 cmdlet 创建、 测试、 部署和管理的解决方案和通过 Azure 平台提供的服务。 在大多数情况下，您可以使用 cmdlet 来执行相同的任务，您可以通过 Azure 管理门户。 例如，您可以创建和配置云服务、 虚拟机、 虚拟网络和 web 应用程序。

模块作为一个可下载的文件分发，并通过公开可用的存储库管理的源代码。 本主题中后面的安装说明中提供了指向可下载文件的链接。 关于源代码的信息，请参阅[Azure PowerShell 代码存储库](https://github.com/Azure/azure-powershell)。

本指南提供了有关安装和设置管理 Azure 平台 Azure PowerShell 的基本信息。

### <a id="Prereq"></a>使用 Azure PowerShell 的先决条件

Azure 是一个基于订阅的平台。 这意味着订阅时需要使用该平台。 在大多数情况下，这也意味着这些 cmdlet 需要执行的任务与您的订阅的订阅信息。 （某些与存储相关的 cmdlet 可以使用不包含此信息。）这可以通过配置您的计算机连接到您的订阅来提供。 在本文中，在下提供说明"如何︰ 连接到您的订阅。"

> [AZURE.NOTE] 在 0.8.5 版本开始，Azure PowerShell 模块需要 Microsoft.net 4.5。

安装本模块时，安装程序将检查您的系统所需的软件，并安装所有依赖项，如 Windows PowerShell 和.NET Framework 的正确版本。

<a id="Install"></a>
## 如何︰ 安装 Azure PowerShell

您可以通过下载和安装的 Azure PowerShell 模块运行[Microsoft Web 平台安装程序](http://go.microsoft.com/fwlink/p/?LinkId=320376)。 出现提示时，单击**运行**。 Web 平台安装程序安装的 Azure PowerShell 模块和所有依赖项。 请按照提示完成安装。

> [AZURE.NOTE] 如果您只想下载 PowerShell 安装程序，请访问 https://github.com/Azure/azure-powershell/releases。
可以在此 repo 也找到 PowerShell cmdlet 的源代码

Azure 的可用的命令行工具的详细信息，请参阅[命令行工具]( http://go.microsoft.com/fwlink/?LinkId=320796)。

安装模块的 Azure PowerShell 也安装自定义的控制台。 您可以从标准 Windows PowerShell 控制台或 Azure PowerShell 控制台运行这些 cmdlet。

打开任一控制台所使用的方法取决于您运行的 Windows 的版本︰

- 至少运行的计算机上 Windows 8 或 Windows Server 2012，您可以使用内置的搜索功能。 从开始屏幕中，开始键入**电源**。 此方法返回指定了作用域的应用程序列表，其中包含 Windows PowerShell 和 Azure PowerShell。 若要打开控制台，请单击任一应用程序。 （若要固定到开始屏幕的应用程序，右击图标。）

- 在计算机上运行的版本早于 Windows 8 或 Windows Server 2012，使用开始菜单。 从开始菜单中，单击**所有程序**，都单击**Azure**，，然后都单击**Azure PowerShell**。

<a id="Connect"></a>
## 如何︰ 连接到您的订阅

使用 Azure 需要预订。 如果您没有订阅，请参阅[开始使用 Azure](http://go.microsoft.com/fwlink/p/?LinkId=320795)。

这些 cmdlet 需要订购，因此他们可以管理您的服务。 有两种方法来向 Windows PowerShell 提供您的订阅信息。 您可以使用管理证书包含的信息，或者您可以登录到 Azure 使用 Microsoft 客户或工作或学校的帐户。 当您登录时，Azure 活动目录 (AD Azure) 验证凭据，并返回一个访问令牌，以便管理您的帐户的 Azure PowerShell。

若要帮助您选择适合您的需要的身份验证方法，请考虑下列问题︰

- Azure 广告是建议的身份验证方法，因为还可以更容易地管理对订阅的访问。 0.8.6 版中的新的更新，它使 Azure AD 身份验证与自动化方案，如果某一工作或学校科目。 它也适用于 Azure 资源管理器 API。
- 使用证书订阅信息时，可用，只要订阅和证书有效。 但是，此方法导致难管理对共享的预订，如多个用户有权访问该帐户的访问。 此外，Azure 资源管理器 API 不接受证书身份验证。

在 Azure 中的身份验证和订阅管理有关的详细信息，请参阅[管理帐户、 预订和管理角色](http://go.microsoft.com/fwlink/?LinkId=324796)。

### 使用 Azure 的广告方法

1. 中说明的那样打开 Azure PowerShell 控制台中，[方法︰ 安装 Azure PowerShell](#Install)。

2. 键入以下命令︰

        Add-AzureAccount

3. 在窗口中，键入电子邮件地址和密码与您的帐户相关联。

4. Azure 进行身份验证并保存的凭据信息，然后关闭该窗口。

5. 从 0.8.6，如果您登录使用某一工作或学校的帐户，可以键入下面的命令绕过弹出窗口。 这将弹出标准 Windows PowerShell 凭据窗口输入您的工作或学校的帐户的用户名和密码。

        $cred = Get-Credential
        Add-AzureAccount -Credential $cred

    > [AZURE.NOTE] 安全性和使用的凭据的详细信息，请参阅[部署密码和其他敏感数据添加到 ASP.NET 和 Azure 网站的最佳做法](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure)。

    > [AZURE.NOTE] 此非交互式登录方法仅适用于工作或学校的帐户中。  工作或学校的帐户是由您的工作或学校，并为您的工作或学校的 Azure Active Directory 实例中定义的用户。 如果当前没有一个工作或学校的账户，并使用 Microsoft 帐户登录到 Azure 订购，您可以轻松创建一个使用下列步骤。
    >
    > 1. 到[Azure 管理门户](https://manage.windowsazure.com)，单击在**Active Directory**中的登录。
    >
    > 2. 如果不存在目录，选择**创建您的目录**，并提供所需的信息。
    >
    > 3. 选择您的目录，并添加新的用户。 该新用户可以登录使用的工作或学校的帐户。
    >
    >     在用户创建后，您将提供用户和临时密码的电子邮件地址。 保存此信息，因为它用在另一个步骤。
    >
    > 4. 从管理门户中，选择**设置**，然后选择**管理员**。 选择**添加**，并作为共同管理员添加新的用户。 这样的工作或学校的帐户管理 Azure 订购。
    >
    > 5. 最后，从 Azure 门户注销，然后重新登录使用工作或学校科目。 如果是第一次登录此帐户，系统将提示您更改密码。
    >
    >注册 Microsoft Azure 的工作或学校的帐户的详细信息，请参阅[Microsoft Azure 作为一个组织注册](sign-up-organization.md)。

### 使用证书的方法

Azure 的模块都包含帮助您下载并导入证书的 cmdlet。

> [AZURE.NOTE] AzureResourceManager 模块中的 cmdlet 要求 Azure 的广告方法 (添加 AzureAccount)。 不支持这些 cmdlet 发布设置文件。 AzureResourceManager 模块中的 cmdlet 的详细信息，请参阅[Azure 资源管理器的 Cmdlet](http://go.microsoft.com/fwlink/?LinkID=394765)。


- **获得 AzurePublishSettingsFile** cmdlet 将打开 web 页在 Azure 管理门户网站，您可从中下载订阅信息。 在.publishsettings 文件中包含的信息。

- **导入 AzurePublishSettingsFile**使用.publishsettings 文件导入模块。 此文件包括具有安全的凭据管理证书。

> [AZURE.IMPORTANT] 我们建议您删除发布配置文件，您可以使用下载<b>Get AzurePublishSettingsFile</b>导入这些设置之后。 管理证书中包含的安全凭据，因为未经授权的用户应该不能访问。 如果您需要有关您的订阅的信息，可以从[Azure 管理门户](http://manage.windowsazure.com/)或[Microsoft Online Services 客户门户](http://go.microsoft.com/fwlink/p/?LinkId=324875)获得。

1. 登录到[Azure 管理门户](http://manage.windowsazure.com)使用 Azure 帐户的凭据。

2. 中说明的那样打开 Azure PowerShell 控制台中，[方法︰ 安装 Azure PowerShell](#Install)。

3. 键入以下命令︰

        Get-AzurePublishSettingsFile

4. 出现提示时，下载并保存发布配置文件并记下的路径和.publishsettings 文件的名称。 当您运行用于导入设置**导入 AzurePublishSettingsFile** ，此信息是必需的。 默认位置和文件名称的格式是︰

            C:\\Users\<UserProfile>\\Download\\[*MySubscription*-...]-*downloadDate*-credentials.publishsettings

5. 键入如下命令，替换 Windows 帐户的名称和路径和文件名称的占位符︰

        Import-AzurePublishSettingsFile C:\Users\<UserProfile>\Downloads\<SubscriptionName>-credentials.publishsettings

> [AZURE.NOTE] 如果您将添加到其他订阅作为共同管理员导入后您发布设置中，您将需要重复此过程以下载一个新的.publishsettings 文件，然后导入这些设置。 有关添加联管理员以帮助管理订阅的服务的信息，请参阅[添加和删除您的 Azure 订阅联管理员](http://msdn.microsoft.com/library/windowsazure/gg456328.aspx)。

### 查看帐户和申请详细信息

您可以有多个帐户和可供使用 Azure PowerShell 的订阅。 通过多次运行添加 AzureAccount，您可以添加多个帐户。

要获取可用的 Azure 帐户，请键入︰

    Get-AzureAccount

要获取您 Azure 的订阅内容，请键入︰

    Get-AzureSubscription

## <a id="Ex"></a>如何使用 cmdlet︰ 示例 ##

已经安装了模块，并配置您的计算机连接到您的订阅后，您可以创建 Azure 的 web 应用程序。 本示例将让您开始使用 Azure 的 cmdlet。

1. 启动 Azure PowerShell 控制台。

2. 选择您的 web 应用程序的名称。 选择符合命名约定的 DNS 名称。 有效的名称可以只包含字母 a 到 z，数字"0"至"9"和连字符 (-)。

    Web 应用程序名称必须是唯一在 Azure 中。 我们将在本示例中，使用"mySite"，但一定要选择一个不同的名称，如您跟大量的帐户名称。  

    您选择的名称后，请键入类似以下的命令。 替换为您的 web 应用程序名称为"mySite"。

        New-AzureWebsite mySite

    该 cmdlet 创建 web 应用程序，并返回一个对象，表示新的 web 应用程序。 对象属性包括有关 web 应用程序的有用信息。

3. 要获取有关 web 应用程序的信息，请键入以下命令。 它返回的所有 web 应用程序的信息在订阅中，包括您刚刚创建的那个。

        Get-AzureWebsite

4. 若要获取有关您的 web 应用程序的详细信息，包括命令中的 web 应用程序名称。 务必用"mySite"的 web 应用程序的名称。

        Get-AzureWebsite -Name mySite

5. 在创建后，会启动 web 应用程序。 若要停止 web 应用程序，请键入此命令中，包括您的 web 应用程序的名称。

        Stop-AzureWebsite -Name mySite

6. 若要验证该站点的状态已停止，请再次运行 Get AzureWebsite 命令。

        Get-AzureWebsite

7. 若要完成此测试，请删除该 web 应用程序。 类型：  

        Remove-AzureWebsite -Name mySite

7. 若要完成此任务，确认 web 应用程序被删除。

        Get-AzureWebsite -Name mySite

##<a id="Help"></a>获取帮助##

这些资源为特定的 cmdlet 提供帮助︰


-   从在控制台中，您可以使用内置的帮助系统。 **获取帮助**cmdlet 提供访问此系统。 下表提供可用来获得帮助的命令的一些示例。 通过键入**help**，可以从控制台中的详细信息。

    <table border="1" cellspacing="4" cellpadding="4">
    <tbody>
    <tr align="left" valign="top">
        <td><b>命令</b></td>
        <td><b>说明</b></td>
    </tr>
    <tr align="left" valign="top">
        <td>获取帮助</td>
        <td>描述如何使用帮助系统。 <p><b>注意</b>︰ 描述包括某些信息的帮助文件不适用于 Azure 的模块。 具体来说，当安装了模块安装帮助文件。 它们不可用于下载分开。</p>
</td>
    </tr>
    <tr align="left" valign="top">
        <td>获取帮助 Azure</td>
        <td>在 Azure 的模块中获取所有 cmdlet。</td>
    </tr>
    <tr align="left" valign="top">
        <td>获取帮助&lt;<b>语言</b>&gt;的开发</td>
        <td>为开发和管理应用程序特定的语言中获取 cmdlet。 例如，帮助节点开发、 帮助 php 开发或帮助 python 开发</td>
    </tr>
        <tr align="left" valign="top">
        <td>获取帮助&lt;<b>cmdlet</b>&gt;</td>
        <td>获取有关 Windows PowerShell cmdlet 的帮助。 更换<cmdlet>使用 cmdlet 名称。</td>
    </tr>
    <tr align="left" valign="top">
        <td>获取帮助&lt;<b>cmdlet</b>&gt; -参数 *</td>
        <td>获取 cmdlet 参数的描述。 星号 （*） 表示"所有"。</td>
    </tr>
    <tr align="left" valign="top">
        <td>获取帮助&lt;<b>cmdlet</b>&gt; -示例</td>
        <td>获取的语法和使用 cmdlet 的示例。</td>
    </tr>
    <tr align="left" valign="top">
        <td>获取帮助&lt;<b>cmdlet</b>&gt; -完整</td>
        <td>获取的 cmdlet，包括技术细节的所有帮助。</td>
    </tr>
    </tbody>
    </table>



- 在 Azure 库还有 Azure PowerShell 模块中的 cmdlet 的参考信息。 有关信息，请参阅[Azure Cmdlet 参考](http://msdn.microsoft.com/library/windowsazure/jj554330.aspx)。

来自社区的帮助，请尝试这些热门的论坛︰

- [在 MSDN 上的 azure 论坛]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
- [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)


## <a id="Resources"></a>其他资源 ##

这些是一些可用于了解如何使用 Azure 和 Windows PowerShell 的可用资源。

- 若要了解有关如何访问 Azure 存储组件，请参阅[使用 Azure 存储使用 Azure PowerShell](storage-powershell-guide-full.md)。

- 若要提供反馈的 cmdlet，报告问题，或访问的源代码，请参阅[Azure PowerShell 代码存储库](https://github.com/WindowsAzure/azure-sdk-tools)。

- 若要了解有关 Windows PowerShell 命令行和脚本编写环境，请参阅[TechNet 脚本中心](http://go.microsoft.com/fwlink/p/?LinkId=320211)。

- 有关安装、 学习、 使用和自定义 Windows PowerShell 的信息，请参阅[使用 Windows PowerShell 的脚本](http://go.microsoft.com/fwlink/p/?LinkId=320210)。

- 有关什么是脚本以及如何在 Windows PowerShell 运行它们的信息，请参阅[运行脚本](http://go.microsoft.com/fwlink/p/?LinkId=320627)。 本文包含有关创建脚本和配置您的计算机运行的脚本的基本信息。

- Azure AD cmdlet 的信息，请参阅[管理 Azure 使用 Windows PowerShell 的广告](http://go.microsoft.com/fwlink/p/?LinkId=320628)。





  [Microsoft Online Services 客户门户]: https://mocp.microsoftonline.com/site/default.aspx

测试
