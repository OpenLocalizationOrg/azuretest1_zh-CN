---
ms.openlocfilehash: cb614d528252828ed8876f44838c55f23d868ebe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="监视您的逻辑应用程序" 
    description="如何查看您的逻辑应用程序已经完成。" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2015"
    ms.author="stepsic"/>

#监视您的逻辑应用程序

[创建一个逻辑应用程序](app-service-logic-create-a-logic-app.md)中所描述的步骤创建一个逻辑应用程序之后，您可以看到它在 Azure 门户执行的完整历史记录。 查看历史记录，单击门户屏幕左侧的**浏览**并选择**逻辑的应用程序**。 将显示在您的订阅中的逻辑应用程序列表。 逻辑应用程序可以是**已启用**或**已禁用**。 **启用**逻辑应用程序意味着触发器将运行您的逻辑应用程序以响应触发事件，**禁用**逻辑应用程序将无法运行以响应事件。

![概述](./media/app-service-logic-monitor-your-logic-apps/overview.png)

刀片式服务器为您的逻辑应用程序出现时，有 2 个部分很有用︰

- **摘要**，告诉您最新的状态，是编辑您的逻辑应用程序的入口点。
- **所有运行**，它显示了此逻辑应用程序已在运行的列表。

##运行

![所有运行](./media/app-service-logic-monitor-your-logic-apps/allruns.png)

运行的此列表中显示**的开始时间**，**运行标识符**（您可以使用此 REST API 调用时），和在特定运行的**持续时间**。 单击任意行以查看运行的详细信息。

详细信息刀片式服务器在运行中显示执行时间以及所有操作的序列图。 在下面，是所有已执行的操作的完整列表。

![运行和操作](./media/app-service-logic-monitor-your-logic-apps/runandaction.png)

最后，在特定的操作，您可以获得的所有数据传递到操作，并接收从**输入**和**输出**部分中的操作。 单击链接以查看完整内容 （您可以复制的链接来下载的内容）。 

另一个重要是信息的**跟踪 ID**。 此标识符的所有操作调用的标头中传递。 如果您有日志记录在您自己的服务，我们建议记录跟踪 ID，然后交叉引用与此标识符日志。

##触发器的历史记录 

轮询触发器在某一时间间隔检查 API，但不一定会开始运行，具体取决于响应 (例如`200`指的是运行和`202`指的是不会运行)。 触发器历史记录为您提供了一种查看发生但不运行逻辑应用程序调用的所有方法 (`202`响应)。

![触发器的历史记录](./media/app-service-logic-monitor-your-logic-apps/triggerhistory.png)

对于每个触发器可以看到如果它**Fired**，如果它没有启动，或者如果有某种类型的错误 (它**失败**)。 要检查为什么触发器失败，您可以单击**输出**链接。 如果则未触发该事件，请单击**运行**链接以查看后它激发出了什么问题。

请注意，对于*推*触发器，您将*不*请参见此处，开始运行的时间改为您将看到该*回调注册*调用-这些是逻辑应用程序注册以获得回叫时。 如果不使用推触发器，它可能在注册时 （这可以在输出中看到），有问题但否则许多需要专门调查该 API。

##版本控制

还有一种附加功能当前不可能在用户界面 （即将提供），但可以通过[REST api](http://go.microsoft.com/fwlink/?LinkID=525617&clcid=0x409)。 当您更新逻辑的应用程序的定义时，定义的以前的版本存储。 这是因为该运行中进度已运行，如果将引用运行启动时存在的逻辑应用程序的版本。 他们正在进行时，无法更改运行过程的定义。 版本历史记录 REST api，使您可以访问到此信息。
 
测试
