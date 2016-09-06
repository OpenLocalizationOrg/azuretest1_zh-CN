---
ms.openlocfilehash: ca07e5478f1957c8633a99e16ca1027890f35ead
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何将云服务配置 |Microsoft Azure" 
    description="了解如何配置在 Azure 的云服务。 了解如何更新云服务配置并配置远程访问角色实例。" 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2015"
    ms.author="adegeo"/>




# 如何配置云服务

> [AZURE.SELECTOR]
- [Azure 门户](cloud-services-how-to-configure.md)
- [Azure 预览门户](cloud-services-how-to-configure-portal.md)

可以在 Azure 管理门户配置为云服务最常用的设置。 或者，如果您想直接更新您的配置文件，下载更新，然后上载已更新的文件并更新云服务使用配置更改服务配置文件。 两种方法配置更新被推送到所有角色实例。

您还可以启用远程桌面连接到运行在云服务中的一个或所有角色。  远程桌面允许您访问应用程序的桌面运行时故障排除和诊断问题。  即使您没有配置的服务定义文件 (.csdef) 远程桌面应用程序开发过程，您可以启用远程桌面连接到您的角色。  没有必要重新部署您的应用程序以启用远程桌面连接。

Azure 只可以配置更新期间确保 99.95%的服务可用性，如果必须为每个角色至少两个角色实例。 这样一个虚拟机来处理客户端请求，而其他正在更新。 有关详细信息，请参阅[服务级别协议要求](http://azure.microsoft.com/support/legal/sla/)。

## 更改一个云服务

1. 在[Azure 预览门户网站](http://portal.azure.com/)中，导航到您的云服务。

2. 单击**设置**图标或**基础/所有设置**链接以打开**设置**刀片式服务器。

    ![设置页面](./media/cloud-services-how-to-configure-portal/cloud-service.png)
    
    从这里可以查看**属性**，更改**配置**、 管理**证书**，和管理人员有权访问此云服务**用户**。

2. 在**监控**部分下，您可以单击任何拼贴来配置警报。 

    ![云服务监视](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)
    
3. 在**角色和实例**部分中，您可以单击任何云服务角色来管理实例。

    ![云服务实例](./media/cloud-services-how-to-configure-portal/cs-instance.png)
    
    远程连接，重新启动，也可以重新映像从这里的云服务。
    
    ![云服务实例按钮](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

>[AZURE.NOTE]
>云服务使用的操作系统不能更改使用**Azure 预览门户**，您只能更改此设置通过[非预览门户](http://manage.windowsazure.com/)。 这将详细说明[此处](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file)。

## 云服务配置文件更新

1. 首先，将现有的云服务配置文件 (.cscfg) 下载。

    1. 在[Azure 预览门户网站](http://portal.azure.com/)中，导航到您的云服务。

    2. 单击**设置**图标或**基础/所有设置**链接以打开**设置**刀片式服务器。

        ![设置页面](./media/cloud-services-how-to-configure-portal/cloud-service.png)
    
    3. 单击**配置**项。

        ![刀片式服务器配置](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
    
    4. 单击**下载**按钮。

        ![下载](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

2. 更新服务配置文件后，上载并应用配置更新︰

    1. 按照从上面的前 3 个步骤来打开**配置**刀片式服务器的云服务。
    
    2. 单击**上载**按钮。

        ![上载](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
    3. 选择.cscfg 文件，然后单击**确定**。

## 配置远程访问角色实例

不能使用**Azure 预览门户**配置远程访问，您只能更改此设置通过[非预览门户](http://manage.windowsazure.com/)。 这将详细说明[此处](cloud-services-role-enable-remote-desktop.md)。
            
 

测试
