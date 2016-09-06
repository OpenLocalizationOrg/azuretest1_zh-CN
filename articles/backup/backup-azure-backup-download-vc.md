---
ms.openlocfilehash: 7f55a5ed222c28e8e682dc2b96f6c86d72765f18
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="下载在 Azure 备份中的存储库凭据 |Microsoft Azure"
   description="了解如何使用存储库凭据来验证您的计算机的备份存储库和 Azure 备份服务"
   services="backup"
   documentationCenter=""
   authors="Jim-Parker"
   manager="shreeshd"
   editor=""/>
<tags
   ms.service="backup"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="storage-backup-recovery"
   ms.date="08/11/2015"
   ms.author="jimpark"; "aashishr"/>

# 使用存储库凭据进行身份验证的 Azure 备份服务
内部部署服务器 （Windows 客户端或 Windows 服务器或 SCDPM 服务器） 需要进行身份验证备份存储库之前，它可以将数据备份到 Azure。 身份验证是使用"保险存储的凭据"来实现的。 保险存储的概念是凭据的类似于"发布设置"文件用于在 Azure PowerShell 的概念。

## 什么是保险存储凭据文件？
存储库凭据文件是生成的每个备份的存储库的门户网站的证书。 实现访问控制服务 （ACS），门户然后上载中的公共密钥。 该证书的私钥可供作为输入提供计算机注册工作流中的流中的用户。 这对将备份数据发送到备份 Azure 服务中识别存储库的计算机身份验证。

值得指出，只在登记工作流过程中使用存储库凭据。 它是用户的责任，以确保存储库凭据文件不会受到影响。 如果它落在任何恶意用户的手中，可以使用存储库凭据文件注册其他计算机免受同一存储库。 但是，随着使用属于客户的密码加密备份数据，不能破坏现有备份数据。 为了缓解这一问题，保险存储凭据设置的过期时间 48hrs。 可以下载的备份存储库存储库凭据任意数量的时间 – 但只有最新的存储库凭据文件适用登记工作流过程。

## 存储库凭据文件下载
存储库凭据文件是通过安全通道从 Azure 的门户网站下载。 Azure 备份服务没有意识到该证书的专用密钥和专用密钥不保留在门户网站或服务。 使用以下步骤来下载到本地计算机中存储库凭据。

1.  登录到[管理门户](https://manage.windowsazure.com/)
2.  单击左侧的导航窗格中的**恢复服务**，然后选择您创建的备份存储区。
3. 单击云图标，以转到备份存储区的快速启动视图。

    ![快速查看](./media/backup-azure-backup-download-vc/quickview.png)

4.  在快速启动页上，单击**下载存储库凭据**。 门户网站生成存储库凭据文件成为可供下载。

    ![下载](./media/backup-azure-backup-download-vc/downloadvc.png)

5.  门户将生成存储库凭据使用电子仓库名称和当前日期的组合。 单击**保存**将下载到本地帐户的下载文件夹，存储库凭据或另存为存储库凭据指定位置保存菜单中选择。

## 注释
- 确保存储库凭据保存在可通过您的计算机的位置。 如果它存储在文件共享和 SMB，检查访问权限。
- 存储库凭据文件只在登记工作流使用。
- 存储库凭据文件过后 48hrs，可以从门户网站上下载。
- 请参阅工作流上的任何问题 Azure 备份的[常见问题解答](backup-azure-backup-faq.md)。

##视频的演练

这是本教程的视频演练。

[AZURE.VIDEO using-vault-credentials-to-authenticate-with-the-azure-backup-service]

## 下一步行动
[下载、 注册和安装 Azure 备份代理](backup-azure-backup-download-register)

测试
