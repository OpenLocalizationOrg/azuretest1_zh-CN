---
ms.openlocfilehash: 0c0fbeb6bbe8bd0587299dfbac0e075447379366
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在服务配置文件中指定的 DNS 设置 |Microsoft Azure"
   description="指定自定义服务配置文件用于虚拟网络的 DNS 设置"
   services="virtual-network"
   documentationCenter="na"
   authors="joaoma"
   manager="jdial"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/01/2015"
   ms.author="joaoma" />

# 在服务配置文件中指定的 DNS 设置

## DNS 的元素

服务配置文件可能包含那么元素的服务将使用的域名系统 (DNS) 服务器的 IPv4 地址的列表。 服务配置文件中的设置优先于网络的配置文件中的设置。 有关详细信息，请参阅[Azure 服务配置架构 (.cscfg 文件)](https://msdn.microsoft.com/library/azure/ee758710.aspx)。

**唇舌元素**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] **DnsServer**元素中的**名称**属性只用作引用名称。 它不表示 DNS 服务器的主机名。 在整个 Microsoft Azure 订阅**DnsServer**属性的每个值必须唯一。

## 请参见

[Azure 服务配置架构 (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure 的虚拟网络配置架构](http://go.microsoft.com/fwlink/?LinkId=248093)

[配置虚拟网络使用网络配置文件](http://go.microsoft.com/fwlink/?LinkId=248094)

[关于管理门户中的虚拟网络设置](http://go.microsoft.com/fwlink/?LinkId=248092)

