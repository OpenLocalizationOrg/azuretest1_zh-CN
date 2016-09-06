---
ms.openlocfilehash: 9df08a10cf32edad39a55abd749c064c1cb9e341
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

本教程是基于[GetStartedWithMobileServices 的应用程序](http://go.microsoft.com/fwlink/p/?LinkID=510826)，这是一个在 Visual Studio 2013年通用 Windows 应用程序项目。 此应用程序的用户界面与应用程序生成的移动服务快速入门，只是添加项目存储在本地内存中。 

1. 从 [开发人员代码示例站点] 下载 C# 版本的 GetStartedWithMobileServices 示例应用程序。 

2. 在 Visual Studio 2013，打开下载的项目并检查 MainPage.xaml.cs 文件中的 GetStartedWithData.Shared 项目文件夹中找到。

    请注意，添加**TodoItem**对象存储在内存中**ObservableCollection&lt;TodoItem&gt;**。

3. 按**F5**键以重新生成该项目并启动应用程序。

4. 在应用程序中，在**插入 TodoItem**，请键入一些文本，然后单击**保存**。

    ![](./media/mobile-services-windows-universal-dotnet-download-project/mobile-quickstart-startup.png) 

    请注意，将显示已保存的文本。

5. 用鼠标右键单击 Windows Phone 8.1 项目，请单击**设为启动项目**，然后按**f5 键**以启动 Windows Phone 存储应用程序。  

    ![](./media/mobile-services-windows-universal-dotnet-download-project/mobile-quickstart-startup-wp8.png)

6. 重复步骤 3 和 4，以检查该示例行为相同的方式。