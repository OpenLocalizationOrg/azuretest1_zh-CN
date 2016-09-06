---
ms.openlocfilehash: 12aa3786045ee6633116b043c3f17b7a3af9e52e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    urlDisplayName="Jenkins Continuous Integration" 
    pageTitle="Jenkins 持续集成解决方案中使用 Azure 存储 |Microsoft Azure" 
    metaKeywords="" 
    description="本教程说明如何使用 Azure blob 服务的知识库构建由 Jenkins 持续集成解决方案的项目。" 
    metaCanonical="" 
    services="storage" 
    documentationCenter="java" 
    title="" 
    authors="rmcmurray" 
    solutions="" 
    manager="wpickett" 
    editor="jimbe" 
    scriptId="" 
    videoId=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="robmcm"/>

# Jenkins 持续集成解决方案使用 Azure 存储

*通过[Microsoft 打开技术公司][打开 ms 科技]*

## 概述

以下信息显示了如何使用 Azure Blob 服务作为生成工件由 Jenkins 持续集成 (CI) 解决方案中，创建一个存储库或源的可下载文件在生成过程中使用。 正在编码 （使用 Java 或其他语言） 的敏捷开发环境中时，您会发现这很有用方案之一是，在持续集成中，正在运行的生成基于您对您生成的项目，需要存储库，以便可以例如，将它们与其他组织的成员，您的客户共享或维护档案。 另一种情况是当您生成作业本身需要其他文件，例如，依赖关系作为生成输入的一部分下载。

在本教程中将使用 Azure 存储插件为 Jenkins CI 由 Microsoft 开放技术，Inc.提供

## Jenkins 的概述 ##

Jenkins 通过使开发人员能够轻松地集成他们的代码更改启用持续集成的软件项目，生成具有产生自动和频繁，从而提高开发人员的工作效率。 更改版本，生成并生成工件可以上载到不同存储库。 本主题将说明如何使用 Azure blob 存储为存储库中的生成工件。 它还将显示如何从 Azure blob 存储下载依赖项。

在[满足 Jenkins][]找 Jenkins 有关的详细信息。

## 使用 Blob 服务的好处 ##

使用 Blob 服务来承载您的敏捷开发生成项目的好处包括︰

- 生成项目和/或下载依赖项的高可用性。
- Jenkins CI 解决方案上载您生成项目时的性能。
- 当您的客户和合作伙伴下载您生成项目时的性能。
- 控制用户访问策略，匿名访问、 基于到期的共享的访问签名访问、 私人之间进行选择，访问，等等。

## Prequisites ##

您将需要下列操作以使用 Blob 服务与 Jenkins CI 解决方案︰

- Jenkins 持续集成的解决方案。

    如果当前没有 Jenkins CI 解决方案，您可以运行 Jenkins CI 解决方案使用下列技术︰

    1. 在启用了 Java 的计算机上，从<http://jenkins-ci.org>下载 jenkins.war。
    2. 在包含 jenkins.war 文件夹打开一个命令窗口，运行︰

        `java -jar jenkins.war`

    3. 在浏览器中，打开`http://localhost:8080/`。 这将打开 Jenkins 仪表板，您将使用它来安装和配置 Azure 存储插件。

        而典型的 Jenkins CI 解决方案还将设置为运行作为一项服务，在命令行中运行 Jenkins 战争将足以满足本教程。

- Azure 帐户。 您可以在<http://www.azure.com>Azure 帐户注册。

- Azure 存储帐户。 如果没有存储帐户，您可以创建一个在[如何创建存储帐户][]使用的步骤。

- 熟悉 Jenkins CI 解决方案建议但不是需要，因为下面的内容将使用一个基本示例向您展示 Jenkins CI 作为资料库，使用 Blob 服务时所需的步骤生成工件。

## 如何使用 Blob 服务 Jenkins CI ##

若要使用 Jenkins Blob 服务，您需要安装 Azure 存储插件，该插件要使用您的存储帐户，配置，然后创建后期生成操作将生成项目上载到您的存储帐户。 以下各节介绍这些步骤。

## 如何安装 Azure 存储插件 ##

