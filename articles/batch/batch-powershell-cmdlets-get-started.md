---
ms.openlocfilehash: 77227d445208139b344ceb75676db596b51df3b3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="开始使用 Azure 批 PowerShell cmdlet |Microsoft Azure"
   description="介绍用于管理 Azure 批服务 Azure PowerShell cmdlet"
   services="batch"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="08/07/2015"
   ms.author="danlep"/>

# 开始使用 Azure 批 PowerShell cmdlet
这篇文章是快速了解到 Azure PowerShell cmdlet，您可以使用管理批次帐户和获取有关您的批处理作业、 任务，以及其他详细信息。

详细的 cmdlet 语法键入`get-help <Cmdlet_name>` [Azure 批 cmdlet 参考](https://msdn.microsoft.com/library/azure/mt125957.aspx)，请参阅或。

## 先决条件

* **Azure PowerShell** -请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)的先决条件和下载和安装说明。 版本 0.8.10 或更高版本中引入了批处理 cmdlet。 更新批处理 cmdlet 使用版本 0.9.6 中的一般可用性 API。

## 使用批处理 cmdlet

使用标准的过程启动 Azure PowerShell 和[连接到 Azure 的订阅](../powershell-install-configure.md#Connect)。 另外︰

* **选择 Azure 订阅**-如果您有多个订阅，请选择订阅︰

    ```
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

* **切换到 AzureResourceManage 模式**-批 cmdlet 装运 Azure 资源管理器模块中。 有关详细信息，请参阅[使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)。 若要使用此模块，请运行[开关 AzureMode](https://msdn.microsoft.com/library/dn722470.aspx) cmdlet:

    ```
    Switch-AzureMode -Name AzureResourceManager
    ```

* **注册与批次提供程序命名空间 （一次性操作）** -您可以管理您的批处理帐户之前，您必须注册与批次提供程序命名空间。 此操作只需执行一次每个订阅。

    ```
    Register-AzureProvider -ProviderNamespace Microsoft.Batch
    ```

## 管理批次帐户和键

Azure PowerShell cmdlet 可以用于创建和管理批处理帐户和键。

### 创建一个批处理帐户

**新 AzureBatchAccount**指定的资源组中创建新的批处理帐户。 如果不已经有一个资源组，请创建一个运行[新建 AzureResourceGroup](https://msdn.microsoft.com/library/dn654594.aspx) cmdlet，**位置**参数中指定的 Azure 的地区之一。 （可以查找可用区域不同的 Azure 资源通过运行[Get AzureLocation](https://msdn.microsoft.com/library/dn654582.aspx)。）例如︰

```
New-AzureResourceGroup –Name MyBatchResourceGroup –location "Central US"
```

然后，创建新的批处理帐户帐户在资源组中，还应指定帐户名为 <*account_name*> 和批服务的位置。 创建帐户可能要花几分钟才能完成。 例如︰

```
New-AzureBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName MyBatchResourceGroup
```

> [AZURE.NOTE] 批帐户名称必须是唯一的 Azure，介于 3 和 24 个字符，并使用小写字母和数字。

### 获取帐户访问键
**获得 AzureBatchAccountKeys**显示了 Azure 批帐户相关联的访问键。 例如，运行以下操作以获取您所创建的帐户的主、 辅键。

```
$Account = Get-AzureBatchAccountKeys –AccountName <accountname>
$Account.PrimaryAccountKey
$Account.SecondaryAccountKey
```

### 生成一个新的访问键
**新 AzureBatchAccountKey**生成一个新的主要或辅助帐户密钥 Azure 批帐户。 例如，要生成批帐户的新主要密钥，请键入︰

```
New-AzureBatchAccountKey -AccountName <account_name> -KeyType Primary
```

> [AZURE.NOTE] 若要生成一个新的第二项，指定"次"为**关键字类型**参数。 您必须单独重新生成主、 辅密钥。

### 删除批次帐户
**删除 AzureBatchAccount**中删除批次帐户。 例如︰

```
Remove-AzureBatchAccount -AccountName <account_name>
```

出现提示时，确认要删除该帐户。 帐户删除可能需要一些时间才能完成。

## 查询作业、 任务和其他详细信息

使用批处理帐户下创建的实体的 cmdlet 例如**Get AzureBatchJob**、**获取 AzureBatchTask**和**获取 AzureBatchPool**查询。

若要使用这些 cmdlet，您首先需要创建 AzureBatchContext 对象来存储您的帐户名称和密钥︰

```
$context = Get-AzureBatchAccountKeys "<account_name>"
```

将此上下文传递到使用**BatchContext**参数与批服务进行交互的 cmdlet。

> [AZURE.NOTE] 默认情况下，该帐户的主关键字用于身份验证，但您可以显式选择要通过更改 BatchAccountContext 对象的**KeyInUse**属性使用的参数︰ `$context.KeyInUse = "Secondary"`。


### 查询数据

举一个例子，使用**Get AzureBatchPools**来查找您的池。 默认情况下所有池，您的帐户，假设您已为此查询存储在*$context*中的 BatchAccountContext 对象︰

```
Get-AzureBatchPool -BatchContext $context
```
### 使用 OData 筛选器

您可以提供 OData 筛选器，**筛选器**参数用于查找您感兴趣的对象。 例如，您可以找到所有池 id"myPool"开始︰

```
$filter = "startswith(id,'myPool')"
Get-AzureBatchPool -Filter $filter -BatchContext $context
```

此方法不是在本地管道中使用"Where 对象"不如灵活。 但是，查询获取直接发送给批量服务，使所有筛选发生在服务器端，节约了互联网的带宽。

### 使用 Id 参数

OData 筛选器的替代方法是使用**Id**参数。 要查询一个特定的池与 id"myPool":

```
Get-AzureBatchPool -Id "myPool" -BatchContext $context

```
**Id**参数支持全 id 搜索、 没有通配符或 OData 式过滤器。

### 使用管道

批处理的 cmdlet 可以利用 PowerShell 管道以 cmdlet 之间发送数据。 这具有指定参数相同的效果，但使清单更容易的多个实体。 例如，您可以找到您的帐户的所有任务︰

```
Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context
```

### 使用 MaxCount 参数

默认情况下，每个 cmdlet 返回 1000年对象的最大值。 如果达到此限制时，可以优化您的筛选器将返回更少的对象，或将最多使用**MaxCount**参数显式设置。 例如︰

```
Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

```

若要删除上限，将**MaxCount**设置为 0 或更少。

## 相关的主题
* [批处理技术概述](batch-technical-overview.md)
* [下载 Azure PowerShell](http://go.microsoft.com/p/?linkid=9811175)
* [Azure 批 cmdlet 引用](https://msdn.microsoft.com/library/azure/mt125957.aspx)
* [有效的列表查询](batch-efficient-list-queries.md)

测试
