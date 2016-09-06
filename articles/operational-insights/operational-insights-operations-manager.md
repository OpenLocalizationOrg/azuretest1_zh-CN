---
ms.openlocfilehash: ef067f4b88aa6ab9eb1e045fd3d1d6cd9f7c67c0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="操作管理器注意事项与运营经验"
   description="如果使用 Microsoft Azure 运营洞察力和运营经理，然后配置依赖于操作管理器代理和管理组来收集和分析的运营洞察力服务发送数据的分布"
   services="operational-insights"
   documentationCenter=""
   authors="bandersmsft"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/02/2015"
   ms.author="banders" />

# 操作管理器注意事项与运营经验

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

如果使用 Microsoft Azure 运营洞察力和运营经理，您的配置依赖操作管理器代理和管理组来收集和分析的运营洞察力服务发送数据的分布。 但是，如果您使用直接连接到 web 服务的代理，然后您不需要运营经理。 请考虑以下问题时使用与操作管理器操作见解。

此外，您需要指定工作负荷的运行方式凭据监视操作的见解到我运营经理。

## 操作的见解软件功能和要求

操作的见解包含中云，并直接连接到 web 服务或代理运营经理和运营经理管理组管理您环境中的计算机的任何代理的 web 服务。

选择操作管理器代理 （收集数据） 和管理组 （以管理代理，并将数据发送到操作的见解服务） 之前，请确保您了解以下︰

- 任何想要收集和分析数据的服务器上安装操作管理器代理。

- 运营经理管理组将数据从代理传输到运营洞察力的 web 服务。 它不会分析所有数据。

- 运营经理管理组必须具有访问 Internet，以将数据上载到 web 服务。

- 为了获得最佳结果，不要也是域控制器的计算机上安装操作管理器管理服务器。

- 操作管理器代理程序必须具有网络连接的运营经理管理组，以便它可以传输数据。

操作的见解可以使用系统中心健康服务来收集和分析数据。 运营经理取决于系统中心的健康服务。 查看您的服务器安装程序时，您将看到系统中心操作管理器代理软件，特别是在添加/删除程序。 因为运营的见解是依赖于它们，不要删除它们。 如果您删除操作管理器代理软件，将不再正常运营的见解。

## 共存与运营经理

当使用运营经理，运营洞察力的支持仅限于系统中心操作管理器 2012 R2 或系统中心操作管理器 2012 SP1 中的操作管理器代理。 与以前版本的系统中心操作管理器不支持它。 操作管理器代理程序用于收集数据，因为它使用特定的凭据 （操作帐户或运行方式帐户），以支持其中一些分析工作负荷，如 SharePoint 2012。

## 操作的见解和 SQL Server 2012

当使用操作管理器，系统中心健康服务运行在本地系统帐户下。 在早于 SQL Server 2008 R2 SQL Server 版本中，本地系统帐户被默认启用，是系统管理员服务器角色的成员。 在 SQL Server 2012 年，本地系统登录不是系统管理员服务器角色的一部分。 因此，使用操作的见解时，它不能完全，监视 SQL Server 2012年实例，并不是所有的规则会生成警报。

## 互联网和内部网络连接

使用时直接与 web 服务连接的工程师，工程师将需要访问互联网将数据发送到 web 服务并从 web 服务接收更新的配置信息。

当使用运营经理，管理服务器需要访问互联网将数据发送到 web 服务并从 web 服务接收更新的配置信息。 但是，您的代理不需要访问互联网。 如果您未连接到 Internet 的服务器上有操作管理器代理，您可以使用 web 服务，如果他们可以与互联网连接管理服务器进行通信。

## 群集支持

在运行 Windows Server 2012，Windows Server 2008 R2 或被配置为进行 Windows 故障转移群集的一部分的 Windows Server 2008 的计算机支持的操作管理器代理。 您可以查看群集操作的见解门户中。 在服务器页上，群集被识别为类型 = 群集 (而不是类型 = 代理，即如何物理计算机标识)。

发现和配置规则的群集，主动和被动节点上运行，但在被动节点上生成的所有警报都将被都忽略。 如果一个节点将从被动转移到活动，通知该节点将自动，显示并且不需要从您的干预。

根据生成警报的规则，可能两次，生成一些预警。 例如，通过检查操作系统检测驱动程序错误的规则将生成警报为物理服务器和群集。

不支持的被动节点的配置分析。

操作的见解不支持分组或链接的计算机运行 Windows Server 相同的故障转移群集的一部分。

## 缩放操作的见解环境

当计划运营洞察力部署 （尤其是当您分析您要传输的数据通过单个管理组的操作管理器代理的数目） 时，考虑在文件空间方面该服务器的容量。

请考虑以下变量︰

- 每个管理组的代理数

