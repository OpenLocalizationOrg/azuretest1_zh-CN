---
ms.openlocfilehash: 96d488eb51ce3d6e516559dbd41f38962522bfb6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建 Azure RemoteApp 图像"
    description="了解可用于为 Azure RemoteApp 创建图像的选项" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="elizapo" />



# 创建 Azure RemoteApp 图像

Azure RemoteApp 使用图像保存您与您的用户共享的应用程序。 若要创建使用您选择的应用程序，Azure RemoteApp 集合，无论是云还是混合，首先与安装这些应用程序创建的图像。 然后，创建一个集合，它使用该映像，将用户分配到集合中，并将应用程序发布到这些用户。 

有几个选项用于创建或使用的图像。 图像的基本[要求](remoteapp-imagereqs.md)是它运行 Windows Server 2012 R2 并安装远程桌面会话主机 (RDSH) 角色。 如何获取，是从哪里获得有趣的事情。

图像时，您有下列选项︰

- 您可以导入并使用[基于映像的 Azure 的虚拟机上](remoteapp-image-on-azurevm.md)。 这是很好的业务线应用程序需要自定义设置。 您可以自定义的图像，可用于该应用程序。
- 您可以[创建和上传自定义映像](remoteapp-create-custom-image.md)。 这是很好，如果您有用于内部部署远程桌面服务部署的映像。
- 您可以使用一个[模板映像](remoteapp-images.md)包含在您的远程应用程序订阅。 这些映像创建和维护的 RemoteApp 团队包含标准，某些应用程序 （如 Office 套件中） 可提供给用户。 注意，可以在生产环境中的使用仅 Office 365 专业人员加上图像。

无论您从哪里获得您的图像或您创建的方式，您需要确保您了解[应用程序要求](remoteapp-appreqs.md)，以确保您的应用程序适用于远程应用程序。 然后下, 一步是创建一个[云](remoteapp-create-cloud-deployment.md)或[混合](remoteapp-create-hybrid-deployment.md)。
 