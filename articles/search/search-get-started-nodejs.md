---
ms.openlocfilehash: 74d381ca7418ab1c2c0faa4c0555debaad051b8b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 搜索范围 NodeJS |Microsoft Azure"
    description="逐步构建自定义 Azure 搜索应用程序作为您的编程语言中使用 NodeJS。"
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="08/18/2015"
    ms.author="heidist"/>

# 开始使用在 NodeJS Azure 搜索

了解如何构建自定义 NodeJS 搜索应用程序使用 Azure 搜索的搜索体验。 本教程使用[Azure 搜索服务 REST API](https://msdn.microsoft.com/library/dn798935.aspx)来构造对象并在本练习中使用的操作。

我们使用[NodeJS](https://nodejs.org)和 NPM，[大文本 3](http://www.sublimetext.com/3)，并且 Windows PowerShell Windows 8.1 在开发和测试此代码。

若要运行此示例，您需要 Azure 搜索服务，您可以注册在[Azure 的门户](https://portal.azure.com)。

> [AZURE.TIP] [AzureSearchNodeJSIndexerDemo](http://go.microsoft.com/fwlink/p/?LinkId=530198)在本教程下载的源代码。

## 有关的数据

此示例应用程序使用从[美国地质服务 (USGS)](http://geonames.usgs.gov/domestic/download_data.htm)，筛选上状态的罗得岛州可减小数据集的数据。 我们将使用此数据来生成返回地标建筑物，如医院和学校，以及如流、 湖泊和峰地质特征的搜索应用程序。

在此应用中， **DataIndexer**程序生成并加载使用[索引器](https://msdn.microsoft.com/library/azure/dn798918.aspx)的构造，从公共的 Azure SQL 数据库检索筛选的 USGS 数据集的索引。 凭据和连接在程序代码中提供对在线数据源的信息。 不不需要任何进一步的配置。

> [AZURE.NOTE] 我们要保持在 10000 文档限制自由定价层下此数据集上应用筛选器。 如果您使用标准层，此限制不适用。 对于每个定价层产能的详细信息，请参阅[限制和约束](https://msdn.microsoft.com/library/azure/dn798934.aspx)。

## 创建服务

1. 登录到[Azure 的门户](https://portal.azure.com)。

2. 在 Jumpbar 中，单击**新建** > **数据 + 存储** > **搜索**。

     ![][1]

3. 配置服务名称，定价层、 资源组、 订阅和位置。 这些设置是必需的并配置该服务后不能更改。

     ![][2]

    - **服务名称**必须是唯一的、 小写字母、 下 15 个字符，不能有空格。 此名称将成为 Azure 搜索服务的终结点的一部分。 有关命名约定的详细信息，请参阅[命名规则](https://msdn.microsoft.com/library/azure/dn857353.aspx)。

    - **定价层**确定容量和帐单。 这两个层提供的功能相同，但在不同的资源级别。

        - 在与其它订阅服务器共享的群集上运行**自由**。 它提供了足够的容量来尝试使用教程和编写的概念证明代码，但不是用于生产应用程序。 部署一项免费服务，通常只需要几分钟。
        - **标准**上专用的资源运行和高可扩展性。 最初，一个副本和一个分区，提供标准服务，但服务创建后，您可以调整产能。 部署标准服务时间较长，通常关于十五分钟。

    - **资源组**是容器的服务和资源用于公共目的。 例如，如果您正在构建基于 Azure 搜索 Azure BLOB 存储在 Azure 网站的自定义搜索应用程序无法创建门户管理页面中一起保持这些服务的资源组。

    - **订阅**可以选择采用不同的多个订阅，如果有多个订阅。

    - **位置**是数据中心区域。 目前，所有的资源必须在同一个数据中心中运行。 不支持在多个数据中心之间分配资源。

4. 单击**创建**来提供服务。

观察 Jumpbar 中的通知。 可以使用服务时，会出现通知。

<a id="sub-2"></a>
## 查找的服务名称和 Azure 搜索服务的 api 键

您在创建服务后，返回到门户网站获取 URL 或`api-key`。 连接到您的搜索服务都要求您拥有两个 URL 和`api-key`进行身份验证的调用。

1. 在 Jumpbar 中，单击**主页**，然后单击搜索服务若要打开服务操控板。

2. 在服务面板中，您将看到拼贴的基本信息，以及用于访问管理密钥的钥匙图标。

    ![][3]

3. 复制服务 URL、 管理和查询项。 以后，当添加到 config.js 文件，您将需要所有这三种。

## 下载示例文件

使用下列方法之一一下载示例。

1. 转到[AzureSearchNodeJSIndexerDemo](http://go.microsoft.com/fwlink/p/?LinkId=530198)。
2. 单击**下载 ZIP**和保存的.zip 文件，然后提取它所包含的所有文件。

会对此文件夹中的文件进行的所有后续文件修改和运行的语句。

或者，如果您有 GIT 路径语句中，您可以打开 PowerShell 窗口并键入 `git clone https://github.com/EvanBoyle/AzureSearchNodeJSIndexerDemo.git`

## 更新 config.js。 与您的搜索服务 URL 和 api 密钥

使用 URL 和更早版本，复制的 api 密钥在配置文件中指定的 URL，管理-键和查询。

管理密钥授予完全控制服务的操作，包括创建或删除索引并加载文档。 与此相反，查询键是用于只读操作，通常由客户端应用程序连接到 Azure 搜索。

在此示例中，我们包括查询键有助于巩固在客户端应用程序中使用查询参数的最佳做法。

下面的屏幕快照显示**config.js**以文本格式打开编辑器，方式来划分，以便您可以看到对于搜索服务是有效的值更新该文件的位置的相关项。

![][5]


## 该示例运行时环境的主机

该示例需要 HTTP 服务器，您可以将安装全局使用 npm。

以下命令使用 PowerShell 窗口。

1. 导航到包含**package.json**文件的文件夹。
2. 类型`npm install`。
2. 类型`npm install -g http-server`。

## 生成的索引并运行应用程序

1. 类型`npm run indexDocuments`。
2. 类型`npm run build`。
3. 类型`npm run start_server`。
4. 直接在您的浏览器 `http://localhost:8080/index.html`

## 根据 USGS 数据搜索

USGS 数据集包含到状态的罗得岛州相关的记录。 如果您单击一个空搜索框进行**搜索**时，您将得到 50 项默认值。

输入一个搜索词将使搜索引擎出现。 请尝试输入一个区域名称。 "李文 Williams"是罗得岛州第一个调控。 很多公园、 建筑物和学校后他命名。

![][9]

您还可以尝试任何这些条款︰

- Pawtucket
- Pembroke
- 鹅 + 佛得角


## 下一步行动

这是基于 NodeJS 和 USGS 数据集中第一个 Azure 搜索方法辅导教程。 随着时间的推移，我们将扩展本教程演示可能要在您的自定义解决方案中使用的其他搜索功能。

如果在 Azure 搜索已经有一些背景知识，可以试用 suggesters （提前键入或自动完成查询）、 过滤器和多面导航 springboard 作为使用此示例。 您还可以通过添加计数和批处理文档，以便用户可以分页结果改善搜索结果页。

新到 Azure 搜索？ 我们建议尝试以了解您可以创建其他教程。 请访问我们的[文档页中](http://azure.microsoft.com/documentation/services/search/)找到更多的资源。 您还可以查看[视频和教程列表](https://msdn.microsoft.com/library/azure/dn798933.aspx)访问详细信息的链接。

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png
