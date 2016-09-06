---
ms.openlocfilehash: 0ea9642acfe1bc9fbc62dfa11c42c50b25b2f009
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="WCF 基于通信栈提供可靠服务 API"
   description="本文介绍了基于 WCF 通信堆栈提供可靠服务的 api。"
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

# WCF 服务可靠的基于通信栈
可靠的服务框架允许服务作者来决定他们想要使用其服务的通信栈。 他们通过他们选择的通讯堆栈可以插件`ICommunicationListener`返回的[`CreateCommunicationListener`](../service-fabric-reliable-service-communication.md)方法。 该框架为希望使用基于 WCF 通信服务作者提供通信栈中，基于 WCF 实现。

## WCF 通信侦听器
WCF 特定实现的`ICommunicationListener`提供的`WcfCommunicationListener`类。

```csharp

public WcfCommunicationListener(
    Type communicationInterfaceType,
    Type communicationImplementationType);

protected override ICommunicationListener CreateCommunicationListener()
    {
        WcfCommunicationListener communicationListener = new WcfCommunicationListener(typeof(ICalculator), this)
        {
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // which identifies the endpoint that the wcf servicehost should listen on.
            //
            EndpointResourceName = "ServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            Binding = this.CreateListenBinding()
        };

        return communicationListener;
    }

```

## 编写客户端进行 WCF 通信栈
框架提供了用于编写客户端通讯的使用 WCF 服务， `WcfClientCommunicationFactory`，这是 WCF 特定实现的[`ClientCommunicationFactoryBase`](../service-fabric-reliable-service-communication.md)。

```csharp

public WcfCommunicationClientFactory(
    ServicePartitionResolver servicePartitionResolver = null,
    Binding binding = null,
    object callback = null,
    IList<IExceptionHandler> exceptionHandlers = null,
    IEnumerable<Type> doNotRetryExceptionTypes = null)

```

可以从访问 WCF 通信通道`WcfCommunicationClient`创建的 `WcfCommunicationClientFactory`

```csharp

public class WcfCommunicationClient<TChannel> : ICommunicationClient where TChannel : class
{
    public TChannel Channel { get; }
    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}

```

客户端代码可以使用`WcfCommunicationClientFactory`以及`ServicePartitionClient`来确定服务终结点，并使与该服务通信。

```csharp

//
// Create a service resolver for resolving the endpoints of the calculator service.
//
ServicePartitionResolver serviceResolver = new ServicePartitionResolver(() => new FabricClient());

//
// Create the binding.
//
NetTcpBinding binding = CreateClientConnectionBinding();

var clientFactory = new WcfCommunicationClientFactory<ICalculator>(
    serviceResolver,// ServicePartitionResolver
    binding,        // Client binding
    null,           // Callback object
    null);          // donot retry Exception types


//
// Create a client for communicating with the calc service which has been created with
// Singleton partition scheme.
//
var calculatorServicePartitionClient = new ServicePartitionClient<WcfCommunicationClient<ICalculator>>(
    clientFactory,
    ServiceName);

//
// Call the service to perform the operation.
//
var result = calculatorServicePartitionClient.InvokeWithRetryAsync(
    client => client.Channel.AddAsync(2, 3)).Result;


```
 
