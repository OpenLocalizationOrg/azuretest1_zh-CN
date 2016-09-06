---
ms.openlocfilehash: f14373c27e3c480068b09ae7b47c142bee5626ed
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 中运行 OpenSUSE Linux 虚拟机上安装 MySQL"
    description="了解如何在 Azure 中的虚拟机上安装 MySQL。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2015"
    ms.author="kathydav"/>

# 在 Azure 中运行 OpenSUSE Linux 虚拟机上安装 MySQL

[MySQL][MySQL]是一个受欢迎的、 开源的 SQL 数据库。 本教程展示如何创建一个运行 OpenSUSE Linux 虚拟机，然后安装 MySQL。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

## 创建一个运行 OpenSUSE Linux 虚拟机

[AZURE.INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## 安装并运行 MySQL 在虚拟机上

[AZURE.INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## 下一步行动
MySQL 的详细信息，请参阅[文档 MySQL][MySQLDocs]。

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com
[AzurePortal]: http://manage.windowsazure.com
