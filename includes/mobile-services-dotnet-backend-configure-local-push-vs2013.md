---
ms.openlocfilehash: e6273c26a6c95b0313c2452e87534caaaacc2b7a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

（可选） 可以与您本地计算机或虚拟机上运行，才能发布到 Azure 的移动服务测试推式通知。 若要执行此操作，必须设置通知中心由您的应用程序的 web.config 文件中的信息。 此信息仅用于运行时本地连接到通知中心;发布到 Azure 时，它将被忽略。

1. 打开 Windows 或 WindowsPhone app 项目文件夹中的 readme.html 文件。 

    这将显示**推送安装已基本完成**页上，如果还没有打开它。 一节**步骤 3︰ 修改 Web 配置**包含通知集线器连接信息。

2. 在 Visual Studio 中的移动服务项目中，打开 Web.config 文件服务，然后在**connectionStrings**中，从**推送安装已基本完成**页中添加**MS_NotificationHubConnectionString**的连接字符串。

3. 在**appSettings**，通知中心**推送安装已基本完成**页中找到的名称替换**MS_NotificationHubName**应用程序设置的值。

4. 用鼠标右键单击移动服务项目单击**调试**，然后**启动新实例**，请记下服务根 URL 的页面，其中显示在浏览器中开始。

    这是本地主机用于.NET 后端项目的 URL。 您将使用此 URL 来测试应用程序对本地计算机上运行的移动服务。

现在，移动服务项目配置为在本地运行时连接到 Azure 中的通知中心。 请注意，一定要用相同的通知中心名称和连接字符串作为门户，因为这些项目的 Web.config 文件中将重写设置门户设置在 Azure 中运行时。 