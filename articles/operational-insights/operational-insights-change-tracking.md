---
ms.openlocfilehash: c7b0bc0f902a9d6a1d8e26ae4476455eaeae53ce
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="确定与更改跟踪的差异"
   description="使用 Microsoft Azure 运营见解中配置更改跟踪解决方案帮助您轻易识别软件和 Windows 服务在您环境中发生的更改"
   services="operational-insights"
   documentationCenter=""
   authors="bandersmsft"
   manager="jwhit"
   editor="" />
<tags
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/05/2015"
   ms.author="banders" />

# 确定与更改跟踪的差异

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

可用于配置更改跟踪解决方案在 Microsoft Azure 的运营洞察力帮助您轻易识别软件和 Windows 服务在您环境中发生的更改 — — 标识这些配置更改可帮助您精确定位操作问题。

安装解决方案以更新操作管理器代理和基本配置模块操作的见解。 更改安装软件和读取被监视的服务器上的 Windows 服务，然后将数据发送到云中的运营洞察力服务进行处理。 逻辑应用于接收到的数据，云服务记录的数据。 当发现更改时，更改与服务器所示更改跟踪仪表板。 通过使用更改跟踪操控板上的信息，您可以轻松查看服务器基础结构中所做的更改。

## 使用跟踪修订信息

您可以使用更改跟踪中运营的见解之前，您必须安装该解决方案。 若要阅读更多关于安装解决方案，请参阅[使用解决方案库中添加或删除解决方案](operational-insights-setup-workspace.md)。

完成安装后，您可以查看更改的摘要被监视的服务器运营见解中**概述**页面上使用**更改跟踪**平铺。

![更改跟踪平铺的图像](./media/operational-insights-change-tracking/overview-change-track.png)

您可以查看对您基础结构，然后为以下类别的钻入详细信息的更改︰

- 通过对软件和 Windows 服务的配置类型的更改

- 对应用程序和不同服务器的更新的软件更改

- 每个应用程序的软件更改的总次数

- 对于单个服务器的 Windows 服务更改

![图像跟踪更改仪表板的](./media/operational-insights-change-tracking/gallery-changetracking-01.png)
![跟踪更改仪表板的图像](./media/operational-insights-change-tracking/gallery-changetracking-02.png)

### 要查看任何更改，将更改类型

1. 在**概述**页上单击**修订**拼贴。

2. **更改跟踪**的仪表板，在审查某个更改类型刀片式服务器中的摘要信息，然后单击其中一个**日志搜索**页中查看有关它的详细的信息。

3. 上的任何日志搜索页面，可以按时间、 详细的结果和日志搜索历史记录查看结果。 您还可以筛选通过缩小结果范围的方面。


[AZURE.INCLUDE [operational-insights-export](../../includes/operational-insights-export.md)]