1. 在 Jenkins 面板中，单击**管理 Jenkins**。
2. 在**管理 Jenkins**页中，单击**管理插件**。
3. 单击**可用**选项卡。
4. 在**项目上载程序**部分中，选中**Microsoft Azure 存储插件**。
5. 单击**安装时不重新启动**或**立即下载并安装后重新启动**。
6. 重新启动 Jenkins。

## 如何配置为使用您的存储帐户的 Azure 存储插件 ##

1. 在 Jenkins 面板中，单击**管理 Jenkins**。
2. 在**管理 Jenkins**页中，单击**配置系统**。
3. 在**Microsoft Azure 存储帐户配置**部分中︰
    1. 请输入您存储的帐户名称，可以从 Azure 的门户中， <https://manage.windowsazure.com>获得的。
    2. 输入存储帐户密钥，也可获得从 Azure 的门户。
    3. 如果您正在使用公共的 Azure 云**Blob 服务终结点**的 url 中使用默认值。 如果您使用的不同的 Azure 云，Azure 管理门户中指定有关您的存储帐户使用端点。 
    4. 单击**验证存储凭据**来验证您的存储帐户。 
    5. [可选]如果您具有所需可供 Jenkins CI 的附加存储帐户，请单击**添加更多存储帐户**。
    6. 单击**保存**以保存您的设置。

## 如何创建将生成项目上载到您的存储帐户的后期生成操作 ##

第一次出于说明的目的，我们将需要创建一个作业，将创建多个文件，然后再添加后期生成操作将文件上载到您的存储帐户中。

1. Jenkins 面板中单击**新项目**。
2. 命名作业**MyJob**，单击**生成样式自由软件项目**，然后单击**确定**。
3. 在作业配置的**生成**部分，请单击**添加的生成步骤**，然后选择**执行 Windows 批处理命令**。
4. 在**命令**中，使用以下命令︰

        md text
        cd text
        echo Hello Azure Storage from Jenkins > hello.txt
        date /t > date.txt
        time /t >> date.txt
 
5. 作业配置**后期生成操作**部分中，单击**添加后期生成操作**并选择**上载到 Azure Blob 存储的项目**。
6. 对于**存储帐户名称**，选择要使用的存储帐户。
7. **容器名称**，指定容器名称。 （如果它尚不存在上载生成项目时，将创建容器。）您可以使用环境变量，所以此示例容器名称作为输入**${JOB_NAME}** 。

    **提示**
    
    **命令**下**执行 Windows 批处理命令**的输入脚本所在的部分是 Jenkins 识别的环境变量的链接。 单击该链接，以了解环境变量的名称和说明。 请注意，作为容器名称或通用的虚拟路径不能包含特殊字符，如**BUILD_URL**环境变量，环境变量。

