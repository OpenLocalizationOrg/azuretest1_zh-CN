---
ms.openlocfilehash: 20faf2af2b3bcd13f212073121e57e22c37efc45
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="在 Azure RemoteApp VNET 的大小信息"
    description="了解如何使用 VNET 运行的 Azure RemoteApp 的 IP 地址要求" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2015" 
    ms.author="elizapo" />



# 在 Azure RemoteApp VNET 的大小信息

与虚拟网络 (VNET) 使用 Azure 远程应用程序时，远程应用程序使用 IP 地址的子网中。 根据您的远程应用程序服务的比例，必须确保您的子网有足够可用的远程应用程序虚拟机的 IP 地址。 虽然本尺寸指南并非完美给定 RemoteApp 动态如何可起速和转速在一个集合中的虚拟机，它将帮助您估计您的子网范围。 这是相当重要，因为一旦远程应用程序服务放置在 VNET 中，您不能增加网大小而不会移除远程应用程序。

要以最大容量运行每个远程应用程序集合，您应有 100 中可用的 IP 地址。 例如，如果标准计划中有一个远程应用程序集合，并且要有最大的 500 个用户，您应有 100 IP 地址为该集合。 同样，100 IP 地址为远程应用程序集合中需要有 800 个用户的基本计划。 如果您计划让用户较少 （小于最大值），则可以减少每个集合所需的 IP 地址。 最小的子网大小要求是 30 个 IP 地址 （/ 27）。 

签出下面的信息以确保您的 VNET 是配置且工作 propertly:

- [将个人 VNET 迁移到 Azure VNET](remoteapp-migratevnet.md)
- [验证要使用 Azure RemoteApp Azure VNET](remoteapp-vnet.md)
  
 