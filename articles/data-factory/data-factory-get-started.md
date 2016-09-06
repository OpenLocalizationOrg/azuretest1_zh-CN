---
ms.openlocfilehash: 7acb1cdb15c2806d41eff97ba88f5b58538ffe1c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="教程︰ 在 Azure 数据工厂管道中使用复制活动"
    description="本教程展示如何在 Azure 数据工厂管线中使用复制活动，若要将数据从 Azure 复制到 SQL Azure 数据库斑点。"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="07/27/2015"
    ms.author="spelluru"/>

# 教程︰ 斑点到 SQL Azure 的 Azure 的数据复制
> [AZURE.SELECTOR]
- [教程概述](data-factory-get-started.md)
- [使用数据工厂编辑器](data-factory-get-started-using-editor.md)
- [使用 PowerShell](data-factory-monitor-manage-using-powershell.md)
- [使用 Visual Studio](data-factory-get-started-using-vs.md)

这篇文章中的教程将帮助您快速开始使用 Azure 数据工厂服务。 在本教程中，您将创建 Azure 数据工厂并在 Azure blob 存储中的数据复制到 SQL Azure 数据库在数据工厂创建的管道。

> [AZURE.NOTE] 数据工厂服务的详细概述，请参阅[Azure 数据工厂简介][数据-工厂的介绍]文章。

##本教程的先决条件
在开始本教程之前，您必须具有以下︰

- **Azure 的订阅**。  如果没有预订，您可以在几分钟创建免费的试用帐户。 请参阅详细信息[免费试用][azure 免费试用]文章。
- **Azure 存储帐户**。 在本教程中，您将使用作为**源**数据存储 blob 存储。 如果您没有一个 Azure 存储帐户，请参阅[创建存储帐户][数据工厂-创建的存储]文章的步骤来创建一个。
- **SQL azure 数据库**。 在本教程中，您将使用作为**目标**数据存储 SQL Azure 数据库。 如果您没有在本教程中可以使用 SQL Azure 数据库，请参阅[如何创建和配置一个 Azure 的 SQL 数据库][数据的工厂-创建的 sql 的数据库]创建一个。
- **SQL Server 2012年/2014年或 Visual Studio 2013年**。 若要创建一个示例数据库，并查看数据库中的结果数据，您将使用 SQL Server 管理 Studio 或 Visual Studio。  

### 收集的帐户名和 Azure 存储帐户的帐户密码
您需要的帐户名和 Azure 存储帐户进行本教程中的帐户密码。 按照下面的说明，记下的**帐户名称**和 Azure 存储帐户的**帐户密码**︰

1. 登录到[Azure 预览门户][azure 预览门户]。
2. 单击**浏览**左侧的中心并选择**存储帐户**。
3. 在**存储帐户**刀片式服务器，选择您想要在本教程中使用**Azure 存储帐户**。
4. 在**存储**刀片式服务器，请单击**键**平铺。
5. 在**管理密钥**刀片式服务器，单击**存储帐户名称**文本框旁边的**副本**（图像） 按钮，保存/粘贴它在某个位置 (例如︰ 在一个文本文件中)。  
6. 重复上一步复制或下**访问主键**的注意。
7. 单击**X**关闭所有刀片式服务器。

### 收集服务器名称、 数据库名称和 SQL Azure 数据库的用户帐户
您将需要的 SQL Azure 服务器、 数据库和用户做本教程中的名称。 按照下面的说明，记下您的 SQL Azure 数据库**服务器**、**数据库**和**用户**的名称︰

1. 在**Azure 预览门户**中，单击左侧的**浏览**并选择**SQL 数据库**。
2. 在**SQL 数据库刀片式服务器**，选择您想要在本教程中使用的**数据库**。 请注意下的**数据库名称**。  
3. 在**SQL 数据库**刀片式服务器，请单击**属性**平铺。
4. **服务器名称**和**服务器管理员登录**，记下的值。
5. 单击**X**关闭所有刀片式服务器。

### 允许访问 SQL Azure 服务器 Azure 服务
确保****允许访问 Azure 服务**设置打开 SQL Azure 服务器，以便数据工厂服务可以访问您的 SQL Azure 服务器**。 若要验证并打开此设置，请执行以下操作︰

1. 单击**浏览**左侧的中心并单击**SQL 服务器**。
2. 选择**您的服务器**，然后单击**SQL 服务器**刀片式服务器上的**设置**。
3. 在**设置**刀片式服务器，单击**防火墙**。
4. **在**设置防火墙**刀片式服务器，单击**Azure 服务允许**访问。**
5. 单击**X**关闭所有刀片式服务器。

### Azure Blob 存储和 SQL Azure 数据库准备教程
现在，准备您的 Azure blob 存储和 SQL Azure 数据库教程，请执行以下步骤︰  

1. 启动记事本、 粘贴以下文本，并将其保存为**emp.txt**到**C:\ADFGetStarted**文件夹在您的硬盘上。

        John, Doe
        Jane, Doe

2. 若要创建**adftutorial**容器，将**emp.txt**文件上载到容器使用[Azure 存储浏览器](https://azurestorageexplorer.codeplex.com/)这样的工具。

    ![Azure 存储资源管理器](./media/data-factory-get-started/getstarted-storage-explorer.png)
3. 使用以下 SQL 脚本在 SQL Azure 数据库中创建了**emp**表。  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **如果您有在您的计算机上安装的 SQL Server 2012年/2014年:**按照说明从[第 2 步︰ 连接到 SQL 数据库，使用 SQL Server 管理 Studio 管理 Azure SQL 数据库的][sql-管理 studio]项目连接到 SQL Azure 服务器并运行 SQL 脚本。 请注意本文使用版本管理门户 (http://manage.windowsazure.com) 不预览门户 (http://portal.azure.com) 来配置 SQL Azure 服务器的防火墙。

    **如果您有在您的计算机上安装的 Visual Studio 2013年:**在[Azure 预览门户](http://portal.azure.com)，单击**浏览**左侧的中心、 单击**SQL 服务器**、 选择您的数据库，然后单击**在 Visual Studio 中打开**工具栏上的按钮来连接到 SQL Azure 服务器，然后运行该脚本。 如果您的客户端不允许访问该 SQL Azure 服务器，您需要配置防火墙，SQL Azure 服务器以允许从您的计算机 （IP 地址） 的访问。 请参见上面的步骤来配置您的 SQL Azure 服务器防火墙文章。


请执行以下操作︰

- 单击[使用数据工厂编辑器](data-factory-get-started-using-editor.md)顶部使用数据工厂编辑器，它是 Azure 门户的一部分来执行的教程的链接。
- 单击[使用 PowerShell](data-factory-monitor-manage-using-powershell.md)顶部使用 Azure PowerShell 执行本教程的链接。
- 单击[使用 Visual Studio](data-factory-get-started-using-vs.md)在顶部使用 Visual Studio 2013年执行本教程的链接。
 

## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-get-started.md)。 

<!--Link references-->
[azure 释放试验]: http://azure.microsoft.com/pricing/free-trial/
[azure 预览门户]: https://portal.azure.com/
[sql 的管理-studio]: http://azure.microsoft.com/documentation/articles/sql-database-manage-azure-ssms/#Step2

[监视器管理-使用 powershell]: data-factory-monitor-manage-using-powershell.md
[数据-工厂简介]: data-factory-introduction.md
[数据工厂-创建的存储]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account
[数据工厂-创建的 sql 的数据库]: ../sql-database-get-started.md 

测试
