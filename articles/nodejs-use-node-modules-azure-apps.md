---
ms.openlocfilehash: efd53ab970e3ee00898118294863b51d7fd417d7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="使用 Node.js 模块" description="了解如何使用 Node.js 模块时使用 Azure 网站或云服务。" services="" documentationCenter="nodejs" authors="MikeWasson" manager="wpickett" editor="mollybos"/>

<tags ms.service="multiple" ms.workload="na" ms.tgt_pltfrm="na" ms.devlang="nodejs" ms.topic="article" ms.date="08/31/2015" ms.author="mwasson"/>





# Node.js 模块使用 Azure 应用程序

本文档对 Node.js 模块使用 Azure 上承载的应用程序提供指导。 它提供了确保您的应用程序使用特定版本的模块，以及本机模块使用 Azure 指南。

如果您已经熟悉使用 Node.js 模块， **package.json**和**npm shrinkwrap.json**文件，下面是本文中讨论的内容摘要︰

* Azure Webites 了解**package.json**和**npm shrinkwrap.json**文件，并可以安装模块基于这些文件中的条目。
* Azure 的云服务预计在开发环境中上, 安装的所有模块和**节点\_模块**目录指定为部署包的一部分。

> [AZURE.NOTE] 因为在虚拟机中的部署经验将取决于承载的虚拟机的操作系统在这篇文章，不讨论 azure 的虚拟机。

> [AZURE.NOTE] 也可以启用对安装使用 Azure 上, **package.json**或**npm shrinkwrap.json**文件的模块，但是这要求云服务项目所使用的默认脚本自定义项的支持。 有关如何完成此操作的示例，请参见[Azure 启动任务运行 npm 安装以避免部署节点模块](http://nodeblog.azurewebsites.net/startup-task-to-run-npm-in-azure)

##Node.js 模块

模块是可加载 JavaScript 包为您的应用程序提供特定的功能。 安装了一个模块通常使用**npm**命令行工具，但是有些文件 （如 http 模块） 作为核心 Node.js 包的一部分提供。

当安装了模块时，它们存储在**节点\_模块**在您的应用程序目录结构的根目录的目录。 每个模块在**节点\_模块**目录保留自己的**节点\_模块**目录，其中包含它依赖，这再次重复了依赖关系链的每个模块的所有模块。 这样，每个模块安装有其从属的但是它可能会导致相当大的目录结构模块自己版本要求。

在部署时**节点\_模块**目录作为应用程序的一部分，它将增加部署比使用**package.json**或**npm shrinkwrap.json**文件的大小然而，它 does 保证在生产中使用的模块的版本正在开发中使用的相同。

###本机的模块

虽然大多数模块只是纯文本的 JavaScript 文件，某些模块都是特定于平台的二进制映像。 这些被编译模块在安装时，通常使用 Python 和节点 gyp。 由于 Azure 云服务依赖于**节点\_模块**文件夹作为应用程序的任何本机模块已安装模块的一部分部署应在云服务中工作，只要它已安装，并且 Windows 开发系统上编译。

Azure 网站不支持本机的所有模块，并可能在编译那些具有非常特殊的系统必备组件失败。 某些常用模块如 MongoDB 有可选的本地相关性和工作很正常，如果没有它们，而两种解决方法证明了当今几乎所有本机模块成功︰

* 具有本机模块的所有系统必备组件安装的 Windows 计算机上运行**npm 安装**。 然后，部署创建**节点\_模块**Azure 网站应用程序的文件夹。
* Azure 网站可以将配置为在部署过程中执行自定义的 bash 或 shell 脚本使您可以执行自定义命令和精确配置方式**npm 安装**正在运行。 视频显示如何执行此操作，请参阅[自定义网站部署脚本中包含 Kudu]。

###使用 package.json 文件

**Package.json**文件是一种方法指定以便托管平台依赖项，而不是要求您包括可以安装应用程序所需的高级别依赖项**节点\_软件包**文件夹作为部署的一部分。 部署完应用程序后，**安装 npm**命令用于分析**package.json**文件，然后安装列出的所有依赖项。

在开发期间，您可以使用**-保存**， **-保存开发**，或**-保存-可选**参数安装模块，以便自动将模块项目添加到**package.json**文件时。 有关详细信息，请参阅[npm 安装](https://npmjs.org/doc/install.html)。

与**package.json**文件的一个潜在问题是它仅指定高级别依赖项的版本。 安装的每个模块可能会或可能不会指定它依赖的模块的版本，它是可能的您可能会得到比在开发中使用的不同的依赖关系链。

> [AZURE.NOTE]
> 如果将部署到 Azure 网站，当您<b>package.json</b>文件引用本机模块发布使用 Git 的应用程序时，您将看到与以下内容类似的错误︰

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###使用 npm shrinkwrap.json 文件

**Npm shrinkwrap.json**文件是试图解决**package.json**文件的模块版本限制。 虽然**package.json**文件只包含顶级模块的版本， **npm shrinkwrap.json**文件中包含完整的模块依赖关系链的版本要求。

为生产准备好您的应用程序时，您可以锁定的版本要求并使用**npm 撕开**命令创建**npm shrinkwrap.json**文件。 这将使用当前安装的版本**节点\_模块**文件夹中，并记录这些**npm shrinkwrap.json**文件。 应用程序已部署到承载环境后， **npm 安装**命令用于分析**npm shrinkwrap.json**文件并安装列出的所有依赖项。 有关详细信息，请参阅[npm 安装](https://npmjs.org/doc/install.html)。

> [AZURE.NOTE]
>如果将部署到 Azure 网站，当您<b>npm shrinkwrap.json</b>文件引用本机模块发布使用 Git 的应用程序时，您将看到与以下内容类似的错误︰

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##下一步行动

现在，您已了解如何使用 Azure Node.js 模块，了解如何[指定 Node.js 版本]、[生成和部署 Node.js 网站]和[如何使用 Mac 和 Linux Azure 命令行界面]。

[指定的 Node.js 版本]: nodejs-specify-node-version-azure-apps.md
[如何使用 Mac 和 Linux 的 Azure 命令行界面]: xplat-cli.md
[生成和部署 Node.js Web 站点]: web-sites-nodejs-develop-deploy-mac.md
[MongoDB (MongoLab) 上存储的 Node.js Web 应用程序]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[使用 Git 发布]: web-sites-publish-source-control.md
[生成和部署 Node.js 对 Azure 云服务的应用程序]: cloud-services-nodejs-develop-deploy-app.md
[网站自定义部署脚本中包含 Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/

测试
