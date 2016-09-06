---
ms.openlocfilehash: a3b9e7f23713856fb9d758e15de7db70cefa14b0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用服务总线中继 (.NET) |Microsoft Azure"
    description="了解如何使用 Azure 服务总线中继服务来连接两个应用程序驻留在不同的位置。"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="07/02/2015"
    ms.author="sethm"/>


# 如何使用 Azure 服务总线中继服务

本文介绍如何使用服务总线中继服务。 这些示例使用属于 Microsoft.NET SDK，Azure 服务总线组件中包含扩展 Windows 通讯基础 (WCF) API 和编写的 C#。 有关服务总线中继的详细信息，请参阅[后续步骤](#Next-steps)部分。

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## 服务总线中继是什么？

服务总线*中继*服务使您能够构建混合在 Azure 数据中心和自己内部的企业环境中运行的应用程序。 服务总线中继简化此操作使您可以安全地公开与公有云，为公司的企业网络内驻留而无需打开防火墙的连接，或者需要侵入性对企业网络基础结构的 Windows 通信基础 (WCF) 服务。

![中继的概念](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

服务总线中继可以与您现有的企业环境中的主机 WCF 服务。 然后可以将委托侦听是否有传入会话，并对这些 WCF 服务的运行在 Azure 服务总线服务请求。 这使您可以公开这些服务在 Azure，运行的应用程序代码或移动工作人员或外部网的合作伙伴的环境。 服务总线，可以安全地控制哪些用户可以访问这些细粒度级别的服务。 它提供了功能强大且安全的方式来公开应用程序功能和数据从现有的企业解决方案，并利用它从云中。

此如何文章演示了如何使用服务总线中继来创建 WCF 的 web 服务，公开使用 TCP 通道绑定，实现双方之间的安全对话。

## 创建服务命名空间

要开始使用在 Azure 服务总线中继，必须首先创建一个服务的命名空间。 命名空间提供用于解决应用程序中的服务总线资源的作用域容器。

若要创建服务命名空间︰

1.  登录到[Azure 的门户][]。

2.  在 Azure 入口的左侧的导航窗格中，单击**服务总线**。

3.  在 Azure 门户的下部窗格中，单击**创建**。

    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-13.png)

4.  在**添加新的名称空间**对话框中，输入命名空间名称。
    系统将立即检查该名称可用。

    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-04.png)

5.  确保命名空间名称是可用后，选择的国家或地区应在其中托管您的名称空间 （请确保您使用同一个国家/地区部署计算资源）。

    > [AZURE.IMPORTANT] 选择您打算部署您的应用程序中选择*相同的区域*。 这将使您获得最佳的性能。

6.  在对话框中的其他字段留下它们的默认值 （**消息传送**和**标准**层），然后单击复选标记。 系统现在创建您的命名空间，并使它。 您可能需要等待几分钟为系统资源调配资源，为您的帐户。

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-27.png)

    然后创建的命名空间出现在 Azure 的门户，并且花费一点时间来激活。 等待，直到在继续操作之前处于**活动**状态。

## 获取默认命名空间的管理凭据

为了执行管理操作，例如创建新的命名空间，在中继连接，您必须配置共享访问签名 (SAS) 授权规则的命名空间。 有关 SA 的详细信息，请参阅[使用服务总线共享访问签名的验证][]。

1.  在左侧的导航窗格中，单击**服务总线**节点，以显示可用的命名空间的列表。
    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-13.png)

2.  双击刚才从显示的列表中创建命名空间的名称。
    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-09.png)

3.  单击顶部的页的**配置**选项卡。

4.  当配置服务总线命名空间时， **SharedAccessAuthorizationRule**，与**键名**设置为**RootManageSharedAccessKey**，是默认创建的。 此页显示该注册表项，以及默认规则的主要和辅助键。

## 获取服务总线 NuGet 程序包

服务总线 NuGet 程序包是最简单的方法来获取服务总线 API 和服务总线的相关的所有配置应用程序。 NuGet Visual Studio 扩展很容易安装和更新库和工具 Visual Studio 和 Visual Studio 速成版中。 服务总线 NuGet 程序包是最简单的方法来获取服务总线 API 和服务总线的相关的所有配置应用程序。

要在您的应用程序安装 NuGet 程序包，请执行以下操作︰

