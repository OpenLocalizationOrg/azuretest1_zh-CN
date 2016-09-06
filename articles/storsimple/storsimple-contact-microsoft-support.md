---
ms.openlocfilehash: 236a4d8a9b00c74e6d8514fa299d2c4b19335418
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="与 Microsoft 支持部门联系 |Microsoft Azure"
   description="了解如何创建支持请求和 StorSimple 设备上启动一个支持会话。"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/31/2015"
   ms.author="alkohli" />

# 与 Microsoft 支持部门联系

如果您遇到的任何问题与您的 Microsoft Azure StorSimple 解决方案，您可以创建对技术支持的服务请求。 在与支持工程师的联机会话，您可能还需要在 StorSimple 设备上启动一个支持会话。 这篇文章将引导您完成创建支持请求和在 StorSimple 设备的 Windows PowerShell 界面中也开始支持会话的过程。

## 创建支持请求

执行以下步骤来创建一个支持请求︰

#### 若要创建支持请求

1. 可以通过[管理门户](http://manage.windowsazure.com/)创建一个支持请求。 在[管理门户网站](http://manage.windowsazure.com/)中，单击您的**帐户名**，然后单击**联系 Microsoft 技术支持**。

    ![通过 ManagementPortal 联系人 MS 支持](./media/storsimple-contact-microsoft-support/IC777286.png)

2. 在**联系 Microsoft 技术支持**对话框中︰                             

    1. 从下拉列表中，选择与您的 StorSimple 管理器服务的**订阅**关联的目标。 **支持类型**指定为**技术**。 您需要启用技术支持的付费的支持计划。

    2. 单击检查图标![检查图标](./media/storsimple-contact-microsoft-support/IC740895.png)向**创建票证**。

3. 在**Microsoft 支持**窗口中，从**产品**下拉列表中，选择**StorSimple**。

    ![请联系 Microsoft 技术支持-产品](./media/storsimple-contact-microsoft-support/IC777288.png)

4. 请按照屏幕说明以正确分类您的请求，并提供了明确、 具体的问题说明。

在提交您的请求之后，技术支持工程师将联系您尽可能快地处理您的请求。

## 对于 StorSimple 启动 Windows PowerShell 支持会话

要解决任何问题，您可能会遇到与 StorSimple 设备，您需要与 Microsoft 技术支持团队接洽。 Microsoft 支持部门可能需要使用支持会话登录到您的设备上。 

执行以下步骤以启动支持会话︰

#### 若要启动支持会话

1. 通过串行控制台直接或通过 telnet 会话从远程计算机访问该设备。 若要执行此操作，请按照[使用 PuTTY 连接到设备的串行控制台](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)。

2. 在会话中打开，请按**Enter**键以获取命令提示符。

3. 在串行控制台菜单中，选择选项 1，**登录的完全访问权限**。

4. 在提示符下，键入以下密码︰ 

    `Password1`

5. 在提示符下，键入以下命令︰

    `Enable-HcsSupportAccess`

6. 将向您显示加密的字符串。 将该字符串复制到文本编辑器 （如记事本） 中。

7. 保存此字符串并将其电子邮件中发送到 Microsoft 支持。 

> [AZURE.IMPORTANT] 您可以通过运行禁用支持访问`Disable-HcsSupportAccess`。 StorSimple 设备还会禁用支持访问 8 个小时后启动该会话。 它是一种最佳做法，以支持会话启动后更改 StorSimple 设备凭据。