---
ms.openlocfilehash: 1dbe8a569008835276316a2e687f6f0fdac217ec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="全局负载平衡的 Web 层 （Azure 的体系结构模式）" 
   description="全局负载平衡 Web 层模式是基础区域，所述广泛 CPIF 体系结构文档的一部分。" 
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

# 全局负载平衡的 Web 层 （Azure 的体系结构模式）

[云平台的集成框架 (CPIF)](azure-architectures-cpif-overview.md)提供服务到 Microsoft 的云解决方案的应用程序工作负载集成指南。 

CPIF 介绍了如何组织、 客户和合作伙伴应设计和部署利用 Azure、 系统中心和 Windows 服务器混合云平台和管理功能为目标云的工作负载。 

**全局负载平衡 Web 层**模式是**基础**区域，所述广泛 CPIF 体系结构文档的一部分。 

##  全局负载平衡的 Web 层

全局负载平衡 Web 层设计模式详细信息的 Azure 功能和提供可以跨越地理边界提供可预知的性能和高可用性的 web 层服务所需的服务。 

这种设计模式网站的目的层被指服务提供传统 HTTP/HTTPS 内容或应用程序服务层以隔离方式或多层 web 应用程序的一部分。  在此模式中，web 层的负载平衡提供了两个本地区域内和跨地区。 从计算的角度，通过 Microsoft Azure 的虚拟机、 web 站点或两者的结合，可以提供这些服务。  提供 web 服务的虚拟机可以承载使用任何支持的 Microsoft Windows 或 Linux 分发来宾操作系统在 Microsoft Azure 中的内容。 


## 体系结构模式概述 

本文档描述出于可用性和冗余的在多个地域提供对 web 服务或 web 服务器内容的访问模式。  关键服务下面所示未注意到的 web 平台约束或 web 服务本身内的开发方法。  有与此模式 – 一个承载的 web 内容或服务，在虚拟机上的两个变体 （使用 Azure 支持操作系统和家族），一个使用 Azure 网站。  下图是简单的相关服务和如何使用属于这种模式的虚拟机为例说明。   

##  其他资源
[全局负载平衡 Web 层 (pdf)](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-2c3c663a) 

## 请参见
[CPIF 的体系结构](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-bd1e434a) 

[负载平衡的数据层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-dfb09e41)

[混合网络](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-5e401f38)

[Azure 搜索层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-e581d65d) 

[批次处理层](https://gallery.technet.microsoft.com/Cloud-Platform-Integration-0bc3f8b1)


测试
