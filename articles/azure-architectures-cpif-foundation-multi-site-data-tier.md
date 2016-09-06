---
ms.openlocfilehash: 5f4a64af9fd6b921a0fe3f3426f9f3cdf39814f0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="多站点数据层 （Azure 的体系结构模式）" 
   description="多站点数据层模式是基础区域，所述广泛 CPIF 体系结构文档的一部分。" 
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

# 多站点数据层 （Azure 的体系结构模式）

[云平台的集成框架 (CPIF)](azure-architectures-cpif-overview.md)提供服务到 Microsoft 的云解决方案的应用程序工作负载集成指南。 

CPIF 介绍了如何组织、 客户和合作伙伴应设计和部署利用 Azure、 系统中心和 Windows 服务器混合云平台和管理功能为目标云的工作负载。 

**多站点数据层**模式是**基础**区域，所述广泛 CPIF 体系结构文档的一部分。 

## 多站点数据层

多站点数据层设计模式详细信息的 Azure 功能和提供可以跨越地理边界提供可预知的性能和高可用性的数据层服务所需的服务。 此设计模式用于作为服务提供一种独立的方式中任何一种传统的数据平台服务层或多层的应用程序的一部分定义一个数据层。  在此模式中，数据层的负载平衡提供了两个本地区域内和跨地区。   

引入与 SQL Server 2012年，AlwaysOn 可用性组则完全支持的 Azure 的基础结构服务的高可用性和灾难恢复功能。  AlwaysOn 可用性组文章中，可以找到详细的信息和 AlwaysOn 上 Windows Azure 基础结构服务的可用性组官方支持的通告。   

本文档提供在 Azure 中一个多站点数据层的体系结构概述利用 SQL AlwaysOn 可用性组。 使用中的可选只读的次要节点附加功能和灾难恢复的附加 Azure 数据中心。 在 Azure 中使用 SQL AlwaysOn 提供可由 web 或应用程序层的高可用性数据层。  

本文档重点介绍体系结构模式和做法，而中正式教程，略述 AlwaysOn 可用性组在 Azure 中的配置和 AlwaysOn 可用性组侦听器配置可以找到完整的部署指南。 

## 体系结构模式概述 

本文档描述了在多个地域提供访问 Microsoft SQL Server 内容，出于可用性和冗余模式。  关键服务下面所示未注意到的应用程序或 web 层访问数据本身。  下图是相关服务和如何为该模式的一部分使用的简单展示。   

下图更详细地概述了每个主要服务领域。 
 
![标记部分资源和资源组刀片](./media/azure-architectures-cpif-foundation-multi-site-data-tier/overview.png)

##  其他资源
[负载平衡数据层 (pdf)](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-dfb09e41)

## 请参见
[CPIF 的体系结构](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-bd1e434a) 

[全局负载平衡的 Web 层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-2c3c663a) 

[混合网络](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-5e401f38)

[Azure 搜索层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-e581d65d) 

[批次处理层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-0bc3f8b1)

测试
