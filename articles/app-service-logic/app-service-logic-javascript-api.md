---
ms.openlocfilehash: 97b8ea63d066b40290549af41a66958a37308adf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="API JavaScript"
   description="API JavaScript"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/16/2015"
   ms.author="stepsic"/>

#JavaScript API 的应用程序
JavaScript API 应用程序使您可以运行简单 JavaScript 的表达式*逻辑应用程序执行时*的简便方法。 

##何时应使用此 API 的应用程序？
此 API 的应用程序的主要方案是时所需的代码编写为相同逻辑的应用程序，并做的生命周期*不*想在任何其他情况下要调用的代码。

另一方面，如果您希望具有独立的逻辑应用程序生命周期的代码可重用代码段，然后应使用 WebJobs API 的应用程序创建简单的代码表达式，并从您的逻辑应用程序中调用它们。

最后，如果您想要包含任何其他包，您还需要使用 WebJobs API 应用程序中，如您不能添加任何 JavaScript API 应用程序使用的库。 

如果您更喜欢在 C# 中编写表达式，请使用[C# 应用程序 API](app-service-logic-cs-api.md) 。

##创建一个 JavaScript API 应用程序
若要使用 JavaScript API 的应用程序，您需要首先创建 JavaScript API 的应用程序的实例。 进行这种线内创建一个逻辑应用程序时，或者从 Azure 市场选择 JavaScript API 的应用程序。

##使用 JavaScript API 应用程序中的逻辑应用程序设计器图面
###触发器
您可以创建一个触发器，它的逻辑应用程序服务将轮询 （在您定义的间隔），以及如果它返回任何内容，该逻辑应用程序将运行，否则，它将一直等到下一次的轮询间隔，再次检查。

触发器的输入是︰
- **JavaScript 表达式**-将计算的表达式。 它将在函数内部调用，并且必须返回`false`时您不希望应用程序逻辑运行，并且可以返回任何其他操作时所需的逻辑应用程序运行时。 您将能够在逻辑的应用程序的操作中使用响应的内容。
- **上下文对象**--执行触发器可以传递一个可选对象。 您可以定义任意数量的属性，但顶级实体必须是一个对象，例如`{ "bar" : 0}`。

例如，您可以拥有时才会运行您的逻辑应用程序之间的简单触发器: 15，︰ 30 小时的︰

```
var d = new Date(); return (d.getMinutes() > 15) && (d.getMinutes() < 30);
```

###操作

同样，您可以提供要运行的操作。 

该操作的输入是︰
- **JavaScript 表达式**-将计算的表达式。 必须包括`return`语句来获取任何内容。 
- **上下文对象**--执行触发器可以传递一个可选对象。 您可以定义任意数量的属性，但顶级实体必须是一个对象，例如`{ "bar" : 0}`。

例如，假设您使用的 Office 365 触发**新的电子邮件**。 返回以下对象︰
```
{
    ...
    "Attachments" : [
        {
            "name" : "Picture.png",
            "content" : {
                "ContentData" : "iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFAQMAAAC3obSmAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAGUExURf///wAAAFXC034AAAASSURBVAjXY2BgCGBgYOhgKAAABEIBSWDJEbYAAAAASUVORK5CYII=",
                "ContentType" : "image/png",
                "ContentTransferEncoding" : "Base64"
            }
        },  
        {
            "name" : "File.txt",
            "content" : {
                "ContentData" : "Don't worry, be happy!",
                "ContentType" : "text/plain",
                "ContentTransferEncoding" : "None"
            }
        }   
    ]
}
```

但是，您要上载到 Yammer 张贴这些附件。 遗憾的是，Yammer 附件的架构是稍有不同。 现在，现在可以分析此逻辑应用程序内。 上下文的对象不仅仅是刀︰ `@triggerBody()`，并在表达式中，将传递︰

```
return Attachments.map(function(obj){var a = obj.Content; a.FileName = obj.Name; return a;})
```

该操作返回您从函数返回的 JSON。 因此，在 Yammer API 应用程序中可以引用`@body('javascriptapi')`**附件**属性。

## 用做更多您的连接器
创建连接器时，可以将其添加到使用逻辑应用程序的业务流程中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视 API 的应用程序和连接器](../app-service-api/app-service-api-manage-in-portal.md)。

<!--References -->

<!--Links -->
[创建一个逻辑应用程序]: app-service-logic-create-a-logic-app.md

测试
