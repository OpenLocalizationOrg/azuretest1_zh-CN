---
ms.openlocfilehash: 0bdea75139893b63266648a420d2637f1b38c37c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="日志中的从 Azure 命令行接口 (CLI Azure) |Microsoft Azure"
    description="连接到 Azure 订阅从 Azure 命令行界面 (Azure CLI)"
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/09/2015"
    ms.author="danlep"/>

# 连接到订阅了 Azure 从 Azure 命令行界面 (Azure CLI)

Azure CLI 是开源，一套跨平台 Azure 平台使用的命令。 本文档介绍如何从 Azure CLI 连接到 Azure 订购。 有关安装说明，请参阅[安装 Azure CLI](xplat-cli-install.md)。

<a id="configure"></a>
## 如何连接到 Azure 订阅

大多数命令的 Azure CLI 提供需要连接到 Azure 帐户。 有两种方法配置 Azure CLI 使用您的订购︰

* 登录到 Azure 使用工作或学校帐户 （也称为组织帐户）。 这将使用 Azure Active Directory 凭据进行身份验证。

或

* 下载并使用发布设置文件。 这将安装允许您执行管理任务的只要订阅，该证书是有效的证书。

有关身份验证和订阅管理的详细信息，请参见["什么是基于帐户的身份验证和基于证书的身份验证的区别"][authandsub]。

如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用][免费试用版]。

> [AZURE.NOTE] 最灵活、 最高级的 Azure 服务使用 Azure 资源管理器或[ARM 模式](xplat-cli-azure-resource-manager.md)。 ARM 模式需要连接到 Azure 的工作或学校的帐户，使用下面描述的方法中的日志。

### 在方法中使用日志

Login 方法仅适用于工作或学校的帐户中。 此帐户是由您的组织，管理和组织的 Azure Active Directory 中定义。

> [AZURE.NOTE] 如果您目前没有工作或学校的帐户，并使用个人帐户登录到 Azure 订购，您可以轻松创建一个使用下列步骤。
>
> 1. 登录到[Azure 门户网站][门户]，并选择**有效的目录**。
>
> 2. 如果不存在目录，选择**创建您的目录**，并提供所需的信息。
>
> 3. 选择您的目录，并添加新的用户。 该新用户是一个工作或学校的帐户。
>
>     在用户创建后，您将提供用户和临时密码的电子邮件地址。 以后需要时，请保存此信息。
>
> 4. 从 Azure 的门户中，选择**设置**，然后选择**管理员**。 选择**添加**，并作为共同管理员添加新的用户。 这样的工作或学校的帐户管理 Azure 订购。
>
> 5. 最后，从 Azure 门户注销，然后重新登录使用的新的工作或学校的帐户。 如果是第一次登录此帐户，系统将提示您更改密码。
>
> 6. 请确保您的工作或学校的帐户登录时，将显示您的订阅。
>
>在工作或学校的帐户的详细信息，请参阅[Microsoft Azure 作为一个组织注册]的[signuporg]。

从 Azure CLI 使用工作或学校的帐户登录，请使用下面的命令︰

    azure login -u <username>

并在系统提示时，输入密码。

> [AZURE.NOTE] 如果这是首次使用这些凭据登录时，您将收到一个提示，询问您确认要缓存的身份验证令牌。 如果您以前使用过，也会出现此提示`azure logout`命令如下所述。 若要跳过该提示的自动化方案，使用`-q`参数与`azure login`命令。

要注销，请使用下面的命令︰

    azure logout -u <username>

> [AZURE.NOTE] 如果与该帐户相关联的订阅仅是使用活动目录验证，登出将删除订阅信息的本地配置文件。 但是，也有预订导入发布配置文件，如果登出将仅删除 Active Directory 相关信息从本地配置文件。

### 使用发布设置文件方法

> [AZURE.NOTE] 如果使用此方法连接，您只能使用 Azure 服务管理 （或 ASM 模式） 的命令。

