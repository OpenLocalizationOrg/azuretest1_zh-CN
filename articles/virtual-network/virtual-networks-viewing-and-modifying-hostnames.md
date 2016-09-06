---
ms.openlocfilehash: e07c68e5462eb8423922f53463d796219b966926
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="查看和修改主机名 |Microsoft Azure"
   description="如何查看和更改 Azure 的虚拟机的主机名、 网络和辅助角色名称解析"
   services="virtual-network"
   documentationCenter="na"
   authors="joaoma"
   manager="jdial"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/25/2015"
   ms.author="joaoma" />

# 查看和修改主机名

若要允许您的角色实例将引用的主机名，必须在每个角色的服务配置文件中设置主机名称的值。 通过将所需的主机名添加到**角色**元素的**vmName**属性来做到这一点。 **VmName**属性的值用于根据每个角色实例的主机名。 例如，如果**vmName**是*webrole* ，并且该角色的三个实例中， *webrole0*、 *webrole1*和*webrole2*将是的实例的主机名。 您不需要在配置文件中，指定用于虚拟机的主机名因为虚拟机的主机名基于虚拟机的名称填充。有关配置 Microsoft Azure 服务的详细信息，请参阅[Azure 服务配置架构 (.cscfg 文件)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## 查看主机名

您可以通过使用各种工具查看在云服务中的虚拟机和角色实例的主机名︰ Azure 的门户，服务配置文件，远程桌面和[Azure 服务管理 REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)。

### Azure 门户

您可以使用 Azure 门户虚拟机仪表板页面上查看虚拟机的主机名。 请记住，仪表板可以显示一个值**名称**和**主机名**。 尽管他们最初都相同，但更改的主机名称不会更改虚拟机或角色实例的名称。

角色实例同样位于 Azure 的入口，但当列表中云服务的实例时，将不会显示主机名。 您将看到每个实例的名称，但该名称不代表的主机名。

### 服务配置文件

您可以从**配置**页中的 Azure 门户的服务下载的已部署服务的服务配置文件。 然后可以查看主机名，请参阅**角色名称**元素的**vmName**属性。 请记住该主机名称用于每一角色实例的主机名为基础。 例如，如果**vmName**是*webrole* ，并且该角色的三个实例中， *webrole0*、 *webrole1*和*webrole2*将是的实例的主机名。

### 远程桌面

启用远程桌面 (Windows)、 Windows PowerShell 远程处理 (Windows) 或 SSH （Linux 和 Windows） 连接到您的虚拟机或角色实例后，您可以以各种方式查看从一个活动的远程桌面连接的主机名︰

- 在命令提示符下或 SSH 终端中输入主机名。

- 键入 ipconfig/所有 at 命令提示 (仅限 Windows)。

- 查看系统设置 (仅限 Windows) 中的计算机名称。

### Azure 服务管理 REST API，

从其他客户端，请按照以下说明︰

1. 请确保您已连接到 Azure 门户的客户端证书。 若要获取客户端证书，请按照中介绍的步骤[如何︰ 下载并导入发布设置和订阅信息](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)。

1. 设置一个名为 x-ms-版本值为 2013年-11-01 的标头项。

1. 发送请求以下面的格式︰ https://management.core.windows.net/\<subscrition id\>/服务/hostedservices/\<服务名称\>?embed-详细信息 = true

1. 查找**主机名** **RoleInstance**的每个元素的元素。

>[AZURE.WARNING] 您可以在远程桌面会话 (Windows) 或通过从 SSH 终端 (Linux) 运行 cat /etc/resolv.conf 为云服务的其余部分调用响应通过检查**InternalDnsSuffix**元素，或者通过运行 ipconfig/从命令提示符处查看内部域后缀。

## 修改主机名

通过上传修改的服务配置文件，或重命名计算机与远程桌面会话，可以修改任何虚拟机或角色实例的主机名。

## 下一步行动

[名称解析 (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure 服务配置架构 (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure 的虚拟网络配置架构）](http://go.microsoft.com/fwlink/?LinkId=248093)

[指定 DNS 设置使用网络配置文件](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
