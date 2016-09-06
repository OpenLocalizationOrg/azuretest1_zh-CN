---
ms.openlocfilehash: 646d8d00ffd56f84eb21412ca34ae442e1d67fef
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 服务结构演员资源治理的设计模式"
   description="有关如何使用服务结构参与者建模应用程序需要缩放，但使用的设计模式约束资源"
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

# 可靠的操作者设计模式︰ 资源管理
此模式和相关的方案是由开发人员轻松地识别 — — 企业或其他方式 — — 谁拥有有限资源部署或在云中，这不能立即对其进行扩展或该希望运送大型缩放应用程序和数据复制到云中。

企业在这些有限的资源，例如数据库、 不断增长的硬件上运行。 对于长企业历史记录的任何人都知道这是常见情形内部。 即使在云规模大，我们已经看到云服务试图超过 64k 地址/端口元组之间的连接的 TCP 限制或尝试连接到一个基于云的数据库时，限制并发连接的数量时，发生这种情况。

通常在过去，这被解决通过调节通过基于消息的中间件或自定义的池和外观的机制。 很难准确地找到，尤其是当我们需要扩展的中间层，但仍保持了正确的连接数。 它是只是脆弱和复杂。

事实上，类似的智能高速缓存模式，此模式跨越多个方案和客户已有工作正常的系统用有限的资源。 他们在构建的系统，它们需要向外扩展而不仅仅是服务，但其状态在内存中，以及稳定存储中保留的状态。

下图阐释了此方案︰

![][1]

## 与参与者建模缓存方案
本质上我们建立的模型与操作者或充当代理服务器 （例如说连接，） 给资源或资源组中的多个参与者的资源的访问。 您可以然后直接管理通过各参与者的资源或使用管理资源者协调者。
若要进行更具体，我们将讨论共同需求的无需针对分区 (亦即 sharded) 的存储层的性能和可伸缩性方面的原因。
我们的第一个选项是相当基本的︰ 我们可以使用静态函数映射，并对下游资源解决我们演员。 此类函数可以返回，例如，连接字符串与给定的输入。 它完全取决于我们是如何实现该功能。 当然，这种方法带有自己缺点，如静态关系，使资源重新分区或重新映射将使用者与资源非常困难。
下面是一个非常简单的例子 — — 我们做模算术运算才能确定使用用户 Id 的数据库名称，用于标识数据库服务器地区。

## 资源管理代码示例静态解析

```csharp
private static string _connectionString = "none";

private static string ResolveConnectionString(long userId, int region)
{
    if (_connectionString == "none")
    {
        // an example of static mapping
        _connectionString = String.Format("Server=SERVER_{0};Database=DB_{0}", region, userId % 4);
    }
    return _connectionString;
}
```

简单但却非常不灵活。 现在让我们看看更高级、 更有用的方法。
首先，我们建立模型的物理资源与参与者之间的关系。 这是通过使用者调用解析器能够理解用户、 逻辑分区与物理资源之间的映射。 冲突解决程序维护自身的数据持久存储区中，但是它将被缓存以方便查找。 正如我们看到在汇率示例中前面的智能高速缓存模式，解析器可以主动提取使用计时器的最新信息。 一旦用户参与者可以解决其需要使用的资源，它将其缓存在本地变量名为 _resolution，并在其生存期内使用它。
我们如缩放输入/输出或将用户从一个资源移动到另一个选择查找基于分辨率 （下面所示） 通过简单的散列或范围散列，因为它提供了在操作中的灵活性。

![][2]

在上图中，我们看到该参与者 B23 首先解决其资源 （也称为解析） — DB1 并将其缓存。 随后的操作现在可以使用缓存的分辨率来访问受限制的资源。 由于参与者支持单线程执行，开发人员不再需要担心对资源的并发访问。
在用户和冲突解决程序参与者如下所示︰

## 资源管理代码示例 – 冲突解决程序

