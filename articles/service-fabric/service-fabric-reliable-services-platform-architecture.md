---
ms.openlocfilehash: 91fe7953eec4afff656f8cbd1d9a09a6d458d89a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="服务结构可靠服务的体系结构"
   description="可靠的服务体系结构的高级别概述"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/13/2015"
   ms.author="alanwar"/>

# 可靠的服务体系结构

服务结构可靠服务可能有状态或无状态。 在本文中介绍的特定体系结构中运行每种类型的服务。
有状态的和无状态服务之间的差异的详细信息，请参阅[可靠服务概述](../Service-Fabric/service-fabric-reliable-services-introduction.md)。

## 可靠的有状态服务

### 有状态的可靠服务的体系结构关系图
![体系结构关系图](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### 可靠的有状态服务

有状态的可靠服务可以从 StatefulService 或 StatefulServiceBase 的类派生。 这些基类提供的服务结构和提供各种支持和有状态服务接口与服务结构，以及参与作为一项服务在群集服务的结构中的抽象级别。
StatefulService 从 StatefulServiceBase;StatefulServiceBase 提供服务的更大的灵活性，但需要对服务结构内部的更多理解。
编写使用 StatefulService 和 StatefulServiceBase 类的服务的细节的详细信息，请参阅[可靠服务概述](../Service-Fabric/service-fabric-reliable-services-introduction.md)和[可靠服务高级用法](../Service-Fabric/service-fabric-reliable-services-advanced-usage.md)。

这两个基本类管理的生存期和服务实现的角色。 如果服务实现有工作要做在服务实现生命周期中的这些时间点或者如果它想要创建通信侦听器对象，服务实现可能重写虚拟方法的任一基类。 请注意，尽管一个服务实现，可能实现公开上，图中的 ICommunicationListener，它自己的通信侦听器对象通信侦听器由服务结构服务实现使用通信侦听器服务结构实现。

您有状态的可靠服务使用可靠状态管理器利用可靠的集合。 可靠的集合是本地数据结构是高可用性服务，也就是说，始终是可用的而不考虑服务的故障切换。 由一个可靠的状态提供程序实现可靠的集合中的每个类型。
可靠的集合的详细信息请参阅[可靠的集概述](service-fabric-reliable-services-reliable-collections.md)

### 可靠的状态管理器和提供程序

可靠的状态管理器是管理的稳定状态提供程序，并具有功能来创建、 删除、 枚举和确保持久和高度可用的稳定状态提供程序的对象。 可靠的状态提供程序实例表示如字典或队列的持久性和高可用的数据结构的一个实例。 每个的稳定状态提供程序用于公开有状态服务用于与可靠的状态提供程序进行交互的接口。 例如使用 IReliableDictionary 与可靠的词典时使用 IReliableQueue 接口与可靠队列交互。 所有的稳定状态提供程序实现 IReliableState 接口。

可靠的状态管理器具有名为 IReliableStateManager 的允许其访问的有状态的服务实现的接口。 通过 IReliableStateManager 将返回到可靠的状态提供程序的接口。  

可靠的状态管理器具有动态插件体系结构的体系结构，以便可以动态地接新的可靠的集合类型。

可靠的词典和可靠队列基于高性能版本差异存储区的实现。

### 事务性复制器

事务性复制器组件负责确保状态的服务，即内可靠状态管理器和可靠的集合的状态，在运行您的服务的所有副本都一致和状态也是保存在日志中。 通过专用的机制执行事务性复制程序可靠状态管理器接口。

事务性复制器使用的网络协议进行通讯与其他副本的服务实例的状态，以便所有副本都具有最新的状态信息。

事务性复制器使用日志保持状态信息，这样的状态信息仍然有效的进程或节点崩溃。 写入日志的接口是通过专用的机制。

### Log

日志组件提供了高性能持久存储区可以优化的写入旋转或稳定状态磁盘和也优化为最有效地利用磁盘空间。 日志的设计为永久存储器 （例如硬盘） 是以本地方式运行状态服务以实现低延迟和高吞吐量，而不是本地节点的持久性存储的节点。

日志组件使用两种类型的日志文件。 没有节点范围内共享的日志文件应仅用于该日志文件的磁盘上。 此文件被放在服务结构节点工作目录中。 为您的服务的每个副本还具有专用的日志文件并被放置在该服务的工作目录中。 共享的日志时专用的日志文件是其终极目标则保存状态信息的过渡区域。 在此设计中的状态信息是首先写入共享日志文件，然后延迟转移到到背景中专用的日志文件。 以这种方式共享日志写入者必须最低的延迟和高吞吐量，从而使服务能够更快地取得了进展。

但是当日志组件配置优化固态磁盘使用 OptimizeForLocalSSD 设置时，状态信息直接写入专用的日志文件，并且绕过共享的日志文件。 固态磁盘头移动的争用的延迟会，因为用于直接写入专用的日志文件没有处罚。

当日志组件进行了优化，以尽量减少使用的磁盘空间使用 OptimizeLogForLowerDiskUsage 时，专用的日志文件将被创建为 NTFS 稀疏文件。  由于日志文件通常不是总是摆满状态的信息，使用稀疏文件允许超量配置可用的多个副本的磁盘空间。 如果不是这样的日志文件空间都已预分配配置和日志组件可以直接写入文件中具有最高的性能。

写入日志的最少的用户模式界面，除了作为一个内核模式驱动程序写入日志。 通过运行作为一个内核模式驱动程序日志可以提供最高性能的使用它的所有服务。

有关配置日志的详细信息请参阅[配置有状态可靠的服务](../Service-Fabric/service-fabric-reliable-services-configuration.md)。

## 可靠的无状态服务

### 无国界的可靠服务的体系结构关系图
![体系结构关系图](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### 可靠的无状态服务

无状态的服务实现派生从 StatelessService 或 StatelessServiceBase 类的 StatelessServiceBase 类，允许比 StatelessService 更大的灵活性。
这两个基本类管理的生命周期和角色服务。 如果您的服务有工作要做在服务生命周期中的这些时间点或者如果它想要创建通信侦听器对象，服务实现可能重写虚拟方法的任一基类。 请注意，尽管您的服务可以实现公开上，图中的 ICommunicationListener，它自己的通信侦听器对象通信侦听器由服务结构为该服务实现使用通信侦听程序实现的服务结构。

编写使用 StatelessService 和 StatelessServiceBase 类的服务的细节的详细信息，请参阅[可靠服务概述](../Service-Fabric/service-fabric-reliable-services-introduction.md)和[可靠服务高级用法](../Service-Fabric/service-fabric-reliable-services-advanced-usage.md)。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

有关服务结构的详细信息，请参阅︰

[可靠的服务概述](../Service-Fabric/service-fabric-reliable-services-introduction.md)

[快速入门](service-fabric-reliable-services-quick-start.md)

[可靠的集概述](service-fabric-reliable-services-reliable-collections.md)

[可靠的服务高级用法](../Service-Fabric/service-fabric-reliable-services-advanced-usage.md)

[可靠的服务配置](../Service-Fabric/service-fabric-reliable-services-configuration.md)  
 