1.  在解决方案资源管理器，右键单击**引用**，然后单击**管理 NuGet 程序包**。
2.  搜索"服务总线"并选择**Microsoft Azure 服务总线**项。 单击**安装**以完成安装，然后关闭下面的对话框。

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)


## 如何使用服务总线来公开和使用 TCP 的 SOAP web 服务

公开外部消耗现有 WCF SOAP 的 web 服务，必须向服务绑定和地址进行更改。 这可能需要对您的配置文件的更改，或可能需要代码更改，具体取决于您如何设置并配置 WCF 服务。 请注意 WCF 使您可以对相同的服务，拥有多个网络终结点，以便您可以同时添加外部访问的服务总线端点时保留现有的内部终结点。

在此任务中，您将构建一个简单的 WCF 服务和服务总线侦听器添加到它。 本练习中假定您熟悉 Visual Studio，有些并因此不遍历创建项目的所有详细信息。 相反，它侧重于代码。

在开始之前执行以下步骤，请完成以下过程来设置您的环境︰

1.  在 Visual Studio 中，创建一个控制台应用程序，其中包含两个，"客户"和"服务"解决方案中的项目。
2.  将 Microsoft Azure 服务总线 NuGet 程序包添加到这两个项目。 这将添加所有必需的程序集引用到您的项目。

### 如何创建服务

首先，创建服务本身。 任何 WCF 服务包括至少三个不同部分︰

-   介绍交换哪些消息以及要调用的操作是合同的定义。
-   上述协定的实施。
-   承载的 WCF 服务，并公开多个终结点的主机。

本节中的代码示例处理每个这类组件。

该合约定义了一个操作， `AddNumbers`，它将两个数字相加并返回结果。 `IProblemSolverChannel`接口使客户端能够更轻松地管理代理生存期。 创建此类接口被认为是最佳做法。 它是一个好主意，以将此合同定义放入一个单独的文件中，以便可以引用该文件从您的"客户端"和"服务"的项目，但您可以将代码复制到这两个项目。

        using System.ServiceModel;

        [ServiceContract(Namespace = "urn:ps")]
        interface IProblemSolver
        {
            [OperationContract]
            int AddNumbers(int a, int b);
        }

        interface IProblemSolverChannel : IProblemSolver, IClientChannel {}

到位的合同，与实现是一件小事。

        class ProblemSolver : IProblemSolver
        {
            public int AddNumbers(int a, int b)
            {
                return a + b;
            }
        }

### 如何以编程方式配置服务主机

合同和实施到位，现在可以承载的服务。 承载发生在一个[System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx)对象，它负责管理该服务的实例，主机侦听消息的终结点。 下面的代码使用常规的本地终结点和服务总线端点来说明的外观，通过并行，内部和外部的终结点的配置的服务。 与您的命名空间名称和*yourKey*替换您在以前的安装步骤中获得的 SAS 键字符串*命名空间*。

    ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

    sh.AddServiceEndpoint(
       typeof (IProblemSolver), new NetTcpBinding(),
       "net.tcp://localhost:9358/solver");

    sh.AddServiceEndpoint(
       typeof(IProblemSolver), new NetTcpRelayBinding(),
       ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
        .Behaviors.Add(new TransportClientEndpointBehavior {
              TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "yourKey")});

    sh.Open();

    Console.WriteLine("Press ENTER to close");
    Console.ReadLine();

    sh.Close();

在示例中，您将创建两个终结点上的同一个合同实现。 一个本地，一个通过服务总线投影。 它们的主要区别是绑定;[`NetTcpBinding`](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx)为本地和[NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx)的服务总线端点和地址。 本地终结点具有带不同端口的本地网络地址。 服务总线端点有组成字符串"sb"、 您的命名空间名称，以及"规划求解"的路径的终结点地址。 这将导致使用完全限定外部 DNS 名称标识为服务总线 TCP 终结点的服务端点的 URI"sb://[serviceNamespace].servicebus.windows.net/solver"。 如果将替换占位符，如上面所述插入的代码`Main`函数的"服务"应用程序中，您将有功能性服务。 如果您希望您的服务仅侦听服务总线，移除本地终结点声明。

### 如何在 App.config 文件中配置服务主机

您还可以配置使用的 App.config 文件的主机。 服务承载代码在这种情况下将出现在下一个示例。

    ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
    sh.Open();
    Console.WriteLine("Press ENTER to close");
    Console.ReadLine();
    sh.Close();