8. 对于此示例，请单击**默认公开新的容器**。 （如果您想要使用专用容器，您将需要创建共享的访问签名允许访问。 这就是超出了本主题的范围。 您可以了解更多有关在[创建共享访问签名](http://go.microsoft.com/fwlink/?LinkId=279889)的共享的访问签名。）
9. [可选]如果想要上载的生成项目之前被清除的内容的容器，请单击**干净容器在上载之前，** （选中它如果不想清洁容器的内容）。
10. 对于**列表中的项目，要上载**，输入**文本 /*.txt**。
11. **已上载项目的常见虚拟路径**，对于本教程，输入**${生成\_ID} / ${生成\_号}**。
12. 单击**保存**以保存您的设置。
13. Jenkins 仪表板，请单击**立即生成**运行**MyJob**。 检查状态的控制台输出。 后期生成操作启动上载生成项目时将在控制台输出中包括 Azure 存储的状态消息。
14. 成功完成后的作业，您可以通过打开公钥 blob 检查生成工件。
    1. 登录到 Azure 的管理门户， <https://manage.windowsazure.com>。
    2. 单击**存储**。
    3. 请单击用于 Jenkins 存储帐户名称。
    4. 单击**容器**。
    5. 单击名为**myjob**，即分配创建 Jenkins 作业时作业名称的小写版本的容器。 容器名称和 blob 名称是小写字母 （和区分大小写） 在 Azure 存储中。 名为**myjob**的容器的 blob 的列表中，您应该看到**hello.txt**和**date.txt**。 这两个项目中复制的 URL，并在您的浏览器中打开它。 您将看到的文件，上载生成工件。

只有一个后期生成操作将项目上载到 Azure blob 存储，可以创建每个作业。 请注意，单个的后期生成操作，可将项目上载到 Azure blob 存储可以指定不同的文件 （包括通配符） 和内使用分号分隔的**列表中的项目，要上载**的文件的路径。 例如，如果 Jenkins 生成生成 JAR 文件和 TXT 文件在您的工作区**创建**文件夹中，并且您希望上载到 Azure blob 存储两个，请使用下面的**列表中的项目，以传**值︰**生成 /\*.jar; 生成 /\*.txt**。 您还可以使用双冒号语法来指定要使用中的 blob 名称的路径。 例如，如果您希望获取上载使用 blob 路径和 TXT 文件中的**二进制**blob 路径中使用**通告**上载 Jar，用于以下**列表中的项目，以传**值︰**生成 /\*。 jar::binaries; 生成 /\*。 txt::notices**。

## 如何创建从 Azure blob 存储下载的生成步骤 ##

下面的步骤演示如何配置从 Azure blob 存储下载项的生成步骤。 这是非常有用的如果您想要包括在生成的项目，例如，保存在 Azure 的 Jar blob 存储。

1. 在作业配置的**生成**部分，请单击**添加的生成步骤**，然后选择**从 Azure Blob 存储下载**。
2. 对于**存储帐户名称**，选择要使用的存储帐户。
3. **容器名称**，指定包含要下载的 blob 容器的名称。 您可以使用环境变量。
4. **Blob 名称**，指定的 blob 名称。 您可以使用环境变量。 此外，可以使用星号，作为通配符指定初始盘符的 blob 名称后。 例如，**项目\***将指定**项目**名称开头的所有 blob。
5. [可选]**下载路径**，Jenkins 计算机想要从 Azure blob 存储下载文件中指定的路径。 此外可以使用环境变量。 （如果您没有提供一个值的**下载路径**，Azure blob 存储中的文件将下载到工作区。）

如果您有想要从 Azure blob 存储下载的其他项目，您可以创建其他生成步骤。

运行生成后，可以检查生成历史记录控制台输出，也可以看一看您的下载位置，以查看是否已成功下载您所期望的 blob。  

## 使用 Blob 服务组件 ##

以下内容概述 Blob 服务组件。

- **存储帐户**︰ 对 Azure 存储的所有访问都是通过存储帐户。 这是最高级别的访问 blob 的命名空间。 一个帐户可以包含任意的数量的容器，只要其总量在 100 TB。
- **容器**︰ 容器提供了一套 blob 的分组。 所有 blob 都必须在容器中。 一个帐户可以包含任意的数量的容器。 一个容器可以存储任意的数量的 blob。
- **Blob**︰ 任何类型和大小的文件。 有的可以在 Azure 存储中存储的 blob 的两种类型︰ 数据块和页面 blob。 大多数文件是块 blob。 单个块斑点可达 200GB 的大小。 本教程使用块 blob。 频繁地修改文件中的字节范围的页面 blob，另一个 blob 类型，可能会达 1 TB 的容量和更高效。 有关 blob 的详细信息，请参阅[了解块 Blob 和页面 Blob](http://msdn.microsoft.com/library/azure/ee691964.aspx)。
- **URL 格式**︰ Blob 是可寻址使用以下 URL 格式︰

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
    
    （以上格式适用于公共 Azure 的云。 如果您使用的不同的 Azure 云，使用 Azure 管理门户中的终结点来确定 URL 端点。）

    在上面的格式`storageaccount`表示您的存储帐户的名称`container_name`表示名称的容器，和`blob_name`分别代表您 blob 的名称。 在容器名称，您可以用一个正斜杠分隔的多个路径**/**。 在本教程中的示例容器名称是**MyJob**，和**${生成\_ID} / ${生成\_号}**用于常见虚拟路径，从而导致该 blob 具有以下形式的 URL:

    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## 下一步行动

  [如何创建存储帐户。]: http://go.microsoft.com/fwlink/?LinkId=279823
  [满足 Jenkins]: https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins
  [打开 ms 科技]: http://msopentech.com
 