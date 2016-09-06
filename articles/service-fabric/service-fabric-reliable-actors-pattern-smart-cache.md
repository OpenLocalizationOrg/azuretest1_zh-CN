---
ms.openlocfilehash: cab1b878048188323ed054835bd2fdd4c49b8778
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="可靠的演员智能高速缓存设计模式"
   description="设计模式如何为基于 web 的应用程序的缓存基础结构使用可靠的演员"
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
   ms.author="claudioc"/>

# 可靠的操作者设计模式︰ 智能缓存
Web 层中，缓存层、 存储层和辅助层偶尔的组合是几乎当今应用程序的标准部件。 在缓存层通常对性能至关重要，并可能，事实上，能组成多层本身。
许多缓存将缓存提供了更丰富的语义用作其他系统如[Redis](http://redis.io)时简单的键 / 值对。 尽管如此，任何特殊的缓存层将限制在语义和更重要的是它还是管理的另一层。
如果对象只是状态保存在局部变量而这些对象可以是快照或自动保存到持久存储区？ 此外，只需来作为成员变量和方法模拟丰富的集合，如列表、 排序的集、 队列和就此而言任何其他自定义类型。

![][1]

## 排行榜示例
以排行榜为例 — — 排行榜对象需要维护的运动员，其成绩排序的列表，以便我们可以对其进行查询。 例如，"顶部 100 参与者"或查找玩家的导引板相对于 + N 玩家上面和下面他/她的地位。 典型的解决方案与传统的工具需要 GET'ing 排行榜对象 （它支持插入新的元组 < 播放机、 点 > 名为分数的集合，） 排序，最后 PUT'ing 回缓存。 我们可能需要锁定 (GETLOCK，PUTLOCK) 排行榜对象的一致性。
让我们基于参与者的解决方案的状态和行为放在一起。 有两个选项︰

* 实现作为主角，排行榜的集合
* 或使用者作为我们可以将其保存在一个成员变量集合的接口。
第一个让我们去看看什么接口可能如下所示︰

## 智能缓存代码示例 – 排行榜接口

```
public interface ILeaderboard : IActor
{
    // Updates the leaderboard with the score - player, points
    Task UpdateLeaderboard(Score score);

    // Returns the Top [count] from the leaderboard e.g., Top 10
    Task<List<Score>> GetLeaderboard(int count);

    // Returns the specific position of the player relative to other players
    Task<List<Score>> GetPosition(long player, int range);
}

```

接下来，我们实现此接口后一个选项并使用封装中主角此集合的行为︰

## 智能缓存代码示例 – 排行榜的主角

```
public class Leaderboard : Actor<LeaderboardCollection>, ILeaderboard
{
    // Specialised collection, could be part of the actor

    public Task UpdateLeaderboard(Score score)
    {
        State.UpdateLeaderboard(score);
        return TaskDone.Done;
    }

    public Task<List<Score>> GetLeaderboard(int count)
    {
        // Return top N from Leaderboard
        return Task.FromResult(State.GetLeaderboard(count));
    }

    public Task<List<Score>> GetPosition(long player, int range)
    {
        // Return player position and other players in range from Leaderboard
        return Task.FromResult(State.FindPosition(player, range));
    }
}

```

类的状态成员提供状态的主角，它上面的示例代码中还提供了方法来读取/写入的数据。

## 智能缓存代码示例 – LeaderboardCollection

```
[DataContract]
public class LeaderboardCollection
{
    // Specialised collection, could be part of the actor
    [DataMember]
    Private List<score> _leaderboard = new List<score>();

    public void UpdateLeaderboard(Score score)
    {
        _leaderboard.add(score);
    }

    public List<Score> GetLeaderboard(int count)
    {
        …
    }

    public List<Score> GetPosition(long player, int range)
    {
        …
    }
}

```

没有发布，没有锁，只操作在分布式运行时，服务于每一个客户端的单个应用程序中的单个对象一样处理多个客户端的远程对象的数据。  
下面是示例客户端︰

## 智能缓存代码示例 – 调用排行榜的主角

```
// Get reference to Leaderboard
const string appName = "fabric:/FunnyGameApp";
var leaderboard = ActorProxy.Create<ILeaderboard>(1001, appName);

// Update Leaderboard with dummy players and scores
await leaderboard.UpdateLeaderboard(new Score() { Player = 1, Points = 500 });
await leaderboard.UpdateLeaderboard(new Score() { Player = 2, Points = 100 });
await leaderboard.UpdateLeaderboard(new Score() { Player = 3, Points = 1500 });

// Finally, Get the Leaderboard. 0 represents ALL, any other number > 0 represents TOP N
var result = await leaderboard.GetLeaderboard(0);
```

输出如下所示︰

```
Player = 3 Points = 1500
Player = 1 Points = 500
Player = 2 Points = 100
```

## 扩展体系结构
它可能会觉得像上面的例子中可以排行榜实例中创建一个瓶颈。 怎么办，例如，我们计划支持数百和数千玩家？ 一种方法来处理，可能会引入无状态的聚合器，就犹如一个缓冲区 — 按住部分的分数 （例如分类汇总），然后定期将它们发送到排行榜主角，可以维护最终排行榜。 我们将讨论在后面详细此"聚合"技术。
此外，我们不需要考虑互斥体、 信号灯或其他传统上所需的正确表现并发程序的并发性构造。
下面是另一个缓存示例演示一个丰富的语义可以实现与演员。 这一次我们实现优先级队列的逻辑 （降低数量，更高的优先级） 作为参与者实现的一部分。
IJobQueue 的界面看起来如下︰

## 智能缓存代码示例 – 作业队列接口

```
public interface IJobQueue : IActor
{
    Task Enqueue(Job item);
    Task<Job> Dequeue();
    Task<Job> Peek();
    Task<int> GetCount();
}
```

我们还需要定义的工作项︰

## 智能缓存代码示例 – 作业

```
public class Job : IComparable<Job>
{
    public double Priority { get; set; }
    public string Name { get; set; }

    public override string ToString()
    {
        return string.Format("Job = {0} Priority = {1}", Name, Priority);
    }

    public int CompareTo(Job other)
    {
        return Priority.CompareTo(other.Priority);
    }
}
```

最后，我们可以在颗粒中实现 IJobQueue 接口。 请注意我们省略为清楚起见这里的优先级队列的实现细节。 随附的示例中，可以找到示例实现。

## 智能缓存代码示例 – 作业队列

```
public class JobQueue : Actor<List<Jobs>>, IJobQueue
{

    public override Task OnActivateAsync()
    {
        State = new List<Job>();
    }

    public Task Enqueue(Job item)
    {
        // this is where we add to the queue

        ...

        return TaskDone.Done;
    }

    public Task<Job> Dequeue()
    {
        // this is where we remove from the head of the queue

        ...

        return Task.FromResult(frontItem);
    }

    public Task<Job> Peek()
    {
        // this is where we peek at the head of the queue

        ...

        return Task.FromResult(frontItem);
    }

    public Task<int> GetCount()
    {

        // this is where we return the number of items in the queue

        return Task.FromResult(data.Count);
    }
}

```

输出如下所示︰

```
Job = 2 Priority = 0.0323341116459733
Job = 3 Priority = 0.125596747792138
Job = 4 Priority = 0.208425460480352
Job = 0 Priority = 0.304352047063574
Job = 8 Priority = 0.415597594070992
Job = 7 Priority = 0.477669881413537
Job = 5 Priority = 0.525898784178262
Job = 9 Priority = 0.921959541701693
Job = 6 Priority = 0.962653734238191
Job = 1 Priority = 0.97444181375878
```

## 操作者提供了灵活性
在示例上面，排行榜和 JobQueue，我们使用两种不同方法︰

* 排行榜的示例中我们封装排行榜对象作为主角中的私有成员变量，只是提供了一个与此对象 – 同时其状态和功能接口。

* 另一方面，JobQueue 示例中我们演员作为实现优先级队列本身而不是在其他地方引用另一个对象定义。

主角演员提供开发人员定义一部分的丰富的对象结构的灵活性或引用对象图的外部参与者。
在缓存字词演员可以写入隐藏或直写，或者我们可以在成员变量粒度使用不同的技术。 换句话说，我们可以完全控制要保持内容以及何时保持不变。 我们不需要保持瞬时状态或从已保存状态上我们可以生成的状态。
以及如何填充这些演员然后缓存？ 有数字的方式来实现这一点。 参与者提供称为 OnActivateAsync() 和 OnDectivateAsync()，让我们知道当主角的一个实例是激活和停用的虚拟方法。 主角激活点播时第一次请求发送给它的注意。
我们可以使用 OnActivateAsync() 从外部稳定商店也许填充状态按需通读，如中所示。 或者，我们可以填充状态更改为打开一个计时器，并说汇率参与者提供基于最新的货币汇率的转换函数。 此参与者可以定期填充从外部服务的状态、 说每隔 5 秒钟，然后使用状态转换函数。 请参阅下面的示例︰

## 智能缓存代码示例 – 率转换器

```
...

private List<ExchangeRate> _rates;
private IActorTimer _timer;

public Task Activate()
{
    // registering a timer that will live as long as the actor...
    _timer = this.RegisterTimer((obj) =>
    {
        Console.WriteLine("Refreshing rates...");
        return this.RefreshRates(); // call to external service/source to retrieve exchange rates
    },
    null,
    TimeSpan.FromSeconds(0), // start immediately
    TimeSpan.FromSeconds(5)); // refresh every 5 seconds

    return TaskDone.Done;
}

public Task RefreshRates()
{
    // this is where we will make an external call and populate rates
    return TaskDone.Done;
}

```

实质上是智能高速缓存提供了︰

* 从内存的服务请求由高的吞吐量低滞后时间。
* 单实例路由和单线程的请求具有持久存储中不存在争用的项的序列化。
* 语义运算，例如，排队 （工作项）。
* 轻松实现直写或写隐藏。
* LRU （最近最少使用） 项 （资源管理） 的自动移出。
* 自动弹性和可靠性。


## 下一步行动
[图案︰ 分布式的网络和关系图](service-fabric-reliable-actors-pattern-distributed-networks-and-graphs.md)

[图案︰ 资源管理](service-fabric-reliable-actors-pattern-resource-governance.md)

[图案︰ 有状态的服务组合](service-fabric-reliable-actors-pattern-stateful-service-composition.md)

[图案︰ 物联网](service-fabric-reliable-actors-pattern-internet-of-things.md)

[图案︰ 分布式的计算](service-fabric-reliable-actors-pattern-distributed-computation.md)

[一些反模式](service-fabric-reliable-actors-anti-patterns.md)

[服务结构演员简介](service-fabric-reliable-actors-introduction.md)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-pattern-smart-cache/smartcache-arch.png
