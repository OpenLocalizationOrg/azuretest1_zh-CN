---
ms.openlocfilehash: 4751c55a009ef0b20e325d1175d3df08c28e329b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="计算机直接连接到操作的见解 "
    description="您需要在主板上的每台计算机上安装代理运营的见解，可以将计算机连接直接到运营的见解。"
    services="operational-insights"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operational-insights"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/02/2015" 
    ms.author="banders"/>
# 计算机直接连接到操作的见解

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

您可以计算机直接连接到操作的见解通过您需要在主板上的每台计算机上安装代理运营的见解。

> [AZURE.TIP] 在 Azure 上运行的虚拟机，用于[分析从 Microsoft Azure 中的服务器的数据](operational-insights-analyze-data-azure.md)中的步骤安装代理

## 下载并安装代理
使用下列步骤下载并安装操作的见解代理。

### 下载代理安装程序文件
1. 在运营洞察力门户中，在**概述**页中，单击**设置**拼贴。  单击顶部**连接源**选项卡。
![设置页面](./media/operational-insights-direct-agent/direct-agent01.png)
2. 下**附加服务器直接 （64 位）**，请单击下载代理按钮以下载安装程序文件。
3. 在**工作区 ID**的右侧，单击复制图标并粘贴到记事本中的 ID。
4. **为主键**的右侧，单击复制图标，将 ID 粘贴到记事本。
![设置页面](./media/operational-insights-direct-agent/direct-agent02.png)

### 若要安装代理使用安装程序
1. 运行安装程序，您想要管理的计算机上安装代理。
2. 选择**连接到 Microsoft Azure 运营洞察力的代理**，然后单击**下一步**。
3. 出现提示时，输入**区 ID**和您在前面的过程中复制到记事本的**主键**。

4. 单击**下一步**。  代理验证它可以连接到操作的见解。
5. 完成后， **Microsoft 管理代理**出现在**控制面板**中。

### 若要安装代理，使用命令行
- 修改，然后使用下面的示例安装代理，使用命令行。
```MMASetup-AMD64.exe /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"```

## 配置 Microsoft 监控代理 （可选）
使用以下信息来启用代理直接与 Microsoft Azure 运营洞察力服务进行通信。 您已经配置了代理后，它将注册代理服务，并将获得必要的配置信息和管理包中包含解决方案信息。

从监控代理的计算机中收集数据后，监视的计算机的数量将显示在**连接源**选项卡中**设置****附加服务器直接 （64 位）**操作的见解门户中。 发送的数据的任何计算机，您可以操作见解门户中查看其数据和评估信息。

如果需要您还可以禁用代理，也可以使用命令行或脚本。

### 若要配置代理
1. 您已经安装了**Microsoft 监视代理**后，打开**控制面板**。
2. 打开 Microsoft 监控代理，然后单击 Azure 运营洞察力选项卡。
3. 选择**连接到 Azure 的运营经验**。
4. 在**工作区 ID**框中，粘贴操作见解门户区 ID。
5. 在**帐户密码**框中，粘贴操作见解门户中的**主键**，然后单击**确定**。

### 若要禁用代理
1. 后安装代理，请打开**控制面板**。
2. 打开 Microsoft 监控代理，然后单击**Azure 运营洞察力**选项卡。
3. 清除**连接到 Azure 的运营经验**。

### 若要启用代理，使用命令行或脚本
- 可以使用下面的示例使用 Windows PowerShell 或 VB 脚本。

```powershell
$healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$healthServiceSettings.EnableOnlineMonitoring('workspacename', 'workspacekey')
$healthServiceSettings.ReloadConfiguration()
```

## 配置代理服务器和防火墙设置 （可选）
如果您的代理服务器或防火墙环境中的限制对 Internet 的访问，您可能需要按照以下步骤启用运营经理或代理进行通信运营经验服务。

- [配置代理服务器和防火墙设置 （可选）](operational-insights-proxy-firewall.md)
