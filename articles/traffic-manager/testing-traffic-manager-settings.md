---
ms.openlocfilehash: cc98a0b66d3f06cf99dfaf1b8faddf96e2851dec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="测试您的流量管理器设置"
   description="这篇文章将帮助您测试您的流量管理器设置。"
   services="traffic-manager"
   documentationCenter="na"
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

# 测试通信管理器设置

测试通信管理器设置的最佳办法是设置的客户端数，然后将端点，云服务和网站，在下一次配置文件组成。 以下提示将帮助您测试您的流量管理器配置文件。

## 基本测试步骤

-**设置 DNS TTL 非常低**，以便更改将传播快速的 30 秒，例如。

-在配置文件中**知道您的 Azure 的云服务和网站的 IP 地址**进行测试。

-**使用的工具，使您可以解析为 IP 地址的 DNS 名称**和地址的显示。 要检查以查看您公司的域名解析为 IP 地址的配置文件中的终结点。 他们应该流量管理器配置文件使用的负载平衡方法一致的方式解决冲突。 如果要在运行 Windows 的计算机上，您可以使用命令或 Windows PowerShell 提示 Nslookup.exe 工具。 其他公开提供的工具，可用于"挖"的 IP 地址也是在 Internet 上随时可用。

### 若要检查使用 nslookup 一个流量管理器配置文件

1 打开命令或 Windows PowerShell 提示以管理员的身份。

2 型`ipconfig /flushdns`刷新 DNS 解析器缓存。

3 型`nslookup <your Traffic Manager domain name>`。 例如，以下命令检查域名前缀*myapp.contoso*与
nslookup myapp.contoso.trafficmanager.net 典型的结果将显示如下︰
- DNS 名称，访问来解析此流量管理器的域名的 DNS 服务器的 IP 地址。
- 通信管理器输入的域名您在命令行后"nslookup"和流量管理器的域将解析的 IP 地址。 第二个 IP 地址是重要的一项检查。 它应与云服务或网站正在测试的流量管理器配置文件中的一个公用的虚拟 IP (VIP) 地址相匹配。

## 测试的负载平衡方法


### 若要测试故障转移负载平衡方法

1 保留上的所有终结点。
2-使用一个客户端。
3 请求 DNS 解析，为您公司的域名使用 Nslookup.exe 工具或类似的实用程序的。
4-确保您获得适用于您主要的终结点 5 解析的 IP 地址-降低主终结点或删除监视文件使该通讯经理认为其已关闭。
6-等待 DNS 的生存时间 (TTL) 的流量管理器配置文件，再加上额外的 2 分钟。 例如，如果您的 DNS TTL 是 300 秒 （5 分钟），您必须等待 7 分钟。
7 刷新您 DNS 客户端缓存和请求 DNS 解析。 在 Windows 中，您可以使用 ipconfig /flushdns 命令发出命令或 Windows PowerShell 提示刷新 DNS 缓存。
8-确保您获得的 IP 地址是您辅助的终结点。
9 重复此过程中，使停机辅助端点，然后第三级，等等。 每次时，请确保 DNS 解析，返回列表中的下一步的终结点的 IP 地址。 关闭所有终结点时，您应该再次获得主要的终结点的 IP 地址。

### 若要测试的循环负载平衡方法

1 保留上的所有终结点。
2-使用一个客户端。
3 请求 DNS 解析，为您的公司域使用 Nslookup.exe 工具或类似的实用程序的。
4-确保您获得的 IP 地址列表中的其中一个。
您的 DNS 客户端缓存和重复步骤 3 和 4 以及 5-刷新。 您会看到不同的 IP 地址为每个您终结点返回。 然后，将重复此过程。

### 若要测试的性能负载平衡方法

若要有效地测试的性能负载平衡方法，您必须具有客户端位于世界的不同地方。 您可以在 Azure 将尝试调用通过您公司的域名服务创建客户端。 或者，如果您的公司是全局的您可以远程登录到客户端在世界上、 从这些客户端测试的其他部分。

有免费的基于 web 的 DNS 查找并发掘可用的服务。 其中一些使您能够检查 DNS 名称解析，从不同的位置。 请不要在"DNS 查找"搜索的示例。 另一个选项是使用 Gomez 或主题演讲等第三方解决方案像预期的那样，将您的配置文件分发通讯。

## 请参见

[有关通信量管理器通信路由方法](../about-traffic-manager-balancing-methods.md)
[通信管理器](../traffic-manager.md)
 