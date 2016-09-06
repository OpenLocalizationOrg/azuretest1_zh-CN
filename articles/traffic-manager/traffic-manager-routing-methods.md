---
ms.openlocfilehash: c1afa06d2b62ec5bdaecd877a327151c97796f93
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle=" 通信管理器-通信路由方法 |Microsoft Azure"
   description="本文将帮助您了解流量管理器使用不同的通信路由方法"
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/19/2015"
   ms.author="joaoma" />

# 通信管理器路由方法

有流量管理器中可用的三种通信路由方法。 每一个流量管理器配置文件可以一次使用只有一个通信路由方法，虽然可以在任何时候为您的配置文件选择不同的通信路由方法。

请务必注意，所有的通信路由方法包括终结点监视。 在配置通信量管理器配置文件，指定通信路由方法最适合您的需求后，配置监视设置。 正确配置监视，流量管理器将监视您的终结点的状态组成的云服务和网站，并不将通信发送到它认为是不可用的终结点。 有关通信量管理器监视的信息，请参阅[通信量管理器监视有关](traffic-manager-monitoring.md)。 

三种流量管理器通信路由方法如下︰

- **故障转移**︰ 选择具有相同或不同 Azure 数据中心 （称为管理门户中的区域） 中的终结点并且想要使用主终结点的所有通信，但提供主要的用例中的备份或备份端点不可用时的故障转移。 有关详细信息，请参阅[故障转移通信路由方法](#failover-traffic-routing-method)。

- **循环**︰ 当您想要在一组的同一个数据中心中的终结点或在不同数据中心之间分配负载循环选择法。 有关详细信息，请参阅[循环复用通信路由方法](#round-robin-traffic-routing-method)。

- **性能**︰ 选择性能时必须终结点在不同的地理位置，可以请求的客户机使用以最低的延迟"最接近"的终结点。 有关详细信息，请参阅[性能通信路由方法](#performance-traffic-routing-method)。

请注意，Azure 网站已经提供故障切换和循环流量路由方法在数据中心，而不考虑网站模式的网站功能。 通信管理器允许您指定故障转移和循环流量的路由在不同数据中心的网站。

>[AZURE.NOTE] DNS 的生存时间 (TTL) 通知 DNS 客户端和 DNS 服务器缓存解析的名称的时间上的冲突解决程序。 客户端将继续直到本地 DNS 缓存条目的名称解析的域名到期时使用给定终结点。

## 故障转移通信路由方法

经常组织希望提供其服务的可靠性。 这是通过提供备份服务，其主要服务出现故障的情况下实现的。 服务故障转移的常见模式是提供一套相同的终结点，并将通信发送到主服务，其中列出的一个或多个备份。 主要服务不可用，如果顺序中的下一个引用发出请求的客户端。 如果同时在列表中的第一和第二个服务不可用，通信转到第三个，等等。

配置故障转移通信路由方法时，所选端点的顺序非常重要。 使用管理门户，您可以在配置文件的配置页上配置故障转移顺序。

图 1 显示了一组终结点的故障转移通信路由方法的示例。

![通信管理器故障转移路由方法](./media/traffic-manager-routing-methods/IC750592.jpg)

**图 1**

以下步骤对应于图 1 中的编号。

1. 通信管理器收到来自通过 DNS 客户端的传入请求，并找到配置文件。
2. 配置文件包含终结点的排序的列表。 通信管理器将检查哪些终结点是列表中的第一个。 如果终结点 （基于日常终结点监视） 处于联机状态，它将对客户端的 DNS 响应中指定该终结点的 DNS 名称。 如果终结点处于脱机状态，流量管理器确定列表中下一个在线的终结点。 在此示例中 CS A 处于脱机状态 （不可用），但 CS-B 处于联机状态 （可用）。
3. 通信管理器返回到客户端的 DNS 服务器，将域名解析为 IP 地址，并将其发送到客户端 CS-B 域的名称。
4. 客户端启动的通信到 CS b。

## 循环复用通信路由方法

常见的通信路由模式是提供一套相同的终结点，并将通信发送到每一轮循机制方式。 循环复用方法跨不同的终结点的通信量将拆分。 它将随机选择一个正常运行的终结点，并不将通信发送到被检测为关闭的服务。 有关详细信息，请参阅[通信量管理器监视](../traffic-manager-monitoring.md)。

图 2 显示了一组终结点的循环流量路由方法的示例。

![通信管理器循环路由方法](./media/traffic-manager-routing-methods/IC750593.jpg)

**图 2**

以下步骤对应于图 2 中的数字。

1. 通信管理器收到来自客户端的传入请求，并找到配置文件。
2. 配置文件包含终结点的列表。 通信管理器终结点从该列表中随机选择，但不包括由通信管理器终结点监视任何脱机 （不可用） 终结点。 在此示例中，这是终结点 CS b。
3. 通信管理器返回到客户端的 DNS 服务器 CS-B 域的名称。 客户机的 DNS 服务器将此域名解析为 IP 地址，并将其发送到客户端。
4. 客户端启动的通信到 CS b。

循环复用通信路由还支持加权的网络流量分布。 图 3 显示了一种加权循环复用通信路由方法为一组终结点。

![循环复用加权路由方法](./media/traffic-manager-routing-methods/IC750594.png)

**图 3**

加权的循环复用通信路由允许您分配到各种基于分配的权重值为每个终结点的终结点的负载。 更高重量，越频繁终结点将返回。 此方法非常有用的方案包括︰

- 逐步升级的应用程序升级︰ 分配占通信将路由到新终结点，并通过 100%的时间逐渐增加的通信量。
- 应用程序迁移到 Azure︰ 创建配置式 Azure 和外部终结点和指定路由到每个终结点的通信的重量。
- 增加容量云暴︰ 迅速将它放后面一个流量管理器配置文件展开内部部署到云中。 当您需要额外的容量，在云环境中时，可以添加或启用多个终结点和指定哪些部分通信转到每个终结点。

到目前为止，不能用于管理门户配置加权的流量路由。 Azure 提供相关的服务管理 REST API 和 Azure PowerShell cmdlet 使用此方法以编程方式访问。

有关使用 REST Api 的信息，请参阅[操作上流量管理器 （REST API 参考）](http://go.microsoft.com/fwlink/p/?LinkId=313584)。

有关使用 Azure PowerShell cmdlet 的信息，请参阅[Azure 流量管理器 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=400769)。 示例配置，请参阅 Azure 的博客中的[Azure 流量管理器外部终结点和通过 PowerShell 加权循环复用](http://azure.microsoft.com/blog/2014/06/26/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/)。

若要测试从单个客户端的配置文件并观察相等或加权循环法的行为，验证 DNS 名称被解析为不同的 IP 地址，根据配置文件中的相同或加权值的终结点。 在测试时，您必须禁用客户端的 DNS 缓存或清除 DNS 缓存每次尝试确保获取发送新的 DNS 名称查询之间。

## 性能通信路由方法

为了将通信路由到终结点在不同数据中心遍布全球各地，可以直接到请求客户端终结点之间的最小延迟方面最接近终结点的传入通信。 通常情况下，"最接近"的终结点直接对应于最短的地理距离。 性能通信路由方法允许您将分发基于位置和滞后时间，但不能考虑到网络配置或负载中的实时更改帐户。

性能通信路由方法定位请求的客户端，并指最接近终结点。 显示各个 IP 地址和每个 Azure 数据中心之间的往返过程时间互联网延迟表取决于"程度"。 此表定期更新并不是要通过 Internet 进行实时反射的性能。 它不会不考虑到给定的服务上的负载虽然通信管理器监视您基于的方法选择和会不将它们包括在 DNS 查询响应如果它们可用的终结点。 换句话说，性能通信路由还采用了故障转移通信路由方法。

图 4 显示一组终结点的性能通信路由方法的示例。

![通信管理器性能路由方法](./media/traffic-manager-routing-methods/IC753237.jpg)

**图 4**

以下步骤对应于图 4 中的编号。

1. 通信管理器定期生成 Internet 延迟表。 通信管理器基础结构运行测试来确定世界上的不同点和 Azure 数据中心承载的终结点之间的往返行程时间。
2. 通信管理器收到来自通过其本地 DNS 服务器的客户端的传入请求，并找到配置文件。
3. 通信管理器定位互联网延迟传入的 DNS 请求的 IP 地址表中的行。 因为用户的本地 DNS 服务器执行迭代 DNS 查询来查找通信管理器配置文件名称具有权威的 DNS 服务器，DNS 查询发送客户端的本地 DNS 服务器的 IP 地址。
4. 通信管理器查找数据中心，其数据中心的最短时间该主机配置文件中定义的终结点。 在此示例中，这就是 CS b。
5. 通信管理器返回到客户端的本地 DNS 服务器，从而将域名解析为 IP 地址，并将其发送到客户端 CS-B 域的名称。
6. 客户端启动的通信到 CS b。

**需要注意的事项︰**

- 如果您的配置文件中包含在同一个数据中心中的多个终结点，然后定向到该数据中心流量均匀分布跨端点可用且根据终结点监视运行状况良好。
- 如果给定数据中心内的所有端点都不可用 （根据终结点监视），为这些终结点的通信将分布在所有其他可用终结点中的配置文件，不给下一步最接近终结点指定的。 这是为了避免级联故障可能发生的下一步最接近终结点变得过载。
- 互联网延迟表更新时，可能会请注意通信模式差异并加载您的端点上。 这些更改应该是微乎其微的。
- 使用时性能通信路由方法外部端点，您需要指定这些终结点的位置。 选择最接近您的部署的 Azure 地区。 有关详细信息，请参阅[管理终结点通信管理器中](traffic-manager-endpoints.md)。

## 通信管理器数据

如果您希望在本主题中的数字作为 PowerPoint 边流量管理器上或修改您自己进行演示，请参见[MSDN 文档中数字通信管理器](http://gallery.technet.microsoft.com/Traffic-Manager-figures-in-887e7c99)。

## 下一步行动

[通信管理器是什么？](traffic-manager-overview.md)

[有关通信量管理器监视](traffic-manager-monitoring.md)

[操作上流量管理器 （REST API 参考）](http://go.microsoft.com/fwlink/p/?LinkID=313584)

[云服务](http://go.microsoft.com/fwlink/p/?LinkId=314074)

[网站](http://go.microsoft.com/fwlink/p/?LinkId=393327)

[Azure 的流量管理器 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=400769)

 