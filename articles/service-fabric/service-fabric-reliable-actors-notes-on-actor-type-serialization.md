---
ms.openlocfilehash: e8e711c9b21376f867cbcacbca8853b574c785d6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="参与者类型序列化的可靠参与者注意事项"
   description="讨论了可用于定义服务结构可靠参与者状态和接口的可序列化的类定义的基本要求"
   services="service-fabric"
   documentationCenter=".net"
   authors="clca"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2015"
   ms.author="claudioc"/>

# 有关服务结构可靠者说明键入序列化

有需要记住保留定义使用者界面和省时的几个重要方面︰ 必须是可序列化数据约定类型。 有关数据约定的详细信息可以在[MSDN](https://msdn.microsoft.com/library/ms731923.aspx)上找到。

## 使用者接口中使用的类型

所有方法和任务返回的[使用者接口](service-fabric-reliable-actors-introduction.md#actors)中定义的每个方法的结果类型的参数必须是可序列化的数据合同。 这同样适用于[使用者事件接口](service-fabric-reliable-actors-events.md#actor-events)中定义的方法的参数。 （主角事件接口方法始终返回 void）。
例如，如果`IVoiceMail`接口定义一个方法︰

```csharp

Task<List<Voicemail>> GetMessagesAsync();

```

`List<T>` 是标准的.NET 类型为已序列化的数据合同。 `Voicemail`类型必须是可序列化的数据合同。

```csharp

[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}

```

## 主角状态类

主角状态必须是可序列化的数据合同。 例如如果我们有主角类定义如下所示︰

```csharp

public class VoiceMailActor : Actor<VoicemailBox>, IVoiceMail
{
...

```

将状态类来定义类，并分别以 DataContract 和 DataMember 属性批注其成员。

```csharp

[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}

```
