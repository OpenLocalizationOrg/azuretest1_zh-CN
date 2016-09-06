---
ms.openlocfilehash: dcb12c3b82c38b415c9dc75c09e3d68145cdf6fd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="要开始使用 Azure 的多因素身份验证服务器" 
    description="这是介绍如何开始使用 Azure MFA 服务器 Azure 的多因素身份验证页。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# 要开始使用 Azure 的多因素身份验证服务器




<center>![云](./media/multi-factor-authentication-get-started-server/server2.png)</center>

现在，我们就能确定是否使用内部多因素身份验证，让我们来了解。  此页介绍新安装的服务器，并获取它的内部部署 Active Directory 安装。  如果已经安装了 Phonefactor 服务器和寻找如何这看到升级到 Azure 多元服务器，或者如果您正在寻找安装不仅仅是 web 服务请参见部署 Azure 多因素身份验证服务器移动应用程序 Web 服务的信息。



## 下载 Azure 的多因素身份验证服务器



有两种不同方法，您可以下载 Azure 的多因素身份验证服务器。 两者都是通过 Azure 的门户来完成的。  首先是通过直接管理多因素身份验证提供程序。  第二个是通过服务设置。  第二个选项需要多因素身份验证提供程序或 Azure 广告增值许可证。


### 从 Azure 的门户网站下载 Azure 多因素身份验证服务器
--------------------------------------------------------------------------------

1. 在 Azure 门户以管理员身份登录。
2. 在左边，选择活动目录。
3. 在活动的目录页上，单击顶部**多因素身份验证提供程序**
4. 在底部单击**管理**
5. 这将打开一个新页面。  单击**下载。**
![下载](./media/multi-factor-authentication-sdk/download.png)
6. 上面**生成激活凭据**，请单击**下载。**
![下载](./media/multi-factor-authentication-get-started-server/download4.png)
7. 将下载文件保存。



### 若要下载通过服务设置 Azure 多因素身份验证服务器


1. 在 Azure 门户以管理员身份登录。
2. 在左边，选择活动目录。
3. 双击您的实例的 Azure 的广告。
4. 在顶部单击**配置**
![下载](./media/multi-factor-authentication-sdk/download2.png)
5. 在多因素身份验证，选择**管理服务设置**
6. 在服务设置页中，在屏幕的底部单击**转至门户**。
![下载](./media/multi-factor-authentication-sdk/download3.png)
7. 这将打开一个新页面。 单击**下载。**
8. 上面**生成激活凭据**，请单击**下载。**
9. 将下载文件保存。




## 安装和配置 Azure 的多因素身份验证服务器
既然您已经下载服务器可以安装它，然后配置它。  请确保服务器安装它满足以下要求︰



Azure 的多因素身份验证服务器的要求|说明|
:------------- | :------------- | 
硬件|<li>200 MB 的硬盘空间</li><li>x32 或 x64 支持的处理器</li><li>1 GB 或更大 RAM</li>
软件|<li>Windows 2003 或更高，如果主机是服务器操作系统的服务器</li><li>Windows Vista 或更高，如果主机是客户机操作系统</li><li>Microsoft.NET 2.0 框架</li><li>IIS 6.0 或更高，如果安装用户门户网站或 web 服务 SDK</li>

### Azure 的多因素身份验证服务器防火墙要求
--------------------------------------------------------------------------------
MFA 的每台服务器必须能够进行通信端口 443 发送到以下︰

- https://pfd.phonefactor.net
- https://pfd2.phonefactor.net
- https://css.phonefactor.net

如果出站防火墙限制在端口 443 上，需要打开下面的 IP 地址范围︰

IP 子网|子网掩码|IP 范围
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1-134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1-134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129-70.37.154.254

如果您未使用 Azure 多因素身份验证事件确认功能，如果用户不进行身份验证的多因素身份验证移动应用程序在企业网络上的设备 IP 范围可以降低如下︰


IP 子网|子网掩码|IP 范围
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72-134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72-134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201-70.37.154.206


### 若要安装和配置 Azure 多因素身份验证服务器
--------------------------------------------------------------------------------


1. 双击可执行文件。 这将开始安装。
2. 在选择安装文件夹屏幕上，请确保该文件夹正确，单击下一步。
3. 一旦完成安装，请单击完成。  这将启动配置向导。
4. 在配置向导欢迎屏幕、**跳过使用的身份验证配置向导**在打勾并单击**下一步**。  这将关闭此向导并启动服务器。
![云](./media/multi-factor-authentication-get-started-server/skip2.png)

5. 重新打开我们下载来自服务器的页面，请单击**生成激活凭据**按钮。  将此信息复制到 Azure MFA 服务器在提供的框中，单击**激活**。


