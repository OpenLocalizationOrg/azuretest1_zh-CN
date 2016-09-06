---
ms.openlocfilehash: 03889966e61e7768a53474b410c0e3e1ca939caa
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="SharePoint 服务器场 |Microsoft Azure"
    description="快速创建新基本或高度可用 SharePoint Server 2013 场在 Azure 预览门户使用 SharePoint 服务器场。"
    services="virtual-machines"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sharepoint"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2015"
    ms.author="josephd"/>

# SharePoint 服务器场

使用 SharePoint 服务器场中，Microsoft Azure 预览门户自动创建预先配置的 SharePoint Server 2013 场。 这可以节省大量的时间用于开发和测试环境中需要的基本或高可用性 SharePoint 场时，或作为一种协作解决方案为您的组织评估 SharePoint Server 2013。

基本的 SharePoint 场包含，在此配置中的三个虚拟机。

![sharepointfarm](./media/virtual-machines-sharepoint-farm-azure-preview/SPFarm_Basic.png)

对于简化的安装 SharePoint 应用程序开发或 SharePoint 2013 的第一次评估，可以使用此服务器场配置。

高可用性 SharePoint 场包含，在此配置中的九个虚拟机。

![sharepointfarm](./media/virtual-machines-sharepoint-farm-azure-preview/SPFarm_HighAvail.png)

此服务器场配置可用于更高版本的客户端负载、 SharePoint 场外部的 SharePoint 网站，以及 SQL Server AlwaysOn 高可用性测试。 此外可以用于高可用性环境中的 SharePoint 应用程序开发中使用此配置。

同时这些服务器场的配置详细信息，请参阅[SharePoint 服务器场配置详细信息](virtual-machines-sharepoint-farm-config-azure-preview.md)。

## 逐句通过配置

使用 SharePoint 服务器场模板创建 SharePoint 服务器场，请执行以下操作︰

1. 在[Microsoft Azure 预览门户](https://portal.azure.com/)中，单击**新建** > **计算** > **SharePoint 服务器场**。 如果**SharePoint 服务器场**没有出现，单击**新建** > **计算** > **市场**，键入**SharePoint** **搜索计算**，然后再单击**SharePoint 服务器场**。 在**SharePoint 服务器场**窗格中，单击**创建**。
2. 在**创建 SharePoint 场**窗格中，键入资源组的名称。
3. 在您的服务器场中每个虚拟机上键入用户名和密码的本地管理员帐户。 选择名称和密码，很难猜测、 记录，并将其存储在安全的位置。
4. 如果所需的高可用性服务器场，请单击**启用高可用性**。
5. 若要配置域控制器，请单击箭头。 您可以指定主机名称前缀 （默认值是资源组的名称），目录林根域名称 （默认为 contoso.com），和域控制器 （默认为 A1） 的大小。
6. 要配置您的 SQL 服务器，请单击箭头。 您可以指定主机名称前缀 （默认值是资源组的名称），大小 （默认为 A5） 您的 SQL 服务器，数据库访问帐户名和密码 （默认设置是使用管理员帐户），（默认值是 sqlservice） 的 SQL 服务器服务帐户名和密码 （默认值是为管理员帐户使用相同的密码）。
7. 若要配置 SharePoint 服务器，请单击箭头。 可以指定主机名称前缀 （默认值是资源组的名称），SharePoint 服务器 （默认值为 A2），大小 （默认为 sp_setup） 的 SharePoint 用户帐户和密码、 （默认值是 sp_farm） 的 SharePoint 服务器场帐户名和密码和 SharePoint 服务器场密码。 默认设置是使用 SharePoint 用户帐户、 服务器场帐户和密码的管理员密码。
8. 要配置可选配置设置虚拟网络、 存储帐户或诊断程序，请单击相应的箭头。
9. 若要指定订阅，请单击箭头。
10. 当完成后时，请单击**创建**。

> [AZURE.NOTE] 域控制器没有默认安装的活动目录管理工具。 若要安装这些组件，**安装 WindowsFeature AD 域服务 IncludeManagementTools**命令管理员级 Windows PowerShell 命令提示符下运行在域控制器虚拟机。

## 访问和管理 SharePoint 服务器场

SharePoint 场有预配置的终结点，以允许对 Internet 连接的客户端计算机到 SharePoint web 服务器的未经身份验证的 web 通信 （TCP 端口 80）。 此端点是预配置的团队站点。 若要访问此工作组站点︰

1.  从 Azure 预览门户，单击**浏览**，然后单击**资源组**。
2.  在资源组列表中，单击您的 SharePoint 服务器场资源组的名称。
3.  在 SharePoint 服务器场资源组的窗格中，单击**部署历史记录**。
4.  在**部署历史记录**窗格中，单击**Microsoft.SharePointFarm**。
5.  在**Microsoft.SharePointFarm**窗格中，在**SHAREPOINTSITEURL**字段中选择的 URL 并将其复制。
6.  从互联网浏览器，该 URL 粘贴到地址字段中。
7.  出现提示时，输入您在创建服务器场时指定的用户帐户凭据。

从中央管理 SharePoint 网站中，您可以配置我的网站、 SharePoint 应用程序和其他功能。 有关详细信息，请参阅[配置 SharePoint 2013](http://technet.microsoft.com/library/ee836142.aspx)。 若要访问中央管理 SharePoint 网站︰

1.  从 Azure 预览门户，单击**浏览**，然后单击**资源组**。
2.  在资源组列表中，单击您的 SharePoint 服务器场资源组的名称。
3.  在 SharePoint 服务器场资源组的窗格中，单击**部署历史记录**。
4.  在**部署历史记录**窗格中，单击**Microsoft.SharePointFarm**。
5.  在**Microsoft.SharePointFarm**窗格中，在**SHAREPOINTCENTRALADMINURL**字段中选择的 URL 并将其复制。
6.  从互联网浏览器，该 URL 粘贴到地址字段中。
7.  出现提示时，输入您在创建服务器场时指定的用户帐户凭据。


注释︰

- Azure 预览门户将创建这些虚拟机指定的订阅中。
- Azure 预览门户将创建两个这些场中仅云虚拟网络与面向 Internet 的 web 平台。 没有站点到站点 VPN 或 ExpressRoute 连接到组织网络。
- 您可以管理远程桌面连接到这些服务器。 有关详细信息，请参阅[如何登录到虚拟机运行 Windows 服务器](virtual-machines-log-on-windows-server.md)。

## Azure 的资源管理器

Azure 预览门户网站的 SharePoint 服务器场功能在服务管理中创建虚拟机。 若要创建 SharePoint Server 2013 场资源管理器中，请参阅[使用 Azure 资源管理器模板部署 SharePoint 服务器场](virtual-machines-workload-template-sharepoint.md)。

## 其他资源

[SharePoint 服务器场配置详细信息](virtual-machines-sharepoint-farm-config-azure-preview.md)

[SharePoint Azure 的基础结构服务](http://msdn.microsoft.com/library/azure/dn275955.aspx)

[设置 SharePoint intranet 服务器场中用于测试的混合云](../virtual-network/virtual-networks-setup-sharepoint-hybrid-cloud-testing.md)

[在 Azure 的基础结构服务承载 SharePoint 服务器场](virtual-machines-sharepoint-infrastructure-services.md)
