---
ms.openlocfilehash: aacfc0b8f41263f62fed0c96278054ea5adb268d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在 Azure RemoteApp 的任何设备上获得相同的 Office 365 体验"
   description="了解如何与您的用户通过使用 Azure 远程应用程序共享任何 Office 365 提供应用程序。"
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/12/2015"
   ms.author="guscatal;elizapo"/>


# 在 Azure RemoteApp 的任何设备上获得相同的 Office 365 体验

这篇文章将介绍如何在您的公司中的任何设备上部署 Office 365。 您的用户可以获得相同的功能和在 Android 和苹果的用户界面体验。 

我们将完成这通过承载 Office 365 比例可以在 Azure 用户可以连接到的虚拟机上使用 Azure 远程应用程序。 这一组虚拟机的我们称之为"云集合"。 

## 创建云集合

首先创建一个 Azure 帐户后，导航到**远程应用程序**通过单击左侧的链接。 
![在 Azure 的门户网站上显示 Azure 的远程应用程序](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/1-menu.png)

然后继续通过单击**新**的底部和"快速创建"集合。 提供一个名称、 地区、 订阅、 计划和我们提供的图像"Office 专业 2013年"。
![创建对话框](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/2-quickcreate.PNG)

完成后应启动窗体的集合创建过程。 这可能需要花一个小时左右。

![等待](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/3-waiting.PNG)

完成此过程后，它看起来像这样。 如果我们单击**发布**，我们可以看到，大多数 Office 应用程序发布了我们已经。
![创建集合](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/4-done.PNG)

![已发布的应用程序](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/5-publish.PNG)

此时您还可以添加更多的用户通过单击**用户的访问权限**有权访问此集合。
![配置用户访问](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/6-user.PNG)

现在让我们来尝试连接到 Office 365 ！

## 连接到 Office 365

我们将头转移到[https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/)，向下滚动并单击**下载客户端**就可以在设备上安装 Azure 远程应用程序客户端。 下面的屏幕快照是针对 Windows。

应用程序启动后将要求您使用您的 Microsoft 帐户 （以前称为"Live ID"） 登录，使用同一个 Azure 帐户作为现在。 在您登录时中应该看到新邀请有关通知，那里单击，您应看到类似列表下面。 接受邀请符合您的 Azure 帐户所有者的电子邮件。 

![新邀请](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/7-araclient.PNG)

它的外观时有新的邀请。

![接受应用程序](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/8-invitation.PNG) 

一旦您接受邀请，您应该看到在 Azure 远程应用程序客户端的所有 Office 应用程序。

![应用程序列表](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/9-work.PNG)

当您单击这些应用程序将启动在 Azure 的虚拟机，您应为所有集 ！ 尽情享受 ！

![启动](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/10-arastart.PNG)

![powerpoint](https://raw.githubusercontent.com/guscatalano/azure-content/master/articles/media/remoteapp-tutorial-o365anywhere/11-pp.PNG)
 