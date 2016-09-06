---
ms.openlocfilehash: 5bb87dfc0ec18d9c311af5058491ee8412dc94ce
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

本教程的最后阶段是生成并运行您的新应用程序。

1. 浏览到保存压缩的项目文件的位置，展开您计算机上的文件在 Visual Studio 中打开解决方案文件。

2. 按**F5**键以重新生成该项目并启动应用程序。

3. 在应用程序中，在**插入 TodoItem**，请键入有意义的文本，例如，*完成本教程*，，然后单击**保存**。

    将 POST 请求发送到 Azure 中承载新的移动服务。 从请求的数据插入到 TodoItem 表中。 表中存储的项目由手机信息服务，并将数据显示在应用程序中的第二列中。

4. （可选）在通用的 Windows 解决方案，到其他应用程序中更改默认启动项目并再次运行该应用程序。

    请注意，应用程序启动后，从移动服务加载上一步中保存的数据。
 
4. 回在管理门户中，单击**数据**选项卡，然后单击**TodoItems**表。

    这允许您浏览该应用程序插入表的数据。

    ![](./media/mobile-services-javascript-backend-run-app/mobile-data-browse.png)