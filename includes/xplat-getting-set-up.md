---
ms.openlocfilehash: b1c4249bb267b2866e10c494b7bcd7c80e6149f0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## 使用 Azure CLI

以下步骤将帮助您轻松地使用 Azure CLI 的最新版本和适当的订阅。 如果您需要安装 Azure CLI 并首次将其连接到您的帐户，请参阅[Azure 命令行界面 (Azure CLI)](xplat-cli.md)。

### 步骤 1︰ 更新 Azure CLI 版本

若要使用 Azure CLI 的强制性命令的服务管理模式，应尽可能具有最新版本。 若要验证您的版本，请键入`azure --version`。 您应该看到类似于︰

    $ azure --version
    0.8.17 (node: 0.10.25)

如果您想要更新您的 Azure CLI 版本，请参阅[Azure CLI](https://github.com/Azure/azure-xplat-cli)。

### 步骤 2︰ 设置 Azure 帐户和订阅

您想要使用的帐户具有连接 Azure CLI 后, 可能有多个订阅。 如果那样的话，您应该查看可用的订阅为您的帐户通过键入`azure account list`，然后选择想要使用通过键入的订阅`azure account set <subscription id or name> true`其中_订阅 id 或名称_是订阅 id 或您想要在当前会话中使用的订阅名称。 您应该看到类似以下内容︰

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] 如果您尚没有一个 Azure 帐户，但必须对 MSDN 订阅的订阅，您可以获得免费 Azure 片尾通过激活您的[MSDN 订阅者权益](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)，或您可以使用免费的帐户。 任何一个适用于 Azure 的访问。