- 数据的平均大小从代理传输到每日管理组。 默认情况下，每个代理将 CAB 文件上载到管理组每天两次。 CAB 文件的大小取决于配置的服务器 （如 SQL Server 引擎和数字数据库的数量） 和服务器 （如生成的通知数） 的健康状况。 在大多数情况下，每日上载大小通常是小于 100 KB。

- 保持数据的管理组中 （默认为 5 天） 的存档时间

例如，如果假定每个代理和默认存档期，100 KB 每日上载大小您将需要以下存储管理组︰

代理数|预计的管理组所需的空间
---|---
5|~2.5 MB (5 代理 x 100 KB 数据每天 x 5 天 = 2500 KB)
50|大约为 25 MB (50 代理 x 100 KB 数据每天 x 5 天 = 25000 KB)

## 操作管理器运行方式帐户操作的见解

运营的见解使用操作管理器代理和管理组来收集并将数据发送到操作的见解服务。 根据工作负载提供的管理包的运营洞察力生成的增值服务。 每个工作负载需要在不同的安全上下文中，例如域帐户运行管理包的特定工作负荷的特权。 您需要通过配置操作管理器运行方式帐户提供凭据信息。

以下各节介绍如何设置操作管理器运行方式帐户下的工作负载︰

- SQL 评估
- Virtual Machine Manager
- Lync 服务器
- SharePoint 

### 设置 SQL 评估的运行方式帐户

 如果您已经在使用 SQL Server 管理包，您应使用该运行方式帐户。

#### 若要在操作控制台中配置的 SQL 运行方式帐户

1. 操作管理器在操作控制台中，打开，然后单击**管理**。

2. 在**运行配置**中，单击**配置文件**，并打开**操作的见解 SQL 评估作为配置文件运行**。

3. 在**运行方式帐户**页上，单击**添加**。

4. 选择包含所需的 SQL Server 的凭据的 Windows 运行方式帐户，或者单击**新建**以创建一个。
    >[AZURE.NOTE] 运行方式帐户类型必须是 Windows。 运行方式帐户还必须是承载 SQL Server 实例的所有 Windows 服务器上的本地管理员组的一部分。

5. 单击**保存**。

6. 修改，然后在运行方式帐户来执行 SQL 评估所需的最小权限授予每个 SQL Server 实例上执行下面的 T SQL 示例。 但是，您不需要这样做，如果运行方式帐户已经是 SQL Server 实例上系统管理员服务器角色的一部分。

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### 若要配置 SQL 运行方式帐户使用 Windows PowerShell

打开一个窗口，PowerShell 和更新您的信息后，请运行下面的脚本︰

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"
     
    $profile = Get-SCOMRunAsProfile -DisplayName "Operational Insights SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```


### 设置为 Virtual Machine Manager 的运行方式帐户

确保运行方式帐户有权限执行以下操作︰

- 若要使用 VMM Windows PowerShell 模块

- VMM 数据库中查询

- 若要远程管理虚拟化主机上运行 VMM 代理

使用以下步骤设置帐户，当您连接到运营经理的运营经验。

#### 对于 VMM 设置凭据

1. 操作管理器在操作控制台中，打开，然后单击**管理**。

2. 在**运行配置**中，单击**配置文件**，并打开**操作的见解 VMM 运行方式帐户**。

3. 在**运行方式帐户**页上，单击**添加**。

4. 选择 Windows 运行方式帐户包含 VMM，所需的凭据，或单击**新建**以创建一个。
    >[AZURE.NOTE] 运行方式帐户类型必须是 Windows。

5. 单击**保存**。


### Lync 服务器设置的运行方式帐户

 运行方式帐户必须是本地管理员和 Lync RTCUniversalUserAdmins 安全组的成员。

#### 若要设置 Lync 帐户的凭据

1. 操作管理器在操作控制台中，打开，然后单击**管理**。

2. 在**运行配置**中，单击**配置文件**，并打开**操作的见解 Lync 运行方式帐户**。

3. 在**运行方式帐户**页上，单击**添加**。

4. 选择一个 Windows 运行方式帐户是本地管理员和 Lync RTCUniversalUserAdmins 安全组的成员。
    >[AZURE.NOTE] 运行方式帐户类型必须是 Windows。

5. 单击**保存**。


### 设置 SharePoint 的运行方式帐户


#### 若要设置 SharePoint 帐户的凭据

1. 操作管理器在操作控制台中，打开，然后单击**管理**。

2. 在**运行配置**中，单击**配置文件**，并打开**操作的见解 SharePoint 运行方式帐户**。

3. 在**运行方式帐户**页上，单击**添加**。

4. 选择 Windows 运行方式帐户包含 SharePoint，所需的凭据，或单击**新建**以创建一个。
    >[AZURE.NOTE] 运行方式帐户类型必须是 Windows。

5. 单击**保存**。



## 地理位置

如果您希望从位于不同地理位置的服务器的数据进行分析，考虑每个位置有一个管理组。 这可以提高从代理到管理组的数据传输的性能。
