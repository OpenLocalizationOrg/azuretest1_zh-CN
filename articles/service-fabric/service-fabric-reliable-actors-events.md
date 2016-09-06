---
ms.openlocfilehash: 2901b980ca2d8dc2186ade72e2d48701b027118d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="可靠的操作者事件"
   description="服务结构可靠参与者的事件介绍。"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2015"
   ms.author="amanbha"/>


# 使用者事件
主角事件提供一种方法将从主角的最佳效果通知发送到客户端。 主角事件主角-客户端通信的设计并不用于演员与演员沟通。

以下代码段显示了如何在您的应用程序中使用事件主角。

定义接口所描述的事件发布的主角。 此接口必须派生自`IActorEvents`接口。 方法的参数必须是[可序列化数据协定](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)。 事件通知是单向且最佳的工作，方法必须返回 void。

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

声明发布的使用者接口中的参与者的事件。

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    [Readonly]
    Task<string> GetGameScore();
}
```

在客户端，实现事件处理程序。

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

在客户端上创建的代理将事件发布者和订阅的事件。

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);
proxy.SubscribeAsync(new GameEventsHandler()).Wait();
```

发生故障转移时使用者可能会故障转移到另一个进程或节点。 使用者代理管理活动订阅，并自动重新订阅。 您可以控制通过重新订阅间隔`ActorProxyEventExtensions.SubscribeAsync<TEvent>`API。 若要取消订阅使用`ActorProxyEventExtensions.UnsubscribeAsync<TEvent>`API。

在主角，只需将事件发布发生时。 如果有事件订阅的订阅服务器，演员运行时将向其发送通知。

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), State.Status.Score);
```
