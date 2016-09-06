---
ms.openlocfilehash: 90b1bed1dc9ac4f119a42333a07eff6331d1de21
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 自动化备份"
   description="介绍如何备份自动化客户的内容，以便删除自动化帐户后，可以保留它们。"
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/22/2015"
   ms.author="bwren" />

# Azure 自动化备份

删除 Microsoft Azure 中的自动化帐户时，帐户中的所有对象被都删除包括运行手册、 模块、 设置、 作业和资产。 该帐户被删除后将无法恢复对象。  以下信息可用于在删除之前备份自动化客户的内容。 

## 运行手册

您可以用 Windows PowerShell Azure 管理门户或[获取 AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) cmdlet 的脚本文件导出运行手册。  这些脚本文件可以导入到另一个自动化的帐户，如下所述[创建或导入 Runbook](https://msdn.microsoft.com/library/dn643637.aspx)。


## 集成模块

不能导出从 Azure 自动化的集成模块。  必须确保有可用自动化帐户之外。

## 资产

您不能导出从 Azure 自动化的[资产](https://msdn.microsoft.com/library/dn939988.aspx)。  使用 Azure 管理门户，您必须注意变量、 凭据、 证书、 连接和时间表的详细信息。  然后，您必须手动创建使用导入另一个自动化的运行手册的任何资产。

可以使用[Azure cmdlet](https://msdn.microsoft.com/library/dn690262.aspx)来检索详细信息的未加密的资产，然后将它们另存供将来参考或其他自动化帐户中创建等效的资产。

无法检索加密的变量或凭据使用 cmdlet 的密码字段的值。  如果您不知道这些值，然后可从它们使用[Get AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx)和[Get AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx)活动 runbook。

您不能从 Azure 自动化导出证书。  您必须确保任何证书可用在 Azure 之外。

## 相关的文章

- [创建或导入 Runbook](https://msdn.microsoft.com/library/dn643637.aspx)
- [自动化的资产](https://msdn.microsoft.com/library/dn939988.aspx)
- [Azure cmdlet](https://msdn.microsoft.com/library/dn690262.aspx)测试
