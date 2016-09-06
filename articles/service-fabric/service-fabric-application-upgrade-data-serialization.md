---
ms.openlocfilehash: aee899248063eebee6cbe5b285c55ef59a18f52e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="我们的服务结构应用程序升级︰ 数据序列化"
   description="数据序列化，以确保成功的应用程序升级的最佳做法。"
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
   ms.author="jesseb"/>


# 我们的服务结构应用程序升级︰ 数据序列化

在[应用程序的滚动升级](service-fabric-application-upgrade.md)，升级被应用到节点，一次一个升级域的子集。 在此过程中，某些升级域将在您的应用程序的较新版本，部分升级域将在您的应用程序的旧版本。 在此期间，新版本的应用程序必须能够读取数据的旧版本和旧版本的应用程序必须能够读取数据的新版本。 如果不向前和向后兼容的数据格式，则升级可能会失败或数据可能会丢失。 本文讨论了构成您的数据格式和最佳做法，以确保您的数据向前和向后兼容。


## 是什么使您的数据格式？

服务结构中保存和复制的数据来自于 C# 类。 对于使用[可靠的集合](service-fabric-reliable-services-reliable-collections.md)的应用程序，这就是可靠的词典和队列中的对象。 对于使用[有状态的可靠者](service-fabric-reliable-actors-introduction.md)的应用程序，这就是主角的备份状态。 这些 C# 类必须是可序列化进行保存和复制。 因此，数据格式定义的字段和属性被序列化，以及如何为其进行序列化。 例如，在`IReliableDictionary<int, MyClass>`序列化的数据是`int`和序列化`MyClass`。

### 数据格式更改

数据格式由 C# 类，由于对类进行更改可能会导致数据格式更改。 必须格外小心，以确保滚动升级可以处理的数据格式更改。 可能会导致数据格式更改的示例︰

- 添加或删除字段或属性
- 重命名字段或属性
- 更改的字段或属性的类型
- 更改类名称或命名空间

### 默认序列化程序

即使数据是在更旧或*更新*版本中，序列化程序通常负责读取数据并插入当前版本中，进行反序列化。 默认的序列化程序[数据约定序列化程序](https://msdn.microsoft.com/library/ms733127.aspx)，它具有明确定义的版本控制规则。 集合允许序列化程序重写，但可靠的演员目前并不可靠。 数据序列化起着重要的作用，在启用滚动升级。 数据约定序列化是建议用于服务结构的应用程序的序列化程序。


## 数据格式是如何影响滚动升级

在滚动升级过程有两个序列化程序可能对您的数据的更旧或*更新*版本会遇到的主要方案︰

1. 新的序列化程序节点升级并重新启动后，将加载已保存的数据到旧版本的磁盘。
2. 滚动升级期间，群集可能混合包含有旧版本和新版本的代码。 因为复制副本可能放在不同的域中升级中，您的数据的新的和旧版本可能会遇到的序列化程序 （其本身可能是新的或旧版本）。

> [AZURE.NOTE] "新版本"和"旧版本"此处引用代码正在运行的版本。 "新序列化程序"是指您的应用程序的新版本中执行的序列化程序代码。 "新建数据"指您的应用程序的新版本 C# 序列化类。

两个版本的代码和数据的格式必须是向前和向后兼容。 它们不是兼容的如果在滚动升级可能会失败或数据可能会丢失。 滚动升级可能会失败，因为代码或序列化程序可能引发异常或故障时遇到另一个版本。 如果，例如，添加新的属性，但旧的序列化程序将丢弃它反序列化期间，数据可能会丢失。


## 数据协定

数据协定是推荐的解决方案确保您的数据是兼容的。 它具有添加、 删除和更改字段的定义完善的版本化规则。 它还具有支持为处理未知字段、 挂钩到序列化和反序列化过程中，和类继承。 有关详细信息，请参阅[使用数据协定](https://msdn.microsoft.com/library/ms733127.aspx)。


## 下一步行动

[升级的教程](service-fabric-application-upgrade-tutorial.md)

[升级参数](service-fabric-application-upgrade-parameters.md)

[高级的主题](service-fabric-application-upgrade-advanced.md)