若要下载发布设置为您的帐户，请使用下面的命令︰

    azure account download

这将打开默认浏览器，并提示您登录到[Azure 门户网站][门户]。 之后，签名`.publishsettings`文件将被下载。 请记下该文件的保存位置。

> [AZURE.NOTE] 如果您的帐户与多个 Azure Active Directory 承租人，可能提示您选择您想要发布设置为下载文件的活动目录。
>
> 一旦选择使用下载页中，或通过访问 Azure 门户，选定的活动目录将成为门户网站和下载页使用的默认值。 一旦已经建立了默认值，您将看到文本__单击此处可返回到选择页__顶部的下载页面。 使用提供的链接返回到选择页。

接下来，导入`.publishsettings`文件通过运行以下命令︰

    azure account import <path to your .publishsettings file>

导入后您发布设置，则应删除`.publishsettings`文件，因为它不再需要的 Azure CLI 并带来安全风险，因为可以用它来访问您的订阅。

> [AZURE.NOTE] 无论您使用工作或学校的帐户登录，还是导入的发布设置，用于访问您 Azure 的订阅的信息存储在`.azure`目录位于您`user`目录。 您`user`目录受操作系统;但是，建议您采取其他步骤来加密您`user`目录。 您可以通过以下方式执行操作︰
>
> * 在 Windows 中，修改的目录属性，或使用 BitLocker。
> * 在 Mac，打开 FileVault 目录。
> * 在 Ubuntu 中，使用加密主目录功能。 其他 Linux 发行版本提供等效的功能。

### 多个订阅

如果您有多个 Azure 的订阅，则连接到 Azure 将授予访问权限与凭据关联的所有订阅。 将选择作为默认值，并执行操作时，Azure CLI 使用一个订阅。 您可以查看订阅，以及这两种程序默认情况下，使用`azure account list`命令。 此命令将返回类似于以下的信息︰

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

在上面的列表中，**当前**列指示当前 Azure-子-1 为默认订阅。 若要更改默认订阅，请使用`azure account set`命令，并指定您希望作为默认订阅。 例如︰

    azure account set Azure-sub-2

这将更改默认订阅到 Azure-子-2。

> [AZURE.NOTE] 更改默认订阅会立即生效，并且是一个全局的更改;是否运行相同的命令行实例或不同的实例，从新的 Azure CLI 命令，将使用新的默认订阅。

如果想要使用 Azure CLI，使用非默认订阅，但不想更改当前的默认值，您可以使用`--subscription`的命令选项，并提供您想要使用的操作将订阅的名称。

一旦连接到 Azure 订购，您可以开始使用 Azure CLI 命令。 有关详细信息，请参阅[如何使用 Azure CLI](xplat-cli.md)。

<a id="additional-resources"></a>
## 其他资源

* [使用 Azure 服务管理 （或多个 ASM 模式） 使用的 CLI 命令][cliasm]

* [使用资源管理 （或 ARM 模式） 使用 Azure CLI 命令][cliarm]

* 了解更多有关 Azure CLI，要下载源代码、 报告问题或参与项目，请访问[GitHub 的 Azure CLI 的存储库](https://github.com/azure/azure-xplat-cli)。

* 如果您遇到问题使用 Azure CLI 或 Azure，访问[Azure 论坛](http://social.msdn.microsoft.com/Forums/windowsazure/home)。

* 在 Azure 上的详细信息，请参阅[http://azure.microsoft.com/](http://azure.microsoft.com)。





[authandsub]: http://msdn.microsoft.com/library/windowsazure/hh531793.aspx#BKMK_AccountVCert
[免费试用版]: http://azure.microsoft.com/en-us/pricing/free-trial/
[门户网站]: https://manage.windowsazure.com
[signuporg]: http://azure.microsoft.com/en-us/documentation/articles/sign-up-organization/
[cliasm]: virtual-machines/virtual-machines-command-line-tools.md
[cliarm]: virtual-machines/xplat-cli-azure-resource-manager.md

测试
