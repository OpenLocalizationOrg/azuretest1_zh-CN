---
ms.openlocfilehash: da1c5e6427b9cc2bd8a248218ef510c20e51d8c2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 服务结构参与者分发网络和图形设计模式"
   description="设计模式如何使用服务结构者模型为分布式的网络和图形应用程序"
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
   ms.date="08/11/2015"
   ms.author="claudioc"/>

# 可靠的操作者设计模式︰ 分布式网络和关系图
服务结构可靠参与者是自然符合建模复杂的解决方案涉及关系和建模作为对象的关系。  

![][1]

如图所示它很容易建立用户模型作为主角的实例 （在网络中的节点）。 例如，"朋友送"（有时称为"从动机构"问题） 允许用户查看来自它们连接到，人的状态更新到 Facebook 和 Twitter 的工作方式类似。
主角模型提供了灵活性处理具体化问题。 我们可以填充的朋友进事件时，更新所有我的朋友的朋友进时刻更新发送，如下所示︰

![][2]


## 智能缓存代码示例 – 社交网络的朋友进 （事件时间）

填充的朋友送的示例代码︰

```csharp
public interface ISocialPerson : IActor
{
    Task AddFriend(long person);
    Task RemoveFriend(long person);
    Task<List<SocialStatus>> GetFriendsFeed();
    Task<SocialStatus> GetStatus();
    Task<List<SocialStatus>> GetMyFeed();
    Task UpdateStatus(string status);
    Task UpdateFriendFeedAsync(SocialStatus status);
}

[DataContract]
Public class SocialPersonState
{
    [DataMember]
    public string _name; // my name
    [DataMember]
    public List<long> _friends; // list of my friends' IDs
    [DataMember]
    public List<SocialStatus> _friendsFeed; // my friends feeds
    [DataMember]
    public List<SocialStatus> _myFeed; // this is my feed, all my status updates
    [DataMember]
    public SocialStatus _lastStatus; // this is my last update
}

public class SocialPerson : Actor, ISocialPerson
{
    public override Task ActivateAsync()
    {
        CreateOrRestoreState();
        return base.ActivateAsync();
    }

    public Task AddFriend(long person)
    {
        State._friends.Add(person);
        return TaskDone.Done;
    }

    public Task RemoveFriend(long person)
    {
        State._friends.Remove(person);
        return TaskDone.Done;
    }

    public Task<List<SocialStatus>> GetFriendsFeed()
    {
        return Task.FromResult(State._friendsFeed);
    }

    public Task UpdateStatus(string status)
    {
        State._lastStatus = new SocialStatus()
        {
            Name = _name,
            Status = status,
            Timestamp = DateTime.UtcNow
        };
        State._myFeed.Add(_lastStatus);

        var taskList = new List<Task>();

        foreach(var friendId in _friends)
        {
            var friend = ActorProxy.Create<ISocialPerson>(friendId);
            taskList.Add(friend.UpdateFriendFeedAsync(_lastStatus));
        }

        return Task.WhenAll(taskList);
    }

    public Task UpdateFriendFeed(SocialStatus status)
    {
        State._friendsFeed.Add(status);

        return TaskDone.Done;
    }

    public Task<SocialStatus> GetStatus()
    {
        return Task.FromResult(State._lastStatus);
    }

    public Task<List<SocialStatus>> GetMyFeed()
    {
        return Task.FromResult(State._myFeed);
    }
}
```

或者我们可以建立模型来扇出和编译查询计时器，在朋友送我们演员换句话说用户要求为他们的朋友进。 我们可以使用的另一种方法物化朋友送上的计时器，例如，每隔 5 分钟。 或者，我们可以优化模型，结合事件时间和查询时间，具体取决于用户的习惯，例如频率登录或发布更新的基于计时器的模型处理。
在建模在社交网络中的主角，其中一个还应该考虑"超级用户，"有数以百万计的用户的用户。 开发人员应该模型的状态和行为的这类用户以不同的方式，以满足需求。
同样，如果我们想要建立模型可以同时完成单个活动参与者 （辐射） 连接用户的许多使用者的活动。 分组聊天或游戏承载方案是两个示例。
让我们组聊天示例;一组参与者的创建可以将消息从一个参与者到如下面的示例中所示的组分发组对话参与者︰

## 智能缓存代码示例 – 群聊

```csharp
public interface IGroupChat : IActor
{
    Task PublishMessageAsync(long participantId, string message);
    Task<List<GroupChatMessage>> GetMessagesAsync();
    Task AddParticipantAsync(long participantId);
    Task RemoveParticipantAsync(long partitipantId);
}

[DataContract]
public class GroupChatParticipantState
{
    [DataMember]
    Public long _groupChatId;
    [DataMember]
    public List<GroupChatMessage> _messages;
}

public class GroupChatParticipant : Actor<GroupChatParticipantState>, IGroupParticipant
{
    public Task SendMessageAsync(string message)
    {
        if (State._groupChatId != -1)
        {
            var groupChat = ActorProxy.Create<IGroupChat>(State._groupChatId);
            return groupChat.PublishMessageAsync(this.Id, message);
        }

        return TaskDone.Done;
    }

    ...
}

[DataContract]
public class GroupChatState
{
    [DataMember]
    Public List<long> _participants;
    [DataMember]
    Public List<GroupChatMessage> _messages;
}


public class GroupChat : Actor<GroupChatState>, IGroupChat
{

public Task PublishMessageAsync(long participantId, string message)
{
    var chatMessage = new GroupChatMessage()
    {
        ParticipantId = participantId,
        Message = message,
        Timestamp = DateTime.Now
    };

    State._messages.Add(chatMessage);

    var taskList = new List<Task>();

    foreach(var id in State._participants)
    {
        if (id != participantId)
        {
            var participant = ActorProxy.Create<IGroupParticipant>.Create(id);
            taskList.Add(participant.ReceiveMessageAsync(chatMessage));
        }
    }
    return Task.WhenAll(taskList);
}

...

}
```

它确实就是利用可靠参与者能够允许以满足 id 由群集中的任何其他使用者并与之通信，而无需担心放置、 寻址、 缓存、 消息传递、 序列化、 或路由的任何使用者。

## 下一步行动
[图案︰ 智能高速缓存](service-fabric-reliable-actors-pattern-smart-cache.md)

[图案︰ 资源管理](service-fabric-reliable-actors-pattern-resource-governance.md)

[图案︰ 有状态的服务组合](service-fabric-reliable-actors-pattern-stateful-service-composition.md)

[图案︰ 物联网](service-fabric-reliable-actors-pattern-internet-of-things.md)

[图案︰ 分布式的计算](service-fabric-reliable-actors-pattern-distributed-computation.md)

[一些反模式](service-fabric-reliable-actors-anti-patterns.md)

[服务结构演员简介](service-fabric-reliable-actors-introduction.md)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-pattern-distributed-networks-and-graphs/distributedNetworks_arch1.png
[2]: ./media/service-fabric-reliable-actors-pattern-distributed-networks-and-graphs/distributedNetworks_arch2.png
