---
ms.openlocfilehash: ba410f046ec9af6eef8008f59554c7cca51932f3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

您下载的移动服务项目可以在您的本地计算机或虚拟机上直接运行您的新移动服务。 这更便于调试服务代码之前甚至将其发布到 Azure。

在本节中，您将测试针对移动服务在本地运行新应用程序。

1. 浏览到保存压缩的项目文件的位置，展开您计算机上的文件在 Visual Studio 中打开解决方案文件。

2. 在 Visual Studio 中的解决方案资源管理器中，右键单击服务项目，单击**设为启动项目**，然后按**F5**键以生成项目并开始移动服务本地。

    ![](./media/mobile-services-dotnet-backend-test-local-service-dotnet/mobile-service-startup.png)

    移动服务成功启动后，将显示 web 页。

3. 要测试存储应用程序，请用鼠标右键单击客户端应用程序项目，单击**设为启动项目**然后按**F5**键以重新生成该项目并启动应用程序。

    这将启动应用程序，连接到本地移动服务实例。   

4. 在应用程序中，在**插入 TodoItem**，请键入有意义的文本，例如，_完成本教程_，，然后单击**保存**。

    将 POST 请求发送到本地移动服务。 从请求的数据插入到 TodoItem 表中。 表中存储的项目由手机信息服务，并将数据显示在应用程序中的第二列中。