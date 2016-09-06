---
ms.openlocfilehash: b8ffcc79006cf1e4d248d2c62468b34d4d34dc05
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="管理基础结构容量"
   description="了解如何使用在运营的见解的容量规划解决方案可帮助您了解您的服务器基础架构的容量"
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

# 管理基础结构容量

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

可以使用 Microsoft Azure 运营见解中的容量规划解决方案可帮助您了解您的服务器基础架构的容量。 安装更新的操作管理器代理和运营的见解中的基本配置模块的解决方案。 该解决方案被监视的服务器上的性能计数器中读取并处理向云中的运营洞察力服务发送使用情况数据。 使用情况数据，应用逻辑和云服务记录的数据。 一段时间，使用模式识别和预测，基于对当前消耗容量。

例如，投影可以确定当将单个服务器的需要附加处理器内核或额外的内存。 在此示例中，投影可能表明在 30 天内完成服务器将需要额外的内存。 这可以帮助您在服务器的下一个维护窗口，该窗口可能会发生一次每两个星期期间内存升级规划。

## 容量管理仪表板

可以使用 Microsoft Azure 运营见解中的容量管理仪表板之前，您必须安装该解决方案。 若要阅读更多关于安装解决方案，请参阅[使用解决方案库中添加或删除解决方案](operational-insights-setup-workspace.md)。 安装的容量规划解决方案后，您可以查看监视服务器的容量运营见解中**概述**页面上使用的**容量规划**的拼贴。

![容量规划的平铺的图像](./media/operational-insights-capacity/overview-cap-plan.png)

平铺打开在其中您可以查看服务器容量的摘要的**容量管理**仪表板。 页显示您可以单击以下拼贴︰

- *虚拟机计数*︰ 演示的虚拟机的产能剩余天数

- *计算*︰ 显示了处理器内核和可用内存

- *存储*︰ 显示使用的磁盘空间以及平均磁盘滞后时间

- *搜索*︰ 运营洞察力系统中使用任何数据搜索到数据资源管理器

>[AZURE.NOTE] 若要查看容量管理数据，您必须启用运营经理连接使用 Virtual Machine Manager (VMM)。 有关将系统连接的其他信息，请参阅如何连接 VMM 与运营经理。

![容量管理仪表板的图像](./media/operational-insights-capacity/gallery-capacity-01.png)

### 若要查看容量页

- 在**概述**页中，单击**容量管理**，然后单击**计算**或**存储**。

## 计算页

可以使用 Microsoft Azure 运营洞察力在**计算**仪表板查看容量信息利用率，投影的容量，天和与您基础结构的效率。 **利用**区域用于查看虚拟机主机 CPU 核心和内存利用率。 投影工具可以用于估计多少容量应适用于给定的日期范围。 可以使用**效率**区域以查看虚拟机主机的效率。 可以通过单击它们来查看有关链接的项的详细信息。

您可以生成 Excel 工作簿为以下类别︰

- 顶部最高核心利用率主机

- 具有最高内存使用率的顶部主机

- 顶部与效率低下的虚拟机的主机

- 按使用率上主机

- 按使用率下主机

![容量管理计算页面的图像](./media/operational-insights-capacity/gallery-capacity-02.png)


在**计算**操控板上显示了以下几个方面︰

**利用率**︰ 查看 CPU 核心和内存利用率，在您的虚拟机的主机。

- *使用内核*︰ 所有主机 （%的 CPU 利用率的物理主机上的内核数倍) 的总和。

- *自由的核心*︰ 总减使用内核的物理核心。

- *核可的百分比*︰ 释放物理除以总次数的物理核心的核心。

- *每个虚拟机的虚拟内核*︰ 总虚拟核心在系统中除系统中的虚拟机总数量。

- *虚拟到物理的内核比核心*︰ 对系统中所使用的虚拟机的物理核心总物理核心的比率。

- *虚拟内核可数*︰ 虚拟核心与物理核心的比率乘以可用物理内核。

- *使用内存*︰ 利用所有主机的内存的总和。

- *可用内存*︰ 负使用的内存的物理内存总量。

