---
ms.openlocfilehash: ce330468b82206d60ebd7de764e84d48b402378a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Java 在 Azure 搜索 |Microsoft Azure"
    description="如何构建自定义 Azure 搜索应用程序，使用 Java 作为您的编程语言。"
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
    ms.date="06/24/2015"
    ms.author="heidist"/>

# 在 Java 中的 Azure 搜索入门

了解如何构建自定义的 Java 搜索应用程序使用 Azure 搜索的搜索体验。 本教程使用[Azure 搜索服务 REST API](https://msdn.microsoft.com/library/dn798935.aspx)来构造对象并在本练习中使用的操作。

我们使用了以下软件生成并测试该示例︰

- [日蚀式的 Java EE 开发 IDE](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar)。 请确保下载 EE 版本。 一个验证步骤要求只能在此版本中的功能。

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache 8.0 Tomcat](http://tomcat.apache.org/download-80.cgi)

若要运行此示例，您需要 Azure 搜索服务，您可以注册[Azure 管理门户](https://portal.azure.com)中。

> [AZURE.TIP] 在 Github 上的[Azure 搜索 Java 演示](http://go.microsoft.com/fwlink/p/?LinkId=530197)本教程下载的源代码。

## 有关的数据

此示例应用程序使用从[美国地质服务 (USGS)](http://geonames.usgs.gov/domestic/download_data.htm)，筛选上状态的罗得岛州可减小数据集的数据。 我们将使用此数据来生成返回地标建筑物，如医院和学校，以及如流、 湖泊和峰地质特征的搜索应用程序。

在此应用中， **SearchServlet.java**程序生成并加载使用[索引器](https://msdn.microsoft.com/library/azure/dn798918.aspx)的构造，从公共的 Azure SQL 数据库检索筛选的 USGS 数据集的索引。 在程序代码中提供了预定义的凭据和在线数据源的连接信息。 在数据访问方面不不需要任何进一步配置。

> [AZURE.NOTE] 我们要保持在 10000 文档限制自由定价层下此数据集上应用筛选器。 如果您使用标准层，此限制不适用，并可修改此代码以使用更大的数据集。 对于每个定价层产能的详细信息，请参阅[限制和约束](https://msdn.microsoft.com/library/azure/dn798934.aspx)。

## 有关的程序文件

下面的列表描述与此示例的文件。

- Search.jsp︰ 提供的用户界面
- SearchServlet.java︰ 提供方法 （类似于在 MVC 控制器）
- SearchServiceClient.java︰ 处理 HTTP 请求
- SearchServiceHelper.java︰ 一个帮助器类提供静态方法
- Document.java︰ 提供数据模型
- config.properties︰ 设置搜索服务 URL 和 api 密钥
- Pom.xml: Maven 依赖项


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

    - **资源组**是容器的服务和资源用于公共目的。 例如，如果您正在构建基于 Azure 搜索 Azure BLOB 存储在 Azure 网站的自定义搜索应用程序可以创建门户管理页面中一起保持这些服务的资源组。

    - **订阅**可以选择采用不同的多个订阅，如果有多个订阅。

    - **位置**是数据中心区域。 目前，所有的资源必须在同一个数据中心中运行。 不支持在多个数据中心之间分配资源。

4. 单击**创建**来提供服务。

观察 Jumpbar 中的通知。 可以使用服务时，会出现通知。

<a id="sub-2"></a>
## 查找的服务名称和 Azure 搜索服务的 api 键

创建服务后，您可以返回到门户网站获取 URL 和`api-key`。 连接到您的搜索服务都要求您拥有两个 URL 和`api-key`进行身份验证的调用。

1. 在 Jumpbar 中，单击**主页**，然后单击搜索服务若要打开服务操控板。

2. 在服务面板中，您将看到拼贴的基本信息以及访问管理密钥的钥匙图标。

    ![][3]

3. 复制服务 URL 和管理密钥。 添加到**config.properties**文件时，您将从以后，需要它们。

## 下载示例文件

1. 在 Github 上转到[AzureSearchJavaDemo](http://go.microsoft.com/fwlink/p/?LinkId=530197) 。

2. 请单击**下载 ZIP**，将该.zip 文件保存到磁盘，然后提取它所包含的所有文件。 请考虑将文件提取到您的 Java 区为了便于以后查找项目。

3. 示例文件都是只读的。 用鼠标右键单击文件夹的属性，清除只读属性。

会对此文件夹中的文件进行的所有后续文件修改和运行的语句。  

## 导入项目

1. 在 Eclipse 中，选择**文件** > **导入** > **常规** > **到工作区中的现有项目**。

    ![][4]

2. 在**选择根目录**，请浏览到包含示例文件的文件夹。 选择包含.project 文件夹的文件夹。 该项目应在**项目**列表中显示为选定的项目。

    ![][12]

3. 单击**完成**。

4. 使用**项目资源管理器**来查看和编辑这些文件。 如果尚未打开，请单击**窗口** > **显示视图** > **项目资源管理器**或使用该快捷方式以打开它。

## 配置服务 URL 和 api 密钥

1. 在**项目资源管理器**中双击**config.properties**编辑包含服务器名称和 api 密钥的配置设置。

2. 请参阅在本文中，您找到的服务 URL 和 api 密钥在[Azure 的门户网站](https://portal.azure.com)，来获取您现在将输入到**config.properties**的值前面的步骤。

3. 在**config.properties**，与您的服务的 api 键替换"Api 密钥"。 接下来，服务名称 （URL http://servicename.search.windows.net 的第一个组件） 替换同一文件中的"服务名称"。

    ![][5]

## 配置项目、 生成和运行时环境

1. 在 Eclipse 中，在项目资源管理器中右击项目 >**属性** > **项目方面**。

2. 选择**动态 Web 模块**、 **Java**和**JavaScript**。

    ![][6]

3. 单击**应用**。

4. 选择**窗口** > **首选项** > **服务器** > **运行时环境** > **添加。**。

5. 展开 Apache 并选择 Apache Tomcat 服务器以前安装的版本。 在我们的系统，我们可以安装版本 8。

    ![][7]

6. 在下一页上指定 Tomcat 安装目录。 在 Windows 计算机上，这将很有可能是 C:\Program Files\Apache 软件 Foundation\Tomcat*版*。

6. 单击**完成**。

7. 选择**窗口** > **首选项** > **Java** > **安装的 Jre** > **添加**。

8. 在**添加的 JRE**，请选择**标准 VM**。

10. 单击**下一步**。

11. 在 JRE 的定义，请在 JRE 家庭，单击**目录**。

12. 定位到**程序文件** > **Java**并选择 JDK 以前安装。 请务必选择作为 JRE 的 JDK。

13. 在安装的 Jre，请选择**JDK**。 您的设置应类似于下面的屏幕快照。

    ![][9]

14. （可选） 选择**窗口** > **Web 浏览器** > **Internet Explorer**在外部浏览器窗口中打开应用程序。 使用外部浏览器为您提供更好的 Web 应用程序体验。

    ![][8]

现在，您已完成的配置任务。 接下来，您将生成并运行该项目。

## 生成项目

1. 在项目资源管理器中右击项目名称，并选择**运行方式** > **Maven 构建...**配置项目。

    ![][10]

8. 在编辑配置中，在目标，键入"干净安装"，然后单击**运行**。

状态消息将输出到控制台窗口。 应该看到，指示没有错误生成的项目的生成成功。

## 运行应用程序

在这最后一步，将在本地服务器运行时环境中运行应用程序。

如果您还没有在 Eclipse 中指定服务器运行时环境，您需要实现这个愿望。

1. 在项目资源管理器中，展开**网站**。

5. 用鼠标右键单击**Search.jsp** > **以运行** > **在服务器上运行**。 Apache Tomcat 服务器中选择，然后单击**运行**。

> [AZURE.TIP] 如果您使用非默认的工作区来存储您的项目，您需要修改**运行配置**为指向该项目位置，以避免服务器启动错误。 在项目资源管理器中，右击**Search.jsp** > **Run As** > **运行配置**。 选择的 Apache Tomcat 服务器。 单击**参数**。 单击**工作区**或**文件系统**设置包含项目的文件夹。

当您运行应用程序时，您应该看到一个浏览器窗口，提供搜索框中输入术语。

等待大约一分钟之前单击**搜索**以使服务时间，来创建并加载索引。 如果收到一个 HTTP 404 错误，只需等待得有点长时间然后重试。

## 根据 USGS 数据搜索

USGS 数据集包含到状态的罗得岛州相关的记录。 如果您单击一个空搜索框进行**搜索**时，您将得到 50 项默认值。

输入一个搜索词将使搜索引擎出现。 请尝试输入一个区域名称。 "李文 Williams"是罗得岛州第一个调控。 很多公园、 建筑物和学校后他命名。

![][11]

您还可以尝试任何这些条款︰

- Pawtucket
- Pembroke
- 鹅 + 佛得角

## 下一步行动

这是基于 Java 和 USGS 数据集中第一个 Azure 搜索方法辅导教程。 随着时间的推移，我们将扩展本教程演示可能要在您的自定义解决方案中使用的其他搜索功能。

Azure 搜索中已经有一些背景知识，如果您可以使用此示例 springboard 作为进一步实验，或许充实[搜索页](search-pagination.md)上，或实现[多面导航](../search-faceted-navigation/)。 您还可以通过添加计数和批处理文档，以便用户可以分页结果改善搜索结果页。

新到 Azure 搜索？ 我们建议尝试以了解您可以创建其他教程。 请访问我们的[文档页中](http://azure.microsoft.com/documentation/services/search/)找到更多的资源。 您还可以查看[视频和教程列表](https://msdn.microsoft.com/library/azure/dn798933.aspx)访问详细信息的链接。

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
