---
ms.openlocfilehash: 541e47296b98b75dd0913d16000aae6c41082c2a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties authors="KBDAzure" editor="tysonn" manager="timlt" />

在服务管理中创建的虚拟机总是放在云服务。 云服务作为一个容器，并提供唯一的公共 DNS 名称、 公用 IP 地址，以及一组终结点来访问 Internet 上的虚拟机。 云服务 （可选） 可以在虚拟的网络中。

如果云服务不在虚拟的网络，它具有称为*独立*的云服务。 独立的云服务中的虚拟机可以只与其他虚拟机通信使用的其他虚拟机的公共 DNS 名称，并通过 Internet 传送的通信量。 如果云服务是在虚拟网络中，虚拟机，与虚拟的网络中的所有其他虚拟机进行通信的云服务可以无需通过互联网发送任何通信。

如果将虚拟机放置在同一个独立的云服务，您可以仍然使用负载平衡和可用性设置。 有关详细信息，请参阅[负载平衡虚拟机](../articles/load-balance-virtual-machines.md)和[虚拟机的可用性管理](../articles/manage-availability-virtual-machines.md)。 但是，不能组织子网中的虚拟机，或连接到内部网络的独立的云服务。 下面是一个示例︰

![独立的云服务中的虚拟机](./media/howto-connect-vm-cloud-service/CloudServiceExample.png)

如果您的虚拟机将在虚拟的网络中，您可以决定多少云服务要使用的负载平衡和可用性。 此外，可以以相同的方式组织子网中的虚拟机为您的内部网络和虚拟网络连接到内部网络。 下面是一个示例︰

![在虚拟网络中的虚拟机](./media/howto-connect-vm-cloud-service/VirtualNetworkExample.png)

虚拟网络是推荐的方式连接在 Azure 中的虚拟机。 最佳实践是将每一层的应用程序配置单独的云服务中。 但是，您可能需要合并相同的云服务，保持 200 的云服务，每个订阅的最大值在不同应用程序层中某些虚拟机。 若要查看此和其他限制，请参阅[Azure 订阅和服务限制、 配额和限制](../azure-subscription-service-limits.md)。

## 在虚拟网络中连接的虚拟机

连接虚拟网络中的虚拟机︰

1.  在[Azure 门户网站](http://manage.windowsazure.com)中创建虚拟网络。 有关详细信息，请参阅[虚拟网络配置任务](../documentation/services/virtual-machines/)。
2.  创建的一套反映对于可用性集设计和负载平衡部署的云服务。 在门户中，请单击**新建 > 计算 > 云服务 > 创建自定义**为每个云服务。
3.  若要创建每个新的虚拟机，请单击**新建 > 计算 > 虚拟机 > 从库**。 为虚拟机选择正确的云服务和虚拟网络。 如果云服务已经加入到虚拟网络，它的名称已被选中为您。

![选择虚拟机的云服务](./media/howto-connect-vm-cloud-service/VMConfig1.png)

## 将虚拟机连接独立的云服务中

要连接独立的云服务中的虚拟机︰

1.  在[Azure 门户网站](http://manage.windowsazure.com)中创建的云服务。 单击**新 > 计算 > 云服务 > 创建自定义**。 或者，当您创建您的第一个虚拟机，您可以创建用于部署的云服务。
2.  当创建虚拟机时，请选择在上一步中创建的云服务的名称。
![将虚拟机添加到现有的云服务](./media/howto-connect-vm-cloud-service/Connect-VM-to-CS.png)

##资源
[负载平衡虚拟机](../articles/load-balance-virtual-machines.md)

[管理虚拟机的可用性](../articles/manage-availability-virtual-machines.md)

[虚拟网络配置任务](../documentation/services/virtual-machines/)

创建虚拟机后，最好添加数据磁盘，因此您的服务和工作负载具有一个位置来存储数据。 请参阅下列项之一︰

[如何附加到 Linux 虚拟机的数据磁盘](../articles/virtual-machines/virtual-machines-linux-how-to-attach-disk.md)

[如何附加到 Windows 虚拟机的数据磁盘](../articles/virtual-machines/storage-windows-attach-disk.md)
