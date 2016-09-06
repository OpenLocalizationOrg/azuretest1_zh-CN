---
ms.openlocfilehash: b3c007e3f171175863d795c0693b6b45d86f8aa6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="从 Visual Studio 部署" 
    description="在 Visual Studio 来管理您的逻辑应用程序创建项目。" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/30/2015"
    ms.author="stepsic"/>
    
# 从 Visual Studio 部署

虽然[Azure 门户网站](https://portal.azure.com)为您提供设计和管理您的逻辑应用程序的好方法，但可能还要改为部署从 Visual Studio 应用程序逻辑。 有这样的几个关键功能︰

- 在解决方案中，存储逻辑应用程序及其他资产，因此它可以包含应用程序的所有方面
- 请您签入到源代码管理，以便可以使用 TFS 或 Git 跟踪修订它的逻辑应用程序定义 

需要具备 Azure SDK 2.7 或稍后安装，才能按照下面的步骤。 在此处查找[最新 SDK 为 VS](http://azure.microsoft.com/downloads/) 。

## 创建项目

1. 转到**文件**菜单并选择**新建** >  **项目**（或者，您可以转到**添加**，然后选择**新的项目**以将其添加到现有解决方案） ![文件菜单](./media/app-service-logic-deploy-from-vs/filemenu.png)

2. 在对话框中查找**云**，然后选择**Azure 资源组**。 键入一个**名称**，然后单击**确定**。
    ![添加新项目](./media/app-service-logic-deploy-from-vs/addnewproject.png)

3. 现在，您需要选择如果您希望**应用程序逻辑**或**逻辑应用程序 + API 的应用程序**。 选择**逻辑应用程序**要求您指向现有的 API。 如果您选择**的逻辑应用程序 + API 应用程序**然后您还可以创建新的空，API 应用程序在同一时间。
    ![选择碧空模板](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

4. 一旦您选择您的**模板**单击**确定**。

现在逻辑应用程序项目将添加到您的解决方案。 您应该看到在解决方案资源管理器中的部署︰![部署](./media/app-service-logic-deploy-from-vs/deployment.png)

## 配置您的逻辑应用程序

一旦您有您可以编辑与内部逻辑应用程序定义的项目。 单击解决方案资源管理器中的 JSON 文件。 您将看到您可以在其中填入您应用程序的逻辑的占位符定义。

建议使用您的定义中的**参数**。 这将是非常有用的如果要部署到环境中的开发和生产。 在这种情况下，您应将所有特定于环境的配置放在`.param`文件，而不是实际的字符串参数。

如今，Visual Studio 不具有内置的设计器中，因此如果您想要使用的图形界面 （而不是编写 JSON），您将需要使用 Azure 门户。 

如果以前创建了 Azure 门户内的逻辑应用程序并且现在要签入到源代码管理，您可以执行不同的三种方法之一︰
- 转到**代码视图**中该门户并复制定义。
- 逻辑应用程序[其余部分 API](https://msdn.microsoft.com/library/azure/dn948510.aspx)可用于获取定义。
- 特别是使用[Azure 资源管理器 powershell](../powershell-azure-resource-manager.md)，[`Get-AzureResource`命令](https://msdn.microsoft.com/library/dn654579.aspx)来下载定义。

## 部署您的应用程序逻辑

最后，在配置您的应用程序后您可以直接从 Visual Studio 中只有两个步骤的部署。 

1. 部署解决方案资源管理器中右键单击，然后转到**部署** > **新部署...**
    ![新部署](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. 系统将提示您登录到您的 Azure 订购。 

3. 现在您需要选择要部署到的逻辑应用程序的资源组的详细信息。 
    ![将部署到资源组](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

    请务必选择正确的模板和参数文件为资源组 （例如如果要部署到生产环境中可能需要选择生产参数文件）。 
    
4. 部署状态将显示在**输出**窗口 （您可能需要选择**Azure 资源调配**。 
    ![Output](./media/app-service-logic-deploy-from-vs/output.png)

将来可以修改您在源代码管理中的逻辑应用程序并使用 Visual Studio 部署新版本。 注意是否您直接修改 Azure 门户中的定义，在下一次，您从 Visual Studio 将这些更改部署将被重写。

如果您不想使用 Visual Studio，但仍要有工具来部署您的逻辑应用程序从源代码管理您始终可以使用[API](https://msdn.microsoft.com/library/azure/dn948510.aspx)或[Powershell](../powershell-azure-resource-manager.md)直接以自动化您的部署。  
测试
