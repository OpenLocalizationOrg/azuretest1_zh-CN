---
ms.openlocfilehash: 2164687878b9a28214888343495c1eb3d153cf93
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="生成和部署 Java API 的应用程序在 Azure 应用程序服务"
    description="了解如何创建一个 Java API 的应用程序的包并将其部署到 Azure 应用程序服务。"
    services="app-service\api"
    documentationCenter="java"
    authors="pkefal"
    manager="mohisri",
    editor="jimbe"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/11/2015"
    ms.author="pakefali"/>

# 生成和部署 Java API 的应用程序在 Azure 应用程序服务

本教程演示如何创建一个 Java 应用程序并将它部署到 Azure 应用程序服务 API 应用程序使用[Git](http://git-scm.com)。 在本教程中的说明进行操作的后面可以在任何能够运行 Java 的操作系统上。 本教程还正在使用[Gradle](https://gradle.org)启用生成自动化和程序包依赖项解析为 Java 应用程序。 最后， [RESTEasy](http://resteasy.jboss.org/)用来创建 RESTful 服务，完全实现[JaxRS](https://jax-rs-spec.java.net/)规范。

下面是完整的应用程序的屏幕快照︰

![][sample-api-app-page]

## 在 Azure 的门户网站中创建 API 的应用程序

> [AZURE.NOTE] 若要完成本教程，您需要一个 Microsoft Azure 帐户。 如果您没有帐户，则可以[激活您的 MSDN 订户权益](/pricing/member-offers/msdn-benefits-details/)或[注册免费试用版](/pricing/free-trial/)。
您还可以尝试免费[的应用程序服务的应用程序示例](http://tryappservice.azure.com)。

1. 登录到[Azure 预览门户](https://portal.azure.com)。

2. 单击**新建**门户的左下角。

3. 单击**Web + 移动 > API 应用程序**。

    ![][portal-quick-create]

4. 输入的**名称**，如 JavaAPIApp 的值。

5. 选择应用程序服务计划，或新建一个。 如果您创建一个新的计划，选择定价层、 位置和其他选项。

    ![][portal-create-api]

6. 单击**创建**。

    ![][api-app-blade]

    如果您选中了**添加到 Startboard**复选框，门户将创建后自动打开刀片式服务器为您的 API 应用程序。 如果清除复选框，单击**通知**在门户的主页上，可以查看 API 的应用程序创建状态，然后单击要转到刀片式服务器的新 API 应用程序的通知。

7. 单击**设置 > 应用程序设置**。

9. 将访问级别设置为**公共 （匿名）**。

11. 单击**保存**。

    ![][set-api-app-access-level]

## 启用新的 API 应用程序的 Git 发布

[Git](http://git-scm.com)是分布式的版本控制系统，可用于部署 Azure 网站。 将存储 API 应用程序在本地 Git 存储库，为您编写的代码，并将在通过推送到远程资源库中将代码部署到 Azure。 这种部署方法是您可以使用 API 应用程序中，因为 API 的应用程序基于 web 的应用程序的应用程序服务 web 应用程序的功能︰ 在 Azure 应用程序服务 API 的应用程序是 web 应用程序与承载 web 服务的其他功能。  

您在门户网站中管理**API 应用程序**刀片式服务器，在 API 应用程序特定的功能和管理**API 应用程序主机**刀片式服务器中共享与 web 应用程序的功能。 因此，在这一节中您将转到**API 应用程序主机**刀片式服务器配置 Git 部署功能。

1. 在 API 的应用程序刀片式服务器，请单击**API 应用程序主机**。

    ![][api-app-host]

2. 找到**应用程序 API**刀片式服务器的**部署**部分，然后单击**设置连续部署**。 您可能需要向下滚动才能看到此刀片式服务器的一部分。

    ![][deployment-part]

3. 单击**选择源 > 本地 Git 存储库**。

5. 单击**确定**。

    ![][setup-git-publishing]

6. 如果您有没有以前设置了部署凭据的发布 API 的应用程序或其他应用程序服务的应用程序，将它们设置现在︰

    * 单击**设置凭据部署**。

    * 创建一个用户名和密码。

    * 单击**保存**。

    ![][deployment-credentials]

1. 在**API 的应用程序主机**刀片式服务器，请单击**设置 > 属性**。 "GIT URL"下显示您将部署到远程 Git 存储库的 URL。

2. 在本教程后面部分复制使用的 URL。

    ![][git-url]

## 启用新的 API 应用程序上的 Java 运行时

API 应用程序已成功设立一个 Java 应用程序，我们必须启用 Java 运行库并选择应用程序服务器。 门户网站提供了简便的方法来执行此操作。 我们将以启用 Java 7 和 Jetty 承载我们的应用程序。

1. 在 API 的应用程序刀片式服务器，请单击**API 应用程序主机**。

    ![][api-app-host]

2. 单击**设置 > 应用程序设置**。 那里，启用 Java 并选择 Jetty 作为应用程序服务器。 单击**保存**

    ![][api-app-enable-java]

这将**启用 Java 运行时**API 应用程序并创建**webapps /**在您的站点的根文件夹。 此文件夹将包含所有应用程序的.war 文件。

## 下载并检查 Java API 的应用程序的代码

在本节中，将下载并看看 JavaAPIApp 示例中提供的代码。

1. 下载[此 GitHub 存储库](http://go.microsoft.com/fwlink/?LinkId=571009)中的代码。 可以克隆存储库，或单击**下载 Zip**下载它以.zip 文件格式。 如果您下载的.zip 文件，则将其解压缩在您的本地磁盘中。

2. 导航到文件夹已解压缩示例，然后定位到`build\libs\`文件夹。

    ![][api-app-folder-browse]

3. 在文本编辑器中打开**apiapp.json**文件，检查内容。

    ![][apiapp-json]

    Azure 应用程序服务以识别为应用程序 API 的 Java 应用程序有两个前提条件︰

    + 名为*apiapp.json*的文件已存在于应用程序的根目录。
    + Swagger 2.0 元数据终结点必须由应用程序公开。 在*apiapp.json*文件中指定此终结点的 URL。

    请注意**apiDefinition**属性。 此 URL 的路径是相对于 API 的 URL，它指向 Swagger 2.0 终结点。 Azure 应用程序服务使用此属性来发现您的 API 定义并启用应用程序服务 API 的应用程序功能的很多。

4. 定位到`src\main\java\com\microsoft\trysamples\javaapiapp`，请打开**App.java**文件，然后检查的代码。

    ![][app-java]

    该代码使用 JaxRS 的 Swagger 软件包创建 Swagger 2.0 终结点。

        beanConfig.setVersion("1.0.0");
        beanConfig.setBasePath("/JavaAPIApp/api");
        beanConfig.setHost(websitehostname);
        beanConfig.setResourcePackage("com.microsoft.trysamples.javaapiapp");
        beanConfig.setSchemes(new String[]{"http", "https"});
        beanConfig.setScan(true);

    `setVersion`方法设置 API 版本中由 Swagger 提供的元数据。

    `setBasePath`方法设置 Swagger 用于生成元数据的基本路径。 此 URL 是相对于您 API 的应用程序基路径。

    `setHost`方法设置 API 正在监听的主机。 在这种情况下，我们使用`websitehostname`变量，我们已分配前的几行动态地设置为`localhost`在本地运行或 API 应用程序主机名 Azure 应用程序服务中运行应用程序时。

    `setResourcePackage`方法设置包 Swagger 将扫描并包括在 Swagger.json 文件中，包含元数据 API。

    `setSchemes`方法定义受支持的方案。

    `setScan`方法使 Swagger 生成的应用程序文档。

    有多个方法可用时使用 RESTEasy 自定义输出的 Swagger 和 Swagger 的[Wiki 页面](https://github.com/swagger-api/swagger-core/wiki/Swagger-Core-RESTEasy-2.X-Project-Setup-1.5#using-swaggers-beanconfig)上可以找到它们

    > [AZURE.NOTE] Swagger 元数据文件可以在访问`/JavaAPIApp/api/swagger.json`。

## 本地运行 API 的应用程序代码

这一节中您运行本地以验证它工作正常然后再部署应用程序。

1. 导航到文件夹您下载的示例。

2. 打开命令提示符并输入以下命令︰

        gradlew.bat

3. 在此命令完成后，请输入以下命令︰

        gradlew.bat jettyRunWar

    命令行窗口输出所示︰

        17:25:49 INFO  JavaAPIApp runs at:
        17:25:49 INFO    http://localhost:8080/JavaAPIApp

5. 浏览器访问 `http://localhost:8080/JavaAPIApp/`

    请参阅下页

    ![][sample-api-app-page]

6. 若要查看 Swagger.json 文件，请导航到`http://localhost:8080/JavaAPIApp/api/Swagger.json`。

## 发布到 Azure 应用程序服务的 API 的应用程序代码

本部分中创建一个本地 Git 存储库和推从该存储库到 Azure 为了部署到 API 应用程序运行在 Azure 应用程序服务的示例应用程序。

1. 如果未安装 Git， [Git 下载页面](http://git-scm.com/download)中安装它。

1. 从命令行中，将目录更改到示例应用程序目录中，然后`build\libs`，并输入以下命令可初始化本地 Git 存储库。

        git init


2. 输入以下命令以将文件添加到存储库︰

        git add .
        git commit -m "Initial commit of the API App"

3. 创建将更新推送到您以前，创建使用先前复制的 Git URL 的 web 应用程序 （API 应用程序主机） 的远程引用︰

        git remote add azure [URL for remote repository]

4. 将更改推送到 Azure，通过输入以下命令︰

        git push azure master

    系统提示您以前创建的密码。

    此命令的输出端与部署成功的一条消息︰

        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
        * [new branch]      master -> master

## 在 Azure 门户中查看 API 定义

既然于 API 应用程序部署了一个 API，您可以看到 Azure 门户中的 API 定义。 首先，将通过重新启动网*网关*，这使 Azure 识别 API 的应用程序的 API 定义已更改。 网关是处理 API 管理及授权 API 的应用程序的资源组中的一个 web 应用程序。

6. 在 Azure 的门户中，转到**API 应用程序**刀片式服务器的前面，创建 API 应用程序并单击**网关**链接。

    ![][click-gateway]

7. 在**网关**刀片式服务器，单击**重新启动**。 现在，您可以关闭此刀片。

    ![][restart-gateway]

8. 在**API 的应用程序**刀片式服务器，请单击**API 定义**。

    ![][api-definition-click]

    **API 定义**刀片式服务器显示一个 Get 方法。

    ![][api-definition-blade]

## 在 Azure 中运行示例应用程序

在 Azure 的门户中，转到您 API 的应用程序，用于**API 应用程序主机**刀片式服务器，单击**浏览**。

![][browse-api-app-page]

浏览器将显示您以前看到过本地运行示例应用程序的主页。  

## 下一步行动

您已经部署了使用到 Azure 的 API 的应用程序的后端的 Java web 应用程序。 在 Azure 中使用 Java 的更多信息，请参见[Java 开发人员中心](/develop/java/)。

您可以尝试使用此示例在[TryApp 服务](http://tryappservice.azure.com)的 API 应用程序

[门户网站快速创建]: ./media/app-service-api-java-api-app/portal-quick-create.png
[门户创建 api]: ./media/app-service-api-java-api-app/portal-create-api.png
[刀片式服务器 api 的应用程序的]: ./media/app-service-api-java-api-app/api-app-blade.png
[api 的应用程序文件夹浏览]: ./media/app-service-api-java-api-app/api-app-folder-browse.png
[api 的应用程序主机]: ./media/app-service-api-java-api-app/api-app-host.png
[部署部分]: ./media/app-service-api-java-api-app/continuous-deployment.png
[组-api 的应用程序的访问级别]: ./media/app-service-api-java-api-app/set-api-app-access.png
[安装 git 发布]: ./media/app-service-api-java-api-app/local-git-repo.png
[部署凭据]: ./media/app-service-api-java-api-app/deployment-credentials.png
[git url]: ./media/app-service-api-java-api-app/git-url.png
[apiapp json]: ./media/app-service-api-java-api-app/apiapp-json.png
[java 应用程序]: ./media/app-service-api-java-api-app/app-java.png
[示例 api 的应用程序页]: ./media/app-service-api-java-api-app/sample-api-app-page.png
[浏览 api 的应用程序页]: ./media/app-service-api-java-api-app/browse-api-app-page.png
[api 的应用程序启用 java]:./media/app-service-api-java-api-app/api-app-enable-java.png
[单击网关]:./media/app-service-api-java-api-app/clickgateway.png
[重新启动网关]:./media/app-service-api-java-api-app/gatewayrestart.png
[api 定义单击]:./media/app-service-api-java-api-app/apidef.png
[api 定义刀片]:./media/app-service-api-java-api-app/apidefblade.png
 
测试
