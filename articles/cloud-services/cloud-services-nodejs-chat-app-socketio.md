---
ms.openlocfilehash: e67df9044e4e233a9e495554f00acd2995c06b24
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用 Socket.io 的 Node.js 应用程序 |Microsoft Azure" 
    description="了解如何在 Azure 上承载的 node.js 应用中使用 socket.io。" 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="TomArcher" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="tarcher"/>





# 在 Azure 的云服务上生成 Socket.IO 的 Node.js 聊天应用程序

Socket.IO 提供之间 node.js 服务器和客户端之间的实时通信。 本教程将引导您通过托管一个套接字。IO 基于 Azure 上的聊天应用程序。 Socket.IO 的详细信息，请参阅<a href="http://socket.io/">http://socket.io/</a>。

下面是完整的应用程序的屏幕快照︰

![一个浏览器窗口，显示在 Azure 上承载的服务][completed-app]  

## 先决条件

确保以下的产品和版本安装成功完成本文中的示例︰

* 安装[Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
* 安装[Node.js](https://nodejs.org/download/)
* 安装[Python 版本 2.7.10](https://www.python.org/)

## 创建一个云服务项目

以下步骤将创建将承载 Socket.IO 应用程序的云服务项目。

1. 从**「 开始 」 菜单**或**启动屏幕**，搜索**Azure PowerShell**。 最后，用鼠标右键单击**Azure PowerShell**并且选择**以管理员身份运行**。

    ![Azure PowerShell 图标][powershell-menu]

2. 创建名为的目录**c:\\节点**。 
 
        PS C:\> md node

3. 将目录改为**c:\\节点**目录
 
        PS C:\> cd node

4. 输入以下命令创建一个名为**chatapp**的新的解决方案和名为**WorkerRole1**的工作角色︰

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    您将看到以下消息︰

    ![新 azureservice 和添加 azurenodeworkerrolecmdlets 的输出](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## 下载聊天示例

对于此项目，我们将使用[Socket.IO GitHub 存储库]中的聊天示例。 执行以下步骤来下载该示例并将其添加到以前创建的项目。

1.  通过使用**克隆**按钮创建存储库中的本地副本。 此外可以使用**邮政编码**按钮要下载的项目。

    ![具有突出显示的 ZIP 下载图标查看 https://github.com/LearnBoost/socket.io/tree/master/examples/chat，一个浏览器窗口][chat-example-view]

3.  浏览本地资源库中的目录结构，直到您到达**示例\\聊天**目录。 复制到此目录的内容**c:\\节点\\chatapp\\WorkerRole1**以前创建的目录。

    ![显示示例的内容的浏览器\\聊天目录从存档中提取][chat-contents]

    在上面的抓图中突出显示的项目是从复制的文件**示例\\聊天**目录

4.  在**c:\\节点\\chatapp\\WorkerRole1**目录，删除**server.js**文件，然后再将**app.js**文件重命名为**server.js**。 这会删除以前创建**添加 AzureNodeWorkerRole** cmdlet 的默认**server.js**文件并替换应用程序文件从聊天的示例。

### 修改 Server.js 并安装模块

之前在 Azure 仿真程序中测试应用程序，我们必须进行一些小的修改。 执行以下步骤以 server.js 文件︰

1.  在 Visual Studio 或任何文本编辑器中打开**server.js**文件。

2.  Server.js 开头**模块依赖关系**部分查找和更改的行包含**sio = require('..//..lib//socket.io')**到**sio = require('socket.io')**如下所示︰

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  要确保在正确的端口上侦听的应用程序，请在记事本或自己喜爱的编辑器中打开 server.js，然后用**process.env.port**替换**3000** ，如下所示更改以下行︰

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

后将所做的更改保存到**server.js**，使用以下步骤来安装所需的模块，然后在 Azure 仿真程序中测试应用程序︰

1.  使用**Azure PowerShell**，更改到目录**c:\\节点\\chatapp\\WorkerRole1**目录并使用下面的命令来安装此应用程序所需的模块︰

        PS C:\node\chatapp\WorkerRole1> npm install

    这将安装的 package.json 文件中列出的模块。 在命令完成后，您应该看到类似于下面的输出︰

    ![Npm 的输出安装命令][The-output-of-the-npm-install-command]

4.  由于本示例原先属于 Socket.IO GitHub 存储库中，而直接通过相对路径引用 Socket.IO 库，Socket.IO 没有引用在 package.json 文件中，因此我们必须通过发出下列命令来安装它︰

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### 测试和部署

1.  通过发出以下命令来启动仿真程序︰

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  打开一个浏览器，然后定位到**http://127.0.0.1**。

3.  浏览器窗口打开时，输入昵称，然后点击输入。
    这会将所有您可为特定昵称发布消息。 多用户功能测试，打开另一个浏览器窗口，使用相同的 URL，并输入不同的昵称。

    ![两个浏览器窗口，显示 User1 和 User2 的聊天](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  测试应用程序之后, 通过发出以下命令来停止该仿真程序︰

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  若要部署到 Azure 应用程序，请使用**发布 AzureServiceProject** cmdlet。 例如︰

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] 应确保使用唯一的名称，否则发布过程将失败。 部署完成后，浏览器将打开，并定位到已部署的服务。
    > 
    > 如果您收到错误，指出提供的订阅名称不存在中的导入的发布配置文件，则必须下载并导入发布配置文件部署到 Azure 之前订购。 请参阅的部分**部署到 Azure 应用程序**[生成和部署 Node.js 对 Azure 云服务的应用程序](https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/)

    ![一个浏览器窗口，显示在 Azure 上承载的服务][completed-app]

    > [AZURE.NOTE] 如果您收到错误，指出提供的订阅名称不存在中的导入的发布配置文件，则必须下载并导入发布配置文件部署到 Azure 之前订购。 请参阅的部分**部署到 Azure 应用程序**[生成和部署 Node.js 对 Azure 云服务的应用程序](https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/)

您的应用程序是现在对 Azure 上运行，并可以将中继使用 Socket.IO 的不同客户端之间的聊天消息。

> [AZURE.NOTE] 为简单起见，此示例仅限于用户连接到的同一实例之间聊天。 这意味着，如果云服务创建两个辅助角色实例，用户仅能聊天与其他人连接到同一个辅助角色实例。 若要缩放应用程序使用多个角色实例，可以使用服务总线等技术在实例之间共享的 Socket.IO 存储状态。 有关示例，请参见[Node.js GitHub 资料库的 Azure SDK](https://github.com/WindowsAzure/azure-sdk-for-node)中的服务总线队列和主题用法示例。

##下一步行动

在本教程中，您学习了如何创建基本的聊天应用程序承载于 Azure 的云服务。 若要了解如何承载此应用程序在 Azure 网站，请参见[生成 Node.js 聊天应用程序与 Socket.IO 在 Azure 网站][chatwebsite]。

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure 的 SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK Node.js GitHub 存储库]: https://github.com/WindowsAzure/azure-sdk-for-node
  [已完成应用程序]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure Node.js SDK]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web 应用程序]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Socket.IO GitHub 存储库]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure 的注意事项]: #windowsazureconsiderations
  [承载辅助角色中的聊天示例]: #hostingthechatexampleinawebrole
  [摘要和下一步行动]: #summary
  [powershell 菜单]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [聊天的示例]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [聊天示例视图]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [聊天的内容]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [发布 AzureService 命令的输出]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 

测试
