---
ms.openlocfilehash: a047a8cda64b62b23dbf5a73f9eac0932919c6de
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 批次开发库和工具 |Microsoft Azure"
    description="获取库和开发 Azure 批处理应用程序所需要的工具"
    services="batch"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="07/24/2015"
    ms.author="danlep"/>


# Azure 批次开发库和工具
<p> 获得这些库和工具来开发 Azure 批处理应用程序。

## 批处理

+ [批次.NET 客户端库](http://www.nuget.org/packages/Azure.Batch/)(NuGet) – 开发客户端应用程序来使用批处理服务运行作业
+ [批次管理库](http://www.nuget.org/packages/Microsoft.Azure.Management.Batch/)(NuGet) – 管理批次帐户
+ [批次资源管理器](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer)(GitHub) 的 GUI 应用程序和示例来浏览、 访问和更新批客户，包括作业和任务中的主要元素计算节点和池，并在计算节点上的文件。 请参阅[博客帖子](http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx)。


## 批处理应用程序

+ [批处理应用程序云 SDK](http://www.nuget.org/packages/Microsoft.Azure.Batch.Apps.Cloud/1.1.1-preview)(NuGet) — 使应用程序能够将下放到批服务作业
+ [批处理应用程序 Visual Studio 扩展](https://visualstudiogallery.msdn.microsoft.com/8b294850-a0a5-43b0-acde-57a07f17826a)（Visual Studio 库） – 使用批处理应用程序云 SDK 的 Visual Studio 中的云启用应用程序
+ [批处理应用程序客户端 SDK](http://www.nuget.org/packages/Microsoft.Azure.Batch.Apps/2.3.0-preview)(NuGet) – 能够卸载作业到批服务的应用程序的交互作用
+ [批次 Python 应用程序客户端](https://github.com/Azure/azure-batch-apps-python)(GitHub)-Python 可以与应用程序进行交互的客户端模块设置在批处理应用程序服务

>[AZURE.IMPORTANT]Azure 将只发布预览批处理应用程序 API。 只应该测试或概念证明项目的开发与它。 关键批处理应用程序功能将会集成到批处理 API 在将来的服务版本。

## 其他资源

+ [代码示例](https://github.com/Azure/azure-batch-samples)() GitHub
+ [Azure 批论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch)

<!--Anchors-->
[批处理]: #batch
[批处理应用程序]: #batch-apps
[其他资源]:#additional-resources

测试