- *内存可用的百分比*︰ 释放除以总物理内存物理内存。

- *每个虚拟机的虚拟内存*︰ 除以系统中的虚拟机总数量在系统中的虚拟内存总量。

- *虚拟内存到物理内存的比率*︰ 在系统中的虚拟内存总量除以总物理内存的系统中。

- *可用虚拟内存*︰ 虚拟内存到物理内存的比率乘以可用的物理内存。

**投影工具**

通过使用投影工具，您可以查看资源利用率历史趋势。 这包括虚拟机、 内存、 核心以及存储的使用情况趋势。 投影功能使用投影算法来帮助您了解当您即将用完每个资源。 这可以帮助您计算适当的容量规划，以便您可以知道何时需要购买更多的容量 （如内存、 核心或存储）。

**效率**

- *空闲的 VM*︰ 指定的时间段内使用的 CPU 和 10%的内存少于 10%。

- *负载过重的 VM*︰ 指定的时间段内使用的 CPU 和 90%的内存的 90%以上。

- *空闲的主机*︰ 指定的时间段内使用的 CPU 和 10%的内存少于 10%。

- *负载过重的主机*︰ 指定的时间段内使用的 CPU 和 90%的内存的 90%以上。

### 若要计算页面上使用的项目

1. 在**计算**仪表板，**利用**区域中，查看 CPU 内核和正在使用的内存的容量信息。

2. 单击**搜索**页面中将其打开并查看详细的信息的项目。

3. 在**投影**工具移动日期滑块，以显示将在您选择的日期使用的产能的投影。

3. 在**效率**区域中，查看有关虚拟机和虚拟机主机的容量效率信息。

##直接连接存储页

可以使用 Microsoft Azure 运营洞察力在**直连存储**仪表板查看容量信息存储利用率、 磁盘性能和磁盘容量的预计的天数。 **利用**区域用于在虚拟机主机查看磁盘空间使用情况。 您可以使用**磁盘性能**区域查看虚拟机主机的磁盘吞吐量和滞后时间。 投影工具还可用于估计多少容量应适用于给定的日期范围。 可以通过单击它们来查看有关链接的项的详细信息。

您可以从以下几个类别此容量信息生成 Excel 工作簿︰

- 通过主机的顶部的磁盘空间使用情况

- 由主机顶部的平均延迟

在**存储**页上显示了以下几个方面︰

- *利用率*︰ 虚拟机主机中查看磁盘空间使用情况。

- *磁盘空间总量*︰ 所有主机的总和 （逻辑磁盘空间）

- *使用的磁盘空间*︰ 所有主机的总和 （使用逻辑磁盘空间）

- *可用磁盘空间*︰ 总共减用的磁盘空间磁盘空间

- *磁盘使用百分比*︰ 已用磁盘空间除以磁盘空间总量

- *百分比磁盘可用*︰ 可用磁盘空间除以磁盘空间总量

![容量管理直连存储页面的图像](./media/operational-insights-capacity/gallery-capacity-03.png)

**磁盘性能**

通过使用操作的见解，可以查看磁盘空间使用率趋势。 投影功能使用到项目将来使用的算法。 对于空间的使用，尤其是投影功能使您能够项目时您可能会用尽磁盘空间。 这将帮助您规划正确存储并知道何时需要购买更多存储空间。

**投影工具**

通过使用投影工具，您可以查看磁盘空间利用率历史趋势。 投影功能还允许您项目时磁盘空间不足。 这将帮助您规划适当的容量，并且知道何时需要购买更多存储容量。

### 若要使用直连存储页上的项目

1. 在**直连存储**仪表板，在**利用**区域中，可以查看磁盘使用率信息。

2. 单击**搜索**页面中将其打开并查看详细的信息链接的项目。

3. 在**磁盘性能**区域中，可以查看磁盘吞吐量和滞后时间信息。

4. **投影工具**中, 移动日期滑块，以显示将在您选择的日期使用的产能的投影。

[AZURE.INCLUDE [operational-insights-export](../../includes/operational-insights-export.md)]