```csharp
public interface IUser : IActor
{
    Task UpdateProfile(string name, string country, int age);
}

[DataContract]
public class UserState
{
    [DataMember]
    private long _userId;
    [DataMember]
    private string _name;
    [DataMember]
    private string _country;
    [DataMember]
    private int _age;
    [DataMember]
    private Resolution _resolution;
}


public class User : Actor<UserState>, IUser
{
    public override async Task ActivateAsync()
    {
        State._userId = this.GetPrimaryKeyLong();
        var resolver = ActorProxy.Create<IResolver>(0);
        State._resolution = await resolver.ResolveAsync(State._userId);
        await base.ActivateAsync();
    }

    public Task UpdateProfile(string name, string country, int age)
    {
        Console.WriteLine("Using {0}", State._resolution.Resource.ConnectionString);
        // this is where we use the resource...
        return TaskDone.Done;
    }
}
```

资源管理 — 解析器示例

```csharp
public interface IResolver : IActor
{
    Task<Resolution> ResolveAsync(long entity);
}

[DataContract]
public class ResolverState
{
    ...
}

public class Resolver : Actor<ResolverState>, IResolver
{
    ...

    public Task<Resolution> ResolveAsync(long entityKey)
    {
        if (State._resolutionCache.ContainsKey(entityKey))
            return Task.FromResult(_resolutionCache[entityKey]); // return from cache

        var partitionKey = State._entityPartitions[entityKey]; // resolve partition;
        var resourceKey = State._partitionResources[partitionKey]; // resolve resource;
        var resolution =
            new Resolution()
            {
                Entity = State._entities[entityKey],
                Partition = State._partitions[partitionKey],
                Resource = State._resources[resourceKey]
            }; // create resolution

        State._resolutionCache.Add(entityKey, resolution); // cache the resolution

        return Task.FromResult(resolution);
    }

    ...
}
```

## 具有有限功能的访问资源
现在让我们看一下另一个例子;宝贵的资源，如数据库、 存储帐户和文件系统带来有限的吞吐能力的独占访问。
我们的方案是，如下所示︰ 我们想要处理事件主角名为 EventProcessor，负责处理和无法持续的事件，在这种情况下使用。简单的 CSV 文件。 虽然我们可以按照分区前面讨论方法来扩展我们的资源，我们仍需要处理的并发问题。 就是为什么我们选择的基于文件的示例来说明此特定点 — — 从多个演员到一个文件中写入将引发并发问题。 解决这个问题我们引入称为 EventWriter 拥有有限资源的独占所有权的另一个主角。 该方案如下所示︰

![][3]

我们将 EventProcessor 操作者标记为"无工作人员，"这样，运行时根据需要群集扩展它们。 因此我们不能在上面图中这些参与者使用任何标识符。 换句话说，无状态的参与者是由运行时维护的工作人员。
在下面的示例代码中，EventProcessor 使用者有两个作用︰ 它首先决定哪个 EventWriter （因此资源） 来使用，并调用所选的参与者编写处理的事件。 为简单起见，我们选择事件类型的标识符为 EventWriter 主角。 换句话说，会有一个且只有一个 EventWriter 提供对资源的单线程和独占访问此事件类型。

## 资源管理代码示例事件处理器

```csharp
public interface IEventProcessor : IActor
{
    Task ProcessEventAsync(long eventId, long eventType, DateTime eventTime, string payload);
}

public class EventProcessor : Actor, IEventProcessor
{
    public Task ProcessEventAsync(long eventId, long eventType, DateTime eventTime, string payload)
    {
        // This where we write to constrained resource...
        var eventWriterKey = ResolveWriter(eventType, eventTime);
        var eventWriter = ActorProxy.Create<IEventWriter>(eventWriterKey);

        return eventWriter.WriteEventAsync(eventId, eventType, eventTime, payload);
    }

    private long ResolveWriter(long eventType, DateTime eventTime)
    {
        // To simplify, we are returning event type as to identify the event writer actor.
        return eventType;
    }
}
```