终结点定义移动到 App.config 文件。 请注意，NuGet 程序包 App.config 文件，它们是服务总线扩展所需的配置已添加的定义范围。 下面的代码示例完全等效于上面的代码示例，应显示下方的**system.serviceModel**元素。 此代码示例假定您项目 C\#命名空间命名为"服务"。
与您的服务总线服务命名空间和 SA 密钥替换占位符。

    <services>
        <service name="Service.ProblemSolver">
            <endpoint contract="Service.IProblemSolver"
                      binding="netTcpBinding"
                      address="net.tcp://localhost:9358/solver"/>
            <endpoint contract="Service.IProblemSolver"
                      binding="netTcpRelayBinding"
                      address="sb://namespace.servicebus.windows.net/solver"
                      behaviorConfiguration="sbTokenProvider"/>
        </service>
    </services>
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
        </endpointBehaviors>
    </behaviors>

完成这些更改之后，服务启动之前，一样，但两个实时的终结点︰ 一个本地的和一个侦听在云中。

### 如何创建客户端

#### 如何以编程方式配置客户端

若要使用该服务，您可以构建使用 WCF 客户端[`ChannelFactory`](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx)对象。 服务总线使用使用 SAS 实现基于令牌的安全模型。 **TokenProvider**类表示具有内置的工厂方法返回某些已知的令牌提供程序的安全令牌提供程序。 下面的示例使用[`CreateSharedAccessSignatureTokenProvider`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx)方法以处理相应的 SAS 令牌的收购。 名称和密钥是指上一节中所述，从门户网站获得。

首先，引用或复制`IProblemSolver`合同从服务代码到客户端项目。

然后，该代码替换中`Main`的客户端上，再次与您的服务总线命名空间和 SA 密钥替换占位符文本的方法。

    var cf = new ChannelFactory<IProblemSolverChannel>(
        new NetTcpRelayBinding(),
        new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

    cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
                { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","yourKey") });

    using (var ch = cf.CreateChannel())
    {
        Console.WriteLine(ch.AddNumbers(4, 5));
    }

现在可以生成客户端和服务，运行它们 （第一次运行该服务），并且客户端将调用服务和打印"**9**。 可以在客户端和服务器上运行不同的机器，甚至可以跨网络，并且仍然有效的通信。 在云中还是本地，也可以运行的客户端代码。

#### 如何在 App.config 文件中配置客户端

下面的代码演示如何配置客户端使用的 App.config 文件。

    var cf = new ChannelFactory<IProblemSolverChannel>("solver");
    using (var ch = cf.CreateChannel())
    {
        Console.WriteLine(ch.AddNumbers(4, 5));
    }

终结点定义移动到 App.config 文件。 下面的代码示例是前面列出的代码相同，应显示下方的**system.serviceModel**元素。 在这里，像以前一样，您必须替换占位符与您的服务总线命名空间和 SAS 键。

    <client>
        <endpoint name="solver" contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </client>
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
        </endpointBehaviors>
    </behaviors>

## 下一步行动

现在，您已经学习了服务总线*中继*服务的基本知识，按照这些链接以了解详细信息。

-   构建服务︰[构建服务总线提供服务][]。
-   构建客户端︰[构建客户端应用程序服务总线][]。
-   服务总线示例︰ 从[Azure 示例][]下载，或请参阅[MSDN][]上的概述。

  [创建服务 Namespace]: #create_namespace
  [Namespace 获得的默认管理凭据]: #obtain_credentials
  [获取服务总线 NuGet 程序包]: #get_nuget_package
  [如何︰ 使用到公开的服务总线，并使用 SOAP Web 服务使用 TCP]: #how_soap
  [Azure 门户]: http://manage.windowsazure.com
  [与服务总线共享访问签名验证]: http://msdn.microsoft.com/library/azure/dn170477.aspx
  [生成服务的服务总线]: http://msdn.microsoft.com/library/azure/ee173564.aspx
  [构建客户端应用程序服务总线]: http://msdn.microsoft.com/library/azure/ee173543.aspx
  [Azure 的示例]: https://code.msdn.microsoft.com/windowsazure/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [MSDN]: https://msdn.microsoft.com/en-us/library/azure/dn194201.aspx
