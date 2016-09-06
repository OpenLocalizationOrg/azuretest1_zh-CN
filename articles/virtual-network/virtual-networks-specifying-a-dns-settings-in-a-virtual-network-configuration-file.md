---
ms.openlocfilehash: 72c5e67f98a1c6d664cd6a26876b938ce4ea748f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在虚拟的网络配置文件中指定的 DNS 设置 |Microsoft Azure"
   description="如何更改使用虚拟网络配置文件的虚拟网络中的 DNS 服务器设置"
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

# 在虚拟的网络配置文件中指定的 DNS 设置

网络配置文件有两个元素，您可以使用指定域名系统 (DNS) 设置︰**那么**和**DnsServerRef**。 可以通过指定其 IP 地址和引用名称，**那么**元素添加 DNS 服务器的列表。 然后可以使用**DnsServerRef**元素来指定哪些 DNS 服务器条目从那么元素用于在虚拟网络中的不同的网络站点。

>[AZURE.IMPORTANT] 有关如何配置网络配置文件的信息，请参阅[配置虚拟网络使用网络配置文件](virtual-networks-using-network-configuration-file.md)。 有关网络配置文件中包含的每个元素的信息，请参阅[Azure 虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100.aspx)。

网络配置文件可能包含以下元素。 每个元素的标题链接到网页，提供有关元素的值设置的其他信息。

[Dns 元素](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] **DnsServer**元素中的**名称**属性用于仅作为参考**DnsServerRef**元素。 它不表示 DNS 服务器的主机名。 每个**DnsServer**属性值必须是唯一的在整个 Microsoft Azure 订阅

[虚拟网络站点元素](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] 要指定此设置虚拟网络站点元素，它必须以前定义的 DNS 元素中。 虚拟的网络站点元素中的 DnsServerRef*名称*必须引用 DnsServer*名称*的 DNS 元素中指定的名称值。

## 下一步行动

[配置虚拟网络使用网络配置文件](virtual-networks-using-network-configuration-file.md)

[Azure 的虚拟网络配置架构](http://go.microsoft.com/fwlink/?LinkId=248093)

[Azure 服务配置架构](https://msdn.microsoft.com/library/windowsazure/ee758710)
