---
ms.openlocfilehash: fedafb5ae14bced64ad13e7c17a945a86a459b50
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="Azure 的自动化数据保留"
   description="介绍的 Azure 自动化数据保留策略。"
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

# Azure 的自动化数据保留

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

## 相关的文章
- [备份 Azure 自动化](https://msdn.microsoft.com/library/dn643635.aspx)测试
