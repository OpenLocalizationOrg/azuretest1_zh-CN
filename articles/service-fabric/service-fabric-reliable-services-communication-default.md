---
ms.openlocfilehash: 505cfcda9cee288fec0cc01569a7e029a27bfc26
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="提供的服务结构的默认通信栈"
   description="本文介绍了为服务和客户端进行通信的可靠服务的框架提供的默认通信堆栈。"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="08/27/2015"
   ms.author="bharatn@microsoft.com"/>

# 默认通信栈提供可靠服务框架
对于服务的作者并不限于通信 （WebAPI，WCF 等等） 的堆栈的特定实现，框架提供了可用于安装的服务和客户端之间的通信的客户端和服务端通信部分。

> [AZURE.NOTE] 请更新至最新的 nuget packges，以获得以下提及的功能。

## 服务通信侦听器
默认通信侦听器服务实现中`ServiceCommunicationListener`类

```csharp

public class ServiceCommunicationListener<TServiceImplementation> : ICommunicationListener where TServiceImplementation : class
{
    public ServiceCommunicationListener(TServiceImplementation serviceImplementationType);
    public ServiceCommunicationListener(TServiceImplementation serviceImplementationType, string endpointResourceName);

    public void Abort();
    public Task CloseAsync(CancellationToken cancellationToken);
    public void Initialize(ServiceInitializationParameters serviceInitializationParameters);
    public Task<string> OpenAsync(CancellationToken cancellationToken);
}

```
服务服务实现，并且想要公开为其客户端的方法被定义为从继承的接口中的异步方法`IService`接口。 然后可以只需实例化该服务`ServiceCommunicationListener`对象并返回其在[`CreateCommunicationListener`方法](service-fabric-reliable-services-communication.md)。 例如，若要设置此通信栈的 HelloWorld 服务代码可能定义如下。

```csharp

[DataContract]
public class Message
{
    [DataMember]
    public string Content;
}

public interface IHelloWorld : IService
{
    Task<Message> GetGreeting();
}

public class HelloWorldService : StatelessService, IHelloWorld
{
    public const string ServiceTypeName = "HelloWorldUsingDefaultCommunicationType";

    private Message greeting = new Message() { Content = "Default greeting" };

    protected override ICommunicationListener CreateCommunicationListener()
    {
        return new ServiceCommunicationListener<HelloWorldService>(this);
    }

    public Task<Message> GetGreeting()
    {
        return Task.FromResult(this.greeting);
    }
}

```
> [AZURE.NOTE] 参数和返回类型在服务界面中，例如邮件上面的类，应是可序列化的.net [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx)。


## 编写客户端通讯的 ServiceCommunicationListener
对客户端进行通信的服务使用`ServiceCommunicationListener`，该框架提供`ServiceProxy`类。

```csharp

public abstract class ServiceProxy : IServiceProxy
{
...

    public static TServiceInterface Create<TServiceInterface>(Uri serviceName);

...
}

```

客户端可以实现相应的服务接口的服务代理对象的实例化并调用代理对象的方法。

```csharp

var helloWorldClient = ServiceProxy.Create<IHelloWorld>(helloWorldServiceName);

var message = await helloWorldClient.GetGreeting();

Console.WriteLine("Greeting is {0}", message.Content);


```

>[AZURE.NOTE] 通讯框架负责传播服务向客户端引发的异常。 因此异常处理逻辑在客户端使用 ServiceProxy 可直接处理的服务可能引发的 execeptions。
 
