---
ms.openlocfilehash: e407063071384092f8f9e5fe1fe76f07c6d0d155
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="SQL Azure 数据库弹性比例将引用添加到 Visual Studio 项目" 
    description="如何将.NET 引用添加到 Visual Studio 项目使用 Nuget 弹性扩展 Api。" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="sidneyh" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/24/2015" 
    ms.author="sidneyh"/>

# 如何︰ 添加到 Visual Studio 项目的弹性数据库客户端库引用 

### 先决条件： 

- 安装的 Visual Studio 的[NuGet Visual Studio 扩展库](http://docs.nuget.org/docs/start-here/installing-nuget) 

### 若要在 Visual Studio 中添加弹性数据库客户端库参考 

1. 打开一个现有项目或创建一个新项目，使用新建项目对话框中，位于**文件** --> **New** --> **项目** 
2. 在解决方案资源管理器中，用鼠标右键单击**引用**和选择**管理 NuGet 程序包**
3. 在管理 NuGet 程序包窗口左侧菜单中，选择**联机**，然后**nuget.org**或"所有" 
4. 在**联机搜索**对话框中，键入**数据库的弹性**、 选择**弹性数据库客户端库**并单击**安装**。

    ![联机搜索][1]
4. 查看许可协议，然后单击**我接受**。 

客户端库参考目前已添加到您的项目。 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-add-references-visual-studio/search-online.png
<!--anchors--> 