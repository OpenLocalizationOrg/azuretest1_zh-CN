---
ms.openlocfilehash: b13817848c8d928d1491da8c1df0c33e84a195fe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Microsoft Azure 应用程序体系结构" 
    description="介绍了通用设计模式的体系结构概述" 
    services="" 
    documentationCenter="" 
    authors="Rboucher" 
    manager="jwhit" 
    editor="mattshel"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015" 
    ms.author="robb"/>

#在 Microsoft Azure 应用程序体系结构
构建使用 Microsoft Azure 应用程序的资源。 这包括帮助您绘制关系图，以直观地描述了软件系统的工具。 

##Microsoft 认证课程的体系结构

Microsoft 只发布了新的体系结构课程支持 Microsoft 认证考试 70-534。 [适用于 EDX.ORG 的免费](https://www.edx.org/course/architecting-microsoft-azure-solutions-microsoft-dev205x)的。  它使用新的[3D 蓝图 Visio 模板](#3d-blueprint-visio-template)。 

![Microsoft 结构认证课程](./media/architecture-overview/EDXCourse.png)

##Microsoft 的体系结构蓝图

Microsoft 发布了一套高级别[体系结构蓝图](http://aka.ms/azblueprints)显示如何生成特定类型的使用 Microsoft 产品的系统。 

每个蓝图包括

- 平面**2D Visio** 2003 基于文件，您可以下载和修改 
- 色彩缤纷的**3D 透视 PDF**文件引入到较少的技术群体的蓝图
- 引导完成的 3D 版本的**视频**。 

蓝图使用[云以及企业符号集](#symbol-and-icon-sets)。   

![Microsoft 的体系结构蓝图 3D 图](./media/architecture-overview/BluePrintThumb.jpg)



##3D 蓝图 Visio 模板
在一个非 Microsoft 工具，最初创建的 3D 版本的[Microsoft 体系结构蓝图](http://aka.ms/azblueprints)，但新的 Visio 2013 模板在 2015 年 8 月 5 日装运[上 EDX.ORG 分布式体系结构 Microsoft 认证课程](#microsoft-architecture-certification-course)的一部分。 

该模板还有课程之外。 

- [查看视频培训](http://aka.ms/3dBlueprintTemplateVideo)第一，因此，您知道它能做什么   
- 下载[Microsoft 3d 蓝图 Visio 模板](http://aka.ms/3DBlueprintTemplate)
- 下载的[云和企业符号](#symbol-and-icon-sets)使用 3D 的模板

通过电子邮件发送我们在[CnESymbols@microsoft.com](mailto:CnESymbols@microsoft.com)的特定问题没有回答的培训资料，或提供反馈。 可用性是因此请让我们知道什么是好，什么方式获取模板的主要目标之一。

![Microsoft Visio 模板 3D 蓝图](./media/architecture-overview/3DBlueprintVisioTemplate.jpg)



##符号和图标集 

[查看 Visio 和培训视频的符号](http://aka.ms/CnESymbolsVideo)，然后[下载云并企业符号集](http://aka.ms/CnESymbols)来帮助您创建技术资料，描述了 Azure、 Windows 服务器和 SQL Server 的详细信息。 如果简介册训练用户如何使用 Microsoft 产品，可以使用体系结构关系图、 培训资料、 演示文稿、 数据表、 infographics、 白皮书和甚至第三方书籍中的符号。 但是，它们并不适合在用户界面中使用。

CnE 符号是在 Visio 和 PNG 格式。 如何在 PowerPoint 中使用 Png 的其他说明都包含在集中。 

符号集附带每季度一次，并发布新的服务更新。 

其他符号可在[Microsoft Office Visio 模具](http://www.microsoft.com/en-us/download/details.aspx?id=35772)中，但不适于像是 CnE 的体系结构图。  


**的反馈︰**如果您使用过 CnE 符号，填写简短 5 问题[调查](http://aka.ms/azuresymbolssurveyv2)或通过电子邮件发送我们在[CnESymbols@microsoft.com](mailto:CnESymbols@microsoft.com)的具体问题和事项。 我们想要知道您的想法，包括积极的反馈，以便我们能够继续投入其中的时间。 


![云和企业符号/图标集](./media/architecture-overview/CnESymbols.png)


##天蓝色的建筑设计模式
Microsoft 发布了系列可帮助您编写自己的自定义设计的体系结构设计模式。 这些模式主要用于简洁这可以按顺序放在一起组成的体系结构指南提供的指导如何以最佳方式利用 Microsoft Azure 平台，以满足您组织的业务需要。


[概述](../azure-architectures-cpif-overview/) - 
[混合网络](../azure-architectures-cpif-infrastructure-hybrid-networking/) - 
[异地分批处理](../azure-architectures-cpif-foundation-offsite-batch-processing-tier/) -
[多站点数据层](../azure-architectures-cpif-foundation-multi-site-data-tier/) -
[全局负载平衡的 web 层](../azure-architectures-cpif-foundation-global-load-balanced-web-tier/) -
[Azure 搜索层](../azure-architectures-cpif-foundation-azure-search-tier/)
 
每个模式包含
 
- 服务说明
- 利用该模式所需的 Azure 服务列表
- 体系结构关系图
- 体系结构相关性
- 设计限制或可能会影响图案的注意事项
- 接口和终结点
- 反模式
- 关键的高级体系结构注意事项包括可用性和恢复能力、 复合使用服务的 Sla、 规模和性能、 成本和操作方面的注意事项。
1

![Azure 的体系结构的设计模式](./media/architecture-overview/AzureArchPatterns.jpg)

##设计模式海报
Microsoft 的模式和做法已经发布了书[云设计模式](http://msdn.microsoft.com/library/dn568099.aspx)也可在 MSDN 和 PDF 下载。 此外，还有大格式海报使用其中列出的所有模式。 

![模式和实践云模式海报](./media/architecture-overview/PnPPatternPosterThumb.jpg)

##体系结构 Infographics
Microsoft 发布了多个体系结构相关海报/infographics。 它们包括[构建真实的云应用程序](http://azure.microsoft.com/documentation/infographics/building-real-world-cloud-apps/)和[云服务使用的缩放比例](http://azure.microsoft.com/documentation/infographics/cloud-services/)。 

![Azure 体系结构 Infographics](./media/architecture-overview/AzureArchInfographicThumb.jpg)

添加行

测试写入访问权限
