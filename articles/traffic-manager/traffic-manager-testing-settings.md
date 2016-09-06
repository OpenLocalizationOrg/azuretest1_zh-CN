---
ms.openlocfilehash: 6b478b2d986e886b95a1a2d04c3c264aa781b5e8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="测试通信管理器设置 |Microsoft Azure"
   description="这篇文章将帮助您测试流量管理器设置"
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

# 测试通信管理器设置

测试通信管理器设置的最佳办法是设置的客户端数，然后将端点，云服务和网站，在下一次配置文件组成。 以下提示将帮助您测试您的流量管理器配置文件。

## 基本测试步骤

- **设置 DNS TTL 非常低**，以便更改将传播快速的 30 秒，例如。
- 在配置文件中**知道您的 Azure 的云服务和网站的 IP 地址**进行测试。
- **使用的工具，使您可以解析为 IP 地址的 DNS 名称**和地址的显示。 要检查以查看您公司的域名解析为 IP 地址的配置文件中的终结点。 他们应该流量管理器配置文件使用通信路由方法一致的方式解决冲突。 如果要在运行 Windows 的计算机上，您可以使用命令或 Windows PowerShell 提示 Nslookup.exe 工具。 其他公开提供的工具，可用于"挖"的 IP 地址也是在 Internet 上随时可用。

### 若要检查使用 nslookup 一个流量管理器配置文件

1. 以管理员身份打开命令或 Windows PowerShell 提示。
2. 类型`ipconfig /flushdns`刷新 DNS 解析器缓存。
3. 类型`nslookup <your Traffic Manager domain name>`。 例如，以下命令检查与前缀*myapp.contoso*的域名︰ nslookup myapp.contoso.trafficmanager.net 典型的结果将显示如下︰
   - DNS 名称，访问来解析此流量管理器的域名的 DNS 服务器的 IP 地址。
   - 通信管理器输入的域名您在命令行后"nslookup"和流量管理器的域将解析的 IP 地址。 第二个 IP 地址是重要的一项检查。 它应与云服务或网站正在测试的流量管理器配置文件中的一个公用的虚拟 IP (VIP) 地址相匹配。

## 测试通信路由方法

### 若要测试故障转移通信路由方法

1. 保持向上的所有终结点。
2. 使用一个客户端。
3. 请求使用 Nslookup.exe 工具或类似的应用工具公司域名的 DNS 解析。
4. 确保您获得适用于您主要的终结点解析的 IP 地址
5. 将您的主终结点向下的或删除监视文件，以使该通讯经理认为其已关闭。
6. 等待 DNS 的生存时间 (TTL) 的流量管理器配置文件，再加上额外的 2 分钟。 例如，如果您的 DNS TTL 是 300 秒 （5 分钟），您必须等待 7 分钟。
7. 将刷新您 DNS 客户端缓存和请求 DNS 解析。 在 Windows 中，您可以使用 ipconfig /flushdns 命令发出命令或 Windows PowerShell 提示刷新 DNS 缓存。
8. 确保您获得的 IP 地址是您辅助的终结点。
9. 重复该过程，使停机辅助端点，然后第三级，等等。 每次时，请确保 DNS 解析，返回列表中的下一步的终结点的 IP 地址。 关闭所有终结点时，您应该再次获得主要的终结点的 IP 地址。

### 若要测试循环复用通信路由方法

1. 保持向上的所有终结点。
2. 使用一个客户端。
3. 请求使用 Nslookup.exe 工具或类似的应用工具您公司域的 DNS 解析。
4. 确保您获得的 IP 地址列表中的其中一个。
5. 将刷新 DNS 客户端高速缓存和反复重复步骤 3 和 4。 您会看到不同的 IP 地址为每个您终结点返回。 然后，将重复此过程。

### 若要测试性能通信路由方法

若要有效地测试性能通信路由方法，您必须具有客户端位于世界的不同地方。 您可以在 Azure 将尝试调用通过您公司的域名服务创建客户端。 或者，如果您的公司是全局的您可以远程登录到客户端在世界上、 从这些客户端测试的其他部分。

有免费的基于 web 的 DNS 查找并发掘可用的服务。 其中一些使您能够检查 DNS 名称解析，从不同的位置。 请不要在"DNS 查找"搜索的示例。 另一个选项是使用 Gomez 或主题演讲等第三方解决方案像预期的那样，将您的配置文件分发通讯。

## 请参见

[有关通信量管理器通信路由方法](traffic-manager-load-balancing-methods.md)

[通信管理器-禁用，请启用或删除配置文件](disable-enable-or-delete-a-profile.md)

[通信管理器-禁用或启用端点](disable-or-enable-an-endpoint.md)

[通信管理器是什么？](traffic-manager-overview.md)

[云服务](http://go.microsoft.com/fwlink/p/?LinkId=314074)

[网站](http://go.microsoft.com/fwlink/p/?LinkId=393327)

[操作的流量管理器 （REST API 参考）](http://go.microsoft.com/fwlink/?LinkId=313584)

 