现在让我们看看 EventWriter 主角。 它实际上并未执行其他操作与控制对受限制资源的独占访问权，在这种情况下文件和向其写入事件。
## 资源管理代码示例事件编写器

```csharp
public interface IEventWriter : IActor
{
    Task WriteEventAsync(long eventId, long eventType, DateTime eventTime, string payload);
    Task WriteEventBufferAsync(long eventId, long eventType, DateTime eventTime, string payload);
}

[DataContract]
public class EventWriterState
{
    [DataMember]
    public string _filename;
}

public class EventWriter : Actor<EventWriterState>, IEventWriter
{
    private StreamWriter _writer;

    public override Task OnActivateAsync()
    {
        State._filename = string.Format(@"C:\{0}.csv", this.Id);
        _writer = new StreamWriter(_filename);

        return base.OnActivateAsync();
    }

    public override Task OnDeactivateAsync()
    {
        _writer.Close();
        return base.OnDeactivateAsync();
    }

    public async Task WriteEventAsync(long eventId, long eventType, DateTime eventTime, string payload)
    {
        var text = string.Format("{0}, {1}, {2}, {3}", eventId, eventType, eventTime, payload);
        await _writer.WriteLineAsync(text);
        await _writer.FlushAsync();
    }
 }
```

具有单个操作者负责管理资源，我们可以添加功能，如缓冲。 我们可以缓冲传入的事件，并编写定期使用计时器或缓冲区已满时这些事件。 下面是一个简单的计时器基于示例︰
## 资源管理代码示例事件缓冲区写入程序

```csharp
[DataMember]
public class EventWriterState
{
    [DataMember]
    public string _filename;
    [DataMember]
    public Queue<CustomEvent> _buffer;
}

public class EventWriter : Actor<EventWriterState>, IEventWriter
{
    private StreamWriter _writer;
    private IActorTimer _timer;

    public override Task OnActivateAsync()
    {
        State._filename = string.Format(@"C:\{0}.csv", this.Id);
        _writer = new StreamWriter(_filename);
        State._buffer = new Queue<CustomEvent>();

        this.RegisterTimer(
            ProcessBatchAsync,
            null,
            TimeSpan.FromSeconds(5),
            TimeSpan.FromSeconds(5));

        return base.OnActivateAsync();
    }

    private async Task ProcessBatchAsync(object obj)
    {
        if (State._buffer.Count == 0)
            return;

        while (State._buffer.Count > 0)
        {
            var customEvent = State._buffer.Dequeue();
            await this.WriteEventAsync(customEvent.EventId, customEvent.EventType, customEvent.EventTime, customEvent.Payload);
        }
    }

    public Task WriteEventBufferAsync(long eventId, long eventType, DateTime eventTime, string payload)
    {
        var customEvent = new CustomEvent()
        {
            EventId = eventId,
            EventType = eventType,
            EventTime = eventTime,
            Payload = payload
        };

        State._buffer.Enqueue(customEvent);

        return TaskDone.Done;
    }
}
```

在上面的代码将可以正常工作，客户端不知道基础存储到其事件是否使它。 允许缓冲并让客户注意到他们的要求发生了什么情况，我们引入了以下的方法来让客户等待，直到他们事件写入到。CSV 文件中︰

## 资源管理代码示例 — 异步批处理

```csharp
public class AsyncBatchExecutor
{
    private readonly List<TaskCompletionSource<bool>> actionPromises;

    public AsyncBatchExecutor()
    {
        this.actionPromises = new List<TaskCompletionSource<bool>>();
    }

    public int Count
    {
        get
        {
            return actionPromises.Count;
        }
    }

    public Task SubmitNext()
    {
        var resolver = new TaskCompletionSource<bool>();
        actionPromises.Add(resolver);
        return resolver.Task;
    }

    public void Flush()
    {
        foreach (var tcs in actionPromises)
        {
            tcs.TrySetResult(true);

        }
        actionPromises.Clear();
    }
}
```

