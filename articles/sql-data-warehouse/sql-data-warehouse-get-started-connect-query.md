---
ms.openlocfilehash: fed381fd870f0750c02410fb4138c846cdad29fc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="入门︰ 连接到 SQL 数据仓库 |Microsoft Azure"
   description="开始使用连接到 SQL 数据仓库和运行某些查询。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/23/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>
   
# 入门︰ 连接到 SQL 数据仓库
本快速入门文章介绍向连接和查询 SQL 数据仓库的调配的实例具有两种不同的工具︰

**Visual Studio**的 Visual Studio 集成的代码编辑器和调试器，SQL Server 数据工具 (SSDT)，是完全符合 SQL 的数据仓库，使您能够方便地连接、 查询和管理 SQL 数据仓库。  

> [AZURE.NOTE] SQL 数据仓库需要至少 SSDT 预览版本 12.0.50623 或更高版本

**sqlcmd** -sqlcmd 是一个命令行工具，它提供了简单连接和查询的能力。  

在完成这篇文章后，您将拥有︰

1. 安装并更新 Visual Studio 2013 年
2. 在 SSDT 创建连接到 SQL 数据仓库
3. 执行 SQL 数据仓库数据库查询

>[AZURE.NOTE] 假定您既已完成资源调配的指南或有一个可用的 SQL 数据仓库。 如果不配置 SQL 数据仓库，然后请回资源调配快速开始。

## 设置 Visual Studio 开发##
对于开发，SQL 数据仓库团队建议与 SQL Server 数据工具结合使用 Visual Studio 2013年或更高版本。 以下将概述了如何下载并更新如果没有可行的安装的 visual studio 版本的 Visual Studio 2013年。

