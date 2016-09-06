---
ms.openlocfilehash: b654cabfbadd3d39fe9eecbaebb1ad05c7270f67
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="服务的通信模型概述"
   description="本文介绍了由可靠的服务 api 支持的通信模型的基础知识。"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="vipulm"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="08/27/2015"
   ms.author="bharatn@microsoft.com"/>

# 服务的通信模型

可靠的服务编程模型允许服务作者指定他们想要使用公开的服务终结点并提供了抽象，客户端可以使用它来发现并与服务终结点进行通信的通信机制。

## 设置服务通信栈

可靠的服务 API 允许服务编写者到他们选择的通信栈的插件在服务中实现下面的方法在其服务中，

```csharp

protected override ICommunicationListener CreateCommunicationListener()
{
    ...
}

```

`ICommunicationListener`接口定义必须实现通信侦听器服务的接口。

```csharp

public interface ICommunicationListener
{
    void Initialize(ServiceInitializationParameters serviceInitializationParameters);

    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```
通过[服务清单](service-fabric-application-model.md)的端点部分介绍了端点所需的服务。

```xml

<Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint" Protocol="http" Type="Input" />
    <Endpoints>
</Resources>

```

通信侦听器可以访问分配给它从终结点资源`CodePackageActivationContext`在`ServiceInitializationParameters`，然后开始打开时侦听请求。

```csharp

var codePackageActivationContext = this.serviceInitializationParameters.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] 终结点资源共有的整个服务包和服务结构的服务包被激活时分配。 因此，所有副本都承载于同一 ServiceHost 共享同一个端口。 这意味着，应支持通信侦听器端口共享。 这样的推荐方式是通信侦听器来生成侦听地址时使用的分区 Id 和副本/实例 Id。

```csharp

var replicaOrInstanceId = 0;
var parameters = this.serviceInitializationParameters as StatelessServiceInitializationParameters;
if (parameters != null)
{
  replicaOrInstanceId = parameters.InstanceId;
}
else
{
  replicaOrInstanceId = ((StatefulServiceInitializationParameters) this.serviceInitializationParameters).ReplicaId;
}

var nodeContext = FabricRuntime.GetNodeContext();

var listenAddress = new Uri(
    string.Format(
        CultureInfo.InvariantCulture,
        "{0}://{1}:{2}/{5}/{3}-{4}",
        scheme,
        nodeContext.IPAddressOrFQDN,
        port,
        this.serviceInitializationParameters.PartitionId,
        replicaOrInstanceId,
        Guid.NewGuid().ToString()));

```

## 客户端与服务通信
可靠的服务 API 提供了以下抽象这写入客户端的通信与方便的服务。

## ServicePartitionResolver
ServicePartitionResolver 类可以帮助客户确定在运行时的服务结构服务的终结点。

> [AZURE.TIP] 确定服务的端点的过程被称为服务终结点解析服务结构术语中。

```csharp

public delegate FabricClient CreateFabricClientDelegate();

// ServicePartitionResolver methods

public ServicePartitionResolver(CreateFabricClientDelegate createFabricClient);

Task<ResolvedServicePartition> ResolveAsync(Uri serviceName,
    long partitionKey,
    CancellationToken cancellationToken);

Task<ResolvedServicePartition> ResolveAsync(ResolvedServicePartition previousRsp,
    CancellationToken cancellationToken);


```
> [AZURE.NOTE] FabricClient 是用于传递到服务结构群集服务结构在群集上的各种管理操作的对象。

通常客户端代码不需要处理的`ServicePartitionResolver`直接。 它是创建并传递给其他的帮助程序类可靠服务的 API 中内部使用冲突解决程序和与服务帮助客户端进行通信。

## CommunicationClientFactory
`ICommunicationClientFactory` 定义生成可以交谈的 ServiceFabric 服务的客户端的通讯客户端工厂由实现的基接口。 CommunicationClientFactory 的实现将取决于服务结构服务客户端想要传达给使用通信栈。 可靠的服务 API 提供了 CommunicationClientFactoryBase<TCommunicationClient>提供的基实现`ICommunicationClientFactory`接口，并执行一些常见的所有通讯堆栈的任务。(如使用`ServicePartitionResolver`来确定服务终结点)。 客户端通常实现抽象的 CommunicationClientFactoryBase 类，以处理通信栈特定逻辑。

```csharp

protected CommunicationClientFactoryBase(
    ServicePartitionResolver servicePartitionResolver = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IEnumerable<Type> doNotRetryExceptionTypes = null);


public class MyCommunicationClient : ICommunicationClient
{
    public MyCommunicationClient(MyCommunicationChannel communicationChannel)
    {
      this.CommunicationChannel = communicationChannel;
    }
    public MyCommunicationChannel CommunicationChannel { get; private set; }
    public ResolvedServicePartition ResolvedServicePartition;

}

public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient1 client)
    {
        throw new NotImplementedException();
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(ResolvedServiceEndpoint endpoint, CancellationToken cancellationToken)
    {
        throw new NotImplementedException();
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
        throw new NotImplementedException();
    }

    protected override bool ValidateClient(ResolvedServiceEndpoint endpoint, MyCommunicationClient client)
    {
        throw new NotImplementedException();
    }
}

```

## ServicePartitionClient
`ServicePartitionClient` 使用 CommunicationClientFactory （和内部 ServicePartitionResolver），并帮助与服务通信通过处理公共沟通和服务结构瞬时异常的重试次数。

```csharp

public ServicePartitionClient(
    ICommunicationClientFactory<TCommunicationClient> factory,
    Uri serviceName,
    long partitionKey);

public async Task<TResult> InvokeWithRetryAsync<TResult>(
    Func<TCommunicationClient, Task<TResult>> func,
    CancellationToken cancellationToken,
    params Type[] doNotRetryExceptionTypes)

```

典型的使用模式，如下所示，

```csharp

public MyCommunicationClientFactory myCommunicationClientFactory;
public Uri myServiceUri;

... other client code ..

// Call the service corresponding to the partitionKey myKey.

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myKey);

  var result = await myServicePartitionClient.InvokeWithRetryAsync(
      client =>
      {
        // Communicate with the service using the client.
        throw new NotImplementedException();
      },
      CancellationToken.None);


... other client code ...

```

## 下一步行动
* [提供可靠的服务框架的默认通信栈](service-fabric-reliable-services-communication-default.md)

* [基于 WCF 通信栈提供可靠的服务框架](service-fabric-reliable-services-communication-wcf.md)

* [编写使用使用 WebAPI 通信栈的可靠服务 API 服务](service-fabric-reliable-services-communication-webapi.md)
 