上面的步骤演示了使用配置向导快速设置。  您可以重新运行的身份验证向导从服务器上的工具菜单中选择。



##从 Active Directory 导入用户

现在，安装和配置服务器可以快速将用户导入 Azure MFA 服务器。 

### 若要从 Active Directory 导入用户
--------------------------------------------------------------------------------


1. 在 Azure MFA 服务器上，在左边，选择**用户**。
2. 在底部，选择**从活动目录导入**。
3. 现在可以为各个用户搜索或搜索广告目录 Ou 中的用户。  在这种情况下，我们将指定的用户 OU。
4. 突出显示所有在右侧的用户，然后单击**导入**。  将出现一个弹出窗口，告诉您，您已成功。  关闭导入窗口中。

![云](./media/multi-factor-authentication-get-started-server/import2.png)

## 向用户发送一封电子邮件
既然您您的用户导入到 Azure 多因素身份验证服务器，建议您将发送您的用户的电子邮件，通知他们，他们已被注册在多因素身份验证。

Azure 多因素身份验证服务器有多种方法可以将您的用户配置为使用多因素身份验证。  例如，如果您知道用户的电话号码，或能够导入从其公司的目录 Azure 多因素身份验证服务器的电话号码，电子邮件就可以让用户知道，它们已配置为使用 Azure 多因素身份验证，提供了有关使用 Azure 多因素身份验证的一些说明并告知的电话号码，他们会在收到其身份验证的用户。  

电子邮件的内容会因身份验证已设置为用户 （例如电话联络，SMS，移动应用程序） 的方法。  例如，如果用户需要验证身份时使用 PIN，电子邮件将告诉他们什么其初始 PIN 已设置为。  用户通常需要在其第一个身份验证过程中更改其 PIN。

如果尚未配置或导入到 Azure 的多因素身份验证服务器，用户的电话号码或用户进行了预配置要用于身份验证的移动应用程序，您可以向他们发送一封电子邮件，让他们知道，他们已被配置为使用 Azure 多因素身份验证，它将指导他们完成 Azure 多因素身份验证的用户门户通过其开户。  超链接将包括，在用户单击以访问用户门户网站。 当用户单击该超链接时，web 浏览器打开并将其带到他们公司的 Azure 多因素身份验证的用户门户。   


### 配置电子邮件和电子邮件模板

通过单击左侧的电子邮件图标上，您可以设置设置发送这些电子邮件。  这是在您可以输入您的邮件服务器的 SMTP 信息并允许您发送总宽电子邮件通过将检查添加到发送邮件到用户的复选框。

![电子邮件设置](./media/multi-factor-authentication-get-started-server/email1.png)

在电子邮件内容选项卡上，您将看到所有各种电子邮件模板可供选择。  根据如何配置您要使用多因素身份验证的用户，您可以选择该模板，以便最好地适合您。

![电子邮件模板](./media/multi-factor-authentication-get-started-server/email2.png)

## 高级 Azure 的多因素身份验证服务器配置
有关高级设置的其他信息和配置信息可以使用下表。

方法|说明
:------------- | :------------- | 
[用户门户](multi-factor-authentication-get-started-portal.md)|  有关安装和配置包括部署和用户自助服务用户门户。
[Active Directory 联合身份验证服务](multi-factor-authentication-get-started-adfs.md)|有关设置 Azure 使用 AD FS 的多因素身份验证信息。
[半径 Authenticaton](multi-factor-authentication-get-started-server-radius.md)|  有关安装和配置 RADIUS Azure MFA 服务器。
[IIS 身份验证](multi-factor-authentication-get-started-server-iis.md)|有关安装和使用 IIS 配置 Azure MFA 服务器。
[Windows Authenticaton](multi-factor-authentication-get-started-server-windows.md)|  有关安装和使用 Windows 身份验证配置 Azure MFA 服务器。
[LDAP 身份验证](multi-factor-authentication-get-started-server-ldap.md)|有关安装和配置 Azure MFA 服务器使用 LDAP 身份验证。
[远程桌面网关和使用 RADIUS Azure 多因素身份验证服务器](multi-factor-authentication-get-started-server-rdg.md)|  有关安装和配置远程桌面网关使用 RADIUS Azure MFA 服务器的信息。
[与 Windows 服务器的 Active Directory 同步](multi-factor-authentication-get-started-server-dirint.md)|有关安装和配置 Active Directory 和 Azure MFA 服务器之间的同步。
[Azure 的多因素身份验证服务器移动应用程序 Web 服务部署](multi-factor-authentication-get-started-server-webservice.md)|有关安装和配置 Azure MFA 服务器 web 服务的信息。
