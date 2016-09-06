---
ms.openlocfilehash: 56354a556c5638f96f99e95f3c9622b8f4036a58
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 为资源管理器模板设置 PowerShell
 
可以使用资源管理器中使用 Azure PowerShell 之前，需要有正确的 Windows PowerShell 和 Azure PowerShell 版本。

### 验证 PowerShell 版本

请验证您具有 Windows PowerShell 3.0 或 4.0 版本。 若要查找 Windows PowerShell 的版本，请在 Windows PowerShell 命令提示符下键入该命令。

    $PSVersionTable

您将收到下面的信息类型︰

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


检查**PSVersion**的值是 3.0 或 4.0。 如果没有，请参阅[Windows 管理框架 3.0](http://www.microsoft.com/download/details.aspx?id=34595)或[Windows 管理框架 4.0](http://www.microsoft.com/download/details.aspx?id=40855)。

您还必须具有 Azure PowerShell 0.9.0 版或更高版本。 如果您不安装并配置 Azure PowerShell，请单击[此处](powershell-install-configure.md)的说明。

您可以检查已安装使用此命令在 Azure PowerShell 命令提示符的 Azure PowerShell 的版本。

    Get-Module azure | format-table version

您将收到下面的信息类型︰

    Version
    -------
    0.9.0

如果您没有 0.9.0 或更高版本，必须删除使用的程序和功能的控制面板的 Azure PowerShell，然后安装最新版本。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md) 。

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

### 切换到 Azure 资源管理器模块

若要使用 Azure 资源管理器模块需要从 Azure 命令的默认设置切换到 Azure 资源管理器的命令集。 运行此命令。

    Switch-AzureMode AzureResourceManager

> [AZURE.NOTE] 您可以切换到**交换机 AzureMode AzureServiceManagement**命令的命令的默认设置。

