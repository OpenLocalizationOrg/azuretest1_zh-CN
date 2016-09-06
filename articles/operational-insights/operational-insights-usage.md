---
ms.openlocfilehash: 3ecbc5b0322823a5876226c436a6c492d931d246
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="管理服务器和使用数据"
   description="了解有关多少数据被发送到操作的见解服务从您的服务器"
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

# 管理服务器和使用数据

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

操作的见解收集数据，并定期将其发送到操作的见解服务。  **使用**仪表板可用于查看到运营洞察力服务正在发送多少数据。 **使用**仪表板也显示您多少数据发送每日的解决方案和管理组频率发送数据。

>[AZURE.NOTE] 如果您有免费的帐户，你每天发送到操作的见解服务 500 MB 的数据有限。 如果达到每日的限制，数据分析将停止，并在第二天开始恢复。

运营的见解中**概述**仪表板上使用的**服务器和使用**拼贴，可以查看您的使用情况。

![服务器和使用图像平铺](./media/operational-insights-usage/overview-servers-usage.png)

如果您已超出了每日的使用限制，或如果您是接近大小限制，您可以选择删除一个解决方案来减少向运营洞察力服务发送的数据量。 有关删除的解决方案的详细信息，请参阅[使用解决方案库中添加或删除解决方案](operational-insights-setup-workspace.md)。

如果运营经理管理组时遇到问题，将数据发送到操作的见解服务，可以解决此问题，或者您可以删除组操作的见解，如果需要。

![图像的使用情况仪表板](./media/operational-insights-usage/usage-dash.png)

**使用**操控板中显示下列信息︰

- 每个工作日的平均使用率

- 今天的每个解决方案的数据使用

- 频率每个管理组中的服务器会将数据发送到操作的见解服务

## 若要使用使用率数据

1. 在**概述**页中，单击**服务器和使用**拼贴。

2. **使用**仪表板，视图显示区域您担心使用类别。

3. 如果存在一个解决方案，它不必要地使用了大量分配配额有关的数据，则可以考虑删除该解决方案。

## 若要排除或删除管理组

1. 在**概述**页中，单击**服务器和使用**拼贴。

2. **使用**仪表板，在查看未发送的数据的管理组的信息。

3. 如果不发送数据的管理组，您可以单击**疑难解答**以获取详细的疑难解答信息。 如果不再想保留管理组并将所有的代理到该报表，单击**删除**。

[AZURE.INCLUDE [operational-insights-export](../../includes/operational-insights-export.md)]
