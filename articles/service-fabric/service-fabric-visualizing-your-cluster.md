---
ms.openlocfilehash: 425d941a1fcc83b1e526f665fd21371f0d0b07fb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="直观显示您使用服务结构资源管理器中的群集"
   description="服务结构资源管理器是一个有用的 GUI 工具用于检查和管理云应用程序和 Microsoft Azure 服务结构群集中的节点。"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/05/2015"
   ms.author="jesseb"/>

# 直观显示您使用服务结构资源管理器中的群集

服务结构资源管理器是一种可视化工具，用于检查和管理云应用程序和 Microsoft Azure 服务结构群集中的节点。 服务结构资源管理器可以连接到本地开发群集和 Azure 的群集。 服务结构 PowerShell cmdlet 的信息，请参阅**下一个步骤**。

> [AZURE.NOTE] 在 Azure 服务结构创建群集尚不可用。

## 服务结构资源管理器介绍

确保本地开发环境由安装程序在[设置服务结构开发环境](service-fabric-get-started.md)的说明。

从您的本地安装路径 （%程序 Files%\Microsoft SDKs\Service Fabric\Tools\ServiceFabricExplorer\ServiceFabricExplorer.exe） 运行服务结构资源管理器。 如果存在，该工具将自动连接到本地开发群集。  它如群集上会显示信息︰

- 在群集上运行的应用程序
- 有关群集的节点信息
- 从应用程序和节点的运行状况事件
- 在群集中的应用
- 监视应用程序升级状态

![群集服务结构和部署的应用程序的可视表示形式][servicefabricexplorer]

重要的可视化效果之一是群集映射，为群集 （例如单击**Onebox/本地群集**） 的仪表板上可见。 群集映射显示一组升级域和故障域，以及哪些节点映射到哪些域。  请参阅[技术概述服务结构](service-fabric-technical-overview.md)熟悉服务结构的关键概念。

![群集映射显示哪些升级域和所属的每个节点的故障域。][clustermap]


## 查看应用程序和服务

服务结构资源管理器允许您浏览群集上运行的应用程序。  展开**应用程序视图**以查看有关您的应用程序、 服务、 分区和复制副本详细的信息。

下图显示的应用程序名为**"结构: / Stateful1Application"**有一个无国界的服务，名为**"结构: / Stateful1Application/MyFrontEnd"**和一个有状态的服务名为**"结构: / Stateful1Application/Stateful1"**。 无状态的服务的一个副本运行在**Node.4**上有一个分区。 有状态的服务有两个分区，每个都有 3 副本，在几个不同的节点上运行。

![在服务结构群集上运行的应用程序的视图][applicationview]

单击应用程序、 服务、 分区或副本上对该实体提供详细的信息。  下图显示了一个有状态的服务的主副本服务复制副本运行状况仪表板。  这包括它的角色、 其运行的节点，它正在侦听的地址、 它在磁盘上，并运行状况事件的文件的位置。

![服务结构副本的详细的信息][replicadetails]


## 连接到远程服务结构群集

若要查看远程群集服务结构，单击**连接**以打开对话框中**连接到群集服务的结构**。  输入您的群集**ServiceFabric 终结点**，然后单击**连接**。  服务结构终结点通常是群集服务侦听端口 19000 的公共名称。

![安装到远程服务结构群集的连接][connecttocluster]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

- [可测试性概述](service-fabric-testability-overview.md)。
- [管理您在 Visual Studio 中的服务结构应用程序](service-fabric-manage-application-in-visual-studio.md)。
- [使用 PowerShell 的服务结构的应用程序部署](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[applicationview]: ./media/service-fabric-visualizing-your-cluster/applicationview.png
[clustermap]: ./media/service-fabric-visualizing-your-cluster/clustermap.png
[connecttocluster]: ./media/service-fabric-visualizing-your-cluster/connecttocluster.png
[replicadetails]: ./media/service-fabric-visualizing-your-cluster/replicadetails.png
[servicefabricexplorer]: ./media/service-fabric-visualizing-your-cluster/servicefabricexplorer.png
