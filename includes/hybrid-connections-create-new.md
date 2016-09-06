---
ms.openlocfilehash: 66761cd04fd7cebb11fd206d1b406fc20b1f3e48
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 在内部计算机上登录到[Azure 管理门户](http://manager.windowsazure.com)（这是旧的门户）。

2. 在导航窗格的底部，选择**新建 +** > **应用程序服务** > **BizTalk 服务** > **自定义创建**。

3. 提供一个**BizTalk 服务名称**并选择一个**版**。 

    本教程使用**mobile1**。 您将需要提供新的 BizTalk 服务的唯一名称。

4. 一旦创建的 BizTalk 服务，选择**混合连接**选项卡，然后单击**添加**。

    ![添加混合连接](./media/hybrid-connections-create-new/3.png)

    这将创建一个新的混合连接。

5. 对于混合连接提供**名称**和**主机名**和**端口**设置为`1433`。 
  
    ![配置混合连接](./media/hybrid-connections-create-new/4.png)

    主机名是本地服务器的名称。 此配置访问 SQL Server 运行在端口 1433年上混合连接。 如果您使用的 SQL Server 的命名的实例，使用前面定义的静态端口。

6. 创建新连接后，状态的新连接的显示，**内部安装不完整**。

7. 向后定位到您的移动服务、 单击**配置**，向下滚动到**混合连接**单击**添加混合连接**，然后选择您刚创建的混合连接并单击**确定**。

    这使您可以使用新混合连接的移动服务。

接下来，您将需要在本地计算机上安装混合连接管理器。