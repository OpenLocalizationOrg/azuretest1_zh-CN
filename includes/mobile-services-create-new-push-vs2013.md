---
ms.openlocfilehash: 41edf58fba35bd71e46f823d2eaa8928f7f2be01
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
以下步骤注册 Windows 应用商店应用程序、 配置移动服务启用推式通知，并将代码添加到您的应用程序，以向您通知中心注册设备通道。 Visual Studio 2013年使用您提供的凭据连接到 Azure 和 Windows 应用商店。 

1. 在 Visual Studio 2013，打开解决方案资源管理器中，右击 Windows 应用商店应用程序项目，单击**添加**，然后**推式通知...**。 

    ![在 Visual Studio 2013年添加推式通知向导](./media/mobile-services-create-new-push-vs2013/mobile-add-push-notifications-vs2013.png)

    这将启动添加推送通知向导。

2. 单击**下一步**，登录到您的 Windows 存储帐户，然后提供**新名称保留**中的名称，然后单击**保留**。

    这将创建新的应用程序注册。

3. 单击新注册在**应用程序名称**列表中，然后单击**下一步**。

4. 在**选择服务**页中，单击您的移动服务名，然后单击**下一步**和**完成**。 

    使用 Windows 通知服务 (WNS) 注册更新通知中心由您的移动服务。 您现在可以使用 Azure 通知集线器将通知从移动服务发送到您的应用程序，使用 WNS。 

    >[AZURE.NOTE]本教程演示如何移动服务后端的发送通知。 相同的通知中心注册可用于从任何后端服务发送通知。 有关详细信息，请参见[通知集线器概述](http://msdn.microsoft.com/library/azure/jj927170.aspx)。

5. 完成该向导后，在 Visual Studio 中打开新**推入安装程序已基本完成**页。 此页面详细介绍了一种方法来配置移动服务项目发送通知不同于本教程。 

    通过添加推式通知向导添加到通用 Windows 应用程序解决方案的代码是特定于平台的。 以后在此部分中，您将删除此冗余通过共享移动服务客户端代码，从而使通用的应用程序更易于维护。  

<!-- URLs. -->
[开始使用移动服务]: /develop/mobile/tutorials/get-started/
[有关数据入门]: /develop/mobile/tutorials/get-started-with-data-dotnet/
[导入您的 publishsettings 文件在 Visual Studio 2013]: ../articles/mobile-services/mobile-services-windows-how-to-import-publishsettings.md
