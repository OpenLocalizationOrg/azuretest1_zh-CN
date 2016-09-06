---
ms.openlocfilehash: 91f86475d89ce3bad4f7bf5edaaf738b36983755
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="始终在为 SQL 配置负载平衡器 |Microsoft Azure"
   description="配置负载平衡器上始终使用 SQL 和如何利用 powershell 创建 SQL 实现的负载平衡器"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/12/2015"
   ms.author="joaoma" />

# 始终在为 SQL 配置负载平衡器

现在可以使用 ILB 运行 SQL Server AlwaysOn 可用性组。 可用性组是 SQL Server 的旗舰产品解决方案的高可用性和灾难恢复。 可用性组侦听程序允许客户端应用程序无缝地连接到的主副本，而不考虑配置中副本的数量。

监听器 (DNS) 名称映射到一个负载平衡的 IP 地址和 Azure 的负载平衡器指示副本集中到只有主服务器的传入通信。 


您可以使用 SQL Server AlwaysOn （监听器） 终结点 ILB 支持。 现在可以控制侦听器的可访问性，并可以负载平衡的 IP 地址从一个特定的子网中虚拟网络 (VNet)。 

通过对侦听器，SQL 服务器终结点使用 ILB (如服务器 = tcp:ListenerName，1433; 数据库 = ' 数据库名称 ') 只能由访问︰

服务和相同的虚拟网络服务中的虚拟机和虚拟机从连接内部网络服务并从间虚拟机连接 VNets

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.jpg) 


只能通过 PowerShell 配置内部负载平衡器。


## 向服务中添加内部负载平衡器 

### 第 1 步。

在以下示例中，我们将配置虚拟网络，其中包含一个称为"子网-1"的子网︰

    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

第 2 步。

## 为每个虚拟机上的 ILB 添加负载平衡的端点

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

在上面的示例中，您有 2 VM 调用"sqlsvc1"和"sqlsvc2"运行在云中服务"Sqlsvc"。 用"DirectServerReturn"开关创建 ILB 之后, 将向 ILB 允许 SQL 配置可用性组的侦听器添加负载平衡的端点。

您可以找到更多的详细信息创建的 SQL Alwayson[教程︰ 在 Azure AlwaysOn 可用性组](https://msdn.microsoft.com/library/dn249504.aspx)或[使用门户网站库](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx)。


## 请参见

[开始配置 Internet 面临负载平衡器](load-balancer-internet-getstarted.md)

[开始配置内部负载平衡器](load-balancer-internal-getstarted.md)

[配置负载平衡器分布模式](load-balancer-distribution-mode.md)

[配置负载平衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)
 
测试
