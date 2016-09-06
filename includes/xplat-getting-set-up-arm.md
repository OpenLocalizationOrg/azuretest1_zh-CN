---
ms.openlocfilehash: 8fe10c156b845274ac6c39fa1fa4b0633ea045a6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## 使用 Azure CLI 使用 Azure 资源管理器 (ARM)

您可以使用资源管理器命令和模板 Azure CLI 部署 Azure 的资源和工作负荷使用资源组之前，您需要使用 Azure 帐户 （当然）。 如果您没有帐户，您可以获得[免费 Azure 的试用](http://azure.microsoft.com/pricing/free-trial/)。

> [AZURE.NOTE] 如果您尚没有一个 Azure 帐户，但必须对 MSDN 订阅的订阅，您可以获得免费 Azure 片尾通过激活您的[MSDN 订阅者权益](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)，或您可以使用免费的帐户。 任何一个适用于 Azure 的访问。

### 步骤 1︰ 验证 Azure CLI 版本

若要使用 Azure CLI 的强制性命令和 ARM 的模板，您需要至少具有版本 0.8.17。 若要验证您的版本，请键入`azure --version`。 您应该看到类似于︰

    $ azure --version
    0.8.17 (node: 0.10.25)

如果您需要更新您的 Azure CLI 版本，请参阅[Azure CLI](https://github.com/Azure/azure-xplat-cli)。

### 第 2 步︰ 验证使用某一工作或学校与 Azure 的标识

如果您使用的[Azure Active Directory 租户](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)或[服务主体名称](https://msdn.microsoft.com/library/azure/dn132633.aspx)只能使用 ARM 命令模式。 （这也称为*组织 id*。

要查看是否有一个，登录通过键入`azure login`并使用您的工作或学校的用户名和密码提示时。 如果您确实有一个，您应看到类似以下内容︰

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

如果未看到此选项，必须创建与您的 Microsoft 帐户身份的新租户 （或主体服务）。 （这通常是与个人的 MSDN 订阅或免费试用订阅情况。）若要创建某一工作或学校从 Azure 帐户创建 Microsoft id 的 id，请参阅[将具有新的 Azure 订阅的 Azure 广告目录相关联](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)。 如果您认为应该已经具有一个组织 id，您可能需要与为您创建帐户的人交谈。

### 步骤 3︰ 选择 Azure 订阅

如果可以在 Azure 帐户中只有一个预订，Azure CLI 本身将与关联该订阅默认情况。 如果您有多个订阅，您需要选择您想要使用通过键入`azure account set <subscription id or name> true`其中_订阅 id 或名称_是订阅 id 或您想要在当前会话中使用的订阅名称。

您应该看到类似以下内容︰

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### 步骤 4︰ 将 Azure CLI 放在 ARM 模式

若要使用 Azure CLI 使用 Azure 资源管理 (ARM) 模式下，键入`azure config mode arm`。 您应该看到类似以下内容︰

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] 您可以切换回使用 Azure 服务管理命令通过键入`azure config mode asm`。
