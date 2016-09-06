---
ms.openlocfilehash: 2f2f98c03e46a09d0f302ffdfc6136994f0caf13
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="为一个 Azure 门户中的虚拟机创建 FQDN |Microsoft Azure"
   description="了解如何创建一个完全限定域名或 FQDN 的资源管理器基于 Azure 预览门户中的虚拟机。"
   services="virtual-machines"
   documentationCenter=""
   authors="dsk-2015"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-management"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/21/2015"
   ms.author="dkshir"/>

# 在 Azure 预览门户网站中创建一个完全限定的域名称

在虚拟机创建在[Azure 预览门户](https://portal.azure.com)使用**资源管理器**部署模型时，门户将创建公用 IP 资源的虚拟机。 此 IP 地址可用于远程访问虚拟机。 不过，门户不会创建一个[完全合格的域名称](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)或 FQDN，默认情况下。 因为它会更容易记住，而不是 IP 地址使用 FQDN，本文将演示如何添加到您的虚拟机。

本文假定您已登录到您的订阅在门户中，并使用可用的图像，使用**资源管理器**创建虚拟机。 一旦您的虚拟机开始运行，请执行以下步骤。

1.  在门户网站上查看虚拟机的设置，然后单击公用 IP 地址。

    ![查找 ip 资源](media/virtual-machines-create-fqdn-on-portal/locatePublicIP.PNG)

2.  **Dissociate**虚拟机从公用 IP。 请注意它不尚未显示域名称。 在您单击**是**按钮后，可能要花费几秒钟才完成 dissociation。

    ![取消关联 ip 资源](media/virtual-machines-create-fqdn-on-portal/dissociateIP.PNG)

    我们将在以下步骤之后与虚拟机关联此公用 IP。 如果公用 IP 是一个_动态的公共 IP_，，您将丢失的 IPV4 地址和一个新的 FQDN 配置后将被分配一个。

3.  一旦出用灰色的**Dissociate**按钮，单击公用 IP 的**所有设置**部分，并打开**配置**选项卡。 输入所需的 DNS 名称标签。 **保存**该配置。

    ![输入 dns 名称标签](media/virtual-machines-create-fqdn-on-portal/dnsNameLabel.PNG)

4.  请返回到门户中的虚拟机刀片并单击虚拟机的**所有设置**。 打开**网络接口**选项卡，然后单击与此虚拟机的网络接口资源。 这将在门户打开刀片式服务器**网络接口**。

    ![打开网络接口](media/virtual-machines-create-fqdn-on-portal/openNetworkInterface.PNG)

5.  请注意网络接口的**公用 IP 地址**字段为空。 单击该网络接口的**所有设置**部分，打开**IP 地址**选项卡。 **IP 地址**刀片式服务器，请单击**公用 IP 地址**字段的**启用**。 选择**IP 地址配置所需的设置**选项卡，然后选择过早取消关联的默认 IP。 单击**保存**。 它可能需要一些时间来重新添加 IP 资源。

    ![配置 IP 资源](media/virtual-machines-create-fqdn-on-portal/configureIP.PNG)

6.  关闭其他所有刀片式服务器，请返回到**虚拟机**刀片。 单击设置中的公用 IP 资源。 请注意，公共 IP 刀片现在表明需的 FQDN 的**DNS 名称**。

    ![创建 FQDN](media/virtual-machines-create-fqdn-on-portal/fqdnCreated.PNG)

    您可以立即远程连接到虚拟机使用该 DNS 名称。 例如，使用`SSH adminuser@testdnslabel.eastus.cloudapp.azure.com`，当连接到 Linux 虚拟机的完全合格的域名的名称`testdnslabel.eastus.cloudapp.azure.com`和用户名`adminuser`。
