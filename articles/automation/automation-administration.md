---
ms.openlocfilehash: 44cfece64e56a611b6918a70617883e92e764338
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 的自动化管理"
   description="这篇文章包含管理 Azure 自动化环境的多个主题。  目前包括数据保留和备份 Azure 自动化。"
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
   ms.date="07/06/2015"
   ms.author="bwren" />

# Azure 的自动化管理

这篇文章包含管理 Azure 自动化环境的多个主题。

## 数据保留

当您删除 Azure 自动化中的资源时，则将其保留为审计目的在被永久删除之前的 90 天。  无法看到或使用该资源，在这段时间。  此政策还适用于属于自动化帐户被删除的资源。

Azure 自动化自动删除并永久删除作业时间超过 90 天。

下表总结了不同资源的保留策略。

|Data|策略|
|:---|:---|
|帐户|永久删除此帐户删除用户后的 90 天。|
|资产|永久删除资产删除一个用户，通过后的 90 天或 90 天后持有资产被删除的用户帐户。|
|模块|永久删除模块删除一个用户，通过后的 90 天或 90 天后保存模块删除用户帐户。|
|运行手册|永久删除资源删除一个用户，通过后的 90 天或 90 天后的帐户包含由用户删除的资源。|
|作业|已删除并永久删除 90 天后最后被修改。 这可能是作业完成、 已停止或暂停后。|

保留策略应用于所有用户，当前不能自定义。

## Azure 自动化备份

删除 Microsoft Azure 中的自动化帐户时，帐户中的所有对象被都删除包括运行手册、 模块、 设置、 作业和资产。 该帐户被删除后将无法恢复对象。  以下信息可用于在删除之前备份自动化客户的内容。 

### 运行手册

您可以用 Windows PowerShell Azure 管理门户或[获取 AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) cmdlet 的脚本文件导出运行手册。  这些脚本文件可以导入到另一个自动化的帐户，如下所述[创建或导入 Runbook](https://msdn.microsoft.com/library/dn643637.aspx)。


### 集成模块

不能导出从 Azure 自动化的集成模块。  必须确保有可用自动化帐户之外。

### 资产

您不能导出从 Azure 自动化的[资产](https://msdn.microsoft.com/library/dn939988.aspx)。  使用 Azure 管理门户，您必须注意变量、 凭据、 证书、 连接和时间表的详细信息。  然后，您必须手动创建使用导入另一个自动化的运行手册的任何资产。

可以使用[Azure cmdlet](https://msdn.microsoft.com/library/dn690262.aspx)来检索详细信息的未加密的资产，然后将它们另存供将来参考或其他自动化帐户中创建等效的资产。

无法检索加密的变量或凭据使用 cmdlet 的密码字段的值。  如果您不知道这些值，然后可从它们使用[Get AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx)和[Get AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx)活动 runbook。

您不能从 Azure 自动化导出证书。  您必须确保任何证书可用在 Azure 之外。


测试
