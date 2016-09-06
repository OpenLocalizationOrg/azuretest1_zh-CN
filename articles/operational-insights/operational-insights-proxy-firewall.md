---
ms.openlocfilehash: b16c55b1a3a194d404683b95a01576b128f075e3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="配置代理服务器和防火墙设置操作的见解"
   description="了解您需要配置为使用运营洞察力的代理程序的类型的代理服务器和防火墙设置"
   services="operational-insights"
   documentationCenter=""
   authors="bandersmsft"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2015"
   ms.author="banders" />

# 配置代理服务器和防火墙设置操作的见解

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

当您使用与 Microsoft 监视的代理程序将直接连接到服务器的操作管理器和它的代理，需要配置代理服务器和防火墙设置操作的见解的操作会有所不同。 查看以下各节为您使用的代理程序的类型。

## 使用 Microsoft 监视 Agent 配置代理服务器和防火墙设置

对于 Microsoft 监控代理连接并注册运营经验服务，它必须具有访问您的域和 Url 的端口号。 如果您使用代理服务器代理和运营经验服务之间的通信，您需要确保适当的资源进行访问。 如果您使用防火墙来限制对 Internet 的访问，您需要配置您的防火墙以允许访问操作的见解。 下表列出了运营的见解需要的端口。

|**代理资源**|**端口**|
|--------------|-----|
|*。 ods.opinsights.azure.com|端口 443|
|*。 oms.opinsights.azure.com|端口 443|
|ods.systemcenteradvisor.com|端口 443|
|*.blob.core.windows.net/*|端口 443|

可以使用以下过程配置 Microsoft 监控代理使用控制面板中的代理服务器设置。 您将需要使用为每台服务器的过程。 如果您有多台服务器，您需要配置，查找容易使用一个脚本来自动执行此过程。 如果是这样，请参阅下一个过程*来配置代理服务器设置，Microsoft 监控代理使用脚本*。

### 要配置代理设置，Microsoft 监控代理使用控制面板

1. 打开**控制面板**。

2. 打开**Microsoft 监控代理**。

3. 单击**代理设置**选项卡。  
  ![代理设置选项卡](./media/operational-insights-proxy-firewall/direct-agent-proxy.png)

4. 选中**使用代理服务器**，并键入的 URL 和端口号，如果有必要，类似于所示的示例。 如果您的代理服务器要求身份验证，请键入用户名和密码以访问代理服务器。

请按下列步骤创建 PowerShell 脚本，您可以运行设置直接连接到服务器的每个代理的代理服务器设置。

### 要配置代理设置，Microsoft 监控代理使用脚本


- 复制下面的示例更新其中的特定于您环境的信息、 PS1 文件扩展名，并将其保存，然后直接连接到操作的见解服务每台计算机上运行该脚本。


```
param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

# First we get the health service configuration object.  We need to determine if we
# have the right update rollup with the API we need.  If not, no need to run the rest of the script.
$healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

$proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

if (!$proxyMethod)
{
    Write-Output 'Health Service proxy API not present, will not update settings.'
    return
}


Write-Output "Clearing proxy settings."
$healthServiceSettings.SetProxyInfo('', '', '')

$ProxyUserName = $cred.username;


Write-Output "Setting Proxy to ${ProxyDomainName} with proxy username of (${ProxyUserName})."
$healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
```

## 配置代理服务器和防火墙设置与操作管理器

对于连接并注册运营的见解服务运营经理管理组，必须具有访问到您的域和 Url 的端口号。 如果运营经理管理服务器和运营经验服务之间的通信中使用代理服务器，您需要确保适当的资源都可访问。 如果您使用防火墙来限制对 Internet 的访问，您需要配置您的防火墙以允许访问操作的见解。 即使是运营经理管理服务器不是代理服务器的后面，可能它的代理。 代理服务器是在这种情况下，应能够配置相同的方式，如代理都以启用并允许被发送给操作见解 web 服务安全性和日志管理解决方案数据。

为了使操作管理器代理与运营经验服务进行通信，您的操作管理器基础结构 （包括工程师） 应有正确的代理服务器设置和版本。 操作管理器控制台中指定代理的代理设置。 您的版本应该是一种下列情况︰

- 操作管理器 2012 SP1 更新汇总 7 或更高版本
- 操作管理器 2012 R2 更新汇总 3 或更高版本


下表列出了与这些任务相关的端口。

>[AZURE.NOTE] 下列资源中的一部分提及的顾问。 但是，将在以后更改列出的资源。

|**代理资源**|**端口**|
|--------------|-----|
|*。 ods.opinsights.azure.com|端口 443|
|*。 oms.opinsights.azure.com|端口 443|
|ods.systemcenteradvisor.com|端口 443|
|*.blob.core.windows.net/*|端口 443|

|**管理服务器资源**|**端口**|
|--------------|-----|
|*。 ods.opinsights.azure.com|端口 443|
|service.systemcenteradvisor.com|端口 443|
|scadvisor.accesscontrol.windows.net|端口 443|
|scadvisorservice.accesscontrol.windows.net|端口 443|
|*.blob.core.windows.net/*|端口 443|
|data.systemcenteradvisor.com|端口 443|
|ods.systemcenteradvisor.com|端口 443|
|*。 systemcenteradvisor.com|端口 443|


|**运营的见解和操作管理器控制台资源**|**端口**|
|---|---|
|*。 systemcenteradvisor.com|端口 80 和 443|
|*。 live.com|端口 80 和 443|
|*。 microsoftonline.com|端口 80 和 443|
|login.windows.net|端口 80 和 443|


使用以下过程注册运营的见解服务运营经理管理组。 如果您有管理组和运营经验服务之间的通信问题，使用验证过程来解决数据传输到运营经验服务。

### 若要请求例外的运营洞察力服务终结点

1. 使用您可能必须从第一个表显示以前以确保运营经理管理服务器所需的资源可以通过防火墙访问信息。

2. 使用以前提供，以确保在运营经理和运营经验操作控制台所需的资源可以通过防火墙访问第二个表中可能有的信息。

3. 如果使用 Internet Explorer 中使用代理服务器，请确保它已配置并且能够正常运行。 若要验证，您可以打开 web 安全连接 (https)，例如[https://bing.com](https://bing.com)。 如果在浏览器中，web 安全连接不起作用，它可能不工作在操作管理器管理控制台在云环境中的 web 服务。

### 若要在操作管理器控制台中配置代理服务器

1. 打开操作管理器控制台，选择**管理**工作区。

2. 展开**运营的见解**，然后选择**连接操作的见解**。
![操作管理器操作见解连接](./media/operational-insights-proxy-firewall/proxy-om01.png)

3. 在运营的见解连接视图中，单击**配置代理服务器**。
![操作管理器操作见解连接配置代理服务器](./media/operational-insights-proxy-firewall/proxy-om02.png)

4. 在运营的见解设置向导︰ 代理服务器，选择**使用代理服务器来访问操作的见解 Web 服务**，然后键入的 URL 和端口号，例如， **http://myproxy:80**。
![操作管理器操作见解代理服务器地址](./media/operational-insights-proxy-firewall/proxy-om03.png)


### 如果代理服务器要求身份验证，指定的凭据
 代理服务器凭据并设置需要传播到会报告给运营经验的被管理的计算机。 这些服务器应为*Microsoft 的系统已由中心顾问监视的服务器组*。 在组中每个服务器的注册表中加密凭据。

1. 打开操作管理器控制台，选择**管理**工作区。

2. 在**RunAs 配置**中，选择**配置文件**。

3. 打开**系统中心顾问运行作为配置文件代理**配置文件。
![系统中心顾问运行为代理配置文件的图像](./media/operational-insights-proxy-firewall/proxyacct1.png)
4. 在运行以配置文件向导中，单击**添加**要使用运行方式帐户。 您可以创建一个新的运行方式帐户或使用现有的帐户。 此帐户必须具有足够的权限，若要通过代理服务器。
![运行以配置文件向导的图像](./media/operational-insights-proxy-firewall/proxyacct2.png)

5. 若要设置用于管理的帐户，选择**选定的类别、 组或对象**打开对象搜索框。
![运行以配置文件向导的图像](./media/operational-insights-proxy-firewall/proxyacct2-1.png)
6. 搜索，然后选择**Microsoft 的系统已由中心顾问监视的服务器组**。
![对象搜索框中的图像](./media/operational-insights-proxy-firewall/proxyacct3.png)
7. 单击**确定**以关闭运行方式帐户中添加![映像运行作为配置文件向导](./media/operational-insights-proxy-firewall/proxyacct4.png)
8. 完成此向导并保存所做的更改。
![运行以配置文件向导的图像](./media/operational-insights-proxy-firewall/proxyacct5.png)


### 若要验证这种见解运营管理下载包

- 如果通过使用操作见解添加解决方案时，您可以为**管理**下的管理包操作管理器控制台中查看它们。 搜索*系统中心顾问*来快速地找到它们。
![下载管理包](./media/operational-insights-proxy-firewall/mpdownloaded.png)
- 或者，您还可以检查运营见解管理包在运营经理管理服务器中使用以下 Windows PowerShell 命令︰

        get-scommanagementpack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken

        get-scommanagementpack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version | ft

### 若要验证该运营经理向服务发送数据操作的见解

1. 在运营经理管理服务器，打开性能监视器 (perfmon.exe)，然后选择**性能监视器**。

2. 单击**添加**，然后选择**健康服务管理组**。

3. 添加以**HTTP**开头的所有计数器。
![添加计数器](./media/operational-insights-proxy-firewall/sendingdata1.png)
4. 如果操作管理器配置是好的您将看到的事件和其他数据项目的健康服务管理计数器的活动，根据运营的洞察力和配置的日志收集策略中添加的管理包。
![性能监视器显示活动](./media/operational-insights-proxy-firewall/sendingdata2.png)
