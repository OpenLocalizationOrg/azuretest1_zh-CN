---
ms.openlocfilehash: 3e7d743b631309535aff253742793177e4a5f356
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="性能注意事项 Azure 流量管理器的 |Microsoft Azure"
   description="了解有关通信管理器以及如何测试您的网站的性能，使用通信管理器时的性能"
   services="traffic-manager"
   documentationCenter=""
   authors="kwill-MSFT"
   manager="adinah"
   editor="joaoma" />

<tags 
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/19/2015"
   ms.author="joaoma" />


# 性能注意事项的流量管理器


关于 Azure 流量管理器的常见问题处理它可能会导致潜在的性能问题。  这些问题通常是沿行"多少延迟将流量管理器添加到我的网站？"，"我监视网站说昨天--几个小时我的网站太慢当时已存在的任何通信管理器问题？"，"哪里有通信管理服务器？ 我想要确保它们在我的网站所在的数据中心，因此性能不会受到影响"。

本页将讨论通信管理器可以对网站造成的直接的性能影响。  如果您东亚美国失败的流量管理器的探测器已经在东部美国网站，另一个在亚洲，然后您的所有用户将被定向到亚洲网站，您将会看到性能的影响，但这种性能影响没有使用通信管理器本身。

  

## 重要说明有关流量管理器的工作原理

[通讯管理器概述](traffic-manager-overview.md)是一个绝佳的资源，若要了解流量管理器的工作原理，但没有在该页面上的信息很多并且很难挑选出与性能相关的关键信息。  若要查看 MSDN 文档中的要点是从图像 3，我来这里更详细地解释一下步骤 #5 和 #6:

- 通信管理器实质上仅执行一件事--DNS 解析。  这意味着仅对性能的影响，流量管理器可以在您的网站上是初始的 DNS 查找。
- 通信管理器 DNS 查找有关要求澄清的一个点。  通信管理器填充，并定期更新，普通的 Microsoft DNS 根服务器基于您的策略和探测结果。  因此即使在初始 DNS 查找过程中没有参与由通信管理器由于 DNS 请求由普通的 Microsoft DNS 根服务器。  如果流量管理器出现 '故障' (ie。 执行策略探测和 DNS 更新虚拟机的故障) 然后将流量管理器 DNS 名称不影响由于仍会保留在 Microsoft DNS 服务器条目 — — 唯一的影响将是，探测并基于策略更新将不会发生 (ie。 如果主站点出现故障通信管理器将不能更新 DNS 指向故障切换站点)。
- 通信不会环绕通过流量管理器。  有没有流量管理器作为一个中间男人之间您的客户和您 Azure 服务器托管服务。  一旦完成 DNS 查找然后流量管理器从客户端和服务器之间的通信完全删除。
- DNS 查找速度是非常快，并缓存。  初始的 DNS 查询将取决于客户端，客户端通常可通过其配置的 DNS 服务器可以执行 DNS 查找 ~ 50 毫秒 （见 http://www.solvedns.com/dns-comparison/）。  第一个查找操作完成后将被结果缓存的 DNS TTL，即流量管理器的默认值为 300 秒。
- 选择性能、 故障转移 （循环） 的流量管理器策略有 DNS 性能没有影响。  性能策略可以对您的用户体验，例如，如果向美国用户发送到服务破坏亚洲，但不引起由流量管理器的这种性能问题产生负面影响。

  

## 测试通信管理器性能

有几个可用于确定您的流量管理器的性能和行为的公开网站。  这些网站是用于确定 DNS 延迟和它在世界各地用户定向到您托管服务。  请记住，大部分这些工具执行的操作不缓存 DNS 结果因此多次运行的测试将显示完整的 DNS 查找中，客户端连接到您的流量管理器终结点时才会看到完整的 DNS 查找性能而影响 TTL 持续时间内一次。


## 示例工具来测量性能


一个最简单的工具就是 WebSitePulse。  输入的 URL，您将看到如 DNS 解析时间，第一个字节，最后一个字节和其他性能统计数据的统计信息。  您可以选择从三个不同的位置来测试您的站点。  在本例中，您将看到第一次执行显示的第一个 DNS 查找时间计 0.204 秒。  第二次我们在同一个流量管理器终结点运行此测试结果已经被缓存后，DNS 查找时间采用 0.002 秒。

http://www.websitepulse.com/help/tools.php


![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

DNS 缓存时的时间︰


![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)



另一个真正有用的工具，同时从多个地理区域获取 DNS 解析时间是 Watchmouse 的检查网站的工具。  输入的 URL，您将看到 DNS 解析时间、 连接时间和从多个地理位置的速度。  这也是很方便测试以查看发送到世界各地不同用户的托管的服务的流量管理器的性能策略。

http://www.watchmouse.com/en/checkit.php


![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

http://tools.pingdom.com/ – 这将测试网站并在页上可视关系图上的每个元素提供性能统计信息。  如果您切换到页分析选项卡可以查看所占的时间百分比花费执行 DNS 查找。

 

http://www.whatsmydns.net/ – 该网站将从 20 个不同的地理位置执行 DNS 查找并将结果显示在地图上。  这是很好的可视表示形式来帮助确定您的客户端将连接到的托管的服务。

 

http://www.digwebinterface.com – 类似的 watchmouse 网站，但这个演示等 Cname 记录的 DNS 信息的更详细。  确保检查彩色化输出和统计数据下的选项，然后在名称服务器下选择全部。

## 结论

给出了我们知道流量管理器将对网站的唯一的性能影响，是第一个 DNS 查找 （时间不同，但平均 ~ 50 毫秒），上述信息和工期的 DNS TTL （300 秒默认值），然后 0 的性能影响，然后再次刷新 DNS 缓存 TTL 到期后。  因此问题的答案"多少延迟将流量管理器添加到我的网站？" 从根本上讲，是零。


## 下一步行动


[有关通信量管理器通信路由方法](traffic-manager-load-balancing-methods.md)

[通信管理器是什么？](../traffic-manmager-overview.md)

[云服务](http://go.microsoft.com/fwlink/?LinkId=314074)

[网站](http://go.microsoft.com/fwlink/p/?LinkId=393327)

[操作上流量管理器 （REST API 参考）](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure 的流量管理器 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=400769)
 
