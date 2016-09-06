---
ms.openlocfilehash: b8ef0d9a383af229d92b38ff6680f276676cb3df
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="用系统更新更新服务器"
   description="了解如何使用系统更新解决方案在 Microsoft Azure 运营洞察力来帮助您对您的基础架构中的服务器应用缺少的更新"
   services="operational-insights"
   documentationCenter=""
   authors="bandersmsft"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/05/2015"
   ms.author="banders" />

# 用系统更新更新服务器

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

可以使用 Microsoft Azure 运营洞察力在系统更新解决方案可帮助您对您的基础架构中的服务器应用缺少的更新。 安装解决方案以更新操作管理器代理和基本配置模块操作的见解。 被监视的服务器上读取更新的信息，然后更新数据发送到云中的运营洞察力服务进行处理。 逻辑应用于更新数据和云服务记录数据。 如果发现缺少的更新时，**更新**仪表板上显示它们。 **更新**仪表板可用于处理缺少的更新和制定计划，将其应用到需要它们的服务器。

## 使用系统更新来更新服务器

您可以在 Microsoft Azure 运营见解使用系统更新之前，您必须安装该解决方案。 若要阅读更多关于安装解决方案，请参阅[使用解决方案库中添加或删除解决方案](operational-insights-setup-workspace.md)。 完成安装后，您可以查看运营见解中**概述**页面上使用**系统更新评估**平铺被监视的服务器中缺少的更新。

### 若要使用更新

1. 在**概述**页中，单击**系统更新评估**拼贴。
![概述页的图像](./media/operational-insights-updates/updates01.png)
2. 在**更新**仪表板，来查看更新类别。
![更新页面的图像](./media/operational-insights-updates/updates02.png)
3. 滚动到要查看**缺少的更新类型**刀片式服务器，然后单击**安全更新**的页的右侧。
![更新页面的图像](./media/operational-insights-updates/updates03.png)
4. 在搜索页显示您的基础架构中未找到服务器中缺少的安全更新的列表。 单击一篇知识库文章 ID (KBID) 以查看有关缺少的更新的详细信息。 在此示例中， *KBID 3032323*。
![更新页面的图像](./media/operational-insights-updates/updates04.png)
5. Web 浏览器将打开知识库文章中介绍的更新。
![更新页面的图像](./media/operational-insights-updates/updates05.png)
6. 使用使用所找到的信息，您可以创建一个应用缺少的更新的计划。

[AZURE.INCLUDE [operational-insights-export](../../includes/operational-insights-export.md)]
