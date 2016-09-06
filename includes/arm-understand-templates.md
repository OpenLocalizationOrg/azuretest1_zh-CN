---
ms.openlocfilehash: 03eb4843c0c86567397b49e00756e0554bb9c533
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 了解模板 Azure 的资源和资源组

大多数应用程序的部署和运行 Microsoft Azure 中会生成不同的云资源类型的组合 (例如一个或多个虚拟机和存储帐户、 SQL 数据库、 一个虚拟网络，CDN，等等)。  [Azure 资源管理器模板](../resource-group-authoring-templates.md)使您可以部署和管理这些不同的资源一起使用 JSON 的资源关联的配置和部署参数说明。

一旦您定义了一个 JSON 基于的资源模板，您可以执行，已在其中定义的资源部署在 Azure 使用 PowerShell 命令。  您可以运行此 PowerShell 命令内 PowerShell 命令外壳程序，无论它们是单机或 PowerShell 脚本包含其他自动化逻辑内部集成它。

您使用 Azure 资源管理器模板创建的资源将被部署到新的或现有的 Azure 资源组。  Azure 资源组可以管理多个已部署的资源组合在一起作为一个逻辑组。 通常情况下，一组将包含在特定应用程序相关的资源。  Azure 资源组提供管理组/应用程序的整个生命周期，并提供管理 Api，使您可以停止、 启动或删除所有组中的资源同时应用角色基于访问控制 (RBAC) 规则，来锁定它们、 审核操作，以及使用附加的元数据以进行更好的跟踪标记资源的安全权限的方法。 若要了解有关 Azure 资源组的详细信息，请参阅[Azure 资源管理器的概述](https://azure.microsoft.com/documentation/articles/resource-group-overview/)。 

下面的自动化示例演示如何使用 Azure 资源管理器模板和部署使用 PowerShell 或 CLI 的资源组。
