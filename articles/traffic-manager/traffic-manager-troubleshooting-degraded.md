---
ms.openlocfilehash: d6f35ea477299b1a111b209f0a524f87c0f77eb3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="故障诊断已降级状态在 Azure 流量管理器"
   description="如何解决通信管理器配置文件，当它显示为已降级状态。"
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

# 故障诊断已降级状态在 Azure 流量管理器
此页将描述如何解决 Azure 流量管理器配置文件显示下降的状态，并提供一些关键点，若要了解有关通信管理器探测。


已配置通信量管理器配置文件指向的一些您。 cloudapp.net 托管服务，几秒钟后查看状态性能下降。

![degradedstate](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degraded.png)

如果您转到该配置文件的终结点选项卡上，您将看到一个或多个终结点，在处于脱机状态︰

![脱机](./media/traffic-manager-troubleshooting-degraded/traffic-manager-offline.png)

## 有关通信量管理器探测的重要说明

- 通信管理器仅考虑终结点作为联机如果探测器获取 200 步探测路径中的日志。
- 30 倍重定向 （或任何其他非 200 响应） 将失败，即使重定向的 URL 返回 200。

- 对于 HTTPs 探测器，会出现证书错误将被忽略。
 
- 探测路径的实际内容并不重要，只要返回 200。  如果实际网站内容不返回 200 的常用技术 (ie。 如果 ASP 页重定向到 ACS 登录页面或某些其他 CNAME URL) 是将路径设置为类似于"/ favicon.ico"。
 
- 最佳做法是将探测路径设置为这种具有足够的逻辑，以确定该网站是否向上或向下。  在上面的示例中将路径设置为"/ favicon.ico"是只测试如果 w3wp.exe 做出反应，但如果您的网站是否正常运行。  更好的选择就是如一个路径设置为"/ Probe.aspx"，并在 Probe.aspx 中包含足够的逻辑来确定您的站点是否正常 (ie。 检查性能计数器，以确保您不是 100 %cpu 在或接收大量失败的请求，工作尝试访问的资源，例如数据库或会话状态，以确保应用程序的逻辑等等)。
 
- 如果在配置文件中的所有终结点已降级然后流量管理器将处理所有终结点作为健康和通信路由到所有终结点。  这是服务的为了确保任何潜在的问题，这会导致不正确的故障探测的探测机制不会导致您完全中断。

  

## 疑难解答

通信管理器探测故障诊断的一种工具是 wget。  您可以从[wget](http://gnuwin32.sourceforge.net/packages/wget.htm)获取二进制文件和依赖关系的软件包。  请注意，可以使用其他程序，如 Fiddler 或卷曲代替 wget — 基本上只需要显示的原始 HTTP 响应的内容。

Wget 安装之后，请转到命令提示符，并对 URL 运行 wget + 探测器端口和流量管理器中配置的路径。  本例中是 http://watestsdp2008r2.cloudapp.net:80/探测。

![疑难解答](./media/traffic-manager-troubleshooting-degraded/traffic-manager-troubleshooting.png)

使用 Wget:

![wget](./media/traffic-manager-troubleshooting-degraded/traffic-manager-wget.png)

 

请注意，wget 指示 URL 返回 301 重定向到 http://watestsdp2008r2.cloudapp.net/Default.aspx。  我们知道从上面的"通讯管理器探测有关的重要说明"一节，30 x 重定向被认为通过流量管理器探测的失败，而这将导致脱机报告的探测器。  在这点很简单，只需检查的网站配置并确保 200 从 /Probe 路径 （或重新配置通信量管理器探测，以指向一个路径，它将返回 200）。

 

如果您探测器使用 HTTPs 协议，您需要添加"— 不检查证书"参数与 wget 这样它将忽略 cloudapp.net URL 上的证书不匹配。


## 下一步行动


[有关通信量管理器通信路由方法](traffic-manager-load-balancing-methods.md)

[通信管理器是什么](../traffic-manmager-overview.md)

[云服务](http://go.microsoft.com/fwlink/?LinkId=314074)

[网站](http://go.microsoft.com/fwlink/p/?LinkId=393327)

[操作上流量管理器 （REST API 参考）](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure 的流量管理器 Cmdlet](http://go.microsoft.com/fwlink/p/?LinkId=400769)
 
