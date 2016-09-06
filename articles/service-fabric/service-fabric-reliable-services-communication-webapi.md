---
ms.openlocfilehash: 6a855ffd3a1e42ac49453e2f13483e750354d76a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="自托管服务结构 Web API 服务与 OWIN |Microsoft Azure"
   description="此服务结构文章解释了如何实现 OWIN 自托管可靠服务中使用 ASP.NET Web API 的服务通信。"
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/23/2015"
   ms.author="vturecek"/>

# 开始使用 Microsoft Azure 服务结构 Web API 服务 OWIN 和自托管

服务结构将电源放在您手中，决定要如何您服务，与用户和互相进行通信时。 本教程着重于实现 OWIN 自托管服务结构*可靠服务*API 中使用 ASP.NET Web API 的服务通信。 我们将深入进入*可靠的服务*可插拔通信 API，并显示您分步如何设置用作示例的 Web API 与您的服务的自定义通信侦听器。 若要查看 Web API 通信侦听程序的完整示例，请检查[在 GitHub 服务结构的 web 应用程序示例](https://github.com/Azure/servicefabric-samples/tree/master/samples/Services/VS2015/WebApplication)。


## Web 服务结构中的 API 简介

ASP.NET Web API 是一个流行和强大的框架，用于构建在.NET Framework 的 HTTP Api。 通过对[www.asp.net/webapi](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)以了解有关 Web API 的详细信息，如果你不熟悉它已经在头。

Web API 服务结构中的是同一个 ASP.NET Web API 熟悉和喜欢。 区别在于如何您*主机*Web API 应用程序 (提示︰ 您不能使用 IIS)。 为了更好地理解差异，让我们将其分为两个部分︰

 1. Web API 应用程序 （您控制器、 模型等）
 2. 主机 （web 服务器，通常 IIS）

Web API 应用程序本身不能在此处更改-从 Web API 应用程序，您可能会编写过去，没有什么两样，能够只是右移大部分应用程序代码。 承载该应用程序可能与什么您习惯于如果您习惯于承载 IIS 上略有不同。 但我们进入承载部件之前，让我们开始与更熟悉部件︰ Web API 应用程序。


## 设置 Web API 应用程序

启动用来创建新的应用程序，一个无国界的服务，在 Visual Studio 2015年:

![创建新的服务结构应用程序](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

![创建一个无状态服务](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

这为我们提供了一个空的无状态服务将承载 Web API 应用程序。 我们将从零开始要如何它全部放在一起，请参阅设置应用程序。

第一步是 Web API 的某些 NuGet 程序包中拉出。 我们希望使用的包是**Microsoft.AspNet.WebApi.OwinSelfHost**。 该软件包包括所有必要的 Web API 包和*主机*包-这将是重要以后。

![](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

安装的软件包，我们可以开始构建基本的 Web API 项目结构。 如果您使用的是 Web API，项目结构应该看起来很熟悉。 通过创建基本 Web API 目录启动︰

 + App_Start
 + 控制器
 + 模型

在 App_Start 目录中添加基本的 Web API 配置类︰

 + FormatterConfig.cs

```csharp

namespace WebApi
{
    using System.Net.Http.Formatting;

    public static class FormatterConfig
    {
        public static void ConfigureFormatters(MediaTypeFormatterCollection formatters)
        {
        }
    }
}

```

 + RouteConfig.cs

```csharp

namespace WebApi
{
    using System.Web.Http;

    public static class RouteConfig
    {
        public static void RegisterRoutes(HttpRouteCollection routes)
        {
            routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { controller = "Default", id = RouteParameter.Optional }
                );
        }
    }
}

```

添加默认控制器控制器目录中︰

 + DefaultController.cs

```csharp

namespace WebApi.Controllers
{
    using System.Collections.Generic;
    using System.Web.Http;

    public class DefaultController : ApiController
    {
        // GET api/values
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5
        public void Delete(int id)
        {
        }
    }
}

```

最后，在项目根目录注册路由、 格式化程序和任何其他配置设置添加启动类。 这是在 Web API 插入到*主机*，它将稍后再重新审查。 虽然设置启动类中，创建一个接口，称为*IOwinAppBuilder*的启动类，它定义的配置方法。 虽然技术上不需要 Web API 来处理，它将允许更灵活地使用启动类的后面。

 + Startup.cs

```csharp

namespace WebApi
{
    using Owin;
    using System.Web.Http;

    public class Startup : IOwinAppBuilder
    {
        public void Configuration(IAppBuilder appBuilder)
        {
            HttpConfiguration config = new HttpConfiguration();

            FormatterConfig.ConfigureFormatters(config.Formatters);
            RouteConfig.RegisterRoutes(config.Routes);

            appBuilder.UseWebApi(config);
        }
    }
}

```

 + IOwinAppBuilder.cs

```csharp

namespace WebApi
{
    using Owin;

    public interface IOwinAppBuilder
    {
        void Configuration(IAppBuilder appBuilder);
    }
}

```

就是这样的应用程序部件。 在这里我们就已经建立了基本的 Web API 项目布局。 相比于过去编写的 Web API 项目或基本 Web API 模板，它不应该有很大反差为止。 您的业务逻辑将控制器和模型中像往常一样。

现在我们做什么有关承载这样我们实际上可以运行它？


## 承载的服务

服务结构中您的服务中运行*服务主机进程*的运行您的服务代码的可执行文件。 到当编写服务使用可靠的服务 API，并且事实上在大多数情况下，当您在.NET 中的服务结构上编写服务，只需编译服务项目。EXE，注册您的服务类型，运行您的代码。 事实上，如果您打开**Program.cs**无状态的服务项目中，您应看到︰

```csharp

public class Program
{
    public static void Main(string[] args)
    {
        try
        {
            using (FabricRuntime fabricRuntime = FabricRuntime.Create())
            {
                fabricRuntime.RegisterServiceType(Service.ServiceTypeName, typeof(Service));

                Thread.Sleep(Timeout.Infinite);
            }
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e);
            throw;
        }
    }
}

```

如果类似可疑的入口点到控制台应用程序，这是因为它是︰

![](media/service-fabric-reliable-services-communication-webapi/webapi-projectproperties.png)

有关服务主机详细信息处理和服务注册已超出本文讨论范围之内，但很重要，要知道现在**在其自身进程中运行服务代码**。

## 自 OWIN 主机托管 Web API

考虑到它自己的进程中承载了应用程序 Web API 代码，如何我们将其挂钩到 web 服务器？ 输入[OWIN](http://owin.org/)。 OWIN 是只是.NET web 应用程序和 web 服务器之间的合同。 传统上使用 ASP.NET 的 MVC 5-达 System.Web 通过 IIS 是紧密关联的 web 应用程序。 但是，Web API 实现 OWIN，它允许您编写从 web 服务器承载的 web 应用程序分离。 这允许您使用的*自我承载*OWIN web 服务器可以启动自己的过程，完全适合我们刚才描述的服务结构承载模型中。

在本文中，我们将使用 Katana 的 OWIN 主机 Web API 应用程序。 Katana 是一个开源的 OWIN 主机实现。

> [AZURE.NOTE] 了解更多有关 Katana，头部转移到[Katana 网站](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana)，并简要概述了如何使用 Katana 自承载 Web API，用于了解如何[使用 OWIN 添加到 Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api)这篇文章。


## 在 web 服务器上设置

可靠的服务 API 为您的业务逻辑提供了两个入口点︰

 + 您可以执行任何工作的负荷所需，开放式入口点方法主要用于长时间运行计算的工作负载︰

```csharp

protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}

```

 + 您可以插入在选择通信叠通信入口点︰

```csharp

protected override ICommunicationListener CreateCommunicationListener()
{
    ...
}

```

Web 服务器和其他任何可能在将来，使用如 Websocket 通信堆栈应该使用 ICommunicationListener 接口正确与系统集成。 此问题的原因将成为在下列步骤中更为明显。

首先创建一个名为 OwinCommunicationListener ICommunicationListener 的实现类︰

 + OwinCommunicationListener.cs:

```csharp

namespace WebApi
{
    using System;
    using System.Fabric;
    using System.Fabric.Description;
    using System.Globalization;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Owin.Hosting;
    using Microsoft.ServiceFabric.Services;

    public class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public void Initialize(ServiceInitializationParameters serviceInitializationParameters)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}

```

ICommunicationListener 接口提供 4 的方法来管理您的服务的通信侦听器︰

 + **初始化**︰ 这通常是在其中设置该服务侦听地址。 对于 web 服务器，这是在其中设置 URL。
 + **OpenAsync**︰ 开始侦听请求。
 + **CloseAsync**︰ 停止侦听请求，完成正在进行的任何请求，然后正常关闭。
 + **中止**︰ 取消一切并立即停止。

要开始使用，请添加 URL 路径前缀和以前创建的**启动**类的私有类成员。 这些将通过构造函数初始化和设置侦听 URL 时以后使用。 添加私有类成员来保存侦听地址和服务器处理在初始化过程中及更高版本时创建启动服务器，分别。

```csharp

public class OwinCommunicationListener : ICommunicationListener
{
    private readonly IOwinAppBuilder startup;
    private readonly string appRoot;
    private IDisposable serverHandle;
    private string listeningAddress;

    public OwinCommunicationListener(string appRoot, IOwinAppBuilder startup)
    {
        this.startup = startup;
        this.appRoot = appRoot;
    }

    ...

```

### 初始化

将此处设置的 web 服务器 URL。 要执行此操作，您需要几条信息︰

 + **URL 路径前缀**。 虽然可选，就很有必要对此进行设置现在因此可以安全地在您的应用程序中承载多个 web 服务。
 + **一个端口**。

我们抓住一个端口的 web 服务器之前，请务必了解服务结构提供了一个应用程序层，充当您的应用程序和底层操作系统上运行之间的缓冲区。 在这种情况下，服务结构提供了一种方法来配置您的服务*终结点*。 服务结构负责确保该终结点可供您使用，以便您不必与基础操作系统环境自行配置的服务。 这允许您轻松地主机服务结构在不同环境中的应用程序而无需对应用程序进行任何更改 （例如，您可以承载 Azure 中或在您自己的数据中心中的同一应用程序）。

在 PackageRoot\ServiceManifest.xml 配置 HTTP 端点︰

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="80" />
    </Endpoints>
</Resources>

```

此步骤很重要，因为服务主机进程下运行受限制的凭据 （在 Windows 上的网络服务），这意味着您的服务不能设置 HTTP 终结点在其自己的访问。 通过使用终结点配置，服务结构知道若要设置适当的 ACL 的 url 服务将侦听同时提供了一个标准配置终结点的位置。

回到 OwinCommunicationListener.cs，在获取该端口的初始化方法中获取终结点信息。 创建将侦听的服务的 URL 上然后将它保存到先前创建的类成员变量。 这将用于在 OpenAsync 中启动 web 服务器。

```csharp

public void Initialize(ServiceInitializationParameters serviceInitializationParameters)
{
    EndpointResourceDescription serviceEndpoint = serviceInitializationParameters.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = String.Format(
        CultureInfo.InvariantCulture,
        "http://+:{0}/{1}",
        port,
        String.IsNullOrWhiteSpace(this.appRoot)
            ? String.Empty
            : this.appRoot.TrimEnd('/') + '/');
}

```

请注意，在此处使用"http://+"。 这是为了确保 web 服务器正在侦听所有可用地址，包括本地主机，FQDN 和机器的 IP。

### OpenAsync

在初始化后由平台调用 OpenAsync。 这是使用在初始化以打开 web 服务器中创建的地址的位置。

实现 OpenAsync 是为什么 web 服务器 （或任何通讯堆栈） 作为 ICommunicationListener 实现的最重要原因之一，而不是仅仅直接从 RunAsync() 服务中打开它。 从 OpenAsync 返回的值是 web 服务器正在侦听地址。 当此地址返回给系统时，它使用服务注册地址。 服务结构提供了一个 API，使客户端或其他服务按服务名称然后询问此地址。 这很重要，因为服务地址不是静态服务为进行资源平衡和可用性群集中四处移动。 这是允许客户端解析服务侦听地址的机制。

这一点，OpenAsync 将启动 web 服务器，并返回它正在侦听地址。 注意它监听"http://+"，但之前返回的地址，"+"将被替换的 ip 地址或正在节点的 FQDN。 这样做的原因是，此方法所返回的地址是什么注册系统，并且它是什么客户端和其他服务时将会看到它们寻求服务的地址。 客户端正确连接到它，需要实际的 IP 或 FQDN 地址中。

```csharp

public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    this.serverHandle = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Configuration(appBuilder));

    string resultAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    ServiceEventSource.Current.Message("Listening on {0}", resultAddress);

    return Task.FromResult(resultAddress);
}

```

请注意，此引用被传递到构造函数中 OwinCommunicationListener 中的**启动**类。 此启动实例的 web 服务器用于引导 Web API 应用程序。

ServiceEventSource.Current.Message() 线将显示在诊断事件窗口以后告诉您 web 服务器已成功启动该应用程序运行时。

### CloseAsync 和中止

最后，实现 CloseAsync 和中止停止 web 服务器。 Web 服务器可以通过释放期间 OpenAsync 创建服务器处理停止。

```csharp

public Task CloseAsync(CancellationToken cancellationToken)
{
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.serverHandle != null)
    {
        try
        {
            this.serverHandle.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}

```

此示例实现，在 CloseAsync 和中止只是停止 web 服务器。 您可以选择进行更妥善协调的关闭关闭 web 服务器在 CloseAsync;例如，等待传输的数据的请求，在返回之前完成。

## 启动 web 服务器

您现在可以创建并返回 OwinCommunicationListener 以启动 web 服务器的一个实例。 在服务类 (Service.cs) 中，重写**CreateCommunicationListener()**方法︰

```csharp

protected override ICommunicationListener CreateCommunicationListener()
{
    return new OwinCommunicationListener("api", new Startup());
}

```

这是 Web API*应用程序*和 OWIN*主机*最后交汇处︰*主机*(**OwinCommunicationListener**) 给出一个实例的*应用程序*(通过**启动**的 Web API) 和服务结构管理生命周期。 通常可以使用任何通信堆后面这个相同的模式。

## 放在一起

在此示例中，您不需要做任何事情在 RunAsync() 方法中，以便可以只删除重写。

最终的服务实现应该非常简单，因为它只需要创建通信侦听器︰

```csharp

namespace WebApi
{
    using Microsoft.ServiceFabric.Services;

    public class Service : StatelessService
    {
        public const string ServiceTypeName = "WebApiType";

        protected override ICommunicationListener CreateCommunicationListener()
        {
            return new OwinCommunicationListener("api", new Startup());
        }
    }
}

```

和完整的 OwinCommunicationListener 类︰

```csharp

namespace WebApi
{
    using System;
    using System.Fabric;
    using System.Fabric.Description;
    using System.Globalization;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Owin.Hosting;
    using Microsoft.ServiceFabric.Services;

    public class OwinCommunicationListener : ICommunicationListener
    {
        private readonly IOwinAppBuilder startup;
        private readonly string appRoot;
        private IDisposable serverHandle;
        private string listeningAddress;

        public OwinCommunicationListener(string appRoot, IOwinAppBuilder startup)
        {
            this.startup = startup;
            this.appRoot = appRoot;
        }

        public void Initialize(ServiceInitializationParameters serviceInitializationParameters)
        {
            EndpointResourceDescription serviceEndpoint = serviceInitializationParameters.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
            int port = serviceEndpoint.Port;

            this.listeningAddress = String.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/{1}",
                port,
                String.IsNullOrWhiteSpace(this.appRoot)
                    ? String.Empty
                    : this.appRoot.TrimEnd('/') + '/');
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            this.serverHandle = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Configuration(appBuilder));

            string resultAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            ServiceEventSource.Current.Message("Listening on {0}", resultAddress);

            return Task.FromResult(resultAddress);
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.serverHandle != null)
            {
                try
                {
                    this.serverHandle.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}

```

与所有组件之后，您的项目现在应该如下典型 Web API 应用程序使用可靠服务 API 入口点和 OWIN 主机︰


![](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## 运行并通过 web 浏览器连接

如果没有，请[设置您的开发环境](service-fabric-get-started.md)。


您现在可以生成并部署您的服务。 在 Visual Studio 生成并部署应用程序，请按**F5** 。 在诊断事件窗口中，应该看到一条消息指出在**http://localhost:80/api**上打开的 web 服务器


![](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] 如果端口已经是由您计算机上的另一个进程打开，您可能会看到此处指示无法打开该侦听器的错误。 如果真是这样，请尝试使用不同的端口，在 ServiceManifest.xml 中的终结点配置。


一旦该服务正在运行，打开的浏览器，然后定位到[http://localhost/api](http://localhost/api)对它进行测试。

## 它扩展

外扩无状态的 web 应用程序通常意味着添加更多的机和转动对这些 web 应用程序。 服务结构的业务流程引擎可以执行此操作为您每当新节点添加到群集。 在创建无状态的服务的实例时，可以指定要创建的实例数。 服务结构将该实例的数目在群集中的节点上相应，并确保不在任何一个节点上创建多个实例。 您还可以指示服务结构总是在每个节点上创建一个实例，通过指定"-1"的实例计数。 这样可以保证，只要添加节点到群集扩展时，您无状态的服务的实例将创建新的节点上。 此值是服务实例的属性，因此它通过 PowerShell 创建服务实例时设置︰

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

或在 Visual Studio 的无状态服务项目中定义的默认服务时︰

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

创建应用程序和服务实例的详细信息，请参阅[如何部署和删除应用程序](service-fabric-deploy-remove-applications.md)。

## ASP.NET 5

在 ASP.NET 5 分开从 web 应用程序中的*宿主**应用程序*的概念和编程模型保持不变。 它也可以应用到其他形式的通信。 此外，尽管 ASP.NET 5 中，*主机*可能会有所不同，但 Web API*应用程序*层保持不变，即实际居住的大量应用程序逻辑。

## 下一步行动

[在 Visual Studio 中的结构服务应用程序的调试](service-fabric-debugging-your-application.md)
