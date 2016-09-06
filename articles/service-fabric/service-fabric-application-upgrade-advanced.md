---
ms.openlocfilehash: 684dd31d94111369037c48e6a87e899c93f0b8c1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="我们的服务结构应用程序升级︰ 高级的主题"
   description="本文介绍了一些高级的主题，属于服务结构应用程序升级。"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="samgeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/17/2015"
   ms.author="subramar"/>

# 我们的服务结构应用程序升级︰ 高级的主题

## 手动升级模式

> [AZURE.NOTE] 只为了失败或暂停升级，监控手动模式甚至认为。 受监视模式是建议为服务结构应用的升级模式。

服务结构提供支持开发和生产群集的多个升级模式。 每个部署的选项非常适合于不同的环境。 监视部署应用程序升级是最典型的升级，在生产中使用。 指定升级策略时，服务结构可确保应用程序正常之前升级的进行。 在某些情况下，在那里多个自定义或复杂的健康评估策略是必需的或非常规升级 （应用程序已经在数据丢失或等），应用程序管理员可以使用手动回滚应用程序升级模式能够通过各种升级域升级的进度的总控制。 最后，自动化回滚应用程序升级可用于开发或测试环境提供服务开发过程中的快速迭代周期。

**手动**-停止在当前 UD 应用程序升级和升级模式更改为手动监控。 管理员需要手动调用**MoveNextApplicationUpgradeDomainAsync**来进行升级或触发回滚通过启动新的升级。 一旦升级进入到手动模式，将一直留在手动模式中，直到开始新的升级。 **GetApplicationUpgradeProgressAsync**命令会返回结构\_应用程序\_升级\_状态\_滚动\_向前\_待决。

## 升级与比较文件包

可以通过提供完整的、 独立的应用程序软件包升级服务结构应用程序。 应用程序通过使用比较包中只包含更新的应用程序文件，还可以升级和更新应用程序清单和服务清单文件。

完整的应用程序包包含启动和运行服务结构的应用程序所需的所有文件。 比较包仅包含文件更改之间最后一次的规定和当前的升级，加上完整的应用程序清单和服务清单文件。 应用程序清单或服务清单，其中服务结构中找不到生成的布局中，ImageStore （链接待发布） 中的引用将搜索服务结构中的任何引用。

完整的应用程序包所需的第一个安装到群集的应用程序。 后续更新可以是完整的应用程序包或比较包。

当使用 diff 包是一个不错的选择的情况下︰

* 大型应用程序软件包引用几个服务清单文件和/或多个代码包、 配置包或数据包后，比较包是首选。

* 在已部署系统，它直接从应用程序生成过程产生的生成布局时，比较包是首选。 在这种情况下，即使未在代码中进行任何更改，新生成的程序集将具有不同的校验和。 使用完整的应用程序包要求您将更新所有的代码包的版本。 使用比较包，您只能提供更改的文件和清单文件的版本发生更改的位置。

## 下一步行动

[升级的教程](service-fabric-application-upgrade-tutorial.md)

[升级参数](service-fabric-application-upgrade-parameters.md)

[数据序列化](service-fabric-application-upgrade-data-serialization.md)

[应用程序升级的疑难解答](service-fabric-application-upgrade-troubleshooting.md)
 
