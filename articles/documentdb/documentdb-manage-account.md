---
ms.openlocfilehash: 6d9a5e1674b2f3764a92d82d0167c2dbbbd05534
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="管理 DocumentDB 帐户 |Microsoft Azure" 
    description="了解如何管理您的 DocumentDB 帐户。" 
    services="documentdb" 
    documentationCenter="" 
    authors="stephbaron" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/28/2015" 
    ms.author="stbaro"/>

#如何管理 DocumentDB 帐户

了解如何使用键，一致性设置，并了解如何删除一个帐户。

## <a id="keys"></a>查看、 复制和重新生成访问密钥
创建一个 DocumentDB 帐户时，该服务生成两个 DocumentDB 帐户访问时可以用于身份验证的主机的访问键。 通过提供两个访问键，DocumentDB 可以重新生成密钥对您的 DocumentDB 帐户没有中断。

在[Microsoft Azure 预览门户](https://portal.azure.com/)中，访问您的**DocumentDB 帐户**刀片式服务器以查看、 复制和重新生成用于访问您的 DocumentDB 帐户的访问键的**键**部分。

![](media/documentdb-manage-account/keys.png)

### 查看和复制访问键

1.      在[Azure 预览门户](https://portal.azure.com/)中，访问您的 DocumentDB 帐户。 

2.      在**摘要**镜头中，单击**项**。

3.      **键**刀片式服务器，请单击**复制**按钮右边的要复制的项。

  ![](./media/documentdb-manage-account/image004.jpg)

### 重新生成访问密钥

为您的 DocumentDB 帐户定期以帮助保持您的连接更加安全，您应该更改访问键。 使您能够维护连接时重新生成的其他访问键使用一个访问键的 DocumentDB 帐户分配给两个访问键。

> [AZURE.WARNING] 重新生成访问密钥会影响依赖于当前密钥的任何应用程序。 使用访问键访问 DocumentDB 帐户的所有客户端必须更新以使用新的密钥。

如果您的应用程序或使用 DocumentDB 帐户的云服务，您将丢失连接如果您重新生成密钥，除非您将您的密钥。 以下步骤概述了涉及滚动您的密钥的过程。

1.      更新应用程序代码来引用辅助快捷键的 DocumentDB 帐户中的访问键。

2.      重新生成您的 DocumentDB 帐户的主访问键。
在[Azure 预览门户](https://portal.azure.com/)中，访问您的 DocumentDB 帐户。

3.      摘要的镜头，在单击**项**。

4.      **键**刀片式服务器，单击**主重新生成**命令，然后单击**确定**以确认您要生成一个新密钥。

5.      验证新的密钥是可供使用 （再生后大约 5 分钟） 后，请更新应用程序代码以引用新的主访问键中的访问键。

6.      重新生成辅助访问键。

*请注意，它可能需要几分钟才能使用新生成的密钥来访问您的 DocumentDB 帐户。*

## <a id="consistency"></a>管理 DocumentDB 一致性设置
DocumentDB 支持四个明确定义用户可配置的数据一致性级别，可以使开发人员进行可预测的一致性可用性延迟权衡。

- 始终读取操作的**强**一致性保证返回上次写入的值。

- **包围，失效**一致性保证读不是太过时了。 特别是保证读取是不超过 K 版本早于最后的书面版本。 

- **会话**一致性保证单调性读取 (永远不会读取旧的数据，再新，然后旧再次)，单调写操作 （排序写入操作），和阅读中任何单个客户端的角度来看最新的写入操作。

- 始终读取操作的**Eventual**一致性保证读写操作的有效子集，并将最终收敛。

*请注意，默认情况下，DocumentDB 帐户调配与会话级别的一致性。  DocumentDB 一致性设置的其他信息，请参阅[一致性级别](http://go.microsoft.com/fwlink/p/?LinkId=402365)节。*

### 若要指定默认一致性为 DocumentDB 帐户

1.      在[Azure 预览门户](https://portal.azure.com/)中，访问您的 DocumentDB 帐户。 

2.      在**配置**镜头中，单击**默认一致性**。

3.      在**默认一致性**刀片式服务器，选择要用于您的 DocumentDB 帐户的默认一致性级别。

![](./media/documentdb-manage-account/image005.png)

![](./media/documentdb-manage-account/image006.png)

4.      单击**保存**。

5.      可能通过 Azure 预览门户通知集线器监视操作的进度。

*请注意，它采用影响跨您的 DocumentDB 帐户的默认一致性设置需要更改前的几分钟时间。*

## <a id="delete"></a> 如何︰ 删除 DocumentDB 帐户
要删除不再使用的 DocumentDB 帐户，请使用**DocumentDB 帐户**刀片式服务器上的**删除**命令。

![](./media/documentdb-manage-account/image009.png)

1.      在[Azure 预览门户](https://portal.azure.com/)中，访问要删除 DocumentDB 帐户。 

2.      在**DocumentDB 帐户**刀片式服务器，请单击**删除**命令。

3.      结果确认刀片上, 键入以确认您要删除的帐户的 DocumentDB 帐户名。

4.      单击确认刀片式服务器上的**删除**按钮。

## <a id="next"></a>下一步行动

了解如何[开始使用您的 DocumentDB 帐户](http://go.microsoft.com/fwlink/p/?LinkId=402364)。

若要了解有关 DocumentDB 的详细信息，请参阅    [azure.com](http://go.microsoft.com/fwlink/?LinkID=402319&clcid=0x409)中的 Azure DocumentDB 文档。

 
 
测试
