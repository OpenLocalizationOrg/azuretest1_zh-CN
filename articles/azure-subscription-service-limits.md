---
ms.openlocfilehash: 2e4a8c8dad852b540c1055e1062caf6f4dc1cf0f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="订阅 Microsoft Azure 和服务限制、 配额和约束"
    description="提供常见的 Azure 订阅和服务限制、 配额和约束的列表。 这包括如何增加最大值限制的信息。"
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor="monicar"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2015"
    ms.author="jroth"/>

# Azure 订阅和服务限制、 配额和约束

## 概述

此文档指定一些最常见的 Microsoft Azure 限制。 请注意，此当前不包括所有的 Azure 服务。 随着时间的推移将扩展和更新，包含了多个平台的这些限制。

> [AZURE.NOTE] 如果您想要提升高于**默认限制**的限制，则可以[打开免费在线客户支持请求](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)。 不能限制提升上面下面的表中的**最大限制**值。 如果没有**最大限制**列，然后指定的资源没有可调的限制。

## 限制和 Azure 的资源管理器

现可以组合到单个的 Azure 资源组中的多个 Azure 资源。 在使用资源组时，过去全球的限制成为托管在地区级别使用 Azure 资源管理器。 关于 Azure 资源组的详细信息，请参阅[使用资源组来管理 Azure 的资源](resource-group-portal.md)。

在下面的限制，已添加一个新表以反映限制任何差异时使用 Azure 资源管理器。 例如，没有一个**订阅数限制**和**订阅数限制的 Azure 资源管理器**表。 当限制仅适用于这两种情形中时，它仅显示第一个表中。 除非另有说明，所有区域间是全局限制。

> [AZURE.NOTE] 值得强调的 Azure 资源组的资源配额是每个地区访问您的订阅，并不是每个订阅服务管理配额进行。 举一个例子，让我们使用核心配额。 如果您需要申请配额增加对内核的支持，您需要决定您想要使用哪个地区，并将 Azure 资源组核心配额的特定请求的金额和所需的区域的核心数量。 因此，如果您需要使用 30 西欧核心运行您的应用程序控制。特别是应请求 30 西部欧洲的核心。 但您将不能在任何其他区域中增加核心配额--只有西欧将有 30-核心配额。
<!-- -->
结果，可能会发现有用考虑决定需要将任何一个地区的工作负载的 Azure 资源组配额和请求部署考虑到其中每个区域中的量。 请参阅更多的帮助，发现当前配额的特定地区[部署问题进行故障排除](resource-group-deploy-debug.md##authentication-subscription-role-and-quota-issues)。


## 特定于服务的限制

- [活动目录](#active-directory-limits)
- [API 管理](#api-management-limits)
- [应用程序服务](#app-service-limits)
- [应用程序的见解](#application-insights-limits)
- [Azure 的 Redis 高速缓存](#azure-redis-cache-limits)
- [Azure 的远程应用程序](#azure-remoteapp-limits)
- [备份](#backup-limits)
- [批处理](#batch-limits)
- [CDN](#cdn-limits)
- [云服务](#cloud-services-limits)
- [数据工厂](#data-factory-limits)
- [DocumentDB](#documentdb-limits)
- [密钥存储库](#key-vault-limits)
- [介质服务](#media-services-limits)
- [移动服务](#mobile-engagement-limits)
- [移动服务](#mobile-services-limits)
- [多因素身份验证](#multi-factor-authentication)
- [网络连接](#networking-limits)
- [通知中心服务](#notification-hub-service-limits)
- [操作建议](#operational-insights-limits)
- [资源组](#resource-group-limits)
- [计划程序](#scheduler-limits)
- [搜索](#search-limits)
- [服务总线](#service-bus-limits)
- [网站恢复](#site-recovery-limits)
- [SQL 数据库](#sql-database-limits)
- [Storage](#storage-limits)
- [StorSimple 系统](#storsimple-system-limits)
- [流分析](#stream-analytics-limits)
- [订阅](#subscription-limits)
- [虚拟机](#virtual-machines-limits)


### 订阅数限制
#### 订阅数限制
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### 订阅数限制的 Azure 的资源管理器

使用 Azure 资源管理器和 Azure 资源组时，将应用以下限制。 没有改变使用 Azure 资源管理器中的限制，下面未列出。 请参阅上表的那些限制。

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### 资源组的限制

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]


### 虚拟机限制
#### 虚拟机限制
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### 虚拟机限制的 Azure 的资源管理器

使用 Azure 资源管理器和 Azure 资源组时，将应用以下限制。 没有改变使用 Azure 资源管理器中的限制，下面未列出。 请参阅上表的那些限制。

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]


### 网络限制
#### 网络限制
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### 通信管理器限制

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### DNS 限制

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### 存储限制

#### 标准的存储限制

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

帐户存储限制的详细信息，请参阅[Azure 存储可扩展性和性能目标](../articles/storage/storage-scalability-targets.md)。


#### 特优的存储限制

[AZURE.INCLUDE [azure-storage-limits-premium-storage](../includes/azure-storage-limits-premium-storage.md)]


#### 存储限制-Azure 的资源管理器

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### 云服务限制

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### 应用程序服务限制
下面的应用程序服务限制包括限制 Web 应用程序、 移动应用程序、 API 的应用程序和应用程序逻辑。

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### 计划程序限制

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### 批次限制

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]


### DocumentDB 限制

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]


### 移动服务限制

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### 搜索限制

[AZURE.INCLUDE [azure-search-limits](../includes/azure-search-limits.md)]

Azure 搜索限制的详细信息，请参阅[限制和约束](https://msdn.microsoft.com/library/azure/dn798934.aspx)。

### 媒体服务限制

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### CDN 限制

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### 移动服务限制

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### 通知中心服务限制

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]


### 服务总线限制

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### 数据工厂限制

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]


### 流分析限制

[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### Active Directory 限制

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### Azure 的远程应用程序限制

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### StorSimple 系统限制

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### 操作建议限制

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### 备份限制

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### 网站恢复限制

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### 理解应用程序限制

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### API 管理限制

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### Azure Redis 缓存限制

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### 密钥存储区限制

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### 多因素身份验证
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### SQL 数据库限制

SQL 数据库的限制，请参阅[SQL 数据库的资源限制](sql-database/sql-database-resource-limits.md)。

## 请参见

[Azure 的限制和增加了解](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[虚拟机和 Azure 的云服务大小](http://msdn.microsoft.com/library/azure/dn197896.aspx)

测试
