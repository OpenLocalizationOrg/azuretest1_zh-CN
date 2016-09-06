---
ms.openlocfilehash: 2dfb79c2a887ccd9592f0da678455b62d74e30ad
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 Visual Studio 的访问私有 Azure 云"
   description="了解如何使用 Visual Studio 访问私有云资源。"
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tglee" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/13/2015"
   ms.author="kempb" />

# 使用 Visual Studio 的访问私有 Azure 云

##概述

默认情况下，Visual Studio 支持公共 Azure 云其他终结点。 这可以是问题，不过，如果您使用的 Visual Studio 与私有的 Azure 云。 您可以使用证书来配置 Visual Studio 访问私有的 Azure 云 REST 端点。 您可以使用这些设置文件发布到您 Azure 的证书。

## 若要访问专用 Azure 云在 Visual Studio 中

1. 在私有云的管理门户网站，下载您发布的设置文件，或与您的管理员联系发布设置文件。 在 Azure 的公用版本中，此下载的链接是[https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/)。 （您下载的文件应具有.publishsettings 扩展名）。

1. 在**服务器资源管理器**在 Visual Studio 中选择**Azure**节点，在快捷菜单中，选择**管理订阅**命令。

    ![管理订阅命令](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. 在**管理 Microsoft Azure 订阅**对话框中，选择**证书**选项卡，然后选择**导入**按钮。

    ![导入 Azure 的证书](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. 在**导入 Microsoft Azure 订阅**对话框中，浏览到文件夹保存发布设置文件并选择该文件，然后选择**导入**按钮的位置。 这向 Visual Studio 中导入的发布设置文件中的证书。 您现在应能够与您的私有云资源进行交互。

    ![导入发布设置](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## 下一步行动

[从 Visual Studio 发布到 Azure 的云服务](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[如何︰ 下载并导入发布设置和订阅信息](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)


测试
