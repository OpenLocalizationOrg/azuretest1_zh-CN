---
ms.openlocfilehash: ffcc92c818125df42da6eeccf8b60568402b708b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 移动接洽 Android SDK 集成" 
    description="最新的更新和 Android SDK Azure 移动服务的步骤"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="piyushjo" />


#发行说明

##4.1.0 （2015 年 08/25）

- 为 Android M 处理新的权限模型。
- 现在可以配置位置功能在运行时，而不是使用`AndroidManifest.xml`。
- 为修复权限错误︰ 如果您使用`ACCESS_FINE_LOCATION`，然后`ACCESS_COARSE_LOCATION`再也不需要。
- 稳定性的改进。

##4.0.0 （2015 年 07/06）

-   要进行分析和推更可靠的内部协议更改。
-   本机的推 (GCM/ADM) 现在也可以用来在应用程序通知因此必须配置推入市场的任何类型的本机推凭据。
-   解决大图片通知︰ 好像被推后显示仅 10 条。
-   在 web 视图中修复 bug︰ 在链接上单击时执行默认操作 URL。
-   解决与本地存储管理相关的极少数故障。
-   修复动态配置字符串管理。
-   更新最终用户许可协议。

##3.0.0 （2015 年 02/17）

-   最初发布 Azure 的移动服务
-   应用程序标识配置将替换为连接字符串配置。
-   删除 API 来发送和接收任意 XMPP 消息从任意 XMPP 实体。
-   删除 API 来发送和接收设备之间的消息。
-   安全性得到了改进。
-   Google 播放和 SmartAd 跟踪删除。

 
测试
