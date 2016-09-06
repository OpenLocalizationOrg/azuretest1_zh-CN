---
ms.openlocfilehash: 935ba70ce6a1c59fa03bd29cc3d6d255ba728131
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="SharePoint 服务器场配置 |Microsoft Azure"
    description="当您使用 Azure 预览门户网站的 SharePoint 服务器场功能了解 SharePoint 服务器场的默认配置。"
    services="virtual-machines"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sharepoint"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2015"
    ms.author="josephd"/>


# SharePoint 服务器场的配置详细信息

SharePoint 服务器场是自动为您创建一个预先配置的 SharePoint Server 2013 场 Azure 预览门户的一项功能。 有两个服务器场的配置︰

- 基本
- 高可用性

下面的部分提供为每个服务器场的配置详细信息。

有关其他信息，请参阅[SharePoint 服务器场](virtual-machines-sharepoint-farm-azure-preview.md)。

## 基本的 SharePoint 服务器场

基本的 SharePoint 场包含，在此配置中的三个虚拟机︰

![sharepointfarm](./media/virtual-machines-sharepoint-farm-config-azure-preview/SPFarm_Basic.png)

下面是配置详细信息︰

-   Azure 订阅︰ 在初始配置过程中指定。
-   Azure 域名 （也称作云服务）︰ 自动为每个虚拟机创建单独的域的名称。
-   存储帐户︰ 在初始配置过程中指定。
-   虚拟网络︰
    -   类型︰ 仅云
    -   地址空间︰ 10.0.0.0/26

- 虚拟机︰
    -   *HostNamePrefix*直流 （AD DS 域控制器）
    -   *HostNamePrefix*-SQL （SQL Server 2014年服务器）
    -   *HostNamePrefix*-SP （SharePoint 2013 服务器）

- 域控制器︰
    -   虚拟机映像︰ Windows Server 2012 R2。
    -   主机名前缀︰ 在初始配置过程中指定。
    -   大小︰ A1 （默认值）。
    -   域名︰ contoso.com （默认值）。
    -   域管理员帐户名称︰ 在初始配置过程中指定。
    -   域管理员帐户的密码︰ 在初始配置过程中指定。

- 数据库服务器
    -   虚拟机映像︰ Windows Server 2012 R2 上的 SQL Server 2014 RTM 企业。
    -   主机名前缀︰ 在初始配置过程中指定。
    -   大小︰ A5 （默认值）。
    -   数据库访问帐户名称︰ 在初始配置过程中指定。
    -   数据库访问帐户密码︰ 在初始配置过程中指定。
    -   SQL Server 服务帐户名称︰ sqlservice （默认值）。
    -   SQL Server 服务帐户的密码︰ 在初始配置过程中指定。

- SharePoint 服务器
    -   虚拟机映像︰ 2013年试用 SharePoint 服务器。
    -   主机名前缀︰ 在初始配置过程中指定。
    -   大小︰ A2 （默认值）。
    -   SharePoint 服务器场帐户名称︰ sp_farm （默认值）。
    -   SharePoint 服务器场帐户密码︰ 在初始配置过程中指定。
    -   SharePoint 服务器场密码︰ 在初始配置过程中指定。


## 高可用性 SharePoint 服务器场

高可用性 SharePoint 场包含，在此配置中的九个虚拟机︰

![sharepointfarm](./media/virtual-machines-sharepoint-farm-config-azure-preview/SPFarm_HighAvail.png)

下面是配置详细信息︰

-   Azure 订阅︰ 在初始配置过程中指定。
-   Azure 域名 （也称作云服务）︰ 根据上图创建单独的域的名称。
-   存储帐户︰ 在初始配置过程中指定。
-   虚拟网络︰
    -   类型︰ 仅云
    -   地址空间︰ 10.0.0.0/26

-   虚拟机︰
    -   *HostNamePrefix*-DC1 （AD DS 域控制器）
    -   *HostNamePrefix*-DC2 （AD DS 域控制器）
    -   *HostNamePrefix*-SQL1 （SQL Server 2014年服务器）
    -   *HostNamePrefix*-SQL2 （SQL Server 2014年服务器）
    -   *HostNamePrefix*-SQL0 （Windows Server 2012 R2 服务器）
    -   *HostNamePrefix*-WEB1 （SharePoint 2013 服务器）
    -   *HostNamePrefix*-WEB2 （SharePoint 2013 服务器）
    -   *HostNamePrefix*-APP1 （SharePoint 2013 服务器）
    -   *HostNamePrefix*-APP2 （SharePoint 2013 服务器）

-   域控制器︰
    -   虚拟机映像︰ Windows Server 2012 R2。
    -   主机名前缀︰ 在初始配置过程中指定。
    -   大小︰ A1 （默认值）。
    -   域名︰ contoso.com （默认值）。
    -   域管理员帐户名称︰ 在初始配置过程中指定。
    -   域管理员帐户的密码︰ 在初始配置过程中指定。

-   数据库服务器︰
    -   虚拟机映像︰ Windows Server 2012 R2 上的 SQL Server 2014 RTM 企业。
    -   主机名前缀︰ 在初始配置过程中指定。
    -   大小︰ A5 （默认值） A0 的数据库服务器 （默认值） 文件共享见证。
    -   数据库访问帐户名称︰ 在初始配置过程中指定。
    -   数据库访问帐户密码︰ 在初始配置过程中指定。
    -   SQL Server 服务帐户名称︰ sqlservice （默认值）。
    -   SQL Server 服务帐户的密码︰ 在初始配置过程中指定。

-   SharePoint 服务器︰
    -   虚拟机映像︰ 2013年试用 SharePoint 服务器。
    -   主机名前缀︰ 在初始配置过程中指定。
    -   大小︰ A2 （默认值）。
    -   SharePoint 服务器场帐户名称︰ sp_farm （默认值）。
    -   SharePoint 服务器场帐户密码︰ 在初始配置过程中指定。
    -   SharePoint 服务器场密码︰ 在初始配置过程中指定。

> [AZURE.NOTE] 从 2013年试用 SharePoint 服务器映像创建 SharePoint 服务器。 要继续使用试用版过期后的虚拟机，您需要将转换安装过程中使用的标准或企业版的 SharePoint Server 2013 零售或批量许可证密钥。

## Azure 的资源管理器

Azure 预览门户网站的 SharePoint 服务器场功能在服务管理中创建虚拟机。 若要创建 SharePoint Server 2013 场 Azure 资源管理器中，请参阅[使用 Azure 资源管理器模板部署 SharePoint 服务器场](virtual-machines-workload-template-sharepoint.md)。

## 其他资源

[SharePoint 服务器场](virtual-machines-sharepoint-farm-azure-preview.md)

[SharePoint Azure 的虚拟机上](http://msdn.microsoft.com/library/azure/dn275955.aspx)

[设置 SharePoint intranet 服务器场中用于测试的混合云](../virtual-network/virtual-networks-setup-sharepoint-hybrid-cloud-testing.md)
