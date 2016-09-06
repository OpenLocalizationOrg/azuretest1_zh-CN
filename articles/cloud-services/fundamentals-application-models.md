---
ms.openlocfilehash: a2726642b9f63a9259cbc137d5e02079ddf2abf2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="计算由 Azure 的宿主选项" 
    description="了解有关 Azure 计算承载选项和它们的工作方式︰ 虚拟机、 网站、 云服务，和其他人。" 
    headerExpose="" 
    footerExpose="" 
    services="cloud-services,virtual-machines"
    authors="Thraka" 
    documentationCenter=""
    manager="timlt"/>

<tags 
    ms.service="multiple" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/16/2015" 
    ms.author="adegeo;cephalin;kathydav"/>




# 计算由 Azure 的宿主选项

Azure 用于运行应用程序提供不同的宿主模型。 每个提供一组不同的服务，并因此选择哪一个取决于确切什么要做。 这篇文章关注的选项，描述每一种技术，并为提供时将使用的示例。

| 计算选项    | 受众   |
| ------------------ | --------   |
| [应用程序服务]      | 可伸缩的 Web 应用程序、 移动应用程序、 API 的应用程序和任何设备的逻辑应用程序 |
| [云服务]   | 高度可用、 可伸缩的 n 层云应用程序的操作系统的更多控制 |
| [虚拟机] | 自定义的 Windows 和 Linux，完全控制操作系统的虚拟机 |

[AZURE.INCLUDE [content](../../includes/app-service-choose-me-content.md)]

[AZURE.INCLUDE [content](../../includes/cloud-services-choose-me-content.md)]

[AZURE.INCLUDE [content](../../includes/virtual-machines-choose-me-content.md)]

## 其他选项

Azure 还提供其他计算托管模型更加专业化的目的，如下所示︰

* [移动服务](/services/mobile-services/)  
  优化用于在移动设备运行的应用程序提供一个后端的云。
* [批处理](/services/batch/)  
  适合处理大量的相似的任务，理论上工作负载，这同样适用于多台计算机上运行尽可能并行任务。
* [HDInsight (Hadoop)](/services/hdinsight/)  
  在 Hadoop 群集上运行[MapReduce](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options/#hadoop)作业进行了优化。 

## 我应该使用什么？ 做出选择

所有三个通用的 Azure 计算承载模型使您可以都构建可伸缩的、 可靠的应用程序在云中。 给出这些基本类似，应该使用哪一个您？

应用程序服务是大多数 web 应用程序的最佳选择。 部署和管理集成到其平台、 站点可以快速扩展，以处理高流量负荷和内置负载平衡和通信管理器提供高可用性。 可以使用[联机迁移工具](https://www.migratetoazure.net/)很容易地为应用程序服务移动现有网站，从 Web 应用程序库中，使用一个开源的应用程序或创建新的网站使用您选择的工具和框架。 [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226)功能容易地添加到您的应用程序处理的后台作业，或甚至可以运行，根本不是一个 web 应用程序的计算工作负荷。 

如果您更需要对 web 服务器环境中，例如为您的服务器能够远程控制或配置服务器启动任务，Azure 云服务通常是最佳的选择。

如果您有现有的应用程序需要实质性的修改，以在 Azure 网站或 Azure 云服务运行，您可以选择 Azure 虚拟机来简化迁移到云。 但是，正确地配置、 保护和维护虚拟机需要更多时间和 IT 专业技能与 Azure 网站和云服务。 如果您正在考虑使用 Azure 的虚拟机，请确保您考虑到需修补程序、 更新和管理您的虚拟机环境的日常维护工作。

有时，没有任何单个选项是正确的。 在这样的情况下，是完全合法的合并选项。 例如，假设您正在构建一个应用程序到您喜欢的云服务 web 角色管理的好处，但您还需要使用标准的 SQL Server 驻留在虚拟机的兼容性或性能方面的原因。 

<!-- In this case, the best option is to combine compute hosting options, as the figure below shows.--

<a name="fig4"></a>
![07_CombineTechnologies][07_CombineTechnologies] 
 
**Figure: A single application can use multiple hosting options.**

As the figure illustrates, the Cloud Services VMs run in a separate cloud service from the Virtual Machines VMs. Still, the two can communicate quite efficiently, so building an app this way is sometimes the best choice.
[07_CombineTechnologies]: ./media/fundamentals-application-models/ExecModels_07_CombineTechnologies.png
!-->

[应用程序服务]: #tellmeas
[虚拟机]: #tellmevm
[云服务]: #tellmecs

## 下一步行动

* [比较](../choose-web-site-cloud-service-vm/)应用程序服务、 云服务和虚拟机
* 了解有关[应用程序服务](../app-service-web-overview.md)
* 了解更多关于[云服务](services/cloud-services/)
* 了解有关[虚拟机](https://msdn.microsoft.com/library/azure/jj156143.aspx) 

测试
