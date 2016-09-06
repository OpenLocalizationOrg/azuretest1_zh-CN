---
ms.openlocfilehash: 2bec39b143d17dd446f74586655710416aeb20b9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 的远程应用程序最佳做法"
    description="配置和使用 Azure 远程应用程序的最佳做法。"
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
    ms.date="08/12/2015" 
    ms.author="elizapo" />

# 配置和使用 Azure 远程应用程序的最佳做法

以下信息可以帮助您配置和高效使用 Azure 远程应用程序。

## 连接


- 始终使用最新的客户端版本。 使用旧的客户端可能会导致连接问题和其他降级的体验。 启用应用程序自动更新为您的设备将确保始终安装了最新的客户端。
- 始终使用最稳定、 最可靠的 internet 连接可用。  
- 仅支持使用代理服务器连接的最佳连接的性能。  不支持的 SOCKS 代理。

## 应用程序


- 保存并关闭远程应用程序的应用程序，当您完成该应用程序。 如果不关闭该应用程序可能会导致数据丢失。
- 在 Azure 远程应用程序中使用它们之前验证自定义应用程序。 这包括确保它们多会话平台工作并不会消耗不必要的资源，如内存和 CPU，可能会使在同一个集合中的其他用户。 有关信息，下载并查看[远程桌面服务的应用程序兼容性最佳实践](http://www.microsoft.com/download/details.aspx?id=18704)。

## 配置和管理


- 保存模板图像的最新动态，根据需要安装的软件更新和其他关键的修补程序。 这将确保随着 Azure RemoteApp 自动-可扩展以满足您的容量，每个实例进行修补。  
- 请确保部署 Active Directory 联合身份验证服务 (AD FS) 是安全、 可靠。 否则客户端身份验证可能失败，阻止用户访问 Azure 远程应用程序。
- 与安装的应用程序、 角色或功能配置模板图像，以便它们是无状态的。 它们不应依赖持久状态的远程应用程序服务中的虚拟机的所有实例。
    - 如内部文件共享或 OneDrive，所有的用户数据存储在用户配置文件或外部服务，其他存储位置。
    - 存储区中的共享数据存储位置的外部服务，如本地文件共享或 OneDrive。
    - 模板映像中，而不是在单个服务中的虚拟机上配置任何系统范围的设置。
    - 禁用发布的应用程序的自动软件更新-改为手动应用于模板图像，并从模板部署之前对其进行测试。
 