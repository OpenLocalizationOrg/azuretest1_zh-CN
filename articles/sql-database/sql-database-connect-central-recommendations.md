---
ms.openlocfilehash: 99c4ba1dd93b8a4e2384e80a2c1ab14f5dee0473
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="连接到 SQL 数据库︰ 链接、 最佳做法和设计指南" 
    description="起始点主题，汇集了链接和从 ADO.NET 和 PHP 之类的技术连接到 SQL Azure 数据库的客户端程序的建议。" 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2015" 
    ms.author="genemi"/>


# 连接到 SQL 数据库︰ 链接、 最佳做法和设计指南


本主题是客户端连接到 SQL Azure 数据库入门的好地方。 它可用于连接到 SQL 数据库进行交互的各种技术提供了一些代码示例的链接。 这些技术包括企业库、 JDBC、 PHP 和若干。 无论哪种特定技术您使用连接到 SQL 数据库提供的信息适用。


## 独立于技术的建议


- [连接到 Azure SQL 数据库编程方式的准则](http://msdn.microsoft.com/library/azure/ee336282.aspx)的讨论包括以下︰
 - [端口和防火墙](sql-database-configure-firewall-settings.md/)
 - 连接字符串
- [Azure SQL 数据库资源管理](https://msdn.microsoft.com/library/azure/dn338083.aspx)-讨论包括以下︰
 - 资源管理
 - 强制执行的限制
 - 带宽限制


## 身份验证的建议


- 使用 SQL Azure 数据库身份验证，不 Windows 身份验证在 SQL Azure 数据库中不可用。
- 指定某个特定的数据库，而不是默认到*主*数据库。
 - 不能使用在 SQL 数据库上的事务处理 SQL**使用 myDatabaseName;**语句要切换到另一个数据库。


### 包含的用户


当将某个人作为用户添加到 SQL 数据库时，您可以有选择︰

- 添加到**主**数据库，用密码*登录*，然后将相应的*用户*添加到同一台服务器上的一个或多个其它数据库。

- 为一个或多个数据库中，并没有链接到任何*登录*在**母版**中添加*包含用户*的密码。


包含的用户方法都有优点和缺点︰

- 优点是，数据库可以轻松地从一个 SQL Azure 数据库服务器之间移动，当数据库中的所有用户都都包含的用户。

- 缺点是在日常管理中的更大困难。 例如︰
 - 它是更具挑战性，除去几个包含的用户不是除去登录一次。
 - 在几个数据库中包含的用户的人可能有多个密码，要记住或更新。


-[包含数据库](http://msdn.microsoft.com/library/ff929071.aspx)中提供进一步的信息。


## 连接的建议


- 在客户端连接逻辑，重写默认超时值为 30 秒。
 - 默认值为 15 秒是太短，依赖于互联网的连接。


- 确保[SQL Azure 数据库防火墙](sql-database-firewall-configure.md)允许在端口 1433年上的传出 TCP 通信。
 - 在 SQL 数据库服务器或到单独的数据库，您可以配置防火墙设置。


- 若要处理*瞬时故障*，为您与 SQL Azure 数据库进行交互的客户端程序中添加[*重试*逻辑](#TransientFaultsAndRetryLogicGm)。


### 连接池


如果您使用的[连接池](http://msdn.microsoft.com/library/8xx3tyca.aspx)，关闭连接即时您的程序没有频繁使用，而且不准备再次使用。

除非您的程序将立即重新使用另一操作的连接并没有暂停，我们建议以下模式︰

- 打开的连接。
- 不要通过连接一次操作。
- 关闭该连接。


#### 使用池时引发的异常


当启用了连接池，并发生超时错误或其他登录错误时，将引发异常。 后续连接尝试将失败下一步的 5 秒钟，这种行为称为*阻塞时间段*。

如果应用程序尝试连接在锁定期内，将再次引发第一次异常。 在锁定期结束后, 后续失败导致新阻塞期间持续倍以前阻塞时间段。

阻塞时间段的最大持续时间为 60 秒。


### 不只是 1433 V12 端口


客户端连接到 Azure SQL 数据库 V12 有时绕过代理，直接与数据库进行交互。 除 1433年端口变得重要。 有关详细信息，请参阅︰<br/>
[除了 ADO.NET 4.5 和 SQL 数据库 V12 1433 端口](sql-database-develop-direct-route-ports-adonet-v12.md)


下一节有详细讨论重试逻辑和瞬时故障处理。



<a name="TransientFaultsAndRetryLogicGm" id="TransientFaultsAndRetryLogicGm"></a>

&nbsp;

## 瞬时故障和重试逻辑


Azure 系统能够动态地重新配置服务器，当 SQL 数据库服务中出现的繁重的工作负载。

但是，重新配置可能会导致客户端程序无法连接到 SQL 数据库。 此错误称为*瞬时性故障*。

客户端程序可以尝试重新建立连接后等待重试之间也许是 6 到 60 秒。 您必须在您的客户端提供的重试逻辑。

有关代码示例，说明了重试逻辑，请参阅︰
- [对 SQL 数据库的客户端快速启动代码示例](sql-database-develop-quick-start-client-code-samples.md)


### 瞬时故障的错误号


与 SQL 数据库任何错误时，将引发[sqlexception:](http://msdn.microsoft.com/library/system.data.sqlclient.sqlexception.aspx) 。 **Sqlexception︰**包含在其**数**属性中的一个数字错误代码。 如果错误代码标识的暂时性错误，您的程序应重试调用。


- [对于 SQL 数据库的客户端程序的错误消息](sql-database-develop-error-messages.md#bkmk_connection_errors)
 - 其**瞬态错误，连接丢失错误**部分是保证自动重试的瞬态错误的列表。
 - 例如，如果错误号 40613 发生，这类似于说重试<br/>*数据库服务器的服务器上的 ' mydatabase' 当前不可用。*


有关详细信息，请参阅︰
- [SQL azure 数据库开发︰ 帮助主题](http://msdn.microsoft.com/library/azure/ee621787.aspx)
- [到 SQL Azure 数据库连接问题的疑难解答](http://support.microsoft.com/kb/2980233/)


## 技术


下面的主题包含几种语言和可用于从客户端程序连接到 SQL Azure 数据库的驱动程序技术的代码示例的链接。


对于运行 Windows、 Linux 和 Mac OS X 的客户端给出了各个代码示例。


**通用示例︰**有多种编程语言，包括 PHP，Python，Node.js 和.NET CSharp[代码示例](sql-database-develop-quick-start-client-code-samples.md)。 另外，样本给出了运行在 Windows 和 Linux，Mac OS X 上的客户端。


**弹性比例︰**有关灵活的伸缩数据库的连接信息，请参阅︰

- [开始使用 SQL Azure 数据库弹性比例预览](sql-database-elastic-scale-get-started.md)
- [数据依赖于路由](sql-database-elastic-scale-data-dependent-routing.md)


**驱动程序库︰**建议使用版本连接驱动程序库，其中包括的信息，请参阅︰

- [对于 SQL 数据库和 SQL Server 连接库](sql-database-libraries.md)

