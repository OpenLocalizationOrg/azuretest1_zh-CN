---
ms.openlocfilehash: bcb38740601164b9380f349266bef8d250ed8391
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="应用程序网关简介 |Microsoft Azure"
   description="此页提供的第 7 层负载平衡的应用程序网关服务概述包括网关大小、 HTTP 负载平衡、 基于 cookie 的会话相似性和 SSL 卸载。"
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/23/2015"
   ms.author="joaoma"/>

# 什么是应用程序网关？


Microsoft Azure 应用程序网关提供了 Azure 托管 HTTP 负载平衡基于第 7 层负载平衡解决方案。 

应用程序负载平衡允许 IT 管理员和开发人员可以创建基于 HTTP 的网络通信的路由规则。  应用程序网关服务是高可用性和计量。 对于 SLA 和定价，请参考[有关 SLA](http://azure.microsoft.com/support/legal/sla/)和[定价](https://azure.microsoft.com/pricing/details/application-gateway/)页。

应用程序网关当前支持层 7 应用程序交付以下︰

- HTTP 的负载平衡
- 基于 cookie 的会话相似性
- SSL 卸载

![应用程序网关](./media/application-gateway-introduction/appgateway1.png)

## HTTP 的第 7 层负载平衡

Azure 提供第 4 层负载平衡通过在传输层 (TCP/UDP) 工作，并且让所有传入的网络通信进行负载平衡的应用程序网关服务的 Azure 负载平衡器。 应用程序网关会将路由规则提供级别 7 (HTTP) 的 HTTP 通信量负载平衡。 当您创建应用程序网关时，请将相关联并用作入站网络通信的公用 IP 终结点 (VIP)。

应用程序网关将路由基于其配置，无论是虚拟机、 云服务、 web 应用程序或外部 IP 地址的 HTTP 通信量。

下图说明了通信的应用程序网关的排列方式︰ ![Gateway2 应用程序](./media/application-gateway-introduction/appgateway2.png)

HTTP 第 7 层负载平衡可用于︰


- 应用程序需要从同一个用户/客户端会话的请求到达同一个后端 VM。 本示例的就购物车应用程序和 web 邮件服务器。
- 想要自由终止 SSL 的开销从 web 服务器群的应用程序。
- 应用程序，例如 CDN，需要多个 HTTP 请求的同一个长时间运行的 TCP 连接来进行路由负载平衡到不同的后端服务器。

## 网关大小和实例

应用程序网关将目前提供 3 种尺寸︰ 小型、 中型和大型。 小实例大小适用于开发和测试方案。 

您可以创建每个订阅的最多 10 个应用程序网关，并且每个应用程序网关可以有最多 10 个实例。 应用程序网关负载平衡 Azure 托管服务允许资源调配软件 Azure 负载平衡器后面的第 7 层负载平衡器。

## 配置和管理

您可以创建和管理应用程序网关，通过使用 REST Api 和使用 PowerShell cmdlet。

## 下一步行动

创建应用程序网关。 请参阅[创建应用程序网关](application-gateway-create-gateway.md)。

配置 SSL 卸载。 请参阅[配置 SSL 卸载使用应用程序网关](application-gateway-ssl.md)。



测试
