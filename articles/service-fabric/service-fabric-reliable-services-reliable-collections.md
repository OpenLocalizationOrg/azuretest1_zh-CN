---
ms.openlocfilehash: a4366bbc207ace8b28c7abe49f6ff4e93df5076f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="可靠的集合"
   description="可靠的集合，您可以编写高度可用、 可伸缩和低延迟云应用程序。"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,jessebenson"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="08/05/2015"
   ms.author="mcoskun"/>

# 可靠的集合

可靠的集合，您可以编写高度可用，可伸缩且低延迟云应用程序，就好像您正在编写一个计算机应用程序。
中的类`Microsoft.ServiceFabric.Data.Collections`命名空间提供了一套自动使您状态高度可用的现成的集合。
开发人员只需要到可靠的集合 Api 进行编程，并让可靠管理复制和本地状态的集合。

可靠的集合和其他高可用性技术 （如 Redis、 Azure 表服务和 Azure 队列服务） 之间的关键区别是，状态保留本地服务实例中同时还进行高可用性。
这意味着︰

1. 所有读取都是本地，这将导致低延迟和高吞吐量读取。
2. 所有写入操作会导致网络 Io 的最小数目，这会导致低延迟和高吞吐量写入。

![集合的演化图像。](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

可靠的集合可以被认为是自然的演变`System.Collections`类︰ 一种全新的集合的旨在用于云和多计算机应用程序而无需开发人员增加复杂性。
在这种情况下，它们是︰

1. 复制︰ 状态更改复制以实现高可用性。
2. 保持不变︰ 保持数据到磁盘，针对大规模中断 （例如，数据中心电源中断） 的耐久性。
3. 异步︰ Api 是异步来确保线程不会阻止时导致 IO。
4. 事务︰ Api 使用交易记录，以便您可以轻松地管理多个可靠集合内服务的抽象。

可靠的集合提供强一致性保证出厂时才能进行有关应用程序状态的理由更容易。
强的一致性被通过确保提交仅完成整个事务应用上的副本包括主仲裁后的事务。
为了实现较弱一致性，应用程序可以确认回客户端 / 申请人在异步提交前的返回。

可靠的集合 Api 是并发集合 Api 的演变 (见`System.Collections.Concurrent`命名空间):

1. 异步︰ 返回由于操作复制，并保持与可靠的集合，不同的任务。
2. 没有输出参数︰ 使用`ConditionalResult<T>`返回一个布尔值，而不是输出参数的值。 `ConditionalResult<T>` 就像`Nullable<T>`但不要求 T 是一个结构。
3. 事务处理︰ 使用事务对象使用户能够对在事务中的多个可靠集合组操作。

今天，`Microsoft.ServiceFabric.Data.Collections`包含两个集合︰

1. [可靠的词典](https://msdn.microsoft.com/library/azure/dn971511.aspx)︰ 表示复制、 事务性和异步键/值对的集合。 类似于`ConcurrentDictionary`，键和值可以是任何类型。
2. [可靠的队列](https://msdn.microsoft.com/library/azure/dn971527.aspx)︰ 表示复制、 事务性和异步严格先入先出 (FIFO) 队列。 类似于`ConcurrentQueue`的值可以是任何类型。

## 隔离级别
隔离级别是可实现的程度隔离措施。
隔离是指事务行为像只允许一个事务要传输的数据在任何给定时间点的系统。

可靠的集合自动选择要用于根据操作和角色的副本给定的读取操作的隔离级别。

有两种可靠的集合支持的隔离级别︰

- **可重复的读取**:"指定语句不能读取数据的已修改但尚未提交的其它事务，任何其他事务都可以修改在当前事务完成之前已由当前事务读取的数据。 ()"https://msdn.microsoft.com/en-us/library/ms173763.aspx
- **快照**:"指定在事务中任何语句读取的数据将在事务开始时存在的数据的事务一致性版本。 交易记录只能识别事务开始之前提交的数据修改。 当前交易记录的开始之后，由其它事务所做的数据修改将不可见于在当前事务中执行的语句。 其效果是在事务开始时存在，在一个事务中的语句就像获取已提交数据的快照。 ()"https://msdn.microsoft.com/en-us/library/ms173763.aspx

可靠的词典和可靠队列支持读您写。
换句话说，任何写在一个事务内的将可见到属于同一事务下读。

### 可靠的词典
| 操作 \ 角色      | 主要          | 第二        |
| --------------------- | :--------------- | :--------------- |
| 单个实体读取    | 可重复的读取  | 快照         |
| 枚举 \ 计数   | 快照         | 快照         |

### 可靠的队列
| 操作 \ 角色      | 主要          | 第二        |
| --------------------- | :--------------- | :--------------- |
| 单个实体读取    | 快照         | 快照         |
| 枚举 \ 计数   | 快照         | 快照         |

## 持久性模型
可靠的状态管理器和可靠集合按照日志和检查点被称为持久性模型。
这是一个模型，其中每个状态的更改是在磁盘上记录，只应用在内存中。
完成状态本身仅保持偶尔 （亦即。 检查点）。
它提供的好处是︰

- 增量会转换为顺序仅限追加的写入磁盘上以提高性能。

为了更好地了解日志和检查点模型，让我们首先看一下无限磁盘的情况。
之前它被复制，可靠状态管理器记录每个操作。
这使得可靠的集合，是只在内存中的操作。
由于日志保持不变，即使在该复制副本失败，需要重新启动时，可靠状态管理器将在其日志重播该复制副本已失去的所有操作有足够的信息。
磁盘是无限，永远不需要被删除的日志记录和可靠的集合只需要管理的内存中状态。

现在让我们看一下有限磁盘的情况。
在一个点可靠状态管理器将用尽磁盘空间。
这种情况的可靠状态管理器之前需要截断其日志来为新记录腾出空间。
它将请求到检查点可靠集合它们的内存中状态。
它是可靠集合的责任，以保持其状态为止。
一旦可靠集合完成其检查点，可靠状态管理器可以截断日志以释放磁盘空间。
这种方式，需要重新启动复制副本时，可靠的集合将恢复其执行检查点操作的状态，并可靠的状态管理器将恢复并播放自检查点以来发生的所有状态更改。

## 锁定
在可靠的集合，所有事务都都将分两个阶段︰ 事务就不会释放以中止或提交事务终止之前获取它的锁。

可靠的集合始终采用排他锁。
对于读取，锁定是依赖于几个因素。
使用 Shapshot 隔离任何读的操作完成是可用的锁。
任何可重复读取默认操作所需的共享锁。
但是，对于支持可重复的读取任何读操作，用户可以要求提供更新锁而不是共享锁。
更新锁是不对称的锁，用于防止常见形式的死锁在多个交易记录锁定为稍后可能更新资源时发生。

锁兼容性矩阵可在下面找到︰

| 请求 \ 授予 | 无         | 共享       | Update      | 独占    |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| 共享            | 没有冲突  | 没有冲突  | Conflict    | Conflict     |
| Update            | 没有冲突  | 没有冲突  | Conflict    | Conflict     |
| 独占         | 没有冲突  | Conflict     | Conflict    | Conflict     |

请注意，在可靠的集合 Api 中的超时参数用作死锁检测。
例如，两个交易记录 （T1 和 T2） 正在尝试读取和更新 K1。
很可能他们出现死锁，因为他们两个最后共享锁定。
在这种情况下，一个或两个操作将超时。

请注意，上面的死锁情况如何更新锁可以防止死锁的极好的例子。

## 建议

- **不要**修改由读取操作返回的自定义类型的对象 (例如`TryPeekAsync`或`TryGetAsync`)。 可靠的集合，就像并发集合，返回对这些对象并不是副本的引用。
- 返回自定义类型的对象，修改它之前**做**的深层副本。 由于结构和内置类型是通过值传递，需要执行它们的深层副本。
- **切勿**使用`TimeSpan.MaxValue`为超时。 应使用超时来检测死锁。
- **不要**创建另一个事务内的事务`using`语句因为它可能导致死锁。

下面是需要记住的一些事项︰

- 默认超时值是可靠的所有集合 Api 为 4 秒。 大多数用户不应重写此。
- 默认取消标记是`CancellationToken.None`在所有可靠的集合 Api 中。
- 枚举是快照一致在一个集合中。 但是，多个集合的枚举不一致跨集合。
- 来实现高可用性的可靠的集合，每个服务应具有至少一个目标和最小副本集大小为 3。

## 下一步行动

- [可靠的服务快速入门](service-fabric-reliable-services-quick-start.md)
- [服务结构 Web API 服务入门](service-fabric-reliable-services-communication-webapi.md)
- [高级编程模型的可靠服务的使用情况](../Service-Fabric/service-fabric-reliable-services-advanced-usage.md)
- [可靠的集合的开发人员参考](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)