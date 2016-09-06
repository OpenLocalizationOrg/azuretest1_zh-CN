---
ms.openlocfilehash: 7ebcfc313ab474977a61fc5eaceb4fa05ce85d43
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="如何在虚拟的网络中使用公用 IP 地址"
   description="了解如何配置要使用公用 IP 地址的虚拟网络"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carolz"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/08/2015"
   ms.author="telmos" />

# 虚拟网络 (VNet) 中的公用 IP 地址空间

您现在可以为您 VNets 添加公用 IP 地址空间。 以前，您只能添加 RFC 1918 地址块 （专用空间） 到您的 VNets。 当您添加一个公用 IP 地址范围时，它将被视为才可到达 VNet，相互连接 VNets，和内部位置的专用 VNet IP 地址空间的一部分。

添加一个公用 IP 地址空间如下概念︰

![公共 IP 的概念](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## 如何添加公用 IP 地址范围内？

将一个公用 IP 地址范围添加相同的方式添加专用的 IP 地址范围;通过使用*netcfg*文件，或在门户中进行配置。 当您创建您的 VNet，或您可以返回并随后将其添加，您可以添加一个公用 IP 地址范围。 下面的示例显示在同一个虚拟的网络中配置公用和专用 IP 地址空间。

![在门户网站中的公用 IP 地址](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## 是否有任何限制？

有几个不允许使用的 IP 地址范围︰

- 224.0.0.0/4 （多播）

- 255.255.255.255/32 （广播）

- 127.0.0.0/8 （环回）

- 169.254.0.0/16 （链路本地）

- 68.63.129.16/32 (内部 DNS)

## 下一步行动

[如何管理虚拟网络 (VNet) 属性](../virtual-networks-settings)

[如何管理 DNS 服务器使用的虚拟网络 (VNet)](../virtual-networks-manage-dns-in-vnet)

[如何删除虚拟网络 (VNet)](../virtual-networks-delete-vnet) 