如果您正在寻找更多的信息，可以使用[完整 SSDT 文档](https://msdn.microsoft.com/library/azure/hh272686.aspx)解析 SSDT 常见问题。

### 获取 Visual Studio 2013 ###
Visual Studio 2013年网站，下载并安装的 Visual Studio 的头上。 请记住，任何免费版本的 SQL 数据仓库是绰绰有余。 感觉可以自由地选择一个适合您的需要

<a href="https://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5" target="blank">获取 Visual Studio 2013</a>

### 更新 Visual Studio 2013 ###
已安装的 Visual Studio 2013年吗？ 很好 ！ 若要确保它是最新只需执行以下步骤。 您可以继续执行下一步。

1. 打开 Visual Studio 2013 年
2. 选择"工具"菜单，然后选择"扩展和更新..."
3. 树控件的"更新"和"产品更新"中导航
4. 如果不没有可用的任何更新，则可以安全地关闭"扩展和更新"窗口并转到本快速入门中的下一个任务。

但是，如果为 Visual Studio 本身存在更新单击更新按钮。 这将启动 Visual Studio 2013年的最新更新的下载。

关闭"扩展和更新"窗口，还在继续之前关闭 Visual Studio 2013年。

5. 单击"运行"按钮以将最新的更新安装到 Visual Studio 2013年。

6. 接受许可协议条款并单击安装按钮接受任何用户帐户控制 (UAC) 提示

7. 单击"启动"按钮以完成安装

Visual Studio 2013年的更新已完成。

### 更新 SSDT 
> [AZURE.IMPORTANT] SQL 数据仓库需要至少 SSDT 预览版本 12.0.50623 或更高版本。

SSDT 工程师及其插件非常定期更新与新功能使您会发现您需要时常更新。 再次，这是一个非常简单的过程。 若要检查您是否需要更新 SSDT，请执行以下步骤︰

1. 打开 Visual Studio 2013年。  
2. 选择"工具"菜单，然后选择"扩展和更新..."
3. 树控件的"更新"和"产品更新"中导航
4. 如果不没有可用的任何更新，则可以安全地关闭"扩展和更新"窗口并转到本快速入门中的下一个任务。

但是，如果更新存在称为"Microsoft SQL Server 数据库工具的更新"单击更新按钮。

这将启动 SSDT 最新版的下载。 下图显示了 SSDTSetup.exe 在 Internet Explorer 的下载管理器。

5. 单击"运行"按钮安装 SSDT 最新版本。

6. 接受许可条款，然后单击安装按钮上

7. 单击"关闭"按钮以完成 SSDT 更新

8. 关闭"扩展和更新"窗口

现在有最新版本的 Visual Studio 2013 SSDT 扩展名为最新的桌面。

> [AZURE.NOTE] 当前建议使用[SSDT 预览 Visual Studio 2013 版本 12.0.50623 或更高版本](http://go.microsoft.com/fwlink/?LinkID=616714&clcid=0x409) 

## 与 Visual Studio 2013
如果您正在运行所需的版本的 Visual Studio 将能够连接到 SQL 数据仓库实例中两种不同方式︰

### 从 Visual Studio
使连接我们需要的只是打开 SQL Server 对象资源管理器并传入的连接信息

1. 打开 Visual Studio
2. 选择"视图"菜单上，选择"SQL Server 对象资源管理器"从下拉列表中向下

> [AZURE.NOTE] 请您输入 SQL Server 对象资源管理器和***没有***服务器资源管理器。 名称听起来类似，但它们是非常不同的控件。 它们位于相邻因此请小心，确保已选择正确 ！

现在，您可以看到 SQL Server 对象资源管理器︰


3. 单击 SQL Server 对象资源管理器中的"添加服务器"按钮。 这已在下图中突出显示︰

4. 连接到服务器对话框中创建逻辑服务器时选择的值来填充。 此外，单击选项按钮，并在连接之前指定要连接到 （SQL 数据仓库实例） 的数据库。

随意检查"记住密码"刻度线框中，如果您愿意的话。 它为您节约大量时间，但请记住这它允许具有物理访问您的配置文件，以便使用此帐户执行查询的任何人。

> [AZURE.NOTE] 请记住，服务器名称必须是完全限定的。 因此，您的服务器名称值应遵循此约定︰***服务器名***。 database.windows.net 其中***服务器名***是名称赋予您的逻辑服务器。

完全填充的凭据后，单击连接

您现在已经完成了连接并在 SQL Server 对象资源管理器中注册您的 SQLDW 逻辑服务器。
    
### 从 Azure 门户
直接从 Azure 门户进入 Visual Studio。

1. 登录到[Azure 的管理门户](http://manage.windowsazure.com/)。
2. 选择浏览刀片式服务器中您所需的 SQL 数据仓库实例。 
3. 顶部的 SQL 数据仓库刀片选择中打开 Visual...
4. 如果您还没有配置您的防火墙客户端计算机的 IP 与选择配置您的防火墙。
    1. 规则名称，输入开始 IP 和结束 IP。 
    2. 顶部的刀片式服务器，请单击保存。   
5. 单击在 Visual Studio 中的打开。 
6. Visual Studio 会打开，并且系统将提示您输入您的凭据 
7. 后输入您的凭据和连接，服务器和数据仓库实例将立即出现在您的 SQL Server 对象资源管理器面板。  

## 使用 sqlcmd 连接到 SQL 数据仓库

您还可以使用[sqlcmd](https://msdn.microsoft.com/library/azure/ms162773.aspx)命令提示符实用程序所包含的 SQL Server 或[Microsoft SQL Server 的命令行实用程序 11](http://go.microsoft.com/fwlink/?LinkId=321501)连接到 SQL 数据仓库。 Sqlcmd 实用程序允许您输入事务处理 SQL 语句、 系统过程和脚本文件在命令提示符下。

时使用的 sqlcmd 您将需要打开命令提示符连接到 SQL 数据仓库的特定实例，然后输入*sqlcmd*跟 SQL 数据仓库数据库的连接字符串。 连接字符串将需要包含以下参数︰

+ **用户 (-U):**在窗体的服务器用户`<`用户`>`@`<`服务器名称`>`。 database.windows.net
+ **密码 (-P):**与用户关联的密码
+ **服务器 (-S):**在窗体中的服务器`<`服务器名称`>`。 database.windows.net
+ **数据库 (-D):**数据库名称 
+ **启用带引号的标识符 (-I):**要连接到的 SQL 数据仓库实例，则必须启用带引号的标识符。 

因此要连接到的 SQL 数据仓库实例，您可以输入以下命令︰

```
C:\>sqlcmd -U <User>@<Server Name>.database.windows.net -P <Password> -S <Server Name>.database.windows.net -d <Database> -I
```

连接后，您可以发出针对该实例的任何支持事务处理 SQL 语句。 例如，以下语句利用[CREATE TABLE](https://msdn.microsoft.com/library/azure/dn268335.aspx)语句来创建新表

```
C:\>sqlcmd -U <User>@<Server Name>.database.windows.net -P <Password> -S <Server Name>.database.windows.net -d <Database> -I
1> CREATE TABLE table1 (Col1 int, Col2 varchar(20));
2> GO
3> QUIT
```
    
有关 sqlcmd 的更多信息请参阅[sqlcmd 文档](https://msdn.microsoft.com/library/azure/ms162773.aspx)。

<!--NOTE: SQL DB does not support the -z/-Z parameters for changing users password with SQLCMD.  Do we support this? -->

## 执行查询示例 ##

既然我们已经注册了我们的服务器接下来，编写一个查询。

1. 单击在 SSDT 用户数据库

2. 单击新建查询按钮

   现在打开新窗口

3. 编写查询

    查询窗口中键入以下代码︰

    ```
    SELECT  COUNT(*)
    FROM    sys.tables;
    ```

4. 执行查询

    在下面的绿色箭头上执行查询单击或使用下列快捷方式`CTRL`+`SHIFT`+`F5`:

## 下一步行动 ##
现在，您可以连接并查询尝试[加载示例数据][]或[开发代码][]。

[加载示例数据]: ./sql-data-warehouse-get-started-load-samples.md  
[开发代码]: ./articles/sql-data-warehouse-overview-develop/

