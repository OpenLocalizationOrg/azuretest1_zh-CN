---
ms.openlocfilehash: 9434c8c15c37200990aa3ce77ead4508801c2636
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="josephd" />

## 设置 PowerShell

您可以使用 Azure PowerShell 之前，请按照下列步骤。

### 验证 PowerShell 版本

您可以使用 Windows PowerShell 之前，必须具有 Windows PowerShell 3.0 版、 4.0。 若要查找 Windows PowerShell 的版本，请在 Windows PowerShell 命令提示符下键入该命令。

    $PSVersionTable

您应该看到如下所示。

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

检查**PSVersion**的值是 3.0 或 4.0。 要安装的兼容版本，请参阅[Windows 管理框架 3.0](http://www.microsoft.com/download/details.aspx?id=34595)或[Windows 管理框架 4.0](http://www.microsoft.com/download/details.aspx?id=40855)。

还应有 Azure PowerShell 版本 0.8.0 或更高版本。 您可以检查已安装使用此命令在 Azure PowerShell 命令提示符的 Azure PowerShell 的版本。

    Get-Module azure | format-table version

您应该看到如下所示。

    Version
    -------
    0.8.16.1

有关说明和指向最新版本的链接，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)。


### 设置 Azure 帐户和订阅

如果您没有订阅了 Azure，可以激活您的[MSDN 订户权益](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或符号组成的[免费试用版](http://azure.microsoft.com/pricing/free-trial/)。

使用此命令打开 Azure PowerShell 命令提示符并登录到 Azure。

    Add-AzureAccount

如果您有多个 Azure 的订阅，您可以列出 Azure 订阅使用此命令。

    Get-AzureSubscription

您将收到下面的信息类型︰

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

可以通过在 Azure PowerShell 命令提示符下运行以下命令来设置当前 Azure 订阅。 引号，包括的所有内容替换 < 和 > 字符，用正确的名称。

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

关于 Azure 预订和帐户的详细信息，请参阅[如何︰ 连接到您的订阅](powershell-install-configure.md#Connect)。
