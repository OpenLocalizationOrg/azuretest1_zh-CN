---
ms.openlocfilehash: 71f1676271642e3944824f490ab2d48640d0a0e0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="设置服务结构开发环境 |Microsoft Azure"
   description="安装服务结构运行时、 SDK 和工具并创建一个本地开发群集。"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="samgeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/24/2015"
   ms.author="seanmck"/>

# 设置服务结构开发环境
 本文介绍您开始构建[服务结构][1]应用程序，包括安装的运行时，SDK，工具，和设置本地群集所需要的一切。

 > [AZURE.NOTE] 以下说明适用于设置新买的计算机。 如果您有在您的 PC 上安装服务结构的早期版本，请按照[说明更新您的开发环境](service-fabric-update-your-development-environment.md)。

## 先决条件
### 受支持的操作系统版本
支持下列操作系统版本︰

- Windows 8/8.1
- Windows Server 2012 R2
- Windows 10

### Visual Studio 2015 年

服务结构的工具取决于 Visual Studio 2015，可以找到[这里][2]。

> [AZURE.NOTE] 如果您不运行受支持的操作系统版本之一或不愿意在您的 PC 上安装 Visual Studio 2015年，您可以设置 Windows Server 2012 R2 和预安装 VM 库中使用图像的 Visual Studio 2015年 Azure 的虚拟机。

## 安装的运行时、 SDK 和工具

安装服务结构组件是通过 Web 平台安装程序。 请按照这些说明来安装︰

1. [下载 SDK][3]使用 Web 平台安装程序。

2. 单击**安装**以开始安装过程。

3. 查看并接受最终用户许可协议。

安装将自动继续。

## 启用 PowerShell 脚本执行

服务结构将 Windows PowerShell 脚本用于创建本地开发群集和部署应用程序从 Visual Studio。 默认情况下，则 Windows 将阻止这些脚本的运行。 若要启用它们，则必须修改您 PowerShell 的执行策略。 打开作为管理员 PowerShell 并输入以下命令︰

    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser


## 安装并启动本地群集
本地群集表示多计算机拓扑结构，您最终将使用单一的开发机器上的生产中。 若要设置本地群集，请执行以下步骤︰


1. 关闭所有其他 PowerShell 窗口，并开始一个新管理员身份。

2. 导航到与群集安装目录 `cd "$env:ProgramW6432\Microsoft SDKs\Service Fabric\ClusterSetup"`

3. Run `.\DevClusterSetup.ps1`

在一段时间，您应看到演示和确认已成功创建群集节点信息的输出。 在某些情况下，您可能会看到警告服务结构主机服务和命名服务启动时。 这是正常的会马上跟群集有关的一些基本信息。

> [AZURE.NOTE] 本地群集使用完全相同的运行时作为什么在 Azure 中运行。 它不是模拟或模拟以任何方式。 唯一的区别是，所有节点运行在一台计算机，而不是因为他们将 Azure 中跨多台计算机分发。

## 验证群集安装

您可以检查您的群集已创建成功使用 SDK 附带服务结构资源管理器工具。

1. 首先，服务结构资源管理器运行`. "$env:ProgramW6432\Microsoft SDKs\Service Fabric\Tools\ServiceFabricExplorer\ServiceFabricExplorer.exe"`。

2. 展开 Onebox/本地群集节点中的左上角。

3. 确保应用程序和节点视图是绿色的。

如果不是绿色的任何元素或您看到错误，请稍等，然后单击刷新按钮。 如果仍有问题，签出[设置的故障排除步骤](service-fabric-troubleshoot-local-cluster-setup.md)。

## 下一步行动
现在，您的开发环境的设置，您可以开始构建和运行应用程序。

- [了解有关编程模型︰ 可靠参与者和可靠的服务](service-fabric-choose-framework.md)
- [开始使用可靠的服务 API](service-fabric-reliable-services-quick-start.md)
- [开始使用可靠的演员 API](service-fabric-reliable-actors-get-started.md)
- [签出在 GitHub 服务结构示例](https://github.com/azure/servicefabric-samples)
- [显示您使用服务结构资源管理器中的群集](service-fabric-visualizing-your-cluster.md)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "服务结构市场活动页"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "与 RC"
[3]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric "WebPI 链接"
