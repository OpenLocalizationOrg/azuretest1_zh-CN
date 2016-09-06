---
ms.openlocfilehash: 4e41dc97bce4cdd51fb705768cbac37aa7abb04b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 的资源管理器支持的负载平衡器预览 |Microsoft Azure "
   description="在预览中使用负载平衡器使用 Azure 资源管理器 (ARM) powershell。 使用模板进行负载平衡器"
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
   ms.date="06/30/2015"
   ms.author="joaoma" />


# Azure 的资源管理器支持的负载平衡器 

Azure 资源管理器 (ARM) 是 Azure 中的服务新的管理框架。 现在可以使用基于 Azure 资源管理器 Api 和工具来管理 azure 负载平衡器。 若要了解有关 Azure 资源管理器的详细信息，请参阅[使用资源组来管理 Azure 的资源](../azure-preview-portal-using-resource-groups.md)。

## 概念

与 ARM，Azure 负载平衡器包含下列子资源︰

- 前端 IP 配置 — 负载平衡器可以包括一个或多个前端 IP 地址，否则称为虚拟 IPs (VIPs)。 这些 IP 地址用作入站通信。

- 后端地址池--这些是关联与虚拟机网络网卡 (NIC) 负载分配到的 IP 地址。

- 负载平衡规则-规则属性映射给定的前端 IP 和端口组合到一组后端 IP 地址和端口组合。 负载平衡器资源的单一定义，您可以定义多个负载平衡的规则，反映前的一个组合的每个规则结束 IP 和端口和后端 IP 和端口与虚拟机相关联。

- 探测器--探测，可以跟踪的虚拟机实例的运行状况。 如果健康探测失败，VM 实例将会自动离开旋转。

- 入站的 NAT 规则 – NAT 规则定义入站的通信流经前面结束 IP，并分发到后端 IP。


![](./media/load-balancer-arm/load-balancer-arm.png)



## 快速启动模板
Azure 的资源管理器允许您配置应用程序使用声明性的模板。 在一个模板中，您可以部署多个服务及其依赖项。 使用相同的模板重复部署您的应用程序在每个阶段的应用程序生命周期

模板包括虚拟机、 虚拟网络、 可用性设置、 网络接口 (Nic)、 存储帐户、 负载平衡器、 网络安全组和公用的 IPs。 使用模板可以创建复杂的应用程序使用一个简单的文件，您可以签入和协作所需的一切。

[了解有关模板的详细信息](http://go.microsoft.com/fwlink/?LinkId=544798)

[了解有关网络资源](../resource-groups-networking)

模板中使用 Azure 负载平衡器可以找到[GitHub 资料库](https://github.com/Azure/azure-quickstart-templates)中承载一套社区生成的模板

模板的示例︰

- [2 虚拟机的负载平衡器和负载平衡规则](http://go.microsoft.com/fwlink/?LinkId=544799)

- [在内部负载平衡器和负载平衡器的规则与 VNET 2 的虚拟机](http://go.microsoft.com/fwlink/?LinkId=544800)

- [2 在负载平衡器的虚拟机并在磅上配置 NAT 规则](http://go.microsoft.com/fwlink/?LinkId=544801)


## 设置 PowerShell 或 CLI Azure 负载平衡器

[Azure 网络 Cmdlet](https://msdn.microsoft.com/library/azure/mt163510.aspx)可以用于创建一个负载平衡器。 开始使用 ARM cmdlet 和 REST Api

- [如何创建使用 Azure 资源管理器的负载平衡器](../load-balancer-arm-powershell)

- [Azure CLI 使用 Azure 的资源管理](../xplat-cli-azure-resource-manager)

- [负载平衡器 REST Api](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## 请参见

[配置负载平衡器分布模式](load-balancer-distribution-mode.md)

[配置负载平衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)
 

测试
