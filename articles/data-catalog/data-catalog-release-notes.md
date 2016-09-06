---
ms.openlocfilehash: ab2df8710818838365dc4251941eec743ca9b0a6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 数据目录发行说明"
   description="Azure 数据目录的 28 8 月 2015年公共预览版本的发行说明。"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="08/28/2015"
   ms.author="maroche"/>

# Azure 数据目录发行说明

## 28 8 月 2015年的说明发布 Azure 数据目录

### 缺少某些已注册的数据资产的数据配置文件

注册时数据源与数据分析的数据源注册工具中的选项，请在以下情况下可能不包含数据的档案文件信息︰

* Azure 的 SQL 数据库表
* SQL Server 表和视图的位置有多个不同的架构中具有相同名称的对象
* SQL Server 表和视图与超过 118 个字符的列名称
* Oracle 表和视图与超过 20 个字符的列名称
* Oracle 表和视图用空格或列名称中的 multu 字节字符

这些限制是由于在 8 月 28 日的版本中，一个已知的问题，将在以后 Azure 数据目录更新中解决。

## 说明 13 7 月 2015年发布 Azure 数据目录

### 注册并连接到 Oracle 数据库

当连接到 Oracle 数据库的数据源的用户必须已安装了正确的 Oracle 驱动程序相匹配所用的软件的位 （32 位或 64 位）。

-   注册时运行 32 位 Windows 的计算机上的 Oracle 数据源，请将使用 32 位 Oracle 驱动程序
-   注册时运行 64 位 Windows 的计算机上的 Oracle 数据源，请将使用 64 位 Oracle 驱动程序
-   连接到 Oracle 数据源运行的 Microsoft Office 的 32 位版本的计算机上使用 Excel 时，包括在 64 位 Windows 上的 32 位 Oracle 驱动程序将使用
-   64 位 Oracle 驱动程序连接到 Oracle 数据源运行的 Microsoft Office 的 64 位版本的计算机上使用 Excel 时，将使用

### 注册和连接到 SQL Server 报告服务

对 SQL Server 报表服务 (SSRS) Azure 数据目录的初始预览版本中的数据源支持仅限于本机模式的服务器。 将在以后的版本中添加 SSRS 支持 SharePoint 模式。

### 在 Excel 中打开数据资产

当在 Microsoft Excel 中打开数据资产，从 Azure 数据目录入口， **Microsoft Excel 安全通知**对话框中可能会提示用户。 这是标准的预期的行为，并且用户可以选择**启用**以继续。

有关详细信息，请参阅[启用或禁用有关链接和文件来自可疑网站的安全警报](https://support.office.com/en-us/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE)。

### 在预览中缺少的斑点和 UDT 列

注册的表和视图包含二进制大对象 (BLOB) 和用户定义的数据类型 (UDT) 列中，选择要包括的数据资产的预览，预览中将不会包括这些列。

### 代理和策略配置和数据源注册

用户可能会遇到他们从何处可以登录到 Azure 数据目录门户，但在尝试登录到数据源注册工具遇到错误消息，可以阻止登录时的情况。

有这种问题行为的两个可能的原因︰

**导致 1: Active Directory 联合身份验证服务配置**数据源注册工具使用窗体身份验证来验证对 Active Directory 用户登录。 对于成功的登录窗体身份验证必须在全局身份验证策略中启用 Active Directory 管理员。

在某些情况下，只有当用户是在公司网络上，或仅从公司网络外部连接的用户，可能发生这种错误行为。 全局身份验证策略允许分别为企业内部网和外部网连接启用的身份验证方法。 如果未启用窗体身份验证的用户进行连接的网络，则可能会出现登录错误。

有关详细信息，请参阅[配置 intranet 基于窗体的身份验证不支持 WIA 设备](https://technet.microsoft.com/library/dn727110.aspx)。

**导致 2︰ 网络代理配置**如果公司的网络使用代理服务器，注册工具可能无法通过代理服务器连接到 Azure 活动目录。 用户可以确保注册工具编辑工具的配置文件，将这一节添加到文件︰


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


为找到 RegistrationTool.exe.config 文件，启动注册工具，然后打开 Windows 任务管理器实用程序。 在任务管理器详细信息选项卡上，在 RegistrationTool.exe 上右击并从弹出式菜单中选择打开文件的位置。

测试
