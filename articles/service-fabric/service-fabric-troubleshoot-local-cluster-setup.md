---
ms.openlocfilehash: e5155218e02ec1623496e446ce4975df11b4f37d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="本地群集安装的疑难解答"
   description="本文介绍了一套建议对本地开发群集进行疑难解答"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/09/2015"
   ms.author="seanmck"/>

# 解决您的本地开发群集安装

如果交互与本地开发群集时遇到问题，请查看以下建议可能的解决方案。

## 群集安装失败

### 不能清理服务结构日志

#### 问题

同时运行 DevClusterSetup 脚本，您将看到如下错误︰

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### 解决方案

关闭当前 Powershell 窗口并启动一个新的 Powershell 窗口以管理员的身份。 您现在应能够成功运行该脚本。

## 群集连接失败

### 类型初始化异常

#### 问题

连接到 PowerShell 或服务结构资源管理器中的群集时，您会看到 System.Fabric.Common.AppTrace TypeInitializationException。

#### 解决方案

在安装过程中未正确设置路径变量。 请从 Windows 注销并重新登录。 这完全将刷新您的路径。

### 使用"关闭对象"的群集连接无法正常工作

#### 问题

对连接 ServiceFabricCluster 失败，错误如下︰

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### 解决方案

关闭当前 Powershell 窗口并启动一个新的 Powershell 窗口以管理员的身份。 您现在应能够成功地连接。

### FabricConnectionDeniedException

#### 问题

在 Visual Studio 中调试时，您将获得 FabricConnectionDeniedException。

#### 解决方案

尝试到尝试启动服务主机处理时手动，而不是允许服务结构运行时启动它，通常会发生此错误。

确保您没有设置为启动项目在解决方案中的任何服务项目。 只有服务结构的应用程序项目应设置为启动项目。


## 下一步行动

- [了解和解决您的群集与系统运行状况报告](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [可视化服务结构资源管理器中的群集](service-fabric-visualizing-your-cluster.md)
