---
ms.openlocfilehash: 97d79bcf981951bcb7a5bd5ba25774c4515179f4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 Visual Studio 创建 Azure 项目"
   description="使用 Visual Studio 创建 Azure 项目"
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2015"
   ms.author="kempb" />

# 使用 Visual Studio 创建 Azure 项目

Visual Studio Azure 工具提供一个模板，使您可以创建用于 Azure 的云服务。 这些工具还可帮助您配置、 调试和部署到 Azure 的云服务。

Azure 的云服务解决方案包含以下类型的项目︰

- **Azure 项目** 
    
    Azure 项目有关联的角色项目在解决方案中。 它还包括的服务定义和服务配置文件。 服务定义文件定义为包括哪些角色所需的应用程序、 终结点和虚拟机大小的运行时设置。 服务配置文件配置运行多少个角色并为角色定义设置的值。 有关这些设置的详细信息，请参阅[如何︰ 配置角色对于 Visual Studio Azure 云服务](https://msdn.microsoft.com/library/azure/hh369931.aspx)。

- **角色的 web 项目**
 
    辅助角色执行后台处理。 与存储服务和其他基于互联网的服务，就可以进行通信辅助角色。 辅助角色可以有任意数量的 HTTP、 HTTPS 或 TCP 终结点。

    - **ASP.NET Web 角色**，用于构建 web 前端的 ASP.NET 应用程序
    - **ASP.NET MVC5 Web 角色**
    - **ASP.NET MVC4 Web 角色**
    - **ASP.NET MVC3 Web 角色**
    - **WCF 服务 Web 角色**，用于生成 WCF 服务
    - **Silverlight 业务应用程序 Web 角色**（需要 Visual Studio 2012）

- **高速缓存辅助角色** 

    提供了专用于您的应用程序缓存的角色。

- **辅助角色与服务总线队列** 

    服务总线队列提供辅助进程进行通信的消息队列功能。 有关详细信息，请参阅[如何使用服务总线队列](http://go.microsoft.com/fwlink/?LinkId=260560)。

## 若要在 Visual Studio 中创建一个 Azure 的云服务项目

1. 以管理员身份启动 Microsoft Visual Studio。

1. 在菜单栏上选择**文件**，**新建****项目**。

1. 在**项目类型**窗格中，选择**云**从 C# 或 Visual Basic 项目模板的节点。

1. 在**模板**窗格中选择**Azure 云服务**。

1. 指定要用于项目开发.NET Framework 的版本。

1. 输入名称和位置为您的项目和解决方案的名称。 选择**确定**按钮。

1. 在**新 Azure 项目**对话框中，选择您要添加的角色，然后选择右箭头按钮将其添加到您的解决方案。 您可以添加您需要的所有角色。

1. 要重命名已添加到您项目中的角色，将鼠标指针悬停在**新的 Azure 项目**对话框中的角色并选择**重命名**图标右侧的角色。 在添加完后，还可以在您的解决方案中重角色。



测试
