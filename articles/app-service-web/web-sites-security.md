---
ms.openlocfilehash: 69c2f4f54fb6d216c573dd5748834e1193e6fb51
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="安全的 web 应用程序在 Azure 应用程序服务"
    description="了解如何确保 Azure 的 web 应用程序的安全。"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="07/03/2015"
    ms.author="cephalin"/>


#安全的 web 应用程序在 Azure 应用程序服务

开发 web 应用程序的挑战之一是如何提供安全服务为您的客户。 在本文中，您将了解[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)来确保您的 web 应用程序的功能。

> [AZURE.NOTE] 基于 web 的应用程序的安全注意事项的完整讨论已超出了本文的讨论范围。 为进一步指导保护 web 应用程序的起始点，请参见[打开 Web 应用程序安全项目 (OWASP)]( https://www.owasp.org/index.php/Main_Page)，特别是[十大项目。](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)，其中列出了当前的顶 10 个关键的 web 应用程序安全漏洞，由 OWASP 成员

##<a name="https"></a> 保护通信的安全

如果您使用***。 azurewebsites.net**域名创建的 web 应用程序，您可以立即使用 HTTPS，SSL 证书提供所有* **。 azurewebsites.net**域名。 如果您的站点使用一个[自定义的域的名称](web-sites-custom-domain-name.md)，您可以为自定义的域[启用 HTTPS](web-sites-configure-ssl-certificate.md)上载 SSL 证书。

##<a name="develop"></a> 安全开发

### 发布配置文件和发布设置

在开发应用程序、 执行管理任务，或者使用**Visual Studio**、 **Web 矩阵**， **Azure PowerShell**和**Azure 命令行界面 (Azure CLI)**，实用程序自动完成任务时可以使用*发布设置*文件或*发布配置文件*。 两者都到 Azure，验证您的身份，应该得到保护，以防止未经授权的访问。

* **发布设置**文件包含

    * Azure 的订阅 ID

    * 管理证书，您可以执行管理任务，为您的订阅*而无需提供用户名或密码*。

* **发布配置**文件包含

    * 发布到您的 web 应用程序的信息

如果您使用一个实用工具，使用发布设置或发布配置文件，导入文件包含的发布设置或配置文件的实用程序，然后**删除**该文件。 如果您必须保留该文件，与为例，在该项目上工作的其他人共享存储在安全的位置，如**加密**目录具有受限制权限。

此外，应确保导入的凭据的安全。 例如， **Azure PowerShell**和**Azure 命令行界面 (Azure CLI)**都将导入的信息存储在**主目录**中 (*~*在 Linux 或 OS X 系统，在 Windows 系统上的*/users/yourusername* 。)为额外的安全性，您可能希望进行**加密**您的操作系统使用可用的加密工具这些位置。

### 配置设置和连接字符串
它是常见的做法在配置文件中存储连接字符串、 身份验证凭据和其他敏感信息。 遗憾的是，可能会在您的网站上公开这些文件或将其登记到一个公用的存储库，将公开此信息。

Azure 应用程序服务使您可以为**应用程序设置**和**连接字符串**作为 Web 应用程序运行时环境的一部分存储配置信息。 向您的应用程序在运行时*环境变量*对于大多数编程语言通过公开值。 对于.NET 应用程序中，这些值注入到您的.NET 配置在运行时。

配置使用[Azure 预览门户](http://portal.azure.com)或 PowerShell 和 Azure CLI 实用程序的**应用程序设置**和**连接字符串**。

应用程序设置和连接字符串的详细信息，请参阅[配置 web 应用程序](web-sites-configure.md)。

### 提取

Azure 提供安全 FTP 访问的文件系统对访问**FTPS**通过 web 应用程序。 这使您可以安全地访问 web 应用程序以及诊断上的应用程序代码日志。 可以使用以下步骤找到 FTPS 链接为您的 web 应用程序︰

1. 打开[Azure 预览门户](http://portal.azure.com)。
2. 选择**浏览所有**。
3. 从**浏览**刀片式服务器，选择**Web 应用程序**。
4. 从**Web 应用程序**刀片式服务器，请选择所需的 web 应用程序。
5. 从 web 应用程序的刀片式服务器，请选择**所有设置**。
6. 从**设置**刀片式服务器，选择**属性**。
7. 提供**设置**刀片式服务器上的 FTP 和 FTPS 链接。 

FTPS 的详细信息，请参阅[文件传输协议](http://en.wikipedia.org/wiki/File_Transfer_Protocol)。

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## 下一步行动

Azure 平台，报告**安全事故或使用不当**，或以通知 Microsoft，您将执行**渗透测试**您的网站，信息的安全性的详细信息，请参阅[微软 Azure 信任中心](http://azure.microsoft.com/support/trust-center/security/)的安全部分。

在 web 应用程序的**web.config**或**applicationhost.config**文件的详细信息，请参阅[配置选项解锁 Azure 应用程序服务 web 应用程序中](http://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/)。

有关信息日志记录信息的 web 应用程序，可能会在检测攻击，请参阅[启用诊断日志记录](web-sites-enable-diagnostic-log.md)。

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)

* 旧的门户与新门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)
 
测试
