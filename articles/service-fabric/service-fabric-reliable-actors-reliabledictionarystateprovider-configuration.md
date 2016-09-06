---
ms.openlocfilehash: 0a7f9267e72b99ca8775febea5bf4ff1dc1bd89b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="服务结构可靠者 'ReliableDictionaryActorStateProvider' 配置概述"
   description="了解如何配置类型 ReliableDictionaryActorStateProvider 的服务结构状态演员"
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="anuragg"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/26/2015"
   ms.author="sumukhs"/>

# 配置可靠参与者-ReliableDictionaryActorStateProvider
可以通过更改为指定的使用者问题中生成在 Visual Studio 包根中的"配置"文件夹下的"settings.xml"文件修改 ReliableDictionaryActorStateProvider 的默认配置。

服务结构运行时查找预定义的节名称"settings.xml"文件中，并创建基础的运行时组件时使用的配置值。

> [AZURE.NOTE] **不要删除/修改以下配置生成 Visual Studio 解决方案中的"settings.xml"文件中的节名称。**

## 复制器安全配置
复制器安全配置用于保护在复制过程中使用的通信信道。 这意味着服务将不能看到彼此的复制流量，确保进行高可用性的数据也安全的。
默认情况下，空安全配置节不启用复制安全性。
### 节名称
&lt;ActorName&gt;ServiceReplicatorSecurityConfig
### 配置名称
请参阅[复制安全性](../service-fabric/service-fabric-replication-security.md)

## 复制配置
复制器配置用于配置复制器，它负责通过复制和保持本地的状态使参与者状态提供程序状态非常可靠。
默认配置由 Visual Studio 模板生成，并应该就足够了。 本部分讨论可用于改善复制器的其他配置。
### 节名称
&lt;ActorName&gt;ServiceReplicatorConfig
### 配置名称

|名称|单位|默认值|备注|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|秒|0.05|其在辅助等待接收发送前请先操作后复制回确认主的时间段。 作为一个响应发送其他确认发送在此时间间隔内处理的操作。||
|ReplicatorEndpoint|N/A|不是适用-RequiredParameter|设置 IP 地址和主要/次要副本复制器将使用与其他副本中的复制器进行通信的端口。 这应引用服务清单中的 TCP 资源的终结点。 请参阅[服务清单资源](service-fabric-service-manifest-resources.md)以便阅读更多关于服务清单中定义终结点资源。 |
|RetryInterval|秒|5|之后，副本复制器重新传输消息如果没有收到对操作的确认信息的时间段。|
|MaxReplicationMessageSize|字节数|50 MB|可以在单个邮件中传输的复制数据的最大大小。|
|MaxPrimaryReplicationQueueSize|工序数|1024|主队列中的操作的最大数目。 主复制程序从所有辅助复制器收到确认后，操作将释放。此值必须大于 64 和 2 的幂。|
|MaxSecondaryReplicationQueueSize|工序数|2048|辅助的队列中的操作的最大数目。 操作将使其状态持久性通过高可用性后释放。 此值必须大于 64 和 2 的幂。|
|MaxStreamSizeInMB|MB|1024|日志文件保留的空间量。 以字节为单位的大小必须大于 MaxRecordSize 字节的 16 倍。|
|MaxRecordSizeInKB|KB|1024|该副本复制器可能会在日志中写入的最大记录大小。 此值必须是 4 和大于 16 的倍数。|
|OptimizeForLocalSSD|布尔|false|当为 true 时使状态信息直接写入副本的专用的日志文件配置日志。 日志文件是在固态磁盘介质上或高度限制虚拟机磁盘 IO 频率时，这可以提供最佳性能。 当为 false 时的状态信息是第一次转移共享的日志文件，然后转移到专用的日志文件到。|
|OptimizeLogForLowerDiskUsage|布尔|false|当为 true 时日志配置为这样使用 NTFS 稀疏文件创建的复制副本的专用的日志文件。 这降低了文件的实际磁盘空间使用情况。 当为 false 时将创建的文件具有固定分配的分配提供了最佳的写入性能。|
|SharedLogId|guid|""|指定一个唯一的 guid 用于标识与此复制副本使用共享的日志文件。 通常服务不应使用此设置，但是如果指定了 SharedLogId，则 SharedLogPath 必须也指定。|
|SharedLogPath|完全限定的路径名|""|指定将在其中创建此复制副本的共享的日志文件的完全限定的路径。 通常服务不应使用此设置，但是如果指定了 SharedLogPath，则 SharedLogId 必须也指定。|


## 示例配置文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="MaxStreamSizeInMB" Value="512" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```

## 备注
BatchAcknowledgementInterval 控制复制滞后时间。 值为"0"产生最低可能滞后时间，但会使吞吐量 （如必须发送多个确认消息并处理，每个包含更少的确认）。
BatchAcknowledgementInterval 的值越大的整体复制吞吐量就越大，其代价是具有较高的操作延迟。 这直接转化到事务提交的滞后时间。

MaxStreamSizeInMB 的值确定复制器可用于将状态信息存储在该复制副本的专用的日志文件的磁盘空间量。 由于存储的信息状态用于允许另一个副本以匹配主副本的状态，就最好要有更大的日志文件因为这将减少其他复制副本以匹配主的状态所需的时间量。 但是更多日志文件可能会使用更多磁盘空间，从而减少可以驻留在特定节点的复制副本的数量。  

OptimizeForLowerDiskUsage 设置允许日志文件空间"过度调配"，以便在其日志文件的活动复制副本存储状态的详细信息时处于非活动状态的复制副本将使用较少的磁盘空间。 虽然它使多个副本驻留在节点不是由于缺乏磁盘空间，否则会出现，通过将 OptimizeForLowerDiskUsage 设置为 false 的状态信息写入日志文件更快。

OptimizeForLocalSSD 设置用于禁用写入状态信息到共享日志文件前离台到专用的日志文件。 它通常是时应设置专用的日志文件的本地磁盘存储固态介质如最小化磁盘头移动不成问题。 它还在 VM 正在执行大量的本地磁盘 IO 和本地磁盘 IO 率被大大限制由 VM 主机时使用。

MaxRecordSizeInKB 定义的记录，可以通过复制器写入到日志文件的最大大小。 在最所有情况下默认 1024KB 记录大小为最佳但是如果服务造成更大的数据项目状态信息的一部分然后此值可能需要增加。 没有什么好处，使 MaxRecordSizeInKB 小于 1024年，较小的记录只使用较小的记录所需的空间。 预计它将需要在只有极少数的情况下更改。

SharedLogId 和 SharedLogPath 设置总是一起使用，并允许的节点使用一个单独的共享的日志从默认共享日志服务。 以最佳的效率，尽可能的多个服务应指定同一个共享的日志。 共享的日志文件应该放在磁盘用于仅共享的日志文件，以便减少磁头的移动争夺了。 预计它将需要在只有极少数的情况下更改。
 
