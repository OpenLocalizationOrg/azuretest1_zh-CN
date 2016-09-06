---
ms.openlocfilehash: 7008791e76bcc21a0fc5d63e80decc6c89567819
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="将 Azure 备份服务准备回了 Windows 服务器的配置 |Microsoft Azure"
    description="使用本教程来学习如何在微软 Azure 云提出要备份 Windows 服务器到云环境中使用的备份服务。"
    services="backup"
    documentationCenter=""
    authors="Jim-Parker"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/21/2015"
    ms.author="jimpark"; "aashishr"/>

# Azure 备份准备回了 Windows 服务器的配置

这篇文章将引导您完成启用的 Azure 备份功能。 要到 Azure 备份 Windows 服务器或 Windows 客户端，您需要一个 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
>[AZURE.NOTE] 以前，您需要创建或获取 X.509 v3 证书，以便注册您的备份服务器。 仍然支持证书，但现在为便于服务器注册 Azure 存储库，您可以生成存储库凭据直接从快速启动页。

## 在开始之前
要备份的文件和数据从您的 Windows 服务器到 Azure，您必须首先︰

- **创建备份存储库**— — 在 Azure 备份控制台中创建一个电子仓库。
- **下载存储库凭据**— 在 Azure 备份中，上载到存储库创建的管理证书。
- **Azure 备份代理程序安装和注册服务器**— 从 Azure 备份，安装代理，在备份存储库中注册服务器。

[AZURE.INCLUDE [backup-create-vault-wgif](../../includes/backup-create-vault-wgif.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## 下一步行动
- [备份 Windows 服务器或 Windows 客户端](backup-azure-backup-windows-server.md)
- [管理 Windows 服务器或 Windows 客户端](backup-azure-manage-windows-server.md)
- [从 Azure 还原 Windows 服务器或 Windows 客户端](backup-azure-restore-windows-server.md)
- [Azure 备份常见问题解答](backup-azure-backup-faq.md)
- [Azure 备份论坛](http://go.microsoft.com/fwlink/p/?LinkId=290933)

测试
