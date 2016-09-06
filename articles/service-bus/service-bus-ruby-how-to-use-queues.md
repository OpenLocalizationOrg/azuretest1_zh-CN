---
ms.openlocfilehash: f4762a7c37ad920c01ab53a05c8731cacfd8149d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何使用服务总线队列 （拼音） |Microsoft Azure"
    description="了解如何使用在 Azure 服务总线队列。 用 Ruby 编写的代码样本。"
    services="service-bus"
    documentationCenter="ruby"
    authors="tfitzmac"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/31/2015"
    ms.author="tomfitz"/>




# 如何使用服务总线队列

本指南将向您介绍如何使用服务总线队列。 这些示例用 Ruby 编写的并使用 Azure 的 gem。 所包含的方案包括**创建队列、 发送和接收邮件**，并**删除队列**。 有关队列的详细信息，请参阅[后续步骤](#next-steps)部分。

## 服务总线队列有哪些？

服务总线队列支持**通过代理消息通信**模型。 当使用队列，分布式应用程序的组件不能直接与彼此，他们改为交换通过队列，充当中介的消息。 消息创建者 （发送方） 移交将消息发送到队列，然后继续进行处理。
异步消息使用者 （接收器） 将消息从队列中提取并处理它。 制造者不必等待从使用者的答复以便继续处理，并向您发送其他消息。 队列提供了**第一个在中，先出 (FIFO)**消息传递给一个或多个相互竞争的使用者。 也就是说，消息通常接收和处理的顺序在其中添加到队列中，每个消息进行接收和处理者只有一个消息接收器。

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

服务总线队列是一种通用的技术，可用于各种不同的情形︰

-   多层 Azure 应用程序中的 web 和辅助角色之间的通信
-   内部应用程序和 Azure 之间的通讯承载混合解决方案中的应用
-   分布式应用程序运行在不同的组织或组织部门内部的组件之间的通信

使用队列可以使您可以扩展您的应用程序更好，并使您的体系结构的更多弹性。

## 创建服务命名空间
要开始使用在 Azure 服务总线队列，必须先创建一个服务的命名空间。 服务命名空间用于解决应用程序中的服务总线资源提供作用域容器。 您必须创建通过命令行接口的命名空间，因为门户不会创建连接到 ACS 服务总线。

若要创建服务命名空间︰

1. 打开一个控制台，Azure Powershell。

2. 键入要创建的 Azure 服务总线的命名空间，如下所示的命令。 提供自定义命名空间值并指定为您的应用程序的同一区域。

    新 AzureSBNamespace-名称 yourexamplenamespace 的位置西部美国 NamespaceType 消息-CreateACSNamespace $true

    ![创建 Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)

## 获取命名空间的管理凭据
为了执行管理操作，例如创建新的命名空间，在队列，您必须获取命名空间的管理凭据。

运行创建的 Azure 服务总线命名空间 PowerShell cmdlet 显示可用于管理命名空间的关键。 复制的**DefaultKey**值。 在本教程后面部分，将在代码中使用此值。

![复制项](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE]
> 如果您登录到[Azure 门户](http://manage.windowsazure.com/)并导航到连接信息服务总线命名空间，您还可以找到此密钥。

## 创建 Ruby 应用程序

创建 Ruby 应用程序。 有关说明，请参阅[创建 Azure 中的 Ruby 应用程序](/develop/ruby/tutorials/web-app-with-linux-vm/)。

## 配置应用程序使用服务总线

若要使用 Azure 服务总线，您需要下载并使用 Ruby azure 程序包，其中包含一组与存储其他服务进行通信的便利库。

### 使用 RubyGems 获取包

1. 使用命令行界面**PowerShell** (Windows)、**终端**(Mac) 或**狂欢**(Unix) 等。

2. 要安装的 gem 和依赖项的命令窗口中键入"gem 安装 azure"。

### 导入包

使用您喜爱的文本编辑器，将以下内容添加到想要使用存储的 Ruby 文件的顶部︰

    require "azure"

## 设置 Azure 服务总线连接

Azure 模块将读取环境变量**AZURE\_SERVICEBUS\_命名空间**和**AZURE\_SERVICEBUS\_ACCESS_KEY**连接到 Azure 服务总线命名空间所需的信息。 如果没有设置这些环境变量，则必须指定命名空间信息，然后用下面的代码使用**Azure::ServiceBusService** :

    Azure.config.sb_namespace = "<your azure service bus namespace>"
    Azure.config.sb_access_key = "<your azure service bus access key>"

将服务总线命名空间值设置为您创建而不是完整的 URL 的值。 例如，使用**"yourexamplenamespace"**，不是"yourexamplenamespace.servicebus.windows.net"。

## 如何创建队列

**Azure::ServiceBusService**对象，可以使用队列。 要创建队列，请使用**create_queue()**方法。 下面的示例创建队列或打印出错误，则为。

    azure_service_bus_service = Azure::ServiceBusService.new
    begin
      queue = azure_service_bus_service.create_queue("test-queue")
    rescue
      puts $!
    end

此外可以传入一个附加选项，允许您重写默认队列设置，例如邮件的时间生存的**Azure::ServiceBus::Queue**对象或最大队列大小。 下面的示例演示为 5 GB 和时间存活 1 分钟的时间中设置最大队列大小︰

    queue = Azure::ServiceBus::Queue.new("test-queue")
    queue.max_size_in_megabytes = 5120
    queue.default_message_time_to_live = "PT1M"

    queue = azure_service_bus_service.create_queue(queue)

## 如何将消息发送到队列

若要将消息发送到服务总线队列，应用程序将调用**发送\_队列\_message()** **Azure::ServiceBusService**对象上的方法。 消息发送到的 （和从接收到的） 服务总线队列是**Azure::ServiceBus::BrokeredMessage**对象，并有一套标准属性 (如**标签**和**时间\_到\_live**)、 用于保存自定义应用程序特定属性的字典和任意应用程序数据的主体。 应用程序可以通过将字符串值作为消息传递设置邮件正文和任何所需的标准属性将使用默认值填充。

下面的示例演示如何对名为"测试队列"使用队列发送测试消息**发送\_队列\_message()**:

    message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
    message.correlation_id = "test-correlation-id"
    azure_service_bus_service.send_queue_message("test-queue", message)

服务总线队列支持的最大邮件大小为 256 KB （头，其中包括标准和自定义应用程序属性，可以有的最大大小为 64 KB）。 保留在队列中的消息的数量没有限制，但持有队列消息的总大小没有一个盖帽。 此队列大小被定义在创建时，具有 5 GB 的上限。

## 如何从队列接收消息

接收消息的队列使用从**接收\_队列\_message()** **Azure::ServiceBusService**对象上的方法。 默认情况下，邮件阅读并锁定而不从队列中删除。 但是，从队列删除消息，如通过设置读取**: peek_lock**选项为**false**。

默认行为可以读取和删除成两个阶段操作，这样就有可能支持的应用程序不能容忍丢失邮件。 服务总线接收请求时，它查找下一条消息中，要消耗就锁定以防止其他使用者，接收它，，然后将它返回给应用程序。 通过调用应用程序完成处理消息 （或可靠地存储以备将来处理后），完成第二阶段接收过程的**删除\_队列\_message()**方法并提供一个参数作为要删除的消息。 **删除\_队列\_message()**方法将标记为正在使用的消息并从队列中移除它。

如果**: peek\_锁**参数设置为**false**，读取和删除邮件成为最简单的模型，和最适用的情况下，应用程序能够容忍不处理出现故障的消息。 要理解这一点，考虑一个方案，其中使用者发出接收请求，然后处理它之前崩溃。 因为服务总线将被标记为正在使用的消息，然后当该应用程序将重新启动，并开始使用消息再次强调，它将丢失了消耗在崩溃之前的消息。

下面的示例演示如何可以接收邮件并处理使用**收到\_队列\_message()**。 第一次接收，然后通过删除消息**: peek\_锁**设置为**false**，则它收到另一封邮件，然后删除邮件使用**删除\_队列\_message()**:

    message = azure_service_bus_service.receive_queue_message("test-queue",
      { :peek_lock => false })
    message = azure_service_bus_service.receive_queue_message("test-queue")
    azure_service_bus_service.delete_queue_message(message)

## 如何处理应用程序崩溃和无法读取消息

服务总线提供的功能来帮助您从您的应用程序或困难处理消息中的错误正常恢复。 如果接收方应用程序不能处理该消息，由于某种原因，那么它可以调用**解除\_队列\_message()** **Azure::ServiceBusService**对象上的方法。 这将导致服务总线来解锁队列中的邮件并使其可再次，同一个使用应用程序或其他使用的应用程序接收。

此外没有与消息在队列中，锁定超时，并且如果应用程序不能处理该消息之前锁定超时过期 （例如，如果应用程序崩溃时），则服务总线将自动解锁消息并使其可再次收到。

在该应用程序崩溃之前处理该消息之后**删除\_队列\_message()**调用方法，然后在它重新启动时，将向应用程序重新传送的消息。 这通常称为**至少处理一次**，即将至少一次处理每个消息，但在某些情况下可能重新发送同样的消息。 如果方案不能容忍重复处理，应用程序开发人员应该向其应用程序来处理传递重复的邮件添加额外的逻辑。 这通常实现使用**消息\_id**属性将保持不变在传递尝试的邮件。

## 下一步行动

现在，学服务总线队列的基本知识，按照这些链接以了解详细信息。

-   请参阅 MSDN 参考︰[队列、 主题和订阅](http://msdn.microsoft.com/library/windowsazure/hh367516.aspx)
-   在 GitHub 访问[Ruby 的 Azure SDK](https://github.com/WindowsAzure/azure-sdk-for-ruby)存储库

有关[如何使用 Azure 队列服务](/develop/ruby/how-to-guides/queue-service/)文章中讨论的 Azure 队列和本文所述 Azure 服务总线队列之间比较，请参见[Azure 队列和 Azure 服务总线队列-比较和 Contrasted](http://msdn.microsoft.com/library/windowsazure/hh767287.aspx)
 
