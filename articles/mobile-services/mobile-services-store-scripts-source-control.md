---
ms.openlocfilehash: 1ce39a345f04df91ba639d6d7282ea87facf8bcf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="将项目存储在源代码管理 |Microsoft Azure"
    description="了解如何在您的计算机上本地 Git repo 存储服务器脚本文件和模块。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="ggailey777"/>

# 将项目存储在源代码管理中

> [AZURE。选择器列表 (平台 |后端）]
- [(任何 |.NET)](mobile-services-dotnet-backend-store-code-source-control.md)
- [(任何 |) Javascript](mobile-services-store-scripts-source-control.md)

本主题演示如何使用 Azure 移动服务所提供的源控件来存储服务器脚本。 脚本和其他 JavaScript 后端代码文件可以提升从本地 Git 存储库为您生产的移动服务。 它还演示如何定义可以通过多个脚本所需的共享的代码以及如何使用 package.json 文件来将 Node.js 模块添加到您的移动服务。

若要完成本教程，必须已完成[移动服务入门]或[添加到现有应用程序的移动服务]本教程创建移动服务。

##<a name="enable-source-control"></a>启用版本控制，在您的移动服务

[AZURE.INCLUDE [mobile-services-enable-source-control](../../includes/mobile-services-enable-source-control.md)]

##<a name="clone-repo"></a>安装 Git 并创建本地资源库中

1. 在您的本地计算机上安装 Git。

    Git 安装所需的步骤因操作系统而异。 有关操作系统特定的分发和安装指南，请参阅[安装 Git] 。

    > [AZURE.NOTE]
    > 在某些操作系统中，命令行和 GUI 的 Git 版本都可用。 本文中提供的说明进行操作使用的命令行版本。

2. 打开命令行，如**GitBash** (Windows) 或**狂欢**（Unix 外壳）。 在 OS X 系统上可以访问命令行通过**终端**应用程序。

3. 从命令行更改将用来储存您的脚本的目录。 例如， `cd SourceControl`。

4. 使用下面的命令来创建新的 Git 存储库中，一个本地副本替换`<your_git_URL>`您的移动服务的 Git 存储库的 url:

        git clone <your_git_URL>

5. 出现提示时，键入用户名和密码，那么当您在您的移动服务中启用源代码管理设置。 身份验证成功后，您将看到一系列类似下面的响应︰

        remote: Counting objects: 8, done.
        remote: Compressing objects: 100% (4/4), done.
        remote: Total 8 (delta 1), reused 0 (delta 0)
        Unpacking objects: 100% (8/8), done.

6. 浏览到运行目录`git clone`命令，并注意到以下目录结构︰

    ![4][4]

    在这种情况下，移动的服务，这是数据服务的本地存储库的名称创建一个新目录。

7. 打开.\service\table 子文件夹，请注意，它包含一个 TodoItem.json 文件，在 TodoItem 表的操作权限的 JSON 表示。

    当此表中已定义服务器脚本时，您还可以一个或多个文件，名为<code>TodoItem. _&lt;operation&gt;_ .js</code>包含给定的表操作的脚本。 在不同的文件夹，这些各自的名称与维护计划程序和 API 的自定义脚本。 有关详细信息，请参见[源代码管理]。

现在，您已经创建了您的本地存储库中，可以对服务器脚本进行更改并将更改推送回移动服务。

##<a name="deploy-scripts"></a>将更新的脚本文件部署到您的移动服务

1. 浏览到.\service\table 子文件夹，然后如果 todoitem.insert.js 文件尚不存在，则立即创建。

2. 在文本编辑器中打开新文件 todoitem.insert.js 并粘贴下面的代码并保存您的更改︰

        function insert(item, user, request) {
            request.execute();
            console.log(JSON.stringify(item, null, 4));
        }

    此代码只需将插入的项写入日志。 如果此文件已包含代码，只是一些有效 JavaScript 将代码添加到此文件中，如对的调用`console.log()`，然后保存您的更改。

3. 在 Git 命令提示符处，键入以下命令以开始跟踪新的脚本文件︰

        $ git add .


4. 键入下列命令以提交更改︰

        $ git commit -m "updated the insert script"

5. 键入以下命令可将所做的更改上载到远程资源库中︰

        $ git push origin master

    您应该看到一系列命令，指示提交被部署到移动服务。

6. 回在管理门户中，单击**数据**选项卡，然后单击**TodoItem**的表，单击**脚本**然后选择**插入**操作。
7.
    请注意显示的插入操作脚本是刚上载到存储库中的 JavaScript 代码相同。

##<a name="use-npm"></a>利用共享的代码和服务器脚本中的 Node.js 模块

移动服务提供了全套的核心访问 Node.js 模块，则可以在代码中使用方法**需要**函数。 您的移动服务还可以使用不是包的一部分核心 Node.js，Node.js 模块，甚至可以作为 Node.js 模块定义共享的代码。 有关创建模块的详细信息，请参阅 Node.js API 参考文档中的[模块]。

将 Node.js 模块添加到您的移动服务的推荐的方法是通过添加对服务的 package.json 文件的引用。 接下来，将添加到您的移动服务[节点 uuid] Node.js 模块通过更新的 package.json 文件。 当更新推送到 Azure 时，移动服务重新启动并安装该模块。 然后使用此模块生成新的 GUID 值的插入项目的**uuid**属性。

2. 定位到`.\service`当地的 Git 存储库中，并打开 package.json 文件在文本编辑器中，并将以下字段添加到该**依赖项**对象的文件夹︰

        "node-uuid": "~1.4.3"

    >[AZURE.NOTE]Package.json 文件到此更新将导致重新启动您的移动服务后提交已被按。

4. 现在浏览到.\service\table 子文件夹，打开 todoitem.insert.js 文件，并对其进行修改，如下所示︰

        function insert(item, user, request) {
            var uuid = require('node-uuid');
            item.uuid = uuid.v1();
            request.execute();
            console.log(item);
        }

    此代码将 uuid 列添加到表中，唯一的 GUID 标识符用填充它。

5. 与上一节中，在 Git 命令提示符中键入以下命令︰

        $ git add .
        $ git commit -m "added node-uuid module"
        $ git push origin master

    此添加新文件、 提交您的更改，并将新节点 uuid 模块并更改为 todoitem.insert.js 脚本保存到您的移动服务。

## <a name="next-steps"> </a>下一步行动

现在，您已完成本教程，您知道如何将脚本存储在源代码管理中。 了解更多有关使用与服务器脚本和自定义的 Api，请考虑︰

+ [与中移动服务的服务器脚本]
    <br/>演示如何使用服务器脚本、 作业计划程序和自定义的 Api。

+ [从客户端调用一个自定义的 API]
    <br/> 演示如何创建自定义的 Api，可以从客户端调用。

<!-- Anchors. -->
[启用版本控制，在您的移动服务]: #enable-source-control
[安装 Git 并创建本地资源库中]: #clone-repo
[将更新的脚本文件部署到您的移动服务]: #deploy-scripts
[利用共享的代码和服务器脚本中的 Node.js 模块]: #use-npm

<!-- Images. -->
[4]: ./media/mobile-services-store-scripts-source-control/mobile-source-local-repo.png
[5]: ./media/mobile-services-store-scripts-source-control/mobile-portal-data-tables.png
[6]: ./media/mobile-services-store-scripts-source-control/mobile-insert-script-source-control.png

<!-- URLs. -->
[Git 网站]: http://git-scm.com
[源代码管理]: http://msdn.microsoft.com/library/windowsazure/c25aaede-c1f0-4004-8b78-113708761643
[安装 Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[开始使用移动服务]: mobile-services-ios-get-started.md
[添加到现有应用程序的移动服务]: mobile-services-ios-get-started-data.md
[与中移动服务的服务器脚本]: mobile-services-how-to-use-server-scripts.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[从客户端调用一个自定义的 API]: mobile-services-ios-call-custom-api.md
[模块]: http://nodejs.org/api/modules.html
[节点 uuid]: https://npmjs.org/package/node-uuid
