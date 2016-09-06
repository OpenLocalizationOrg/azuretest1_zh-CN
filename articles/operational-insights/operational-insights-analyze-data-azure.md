---
ms.openlocfilehash: e7719102471f675d50771bb5cf9cc271da8cb1f1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="分析中 Microsoft Azure 服务器中的数据"
   description="通过启用 Azure 诊断搜索事件和云服务和虚拟机的 IIS 日志。"
   services="operational-insights"
   documentationCenter=""
   authors="bandersmsft"
   manager="jwhit"
   editor=""/>

<tags
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/05/2015"
   ms.author="banders"/>
# 分析中 Microsoft Azure 服务器中的数据

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

运营的见解使用的数据的服务器在内部或云基础结构。 您可以从 Azure 存储时生成的 Azure 的诊断收集计算机数据。

使用 Azure 存储中所收集的数据，您可以快速搜索事件和云服务和虚拟机的 IIS 日志。 通过安装 Microsoft 监控代理，还可以从您的虚拟机获取其他见解。

所有的更新评估、 更改跟踪、 和 SQL 评估解决方案使用 Microsoft 监控代理在虚拟机上提供更深入的洞察力。 如果尚未启动，则可以[添加解决方案](operational-insights-setup-workspace.md)时登录到[门户操作的见解](https://www.microsoft.com/oms/)。

Azure 的虚拟机中，有两个简单方法启用基于代理的数据集︰

- 在 Microsoft Azure 管理门户

- 使用 PowerShell

在使用基于代理的集合日志数据时，必须配置要收集日志管理[运营的见解门户](https://www.microsoft.com/oms/)配置页中的日志。

 >[AZURE.NOTE] 如果您已配置索引操作见解记录数据使用 Azure 的诊断程序并配置代理收集日志，然后将两次索引相同的日志。
将收取这两个数据源的普通数据的率。
如果您有安装代理，您应该收集使用代理和不索引的 Azure 的诊断收集日志的日志数据。

## Microsoft Azure 管理门户

您可以从[Azure 门户](https://manage.windowsazure.com/#Workspaces/OperationalInsightExtension/Workspaces)安装的代理运营的见解。

### 若要安装的代理运营的见解
1. 在 Azure 的门户中，请转到运营洞察力区并选择**服务器**选项卡。
2. 在选项卡，您将看到您的虚拟机的列表。 选择您想要上安装代理，然后单击**启用 Op 见解**的虚拟机。

自动安装和配置为运营见解工作区代理。

![运行的见解服务器页面的图像](./media/operational-insights-analyze-data-azure/servers.png)

 >[AZURE.NOTE] [Azure VM 代理](https://msdn.microsoft.com/library/azure/dn832621.aspx)必须安装自动安装的代理运营的见解。



## PowerShell

如果您希望脚本到 Azure 的虚拟机中进行更改，您可以启用 Microsoft 监控代理使用 PowerShell。

Microsoft 监控代理是[Azure 的虚拟机的扩展](https://msdn.microsoft.com/library/azure/dn832621.aspx)，您可以管理它使用 PowerShell，如下面的示例中所示。

```powershell
Add-AzureAccount

$workspaceId="enter workspace here"
$workspaceKey="enter workspace key here"
$hostedService="enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService
Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId':  '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

在配置时使用 PowerShell，您需要提供的工作区 ID 和主键。 操作的见解门户**设置**页上，您可以找到您的工作区 ID 和主键。

![来源](./media/operational-insights-analyze-data-azure/sources01.png)

## 使用 Azure 的诊断收集数据

操作的见解可以分析 Azure 诊断程序写入到 Azure 存储的数据。 有两个主要步骤执行︰

- 配置诊断数据到 Azure 存储的集合
- 配置操作的见解来分析存储帐户中的数据

下面的主题介绍如何启用诊断到 Azure 存储的数据的集合，然后如何配置操作的理解，以便分析在 Azure 存储中的数据。

Azure 的诊断是 Azure 的扩展，使您可以从辅助角色、 web 角色或在 Azure 上运行的虚拟机中收集诊断数据。 数据存储在 Azure 存储帐户，然后由操作的见解。

>[AZURE.NOTE] 您将需要支付正常的数据传输率为存储和诊断发送到存储帐户的交易记录和运营见解时从您的存储帐户读取数据。

Azure 的诊断可以收集遥测的以下类型︰

数据源|说明
 ---|---
IIS 日志|有关 IIS web 站点的信息。
Azure 诊断基础结构日志|诊断程序本身有关的信息。
失败请求的 IIS 日志 |有关 IIS 网站或应用程序失败的请求的信息。
Windows 事件日志|发送到 Windows 事件日志记录系统的信息。
性能计数器|操作系统和自定义性能计数器。
故障转储|有关在应用程序崩溃时的进程的状态信息。
自定义错误日志|由您的应用程序或服务创建的日志。
NET EventSource|通过使用[EventSource 类](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx).NET 代码生成的事件
基于 ETW 清单|ETW 事件生成的任何进程
系统日志|发送到系统日志或 Rsyslog 守护程序事件


目前，运营的见解是能够分析︰

- IIS 日志从 Web 角色和虚拟机
- 从 Web 角色、 辅助角色和 Azure 的虚拟机运行 Windows 操作系统的 Windows 事件日志
- 从 Azure 运行 Linux 操作系统的虚拟机的系统日志

日志必须位于以下位置︰

- WADWindowsEventLogsTable （表存储） – Windows 事件日志中包含信息。
- wad iis 日志 （Blob 存储）-包含关于 IIS 的信息记录。
- LinuxsyslogVer2v0 （表存储）-包含 Linux 系统日志事件。

 >[AZURE.NOTE] 目前不支持从 Azure 网站的 IIS 日志。

为虚拟机，也有您的虚拟机上安装[Microsoft 监视代理](http://go.microsoft.com/fwlink/?LinkId=517269)的选项以启用其他见解。 除了能够分析 IIS 日志和事件日志还将允许能够执行其他分析包括配置更改跟踪、 SQL 评估和更新评估。

您可以帮助我们优先其他日志中的操作的见解，通过我们的[反馈页面](http://feedback.azure.com/forums/267889-azure-operational-insights/category/88086-log-management-and-log-collection-policy)投票来分析。

## 启用 IIS 日志和事件集合的 Web 角色在 Azure 诊断

请参阅[如何启用诊断在云服务中](https://msdn.microsoft.com/library/azure/dn482131.aspx)。 您将从这里使用的基本信息和自定义用于 Microsoft Azure 运营洞察力的步骤。

使用启用了 Azure 诊断程序︰

- IIS 日志在默认情况下，scheduledTransferPeriod 传输时间间隔传输的日志数据存储。

- 默认情况下不传输 Windows 事件日志。

### 若要启用诊断

若要启用 Windows 事件日志，或更改 scheduledTransferPeriod，配置 Azure 诊断使用 XML 配置文件 (diagnostics.wadcfg)，如中所示[第 2 步︰ 将 diagnostics.wadcfg 文件添加到 Visual Studio 解决方案](https://msdn.microsoft.com/library/azure/dn482131.aspx#BKMK_step2)和[第 3 步︰ 配置您的应用程序的诊断](https://msdn.microsoft.com/library/azure/dn482131.aspx#BKMK_step3)如何到启用诊断过程中云服务主题中。下面的示例配置文件会从应用程序和系统日志收集 IIS 日志以及所有事件︰

    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>


在[步骤 4︰ 配置您的诊断数据的存储区](https://msdn.microsoft.com/library/azure/dn482131.aspx#BKMK_step4)如何到启用诊断程序中的云服务主题，确保您 ConfigurationSettings 指定存储帐户，如以下示例所示︰


    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>


**帐户名**和**AccountKey**值的 Microsoft Azure 管理门户中存储帐户仪表板管理访问键下找到。 为连接字符串的协议必须是**https**。

更新诊断配置应用到您的云服务并到 Azure 存储写入诊断程序后，您已准备配置操作的见解。

## 启用虚拟机中的 Azure 诊断事件日志和 IIS 日志集合的

使用以下过程启用虚拟机的使用 Microsoft Azure 管理门户的事件日志和 IIS 日志集合在 Azure 诊断。

### 若要启用虚拟机使用 Azure 管理门户中的 Azure 诊断

1. 创建虚拟机时，请安装虚拟机代理。 如果虚拟机已经存在，请验证已安装了 VM 代理。
    - 如果您使用默认 Azure 管理门户创建虚拟机，执行**自定义创建**并选择**安装虚拟机代理**。
    - 如果您使用新的 Azure 管理门户创建虚拟机，选择**可选配置**，则**诊断**并将**状态**设置为**上**。

    完成时，虚拟机将自动拥有 Azure 诊断扩展安装并运行它将负责收集诊断数据。

2. 启用监视和配置事件日志记录在现有的虚拟机上。 您可以启用诊断程序在虚拟机级别。 要启用诊断程序并配置事件日志记录，请执行以下步骤︰
    1. 选择虚拟机。
    2. 单击**监视**。
    3. 单击**诊断**。
    4. 将**状态**设置为**ON**。
    5. 选择您要使用每个诊断指标。 Windows 事件系统日志、 Windows 应用程序日志事件和 IIS 日志，可以分析操作的见解。
    7. 单击**确定**。

使用 Azure PowerShell 可以更精确地指定写入到 Azure 存储的事件。 请参阅 Azure 诊断 1.2 配置架构的配置文件示例和详细的资料，对其架构。 确保您安装和配置 Azure PowerShell 版本 0.8.7 或更高版本中[如何安装和配置 Azure PowerShell](powershell-install-configure)。 如果您有安装 Microsoft Azure 诊断的版本早于版本 1.2 新门户不能用于启用或配置诊断。

您可以启用和使用以下 PowerShell 脚本代理更新。 您还可以使用自定义日志记录配置使用此脚本。 您将需要修改脚本以设置存储帐户、 服务名称和虚拟机的名称。

### 若要启用虚拟机使用 Azure PowerShell 中的诊断

检查下面的脚本示例、 将其复制、 根据需要进行修改、 将此示例保存为 PowerShell 脚本文件，然后运行该脚本。


    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM


## 启用通过运营洞察力的 Azure 存储分析

使用以下过程来启用存储分析和配置操作的理解，以便从 Azure 存储帐户使用 Azure 诊断程序读取。

### 若要启用操作的见解的分析

1. 在默认 Azure 的门户网站中，导航到您的运营洞察力工作区，选择**存储**选项卡。
![工作区存储选项卡](./media/operational-insights-analyze-data-azure/workspace-storage-tab.png)
2. 单击打开**添加存储帐户**框中**添加存储帐户**。
3. 选择您要使用的存储帐户。
4. 在**数据类型**列表中，选择数据类型︰**事件**、 **IIS 日志**或**系统日志 (Linux)**。
5. 单击**确定**。<br>
![存储帐户中](./media/operational-insights-analyze-data-azure/storage-account.png)
6. 为您想要收集每个数据类型和存储帐户组合重复上述步骤。

在大约 1 个小时内就可以开始查看存储帐户可供操作的见解中的分析数据。

## 相关的内容

- [计算机直接连接到操作的见解](operational-insights-direct-agent)
- [博客文章︰ 启用运营洞察力的 Azure 虚拟机](http://azure.microsoft.com/updates/easily-enable-operational-insights-for-azure-virtual-machines/)

## 下一步行动

[配置代理服务器和防火墙设置 （可选）](../operational-insights-proxy-filewall.md)
