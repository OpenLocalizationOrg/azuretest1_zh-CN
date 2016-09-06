---
ms.openlocfilehash: 1ff7fd10c7d74265437e0e70424ff38ffb407d81
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="从服务器资源管理器访问 Azure 的虚拟机"
   description="概述如何查看创建和管理服务器资源管理器在 Visual Studio 中的 Azure 虚拟机 (Vm) 的获取。"
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

# 从服务器资源管理器访问 Azure 的虚拟机

通过在 Visual Studio 中使用服务器资源管理器中，可以显示信息，关于由 Azure 托管的虚拟机。

## 访问服务器资源管理器中的虚拟机

如果您有由 Azure 托管的虚拟机，您可以在服务器资源管理器中访问它们。 您必须首先登录到 Azure 订阅以查看您的移动服务。 若要登录，在服务器资源管理器中打开 Azure 节点的快捷菜单，然后选择**连接到 Microsoft Azure**。

### 若要了解您的虚拟机

1. 在服务器资源管理器中选择虚拟机，，然后选择 F4 键以显示其属性窗口。
 
    下表显示了哪些属性是可用的但它们都是只读的。 若要更改它们，请使用管理门户。

  	|属性|说明|
  	|---|---|
  	|DNS 名称|虚拟机的互联网地址 URL。|
  	|环境|为虚拟机，则此属性的值始终是生产。|
  	|名称|虚拟机的名称。|
  	|大小|虚拟机，反映的是可用的内存和磁盘空间量的大小。 有关详细信息，请参阅如何︰ 配置虚拟机大小。|
  	|。|值包括启动、 已启动、 停止、 停止、 和检索的状态。 检索状态显示，如果当前的状态是未知的。 此属性的值与在管理门户站点使用的值不同。|
  	|SubscriptionID|Azure 帐户订阅 ID。 您可以通过查看订阅属性管理门户网站上显示此信息。|
    
1. 选择一个端点节点，然后再查看**属性**窗口。

1. 下表描述了可用的属性的终结点，但它们都是只读的。 若要添加或编辑虚拟机的终结点，请使用管理门户。 

  	|属性|说明|
  	|---|---|
  	|名称|终结点的标识符。|
  	|专用端口|您的应用程序的内部网络访问的端口。|
  	|协议|使用的协议为此终结点的传输层，TCP 或 UDP。|
  	|公用端口|针对公众对于您的应用程序使用的端口。|

## 下一步行动

[远程桌面使用 Azure 角色](http://go.microsoft.com/fwlink/p/?LinkID=623091)




测试
