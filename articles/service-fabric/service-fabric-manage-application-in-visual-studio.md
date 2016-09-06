---
ms.openlocfilehash: 543ebfcdf69747d5ac610ea5ae1b9896c9c281be
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="管理您在 Visual Studio 中的服务结构应用程序"
   description="您可以管理您的 Microsoft Azure 服务结构的应用程序和服务通过 Visual Studio。"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/05/2015"
   ms.author="jesseb"/>

# 管理您在 Visual Studio 中的服务结构应用程序

您可以管理您的 Microsoft Azure 服务结构的应用程序和服务通过 Visual Studio。 一旦您已经[安装您的开发环境](../service-fabric-setup-your-development-environment)，您可以使用 Visual Studio 创建服务结构的应用程序、 添加服务或打包、 登记，并在本地开发群集中部署应用程序。

要管理服务结构应用程序，请在解决方案资源管理器中，右键单击应用程序项目中。

![通过右键单击应用程序项目管理应用程序服务结构][manageservicefabric]

## 部署服务结构应用程序

部署服务结构的应用程序组合到一个简单操作的下列步骤。

1. 创建该应用程序包
2. 上载到映像存储的应用程序包
3. 注册应用程序类型
4. 删除任何正在运行的应用程序实例
5. 创建新的应用程序实例

在 Visual Studio 中，您可以通过从生成菜单中选择部署的解决方案部署应用程序。 按**f5 键**还将部署您的应用程序并将调试器附加到应用程序的所有实例。


## 将服务添加到应用程序服务结构

您可以添加您的应用程序来扩展其功能结构的新服务。  为了确保该服务包括在应用程序包中，添加**新结构服务...**菜单项通过服务。

![向应用程序添加新的结构服务][newservice]

选择服务结构项目类型将添加到您的应用程序，并指定服务的名称。  请参见[选择一个框架，用于您的服务](service-fabric-choose-framework.md)可以帮助您决定要使用的服务类型。

![选择要添加到您的应用程序的结构服务项目类型][addserviceproject]

新服务将添加到您的解决方案和现有的应用程序包。 将应用程序清单添加服务引用和默认的服务实例。 将创建并部署应用程序下一次启动该服务。

![新的服务将被添加到您的应用程序清单][newserviceapplicationmanifest]

## 打包服务结构应用程序

应用程序包需要创建要部署到群集的应用程序和服务。  应用程序清单、 服务清单和其他必要的文件以特定布局组织中包。  Visual Studio 的设置和管理应用程序项目的文件夹，在 pkg 目录中的文件包。  单击**程序包**创建或更新应用程序软件包。  您可能需要这样做，如果您部署使用 Powershell 的自定义脚本的应用程序。

## 删除应用程序

可以在本地群集使用服务器资源管理器中删除应用程序。  此操作将还原上面描述的部署步骤︰

1. 删除任何正在运行的应用程序实例
2. 取消注册的应用程序类型
3. 从映像存储中删除该应用程序包

![删除应用程序](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

- [服务结构应用程序模型](service-fabric-application-model.md)
- [服务结构的应用程序部署](service-fabric-deploy-remove-applications.md)
- [调试服务结构应用程序](service-fabric-debugging-your-application.md)
- [直观显示您使用服务结构资源管理器中的群集](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
