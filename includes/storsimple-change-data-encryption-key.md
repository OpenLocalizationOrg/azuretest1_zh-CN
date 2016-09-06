---
ms.openlocfilehash: d94af480bc8c53df25152e57939169bea0f489c5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="更改的 StorSimple 数据加密密钥"
   description="描述如何授权 StorSimple 设备，以便它可以更改数据的加密密钥，并再解释的键发生了更改过程。"
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carolz"
   editor="tysonn" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/17/2015"
   ms.author="v-sharos" />

### 步骤 1︰ 授权的设备来更改管理门户中的服务数据加密密钥

通常情况下，设备管理员将请求服务管理员授权的设备来更改服务数据加密密钥。 服务管理员然后将授权的设备来更改关键字。

管理门户中执行此步骤。 服务管理员可以从有资格获得授权的设备显示的列表中选择一个设备。 然后，设备有权启动服务数据加密密钥更改进程。

#### 更改服务数据加密密钥可以授权哪些设备？

它可以授权启动服务数据加密密钥更改之前，设备必须满足以下条件︰

- 该设备必须处于联机状态，将有资格获得服务数据加密密钥更改授权。

- 如果尚未启动键发生了更改，您可以在 30 分钟后再次授权相同的设备。

- 可以授权其他的设备，前提是由以前的授权的设备尚未启动键发生了更改。 新设备已被授权后，旧设备无法启动更改。

- 鼠标经过图像的服务数据加密密钥时，您不能授权设备。

- 当某些服务中注册的设备具有转存加密，而其他人不可以授权一个设备。 在这种情况下，符合条件的设备都已完成服务数据加密密钥的那些更改。

> [AZURE.NOTE]
> 在管理门户中，StorSimple 虚拟设备可以授权启动键发生了更改的设备的列表中不显示。

执行以下步骤以选择和授权的设备来启动服务数据的加密密钥更改。

#### 若要更改密钥设备进行授权

1. 在服务控制板页上，单击**更改服务数据加密密钥**。

    ![更改服务加密密钥](./media/storsimple-change-data-encryption-key/HCS_ChangeServiceDataEncryptionKey-include.png)

2. 在**更改服务数据加密密钥**对话框中，选择并授权的设备来启动服务数据的加密密钥更改。 下拉列表中有可以授权的所有符合条件的设备。

3. 单击检查图标 ![检查图标](./media/storsimple-change-data-encryption-key/HCS_CheckIcon-include.png).

### 步骤 2︰ 使用 Windows PowerShell StorSimple 启动服务数据的加密密钥更改为

在 Windows PowerShell 授权的 StorSimple 设备上的 StorSimple 接口的情况下执行此步骤。

> [AZURE.NOTE] 可以您的 StorSimple 管理器服务管理门户中不执行任何操作，直到完成密钥翻转。

如果使用的串行控制台设备来连接到 Windows PowerShell 界面，请执行以下步骤。

#### 要启动服务数据的加密密钥更改

1. 选择选项 1 以登录具有完全访问权限。

2. 在命令提示符下，键入︰

     `Invoke-HcsmServiceDataEncryptionKeyChange`

3. Cmdlet 成功完成后，您将得到一个新的服务数据加密密钥。 复制并保存在本过程的步骤 3 中使用此密钥。 该密钥将用于更新 StorSimple 管理器服务注册的所有剩余设备。

    > [AZURE.NOTE] 必须在授权 StorSimple 设备的四小时内启动此过程。

   这个新的密钥然后被发送到服务被推到服务注册的所有设备。 警报将显示在服务面板上。 该服务将禁用注册设备上的所有操作和设备管理员需要更新服务数据加密密钥在其他设备上。 但是，不会中断的 i/o 操作 （将数据发送到云中的主机）。

   如果您有一个设备注册到您的服务时，翻转过程现在已完成，则可以跳过下一步。 如果您有多个设备注册到您的服务，请继续执行步骤 3。

### 步骤 3︰ 更新 StorSimple 中的其他设备上的服务数据加密密钥

如果您有多个设备注册到您的 StorSimple 管理器服务，必须在 StorSimple 设备的 Windows PowerShell 界面中执行这些步骤。 第 2 步中获得的密钥︰ StorSimple 启动服务数据的加密密钥更改为使用 Windows PowerShell 必须用于更新 StorSimple 管理器服务中注册的所有剩余 StorSimple 设备。

执行以下步骤来更新您的设备上的服务的数据加密。

#### 若要更新服务数据加密密钥

1. 使用 Windows PowerShell 的 StorSimple 连接到控制台。 选择选项 1 以登录具有完全访问权限。

2. 在命令提示符下，键入︰

    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`

3. 提供服务的数据加密密钥，您在中获得[第 2 步︰ 使用 Windows PowerShell StorSimple 启动服务数据的加密密钥更改为](#to-initiate-the-service-data-encryption-key-change)。



