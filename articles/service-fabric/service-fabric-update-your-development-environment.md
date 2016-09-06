---
ms.openlocfilehash: 89febecfe45016a705b655737ab37437c07c1c7a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="更新服务结构开发环境 |Microsoft Azure"
   description="更新服务结构开发环境使用运行时、 SDK 和工具。"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="samgeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2015"
   ms.author="seanmck"/>

# 更新服务结构开发环境

 服务结构定期提供最新版本的运行库，SDK，并用于在本地开发工具。 保持更新这些版本与您本地开发环境，以确保始终能够访问最新功能、 缺陷修补程序，以及构建和测试您的应用程序本地时提高性能。

## 清理您的本地群集

 服务结构目前不支持升级服务结构运行时，本地群集运行时。 为了保证全新升级，务必要先清理本地群集。

 > [AZURE.NOTE] 清理本地群集将删除所有已部署的应用程序和它们的数据。

 您可以按如下所述清洁您的本地群集︰


 1. 关闭所有其他 PowerShell 窗口，并开始一个新管理员身份。

 2. 导航到与群集安装目录 `cd "$env:ProgramW6432\Microsoft SDKs\Service Fabric\ClusterSetup"`

 3. Run `.\CleanCluster.ps1`


## 更新 SDK，运行时和工具

 一旦成功清理现有群集，则可以进行升级，如下所示︰


 1. 启动 Web 平台安装程序[更新到最新版本][1]。

 2. 完成后，以管理员身份启动新 PowerShell 窗口，然后定位到群集安装目录与`cd "$ENV:ProgramFiles\Microsoft SDKs\ServiceFabric\ClusterSetup"`。

 3. 运行`.\DevClusterSetup.ps1`以设置本地群集。

就这么简单 ！ 您现在可以启动 Visual Studio 并继续构建服务结构的应用程序。

>[AZURE.NOTE] 在此版本中，默认项目结构已更改。 您应该能够打开并运行 Visual Studio 中的现有项目。 但是，如果您遇到任何与构建、 部署或调试应用程序的问题，考虑创建一个新项目并通过迁移您的代码。

 [1]:  http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric "WebPI 链接"
