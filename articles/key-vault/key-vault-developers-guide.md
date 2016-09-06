---
ms.openlocfilehash: b5f747b934e22b76ac8718c6e7a1ef2f215c9777
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="密钥存储库开发人员指南 》 |Microsoft Azure"
   description="开发人员可以使用 Azure 密钥存储库来管理 Microsoft Azure 环境内的加密密钥。 "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/11/2015"
   ms.author="mbaldwin" />

# Azure 的密钥存储库开发人员指南 》

> [AZURE.VIDEO azure-key-vault-developer-quick-start]

开发人员可以使用 Azure 密钥存储库来管理 Microsoft Azure 环境内的加密密钥。 密钥存储库支持多个密钥类型和算法，并对高价值客户密钥可以与硬件安全模块 (HSM) 一起使用。 此外，可以使用密钥存储库安全地存储机密，它是与任何特定的语义的规模有限的八位位组对象。 密钥存储库可以包含密钥和密码的组合。 单独管理的对象的类型的访问控制。

经过成功的授权，用户可以执行以下操作︰

- 管理加密密钥，使用[创建](https://msdn.microsoft.com/library/azure/dn903634.aspx)、[导入](https://msdn.microsoft.com/library/azure/dn903626.aspx)、[更新](https://msdn.microsoft.com/library/azure/dn903616.aspx)、[删除](https://msdn.microsoft.com/library/azure/dn903611.aspx)和其他操作

- 管理机密信息[获取](https://msdn.microsoft.com/library/azure/dn903633.aspx)，请使用 [更新] （https://msdn.microsoft.com/library/azure/dn986818.aspx、[删除](https://msdn.microsoft.com/library/azure/dn903613.aspx)以及其他操作

- 加密密钥使用[符号](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[验证](https://msdn.microsoft.com/library/azure/dn878082.aspx)、 [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx)和[加密](https://msdn.microsoft.com/library/azure/dn878060.aspx)/[解密](https://msdn.microsoft.com/library/azure/dn878097.aspx)操作

对密钥存储库的操作进行身份验证和授权使用 Azure 活动目录。

## 对密钥存储库进行编程

密钥存储库管理系统的程序员包含几个接口，与其余部分为基础。

### REST

REST API 是使用密钥存储库的所有编程交互的基础。

密钥存储库已在[密钥存储库 REST API 参考](https://msdn.microsoft.com/library/azure/dn903609.aspx)中描述自己 REST 端点

### .NET

.NET API 是一组的包装，允许通过 C# 实现编程模型，而无需直接与 REST 端点进行交互。 这里您可以找到[Azure 密钥存储库.NET 客户端 API 参考](https://msdn.microsoft.com/library/azure/dn903301.aspx)。

### Node.js

Node.js API 是一组包装，允许通过 JavaScript 编程模型的实现，而无需直接与 REST 端点进行交互。  这里您可以找到[Node.js-密钥存储库管理的 Microsoft Azure SDK](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest/)。

## 管理密钥存储库，使用 PowerShell 和 CLI

Azure 密钥存储库密钥和密码也可以进行管理使用 PowerShell 和 CLI，以下文章中所述︰

- [创建和管理密钥存储库使用 PowerShell](key-vault-get-started.md)
- [创建和管理密钥存储库使用 CLI](key-vault-manage-with-cli.md)
- [如何生成和转移的 Azure 的密钥存储库的 HSM 保护密钥](https://msdn.microsoft.com/library/azure/dn903624.aspx)
- [有关密钥和密码](https://msdn.microsoft.com/library/azure/dn903623.aspx)

## 请参见

- [Azure 的密钥存储库代码示例](http://www.microsoft.com/download/details.aspx?id=45343)

测试
