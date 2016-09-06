---
ms.openlocfilehash: 53e682dddf2e4b6a2632bef0aea6d6e0da7829d5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
 pageTitle="计划程序的概念、 术语和实体 |Microsoft Azure"
 description="Azure 计划程序概念、 术语和实体层次结构，包括作业和作业集合。  显示调度作业的完整示例。"
 services="scheduler"
 documentationCenter=".NET"
 authors="krisragh"
 manager="dwrede"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/04/2015"
 ms.author="krisragh"/>

# 计划程序的概念、 术语、 + 实体层次结构

## 计划程序实体层次结构

下表描述了公开或使用计划程序 API 的主资源︰

|资源 | 说明 |
|---|---|
|**云服务**|从概念上讲，云服务表示一个应用程序。 预订可能有多个云服务。|
|**作业集合**|包含一组作业和维护设置、 配额和限制共享集合内的作业的作业集合。 作业集合创建的订阅所有者和组作业一起根据使用情况或应用程序的边界。 它是限制到一个区域。 它还允许配额来限制该集合中的所有作业使用的强制执行。 配额包括 MaxJobs 和 MaxRecurrence。|
|**作业**|作业定义单个的重复性操作，执行简单或复杂的策略。 操作可能包括 HTTP 请求或存储队列请求。|
|**作业历史记录**|作业历史记录表示作业执行的详细信息。 它包含了成功与失败，以及任何响应详细信息。|

## 计划程序实体管理

在高级别上，计划程序和服务管理 API 公开资源上的以下操作︰

|Capability|说明和 URI 地址|
|---|---|
|**云服务管理**|获取，放入和删除用于创建和修改云服务的支持 <p>`https://management.core.windows.net/{subscriptionId}/cloudservices/{cloudServiceName}`</p>|
|**工作集管理**|建立联系，并删除支持创建和修改作业的集合，其中包含的作业。 作业集合是作业和映射到配额和共享的设置的容器。 配额，稍后，所述的示例包括作业和重复出现的最小间隔的最大数目。 <p>传和删除: `https://management.core.windows.net/{subscriptionId}/cloudservices/{cloudServiceName}/resources/scheduler/jobcollections/{jobCollectionName}`</p><p>获取： `https://management.core.windows.net/{subscriptionId}/cloudservices/{cloudServiceName}/resources/scheduler/~/jobcollections/{jobCollectionName}`</p>
|**作业管理**|放、 过帐、 修补程序，并删除支持创建和修改作业。 所有作业必须都属于作业集合已经存在，因此没有任何隐式创建。 <p>`https://management.core.windows.net/{subscriptionId}/cloudservices/{cloudServiceName}/resources/scheduler/~/jobcollections/{jobCollectionName}/jobs/{jobId}`</p>|
|**作业历史记录管理**|对提取的作业执行历史记录，例如，作业时间和作业执行结果的 60 天内获得支持。 添加查询字符串参数支持根据状态和状态进行筛选。 <P>`https://management.core.windows.net/{subscriptionId}/cloudservices/{cloudServiceName}/resources/scheduler/~/jobcollections/{jobCollectionName}/jobs/{jobId}/history`</p>|

## 作业类型

有两种类型的作业︰ HTTP 作业 （包括支持 SSL 的 HTTPS 作业） 和存储队列作业。 HTTP 的作业是理想选择，如果您有现有的工作负载或服务的终结点。 您可以使用存储队列作业将消息张贴到存储队列，以便这些作业是使用存储队列的工作负载的理想选择。

## 详细信息中的"作业"实体

在基本级别，计划的作业包括几个部分︰

- 要执行的作业计时器触发时的操作  

- （可选）运行作业的时间  

- （可选）何时以及如何经常重复作业  

- （可选）要激发的主要操作失败时的操作  

在内部，一个计划的作业还包含系统提供的数据，例如下, 一步计划的执行时间。

下面的代码提供了一个全面计划作业的示例。 后续各节中提供了详细信息。

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

看到在上面的示例已排定作业，作业定义中包含几个部分︰

- 开始时间 （"开始时间"）  

- 操作 （"动作"），其中包括错误处理操作 ("errorAction")

- 定期 （"周期"）  

- 状态 （""）  

