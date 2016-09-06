---
ms.openlocfilehash: cba13041ca5643efb54423b50831270a70643d54
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="入门︰ 配置 SQL 数据仓库 |Microsoft Azure"
   description="按照这些步骤和原则设置 SQL 数据仓库。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/23/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# 入门︰ 配置 SQL 数据仓库 #

这篇文章是加速的指南，可帮助您设置 SQL Azure 中数据仓库。 通过遵循本指南，您将执行以下任务︰

1. 创建新的 SQL 数据仓库数据库。
2. 配置新的逻辑服务器。
3. 设置 Azure 防火墙规则以允许外部客户机访问。

## Azure 的免费试用版 ##
您将需要 Azure 的订阅来完成以下任务。 如果您没有访问到 Azure 的订阅，然后解决它实际上是第一步 ！

您可以获得[免费试用版][]，您可以尝试任何 Azure，包括 SQL 数据仓库中的服务。


## 登录到 Azure 门户 ##

一旦您可以登录到[Azure 的门户网站][]的订阅。 继续操作并立即登录。

在此下一步骤系列中我们将快速启动全新的逻辑服务器并创建新的 SQL 数据仓库数据库。

## 找到的 SQL 数据仓库服务

我们所要做的第一件事是 Azure 门户中找到 SQL 数据仓库服务。

在底部的 Azure 门户网站左上的角是新建按钮。 新建按钮是用于创建任何新的服务，在 Azure 的起始点。

- 现在单击新建按钮。

### 数据 + 存储

单击新建按钮打开了 Azure 中的所有服务类别。 SQL 数据仓库"数据 + 存储"类别中的生存之地。

- 请单击**数据 + 存储**钻取和 Azure 此类别所提供的服务，请参阅。

### SQL 数据仓库

正如您可以看到 Azure 提供了大量的数据和存储引擎。 但是，此获取指南是为 SQL 数据仓库。

- 继续操作并选择**SQL 数据仓库**。

## 配置 SQL 数据仓库

若要完成资源调配过程只需配置 SQL 数据仓库。


### 数据库名称

第一种配置是命名数据库。



- 本快速入门的命名数据库"MySQLDW"。


> [AZURE.NOTE] 当您创建您自己的数据库可以当然命名它根据需要。 但是，它需要符合 Azure 的基本命名要求。

### 表现

性能选项是*重要*一个。 SQL 数据仓库提供了通过此滑块可扩展电源。 您可以增加或减少您在任何时候-不只是配置数据仓库时的性能。 进一步则滑到右边较大的资源供您选择。 如果不再需要这些资源，则可以立即移动滑块后退;节省了成本。 SQL 数据仓库，可以更改而无需重新创建数据仓库，或者将数据移动需求上的性能配置文件。

- 现在使用滑块来查看数据仓库单位根据回向左移动幻灯片的右侧和减小的增加。

- 离开此步骤之前请确保您已返回滑块向左。 您新的数据仓库很小，所以我们不需要太多;保存您为您的试用版的其余部分的资源 ！

### 选择源

此选项使开始建立一个空数据库的选择。 选择新的数据库作为起点。

> [AZURE.NOTE] 第二个选项也是可用的。 这也是允许从预先存在的还原点; 创建数据库还原选项。

### 逻辑服务器

新的 SQL 数据仓库数据库驻留在逻辑服务器上。 逻辑服务器带来的大量数据库配置的一致性，并查找到 Azure 数据中心服务。

需要设置的选项有︰
1. 服务器名称
2. 服务器管理员名
3. Password
4. 数据中心位置
5. Azure 服务来访问该服务器的权限

随意设置这些值，您可以根据。 服务器名称必须是唯一的。 最好选择接近可减少网络延迟的数据中心。 SQL 数据仓库还包含利用 Azure 的其他服务的强大功能。 它因此是最好保留复选框启用 Azure 服务访问。

> [AZURE.NOTE] SQL 数据仓库必须使用 V12 服务器。 确保此选项设置为是。 此外可以通过 Azure SQL 数据库和 SQL 数据仓库数据库共享的逻辑服务器。 但是，它必须是 V12 服务器。

> [AZURE.NOTE] 记录服务器名称、 服务器管理员名称和密码在某处，并保证他们的安全。 您将需要此信息来连接到 SQL 数据仓库数据库。

### 资源组
资源组是旨在帮助您管理 Azure 的资源集合的容器。

本快速入门的是确定将保留其默认值配置资源组。

了解有关[资源组](../azure-portal/resource-group-portal.md)的详细信息。

### 订阅
一个用户可以具有一个或多个 Azure 订阅。 如果您有多个与您的登录名，则可以选择使用哪种订阅的订阅。

但是，对于本指南的目的，则应该没有问题。

让我们继续并创建 SQL 数据仓库 ！

## 创建数据仓库 ##
用于创建数据仓库剩下的就是单击创建按钮。

祝贺您 ！ 您创建的第一个 SQL 数据仓库数据库。

您现在应该返回到[Azure 的门户][]。 请注意，SQL 数据仓库数据库已添加到页面。


此时没有人可以访问 SQL 数据仓库数据库。 以保持一切安全，默认情况下数据库尚未尚未配置客户端可以访问它。

因此在资源调配过程中的最后一步是配置用于外部访问的服务。

## 将 Azure 防火墙配置 ##

第一次配置 Azure 防火墙︰

1. 左侧的导航刀片式服务器中，请单击**浏览**。

2. 选择**SQL 服务器**。

3. 选择您逻辑的 SQL Server。

4. 选择设置。

5. 单击**防火墙**。

6. 设置防火墙规则。

    有几个要做的事情。 它们是︰
    - 防火墙规则的名称。
    - 如果您不具有静态 IP 地址，请提供 IP 范围。

    > [AZURE.NOTE] 需要包含的客户端 IP 地址范围是您外部或公开面临的 IP 地址。若要查找您的外部 IP 地址可以使用大量的网站，如<a href="http://www.whatismyip.com" target="\_blank">www.whatismyip.com</a>

7. 保存您的防火墙规则。


现在，您已经配置的防火墙可以建立从桌面到 Azure SQL 数据仓库，您刚刚创建的连接。

## 下一步行动

已成功设置 SQL 数据仓库服务现在我们可以继续学习如何使用它。

接下来的步骤是因此，了解如何︰

1. [连接和查询][]数据仓库。
2. 加载[示例数据]。

<!--Image references-->


<!-- Articles -->
[连接和查询]: sql-data-warehouse-get-started-connect-query.md
[示例数据]: ./sql-data-warehouse-get-started-load-samples.md  

<!--External links-->
[免费试用版]: https://azure.microsoft.com/en-us/pricing/free-trial/
[Azure 门户]: https://portal.azure.com/
