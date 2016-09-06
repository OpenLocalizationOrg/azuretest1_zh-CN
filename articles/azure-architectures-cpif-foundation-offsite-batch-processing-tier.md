---
ms.openlocfilehash: aa94c0e270a4bdf7be2a21701fd79bcad33ee45d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="异地批处理层 （Azure 的体系结构模式）" 
   description="异地批次处理层模式是基础架构区域，CPIF 体系结构文档中广泛地介绍的一部分。" 
   services="" 
   documentationCenter="" 
   authors="arynes" 
   manager="fredhar" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple" 
   ms.date="03/25/2015"
   ms.author="arynes"/>

# 异地批处理层 （Azure 的体系结构模式）

[云平台的集成框架 (CPIF)](azure-architectures-cpif-overview.md)提供服务到 Microsoft 的云解决方案的应用程序工作负载集成指南。  

CPIF 介绍了如何组织、 客户和合作伙伴应设计和部署利用 Azure、 系统中心和 Windows 服务器混合云平台和管理功能为目标云的工作负载。 

**异地批次处理层**模式是**基础架构**区域，CPIF 体系结构文档中广泛地介绍的一部分。 

##  异地批次处理层

异地批次处理级别设计模式详细信息的 Azure 功能和提供这两种故障容错和可伸缩性的后端数据处理所需的服务。  这些服务作为辅助角色在云服务中意识到在 Azure，目前可以将它们部署到任何 Azure 数据中心上。   

批次处理工作负载是唯一的因为它们通常提供很少或没有用户界面。  这种类型的工作负载在场所的例子是在 Windows 服务器上运行的 Windows 服务。  在考虑这种类型的工作负载在云环境中，它将浪费部署整个服务器时要运行的工作负荷，真正需要的是计算、 存储和网络连接。  辅助角色是此在 Azure 上的实现。 

按照定义，批处理作业运行在 Azure 是负载连接到资源，提供一些业务逻辑 （计算），并提供了一些输出。  输入和输出资源由用户定义，范围可以从平面文件、 在 Azure blob 存储 blob，NoSQL 数据库或关系数据库。   

实现业务逻辑在 Azure 辅助角色中，通常通过.NET 库中定义所需的业务逻辑。  部署工作人员到 Azure 角色的是一个简单的操作，而部署故障容错和可伸缩性的工作角色需要考虑如何执行和保留在 Azure 服务设计。  此模式将详细考虑了这些要求，并介绍如何实现这些设计。 

## 体系结构模式概述 

本文介绍利用辅助角色实例中 Azure 的云服务中包含的异地批处理模式。  这种设计的关键组成部分如下所示。  此图说明了最小所需的实例，来实现容错。  可以部署其他实例，以增加服务的性能。  此外，为协助按时间或其他服务器指标扩展实例启用自动缩放。 

##  其他资源
[批次处理层 (pdf)](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-0bc3f8b1)

## 请参见
[CPIF 的体系结构](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-bd1e434a) 

[全局负载平衡的 Web 层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-2c3c663a) 

[负载平衡的数据层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-dfb09e41)

[混合网络](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-5e401f38)

[Azure 搜索层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-e581d65d) 


测试