- 状态 （"状态"）  

- 重试策略 ("retryPolicy")  

让我们看一下每个细节中︰

## 开始时间

"开始时间"是开始时间，并允许调用方指定了时区偏移量[ISO 8601 格式](http://en.wikipedia.org/wiki/ISO_8601)在网络上。

## 操作和 errorAction

"操作"是在每个匹配项时调用的操作，介绍了一种类型的服务调用。 操作是什么将提供按计划执行。 计划程序支持 HTTP 和存储队列操作。

在上面的示例中的操作是 HTTP 操作。 以下是存储队列操作的示例︰

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

"ErrorAction"是错误处理程序中，主要操作失败时调用的操作。 您可以使用此变量调用错误处理终结点或发送用户通知。 这可以用于到达在主不可用 （例如，对于灾难，终结点的网站） 的情况下的辅助终结点也可以用于通知错误处理终结点。 主要的行动，就像错误操作可以基于其他操作的简单或复合逻辑。 要了解如何创建一个 SAS 令牌，请参阅[创建和使用共享访问签名](https://msdn.microsoft.com/library/azure/jj721951.aspx)。

## 重复周期

定期有几个部分︰

- 频率︰ 在将一种分钟、 小时、 天、 周、 月、 年  

- 间隔重复执行给定频率间隔︰  

- 规定的计划︰ 指定分钟、 小时、 工作日、 月数和重复周期的 monthdays  

- 发生次数的计数︰ 计数  

- 结束时间︰ 没有作业将在指定的结束时间后执行  

如果它有一个定期的对象，该对象的 JSON 定义中指定，定期作业。 如果指定了计数和结束时间，则接受最先完成规则。

## 状态

该作业的状态为四个值之一︰ 启用、 禁用、 完成或出现故障。 您可以放置或修补作业，以便更新为启用或禁用状态。 如果作业已完成，或出现故障，则不能更新 （虽然仍会删除作业） 的最终状态。 状态属性的一个示例是，如下所示︰


        "state": "disabled", // enabled, disabled, completed, or faulted
已完成的和有故障的作业将在 60 天后删除。

## status

一旦启动计划程序作业，信息将返回有关该作业的当前状态。 此对象不是可由用户设定 — — 它由系统设置。 但是，它包括在作业对象 （而不是单独链接的资源），一个可以很容易地获取作业的状态。

作业状态包括︰ 前一次的执行 （如果有的话） 的时间，（对于正在进行中的作业） 的下一步计划执行的时间以及执行计数的作业。

## retryPolicy

如果计划程序作业失败，则可以指定重试策略以确定是否以及如何将重试该操作。 这由**retryType**对象 — — 如果它就设置为**无**没有重试策略，如上所示。 将其设置为**固定**时重试策略。

重试策略设置，可以指定两个附加的设置: (**retryInterval**) 的重试间隔和次数 (**retryCount**)。

**RetryInterval**对象时，指定的重试间隔是重试之间的时间间隔。 其默认值为 1 分钟，其最小值为 1 分钟，其最大值是 18 个月。 它以 ISO 8601 格式定义。 同样，与**retryCount**对象; 指定的重试次数值它是尝试重试的次数。 其默认值为 5，且最大值为 20\. / **retryInterval**和**retryCount**是可选的。 为它们指定了的默认值，如果将**retryType**设置为**固定**和不值显式指定。

## 请参见

 [计划是什么？](scheduler-intro.md)

 [开始在 Azure 门户使用 Azure 计划程序](scheduler-get-started-portal.md)

 [计划和 Azure 计划程序中的计费](scheduler-plans-billing.md)

 [如何构建复杂的调度和高级的定期使用 Azure 调度程序](scheduler-advanced-complexity.md)

 [Azure 计划其余部分 API 参考](https://msdn.microsoft.com/library/dn528946)

 [Azure 计划 PowerShell cmdlet 引用](scheduler-powershell-reference.md)

 [Azure 的计划程序的高可用性和可靠性](scheduler-high-availability-reliability.md)

 [Azure 计划限制、 默认值和错误代码](scheduler-limits-defaults-errors.md)

 [Azure 计划程序的出站身份验证](scheduler-outbound-authentication.md)
