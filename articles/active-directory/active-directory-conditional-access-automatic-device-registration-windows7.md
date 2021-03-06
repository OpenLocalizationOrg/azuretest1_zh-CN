---
ms.openlocfilehash: 042a109a222f2cf0059f28afb33065d40f50ab17
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="# 配置设备自动注册 Windows 7 加入域设备 |Microsoft Azure"
    description="配置 Windows 7 域的步骤加入设备自动注册 Azure 的广告。 然后将设备注册软件程序包部署到您的 Windows 7 域的步骤联接使用软件分发系统系统中心配置管理器等设备。"
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2015"
    ms.author="femila"/>

# 配置 Windows 7 加入域设备的设备自动注册

作为一名 IT 管理员，您可以配置 Windows 7 加入域设备自动注册 Azure 的广告。 要这样做，您必须将设备注册软件程序包部署到 Windows 7 域联接使用软件分发系统系统中心配置管理器等设备。 请务必仔细阅读并完成自动设备注册了 Azure Active Directory for Windows Domain-Joined 设备中列出的前提条件。

##在 Windows 7 上安装设备注册软件程序包加入域的设备

Windows 7 的设备注册是可以作为可下载的 MSI 程序包。 加入到 Active Directory 域的 Windows 7 计算机上必须安装该程序包。 应部署包使用软件分发系统系统中心配置管理器等。 MSI 程序包支持使用 /quiet 标准无提示安装选项参数。
软件包是 Microsoft 连接网站上下载。 在此处可以选择然后下载 Windows 7 的工作场所加入。

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## Azure 的 Active Directory 联接的工作场所
Windows 7 加入域设备的设备注册并不要求或包含用户界面。 一旦安装在机器上，任何登录到该计算机的域用户，将自动地默默使用注册设备对象在 Azure 的广告。 在 Azure AD 为物理设备的每个已注册用户都有一个设备对象。

安装程序在用户的上下文中运行，并会触发用户登录的系统上创建计划的任务。 该任务自动注册用户和 Azure 广告后用户信号中具有设备已完成。 计划任务可以找到在任务计划程序库中下**Microsoft** > **联接的工作场所**。
该任务将运行并注册所有活动目录的用户登录到计算机。
下图列出了设备自动注册的分步过程。

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. 对 Windows 7 客户机使用 Active Directory 域凭据登录的用户 （信息工作人员）。
1. 执行工作场所加入计划的任务。
1. AD FS 使用 Windows 集成身份验证是以静默方式验证用户。
1. Windows 7 电脑用户注册 Azure AD 中。
1. 在 Azure AD 中创建设备对象和证书。 User@device 表示的对象。
1. 工作场所加入证书存储在计算机上。

## 注销您的 Windows 7 域加入设备

您可以通过执行以下注销您加入域的 Windows 7 设备︰ 卸载工作场所加入软件包从 Windows 7 域加入使用软件分发系统系统中心配置管理器等设备。

然后，打开 Windows 7 计算机上的命令提示符下执行以下命令以取消注册设备︰
    
    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>到计算机，必须在已签名的每个域用户的上下文中运行该命令。
事件查看器和 Windows 7 的错误的加入域设备。

Windows 7 计算机上的 Windows 事件日志将显示与工作场所加入相关的消息。 成功和不成功加入工作区的事件，可以查找邮件。 可以在事件找到应用程序和服务日志查看器事件日志 > Microsoft 工作场所加入。
测试
