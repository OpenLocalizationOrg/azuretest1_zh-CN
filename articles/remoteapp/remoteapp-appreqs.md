---
ms.openlocfilehash: 3b4a5d65b22f47201ce3da1619d0a2f25b661eed
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="Azure 远程应用程序的应用程序要求"
    description="了解有关您想要在 Azure 远程应用程序中使用的应用程序的要求" 
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



# 应用程序要求
Azure RemoteApp 支持流 32 位或 64 位的基于 Windows 的应用程序从 Windows Server 2012 R2 映像。 大多数现有 32 位或 64 位的基于 Windows 的应用程序运行在 Azure 远程应用程序"按原样"(远程桌面服务或以前称为终端服务) 的环境。 但是，没有运行，并且能够正常运行之间的区别-有些应用程序正常运行并执行，而其他人则不能。 以下信息提供指南远程桌面服务环境中的应用程序开发和测试，以确保兼容性。

提示︰ 我们正在创建您的某些应用程序的示例。 您将看到在远程应用程序中使用 Microsoft Access QuickBooks，以及 APP-V 讨论的新主题。

## 要求
下列三项要求，如果得到遵守，可以帮助您很好地在远程应用程序中运行的应用程序︰ 

1.  满足所有[Windows 桌面应用程序认证要求](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx)和遵守[远程桌面服务编程原则](https://msdn.microsoft.com/library/aa383490.aspx)的应用程序必须与远程应用程序的完全兼容。 
2.  应用程序应永远不会将数据存储本地远程应用程序实例，则可能会丢失的图像上。  创建远程应用程序集合后，则克隆是无状态的实例并将其应只包含应用程序。 将数据存储在外部源中或在用户的配置文件。 
3.  自定义图像不应包含数据，则可能会丢失。  

## 测试您的应用程序
使用以下步骤来测试应用程序︰

1.  安装 Windows Server 2012 R2 和您的应用程序
2.  启用远程桌面
3.  创建两个用户帐户，用户 a 和用户 b，这两个用户帐户添加到远程桌面的安全组。 
4.  检查通过建立两个同时 RDS 会话到计算机时启动的应用程序的多会话的兼容性。
5.  验证应用程序行为

## 应用程序开发指南
开发远程应用程序的应用程序中使用以下准则。 

### 多个用户
 
- 安装[单个用户的应用程序](https://msdn.microsoft.com/library/aa380661.aspx)可以在多用户环境中创建的问题。 
- 应用程序应在特定于用户的位置，与适用于所有用户的全局信息分开[存储用户特定的信息](https://msdn.microsoft.com/library/aa383452.aspx)。 
- 远程应用程序使用多个[命名空间的内核对象](https://msdn.microsoft.com/library/aa382954.aspx);主要由在客户端/服务器应用程序服务使用一个全局命名空间。 
- 不可以安全地假定的计算机名称或[IP 地址](https://msdn.microsoft.com/library/aa382942.aspx)指派给该计算机是与某一单个用户因为多个用户可以登录同时到远程桌面会话主机 （RD 会话主机） 服务器。 

### 表现
- 将您的应用程序添加到远程应用程序之前，请禁用[图形效果](https://msdn.microsoft.com/library/aa380822.aspx)。
- 若要最大限度地对所有用户可用的 CPU 资源，请禁用[后台任务](https://msdn.microsoft.com/library/aa380665.aspx)或创建不是资源密集型的有效的后台任务。 
- 应该调整和平衡应用程序[线程使用](https://msdn.microsoft.com/library/aa383520.aspx)多用户、 多处理器环境的。
- 若要优化性能，最好到[检测](https://msdn.microsoft.com/library/aa380798.aspx)应用程序是否运行在客户端会话。 
 
