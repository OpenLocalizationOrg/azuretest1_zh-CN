---
ms.openlocfilehash: c684bcf4f66389cb1c28ae4e93e6de6f67e0922c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="调试发布的云服务使用 IntelliTrace 和 Visual Studio |Microsoft Azure"
   description="调试发布的云服务使用 IntelliTrace 和 Visual Studio"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="patshea123"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/14/2015"
   ms.author="patshea" />



# 调试发布的云服务使用 IntelliTrace 和 Visual Studio

##概述

Intellitrace，可以记录在 Azure 运行时角色实例的大量调试信息。 如果您需要查找问题的原因，您可以使用 IntelliTrace 日志从 Visual Studio 逐句通过代码，好像它在 Azure 中运行。 实际上，IntelliTrace 记录关键代码执行和环境数据在 Azure 应用程序作为在 Azure，云服务运行并且可以重播 Visual Studio 中的记录的数据。 作为一种替代方法，您可以使用远程调试来直接连接到运行在 Azure 的云服务。 请参见[调试云服务](http://go.microsoft.com/fwlink/p/?LinkId=623041)。

>[AZURE.IMPORTANT] IntelliTrace 适用于调试方案，并不能用于生产部署。

>[AZURE.NOTE] 您可以使用 IntelliTrace，如果您有安装的 Visual Studio 的企业和您的 Azure 应用程序目标.NET Framework 4 或更高版本。 只收集您 Azure 角色的信息。 这些角色始终运行 64 位操作系统的虚拟机。

## 配置用于 IntelliTrace Azure 应用程序

若要启用 IntelliTrace Azure 应用程序，必须创建并发布该应用程序从 Visual Studio Azure 项目。 您必须配置 IntelliTrace Azure 应用程序，然后将它发布到 Azure。 如果您不配置 IntelliTrace 发布您的应用程序，但然后决定要做到这一点，您将必须再次从 Visual Studio 应用程序发布。 有关详细信息，请参阅[发布云服务使用 Azure 工具](http://go.microsoft.com/fwlink/p/?LinkId=623012)。

1. 当准备好部署 Azure 应用程序，请验证您的项目生成目标设置为**调试**。

1. 在解决方案资源管理器中打开 Azure 项目的快捷菜单，然后选择**发布**。
 
    将出现发布 Azure 应用程序向导。

1. 发布在云中时收集 IntelliTrace 日志的应用程序，选择**启用 IntelliTrace**复选框。

    >[AZURE.NOTE] 您可以启用 IntelliTrace 或分析发布 Azure 应用程序时。 不能同时启用。

1. 若要自定义的基本的 IntelliTrace 配置，请选择**设置**超链接。

    IntelliTrace 设置对话框出现，如下图中所示。 您可以指定的事件日志是否收集调用信息、 哪些模块和进程来收集并记录，然后录制分配多少空间。 IntelliTrace 有关详细信息，请参阅[使用 IntelliTrace 调试](http://go.microsoft.com/fwlink/?LinkId=214468)。

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

IntelliTrace 日志是循环的日志文件 （默认大小为 250 MB） 的 IntelliTrace 设置中指定的最大大小。 IntelliTrace 日志收集到虚拟机的文件系统中文件中。 当您请求日志时，快照是拍摄在该点的时间，并且下载到本地计算机。

Azure 应用程序发布到 Azure 之后，您可以确定是否已启用从服务器资源管理器的 Azure 计算节点 IntelliTrace 如下图中所示︰

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## 正在下载 IntelliTrace 日志中的一个角色实例

从**服务器资源管理器**中的**云服务**节点，您可以下载 IntelliTrace 日志中的一个角色实例。 展开**云服务**节点，直到找到您所感兴趣的实例，打开快捷菜单，此实例并选择**查看 IntelliTrace 日志**。 IntelliTrace 日志将被下载到本地计算机上的目录中的文件。 每次您请求 IntelliTrace 记录、 创建新快照。

当日志会被下载时，Visual Studio 在 Azure 活动日志窗口中显示操作的进度。 下图中所示，您可以展开行项操作的详细信息，请参阅。

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

您可继续下载 IntelliTrace 日志时在 Visual Studio 中工作。 日志已下载结束时，将自动在 Visual Studio 中打开。

>[AZURE.NOTE] IntelliTrace 日志可能包含框架生成并随后处理的异常。 内部框架的代码生成这些异常作为启动一个角色，因此您可以放心地忽略它们的正常组成部分。

## 请参见

[调试的云服务](https://msdn.microsoft.com/library/ee405479.aspx)


测试
