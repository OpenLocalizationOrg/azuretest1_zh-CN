---
ms.openlocfilehash: cd667762a6bf90388c9f909d3bd0dcc0291e3ebe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Emoji 中推送通知的图释" 
    description="如何使用推式通知内的 Emoji 图释"                 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/06/2015" 
    ms.author="piyushjo" />

#使用 Emoji 中推送通知的图释

您可以包括 Emoji 在您的图释推式通知。 目前，Azure 移动服务仅支持 Emoji 表情图标设置为内部和外部应用程序文本通知 3 个字节。 请按照下列步骤操作︰

1.  首先，您需要找到 3 个字节 Emoji 图释库。 您可以查找所有 Emoji 图释，您可以使用下面的[链接](http://stackoverflow.com/questions/10153529/emoji-on-mysql-and-php-why-some-symbol-yes-other-not)。

    ![][1]

2. 请转到访问选项卡上 Azure 移动服务门户。

3. 选择您推式通知 （公告，轮询等） 的类型。 对于本示例，我们选择发布推。

4. 指定通知的不同的字段，直到您到达的通知的文本。 这是您将在其中添加 Emoji 图释。 您可以将其放在标题和 / 或消息。

    ![][2]

5. 剪切您要从以前的链接使用的 Emoji 图释。 在标题和/或消息，您选择的位置中直接将其粘贴。 

6. 完成通知的其他字段，并将其保存。 

7. 当您运行一个测试或激活公告则看图释一个通知，通知规定。   

    ![][3] 
    ![][4]

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/emoji.png
[2]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/notification_android.png
[4]: ./media/mobile-engagement-use-emoji-with-push/notification_ios.png
 
测试
