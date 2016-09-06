---
ms.openlocfilehash: b5e8b72dab74ce395bb2c3b68c746c36be00bc4c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 应用程序服务创建 Node.js web 应用程序 |Microsoft Azure"
    description="了解如何生成和部署在 Azure Node.js web 应用程序。"
    services="app-service\web"
    documentationCenter="nodejs"
    authors="MikeWasson"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.date="08/18/2015"
    ms.author="mwasson"/>

# 生成和部署 Node.js web 应用程序在 Azure 应用程序服务

本教程展示如何创建[节点]的 [nodejs.org]应用程序并使用[Git]将其部署到[Web 应用程序在 Azure 应用程序服务的功能](http://go.microsoft.com/fwlink/?LinkId=529714)。 在本教程中的说明进行操作的后面可以在任何能够运行节点的操作系统上。

下面是完整的应用程序的屏幕快照︰

![浏览器显示 Hello World 消息。][helloworld-completed]

##创建 web 应用程序并启用 Git 发布

请按照以下步骤来创建一个 web 应用程序并启用 Git 发布。

> [AZURE.NOTE]
> 若要完成本教程，您需要一个 Microsoft Azure 帐户。 如果您没有帐户，则可以[激活您的 MSDN 订户权益](/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)或[注册免费试用版](/en-us/pricing/free-trial/?WT.mc_id=A261C142F)。

1. 登录到[Azure 的门户](https://portal.azure.com)。

2. 单击顶部的**+ 新**图标左侧的门户。

3. 单击**Web + 移动**，然后单击**Web 应用程序**。

    ![][portal-quick-create]

4. 输入**URL**的值。

5. 选择应用程序服务计划，或新建一个。 如果您创建一个新的计划，选择定价层、 位置和其他选项。

    ![][portal-quick-create2]

6. 单击**创建**。

7. 一次对**运行**的状态更改，门户将自动打开刀片式服务器的 web 应用程序。 您也可以通过单击**浏览**找到刀片式服务器。

    ![][go-to-dashboard]

8. 单击**部署**。 您可能需要滚动才能看到此部件的刀片式服务器。

    ![][deployment-part]

9. 单击**选择源**，**本地 Git 存储库**，请单击，然后单击**确定**。

    ![][setup-git-publishing]


10. 单击**部署凭据**部件 （如下面的红色所示）。 创建一个用户名和密码。 单击**保存**。 如果先前已启用 web 应用程序的发布，您不必执行此步骤。

    ![][deployment-credentials]


11. 若要发布，将推送到远程 Git 存储库。 查找存储库中的 URL，**所有设置**，请单击，然后单击**属性**。 在**git 中获取 URL**中列出 URL。

    ![][git-url]

##生成和测试您的应用程序本地

在本部分中，您将创建一个**server.js**文件，包含[nodejs.org]的 Hello World 示例。 从原始示例已修改此示例通过将 process.env.PORT 添加为 Azure 的 web 应用程序在运行时侦听的端口。

1. 通过使用文本编辑器，创建一个名为**server.js** **helloworld**目录中的新文件。 如果**helloworld**目录不存在，则创建它。

2. 添加的**server.js**文件中，内容如下，然后保存它︰

        var http = require('http')
        var port = process.env.PORT || 1337;
        http.createServer(function(req, res) {
          res.writeHead(200, { 'Content-Type': 'text/plain' });
          res.end('Hello World\n');
        }).listen(port);

3. 打开命令行，然后使用下面的命令以启动本地 web 应用程序︰

        node server.js

4. 打开 web 浏览器，然后定位到 http://localhost:1337。 将出现显示"Hello World"的网页，如下面的屏幕快照中所示︰

    ![浏览器显示 Hello World 消息。][helloworld-localhost]

##发布您的应用程序

1. 从命令行中，将目录更改到**helloworld**目录并输入以下命令可初始化本地 Git 存储库。

        git init

    > [AZURE.NOTE] 是的 Git 命令不可用？
    [Git](http://git-scm.com/%20target="_blank)是一个分布式的版本控制系统，可用于部署 Azure 网站。 有关用于您的平台的安装说明，请参阅[Git 下载页面](http://git-scm.com/download%20target="_blank")。

2. 使用以下命令来将文件添加到存储库︰

        git add .
        git commit -m "initial commit"

3. 添加 Git 远程将更新推送到以前，通过使用下面的命令创建的 web 应用程序︰

        git remote add azure [URL for remote repository]


4. 将更改推送到 Azure，通过使用下面的命令︰

        git push azure master

    您将被提示输入密码之前创建。 输出内容应类似如下︰

        Counting objects: 3, done.
        Delta compression using up to 8 threads.
        Compressing objects: 100% (2/2), done.
        Writing objects: 100% (3/3), 374 bytes, done.
        Total 3 (delta 0), reused 0 (delta 0)
        remote: New deployment received.
        remote: Updating branch 'master'.
        remote: Preparing deployment for commit id '5ebbe250c9'.
        remote: Preparing files for deployment.
        remote: Deploying Web.config to enable Node.js activation.
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
         * [new branch]      master -> master


5. 若要查看您的应用程序，请单击在 Azure 门户的**Web 应用程序**部分的**浏览**按钮。

##将更改发布到您的应用程序

1. 在文本编辑器中打开**server.js**文件，并将 Hello World\n' 改为 ' Hello Azure\n。 保存该文件。
2. 从命令行中，将目录更改到**helloworld**目录并运行下面的命令︰

        git add .
        git commit -m "changing to hello azure"
        git push azure master

    您将被提示输入密码之前创建。

3. 通过单击**浏览**，浏览到您的应用程序，并注意尚未应用这些更新。

    ![Web 页显示 Hello Azure][helloworld-completed]

4. 您可以通过在**部署**中选择恢复以前的部署。

>[AZURE.NOTE] 如果您想要开始使用 Azure 应用程序服务注册 Azure 帐户之前，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)。 那里，您立即可以应用程序服务中创建短期初学者 web 应用程序 — 需要，没有信用卡，没有承诺。

##下一步行动

虽然这篇文章中的步骤使用 Azure 门户创建一个 web 应用程序，您可以使用[Azure 命令行界面](../xplat-cli.md)来执行相同的操作。

Node.js 提供了一个丰富的生态系统，可供您的应用程序的模块。 Web 应用程序与模块的工作方式，请参阅[使用 Node.js Azure 应用程序的模块](../nodejs-use-node-modules-azure-apps.md)。

Node.js 附带 Azure 以及如何指定要与应用程序一起使用的版本的版本有关详细信息，请参阅[指定 Node.js 版本 Azure 应用程序中](../nodejs-specify-node-version-azure-apps.md)。

如果您遇到问题的应用程序部署到 Azure 后，请参阅[如何调试 Azure 应用程序服务中的 Node.js 应用](web-sites-nodejs-debug.md)诊断问题的信息。


##其他资源

* [Azure PowerShell](../install-configure-powershell.md)
* [Azure 的命令行界面](../xplat-cli.md)

## 会发生什么变化
* 从对应用程序服务的网站更改的指南，请参阅[Azure 应用程序服务和现有的 Azure 服务](http://go.microsoft.com/fwlink/?LinkId=529714)。
* 从旧到新门户网站门户更改的指南，请参阅[参考的导航 Azure 的门户](http://go.microsoft.com/fwlink/?LinkId=529715)。


[nodejs.org]: http://nodejs.org
[Git]: http://git-scm.com


[helloworld 完成]: ./media/web-sites-nodejs-develop-deploy-mac/helloazure.png
[helloworld 本地主机]: ./media/web-sites-nodejs-develop-deploy-mac/helloworldlocal.png

[门户网站快速创建]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website.png

[门户网站的快速-create2]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website2.png


[安装 git 发布]: ./media/web-sites-nodejs-develop-deploy-mac/setup_git_publishing.png

[转到控制板]: ./media/web-sites-nodejs-develop-deploy-mac/go_to_dashboard.png

[部署部分]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-part.png

[部署凭据]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-credentials.png


[git url]: ./media/web-sites-nodejs-develop-deploy-mac/git-url.png

测试