我们将使用此类来创建和维护一份未完成的任务 （为阻止客户端） 和一个定位中完成它们，一旦我们写存储缓冲的事件。
在 EventWriter 类中，我们需要做三件事︰ 演员将类标记为重入，SubmitNext() 的结果和刷新我们的计时器。 修改后的代码如下所示︰

## 资源管理代码示例 – 使用异步批处理缓冲

```csharp
public class EventWriter : Actor<EventWriterState>, IEventWriter
{
    public override Task OnActivateAsync()
    {
        State._filename = string.Format(@"C:\{0}.csv", this.GetPrimaryKeyLong());
        _writer = new StreamWriter(_filename);
        State._buffer = new Queue<CustomEvent>();
        _batchExecuter = new AsyncBatchExecutor();

        this.RegisterTimer(
            ProcessBatchAsync,
            null,
            TimeSpan.FromSeconds(5),
            TimeSpan.FromSeconds(5));

        return base.OnActivateAsync();
    }

    private async Task ProcessBatchAsync(object obj)
    {
        if (_batchExecuter.Count > 0)
        {
            // take snapshot of the batch tasks
            var batchSnapshot = _batchExecuter;
            _batchExecuter = new AsyncBatchExecutor();

            if (State._buffer.Count == 0)
                return;

            while (State._buffer.Count > 0)
            {
                var customEvent = State._buffer.Dequeue();
                await this.WriteEventAsync(customEvent.EventId, customEvent.EventType, customEvent.EventTime, customEvent.Payload);
            }

            _batchExecuter.Flush();
        }
    }
    ...

    public Task WriteEventBufferAsync(long eventId, long eventType, DateTime eventTime, string payload)
    {
        var customEvent = new CustomEvent()
        {
            EventId = eventId,
            EventType = eventType,
            EventTime = eventTime,
            Payload = payload
        };

        State._buffer.Enqueue(customEvent);

        // we are adding an incomplete task to batch executer and returning this task.
        // this will block until task is completed when we call Flush();
        return _batchExecuter.SubmitNext();
    }
}
```

看起来容易吗？ 它是很好。 但易用性 belies 的企业能力。 使用该体系结构中，我们得到︰

* 位置无关资源寻址。
* 可调的池的大小只是根据不断变化的演员代表的作用，资源的数量。
* 客户端协调池使用情况 （如所示） 或服务器端 （设想一下前面这些池图片中的每一个演员）。
* 可扩展的池添加 （只需添加参与者表示新资源）。
* 参与者 （如我们前面所示） 可以缓存从按需或使用计时器可减少命中后端资源的预缓存后端资源的结果。
* 有效的异步调度。
* 将为任何开发人员所熟悉的编码环境不只是中间件专家。

这种模式是很常见的在其中开发人员既具有约束的情况下开发针对他们需要的资源或构建大型横向扩展系统。


## 下一步行动
[图案︰ 智能高速缓存](service-fabric-reliable-actors-pattern-smart-cache.md)

[图案︰ 分布式的网络和关系图](service-fabric-reliable-actors-pattern-distributed-networks-and-graphs.md)

[图案︰ 有状态的服务组合](service-fabric-reliable-actors-pattern-stateful-service-composition.md)

[图案︰ 物联网](service-fabric-reliable-actors-pattern-internet-of-things.md)

[图案︰ 分布式的计算](service-fabric-reliable-actors-pattern-distributed-computation.md)

[一些反模式](service-fabric-reliable-actors-anti-patterns.md)

[服务结构演员简介](service-fabric-reliable-actors-introduction.md)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-pattern-resource-governance/resourcegovernance_arch1.png
[2]: ./media/service-fabric-reliable-actors-pattern-resource-governance/resourcegovernance_arch2.png
[3]: ./media/service-fabric-reliable-actors-pattern-resource-governance/resourcegovernance_arch3.png
