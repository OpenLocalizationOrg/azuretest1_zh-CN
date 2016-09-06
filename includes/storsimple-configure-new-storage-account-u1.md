---
ms.openlocfilehash: 4534148466d2437ab6941bf60a5404bbb75ca5f1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="可选︰ 配置新的存储帐户为服务"
   description="解释如何配置 StorSimple 管理器服务运行更新 1 存储帐户。"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/14/2015"
   ms.author="alkohli" />

#### 若要添加存储帐户在 StorSimple 8000 系列更新 1.0

1. StorSimple 管理器服务登录页上，选择您的服务并双击它。 这将为**快速启动**页。 选择**配置**页面。

2. 单击**添加/编辑存储帐户**。

3. 在**添加/编辑存储帐户**对话框中，单击**新加**。

4. 在**提供程序**字段中，选择适当的云服务提供商。 受支持的提供程序是 Azure，Amazon S3，RR、 HP 和 OpenStack Amazon S3。 指定的凭据和与云服务提供商存储帐户相关联的位置。 提供的凭据字段将有所不同取决于您所指定的云服务提供商。 
  - 如果选择了 Azure 为云服务提供商，提供**名称**和您的 Microsoft Azure 存储帐户主要**访问键**。 Azure 帐户，该位置将自动填充。

        ![Add Azure storage account](./media/storsimple-configure-new-storage-account-u1/AddAzureStorageaccount-include.png)

 - 如果选择了 Amazon S3 和 Amazon S3 与 RR，提供友好**的存储帐户名称**、**访问键**和**密钥**。 Amazon 的 S3 和 Amazon S3 与 RR，支持以下位置︰

        - US Standard
        - US West (Oregon)
        - US West (Northern California)
        - EU (Ireland)
        - Asia Pacific (Singapore)
        - Asia Pacific (Sydney)
        - Asia Pacific (Tokyo)
        - South America (Sao Paulo)

        ![Add Amazon storage account](./media/storsimple-configure-new-storage-account-u1/AddAmazonStorageaccount-include.png)
            
 - 如果选择了 HP 为云服务提供商，提供友好的**存储帐户名称**、**租户 ID**、**用户名**和**密码**。 先说 HP，对于支持以下位置︰

        - US East
        - US West
      
        ![Add HP storage account](./media/storsimple-configure-new-storage-account-u1/AddHPStorageaccount-include.png)
            
 - 如果选择了**Openstack**为云服务提供商提供的**主机名**、**访问键**和**机密密钥**。

        > [AZURE.NOTE] 对于所有的云服务提供商，不包括 Azure，允许一个友好的名称。 可以使用不同的友好名称，并使用相同的凭据集创建多个存储帐户。

        ![Add Openstack storage account](./media/storsimple-configure-new-storage-account-u1/AddOpenstackStorageaccount-include.png)

5. 选择**启用 SSL 模式**来创建用于移动设备和云之间的网络通信的安全通道。 仅当您在私有云内操作，请清除**启用 SSL 模式**复选框。

      > [AZURE.NOTE] 如果您使用 HP 作为您的提供商，将始终启用 SSL。
        
6. 单击检查图标 ![检查图标](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png). 成功创建存储帐户后，您将收到通知。

7. 将**存储**帐户**配置**页面上显示新创建的存储帐户。 单击**保存**以保存新的存储帐户。 单击**确定**时提示您进行确认。
