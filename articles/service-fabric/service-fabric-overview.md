---
ms.openlocfilehash: 34d7167077b900fab7660befbebcb660693df6ac
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="服务结构概述 |Microsoft Azure" 
   description="应用程序由组成 microservices 服务结构的概述。 服务结构是一个分布式的系统平台，用于构建可伸缩的、 可靠的和易于管理的云应用程序" 
   services="service-fabric" 
   documentationCenter=".net" 
   authors="msfussell" 
   manager="timlt" 
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA" 
   ms.date="08/25/2015"
   ms.author="mfussell"/>

# 服务结构的概览
服务结构是一个分布式的系统平台，用于构建可伸缩的、 可靠的和易于管理的云应用程序。 服务结构解决了在开发和管理云应用程序中的重大难题。 通过使用服务结构开发人员和管理员可以避免实现任务关键型的而解决复杂的基础结构问题和焦点苛刻的工作负载，知道它们可缩放、 可靠且易于管理。 服务结构表示的下一代中间件平台，用于构建并管理这些企业级，第 1 层云规模服务。

## 由 microservices 组成的应用程序
服务结构使您能够生成和管理可扩展性和可靠性的应用程序组成，microservices 在共享池中 （通常称为服务结构群集） 的计算机上运行在非常高的密度。  它提供了一个复杂的运行时构建分布式、 可伸缩的无状态和有状态 microservices 和全面的应用程序管理功能的配置、 部署、 监控、 升级/修补，并删除已部署的应用程序。 

服务结构动力 Azure SQL 数据库，Azure DocumentDB、 Cortana、 电源 BI、 Microsoft Intune，Azure 事件集线器如今天许多 Microsoft 服务，许多核心 Azure 服务和业务的 Skype，仅举几例。

创建群集服务结构，跨可用性设置在某个地区或跨地区创建"出生于云"的服务，可以开始的根据需要并涨至数百或数千台机器，具有大规模，定制服务结构。

今天的互联网规模服务是使用 microservices 生成的。 示例 microservices 是协议网关、 用户配置文件，购物车，库存处理、 队列、 缓存等。服务结构是提供唯一的名称，可以是无状态或有状态的每个 microservice microservices 平台。 

服务结构提供对应用程序组成，这些 microservices 全面运行时和生命周期管理功能。 它承载的部署和激活服务结构在群集内对容器内的 microservices。 就像密度的数量级增加可将 Vm 移到容器，将容器移动到 microservices 时密度的类似数量级成为可能。 例如，单个 SQL Azure 数据库群集，它构建服务结构上，由数百个运行数以千计的容器承载几十万个数据库 （每个数据库都服务结构状态 microservice） 总共十机组成。 是如此的事件集线器和上面提到的其他服务。 这就是为什么术语 hyperscale 可用于描述服务结构功能 — — 如果容器使密度较高，则 microservices 为您提供 hyperscale。 

![服务结构平台][Image1]

## 无状态和有状态服务结构 microservices

无状态 （例如协议网关、 web 代理服务器等） 的 microservices 不维护任何超出任何请求，就其从服务响应的可变状态。 Azure 的云服务工作者角色是服务的无状态的例子。 有状态 （如用户帐户、 数据库、 设备、 购物车、 队列等） 的 microservices 维护超出请求和回应的可变、 权威状态。 今天的互联网应用程序包含的无状态和有状态 microservices 组合。
 
有状态 microservices 为什么很重要？ 为什么不只是使用无状态服务的所有内容？ 两个原因︰

1) 构建高吞吐量、 低延迟、 故障容错 OLTP 的能力服务如交互式商店展开斗争、 搜索、 互联网内容 (IoT) 系统、 交易系统、 信用卡处理和欺诈检测系统，在同一台计算机上关闭个人记录管理等，通过保持代码和数据。

2) 应用程序设计简化为有状态的 microservices 移除需要附加队列和传统上需要解决纯粹是无状态的应用程序的可用性和延迟需求的缓存。 由于有状态服务自然是高可用性和低延迟这意味着少移动部件来管理整个应用程序中。 

应用程序模式和使用服务结构设计的详细信息请参阅[应用程序方案](service-fabric-application-scenarios.md)

## 应用程序生命周期管理
服务结构为云应用程序的完整的应用程序生命周期管理 (ALM) 提供了第一类的支持︰ 从开发到部署、 日常管理、 维护，和到最终退役。

服务结构 ALM 功能允许应用程序管理员 / IT 运算符使用简单、 低接触到规定的工作流部署修补程序，并监视应用程序。 这些内置的工作流极大地减少了 IT 操作员保持持续可用的应用程序的负担。 

大多数应用程序包含的无状态和有状态的 microservices 和共同部署其他 EXE/运行时组合。 通过使用强类型的应用程序以及打包的 microservices，服务结构使多个应用程序实例，其中每个可进行管理和升级独立的部署。 重要的是服务结构是能够将*所有*可执行文件或运行时部署，并使这些可靠的。 例如它可以用于部署 ASP.NET 5、 node.js，脚本或任何使您的应用程序。 
  
在应用程序生命周期管理的详细信息，请参阅[应用程序生命周期](service-fabric-application-lifecycle.md)

## 关键功能
通过使用服务结构，您可以︰

- 开发高度可伸缩的应用程序，可自我修复。

- "在您的计算机上的数据中心"的方法进行开发。 在本地开发环境是在 Azure 数据中心运行相同的代码。
 
- 开发应用程序 microservices，可执行文件和 ASP.NET、 nodejs 等您选择的其他应用程序框架组成。

- 开发无状态和有状态 （微） 服务，并使这些高度可靠。

- 通过使用缓存和队列的位置状态 （微观） 服务简化您的应用程序的设计。
 
- 以秒为单位部署的应用程序。

- Azure，或以部署云与零代码更改运行 Windows Server 部署。 一次写入，然后将部署到任何服务结构的群集。

- 部署应用程序在更高的密度比数百或上千的每台计算机的应用程序部署的虚拟机。 

- 将部署不同版本的同一应用程序的并行，每个独立的能升级。
 
- 管理不停机，包括中断和非中断升级有状态应用程序的生命周期。

- 管理应用程序使用.NET Api，Powershell 或 REST 接口。
 
- 升级和修补程序的应用程序中的 microservices 独立。

- 监视和诊断您的应用程序的运行状况和设置策略以执行自动修复。

- 如果大规模向上或向下伸缩服务结构群集轻松地了解应用程序可根据可用资源进行扩展。

- 注意群集内的服务结构从故障中恢复和优化负载根据可用资源的分布协调的应用程序重新分发自我康复资源平衡。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

* 详细信息︰[技术概述](service-fabric-technical-overview.md)。
* 设置服务结构[开发环境](service-fabric-get-started.md)。  
* 选择您的服务[框架](service-fabric-choose-framework.md)。


[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png

 