---
ms.openlocfilehash: 40946439654dffbf61e7aece3459b316b853f3dc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 的多因素身份验证报告" 
    description="描述了如何更改用户的设置，如强制用户再次完成证明过程。" 
    documentationCenter="" 
    services="multi-factor-authentication" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# 管理与 Azure 云环境中的多因素身份验证的用户设置

作为管理员，您可以管理以下的用户和设备设置。  

- [要求所选的用户再次提供联系方法](#require-selected-users-to-provide-contact-methods-again)
- [删除现有的应用程序密码的用户](#delete-users-existing-app-passwords)
- [（公共预览） 的用户的所有挂起设备还原 MFA](#restore-mfa-on-all-suspended-devices-for-a-user)






如果丢失或被盗的计算机或设备，或者您需要删除用户访问权限，这是很有帮助。


## 要求所选的用户再次提供联系方法

此设置会强制用户在他或她登录时再次完成注册过程。 请注意非浏览器应用程序将继续工作如果用户对其具有应用程序密码。  此外可能选择**删除生成的所选用户的所有现有应用程序密码**，您可以删除用户的应用程序密码。

### 如何要求用户再次提供联系方法

<ol>
<li>登录到 Azure 的管理门户。</li>
<li>在左边，单击撤销。</li>
<li>在下，目录单击想要要求再次提供他们的联系方法的用户目录。</li>
<li>在顶部，单击用户。</li>
<li>在页面的底部，单击管理多元验证 这将会打开多因素身份验证页。
<li>查找您想要管理和名称旁边的框中打勾的用户。 您可能需要更改在顶部的视图。</li>
<li>此操作将显示在右侧的**管理用户设置**链接。 单击该按钮。</li> 
<li>在**要求选定的用户，提供联系方法再次**打勾。</li>

![提供联系方法](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)

<li>单击保存。</li>
<li>单击关闭</li>

## 删除现有的应用程序密码的用户

这将删除所有用户已创建的应用程序密码。 非浏览器应用程序与这些应用程序密码将停止工作，直到创建了一个新的应用程序密码。

### 如何删除现有的应用程序密码的用户

<ol>
<li>登录到 Azure 的管理门户。</li>
<li>在左边，单击撤销。</li>
<li>在下，目录单击想要删除的应用程序的密码的用户目录。</li>
<li>在顶部，单击用户。</li>
<li>在页面的底部，单击管理多元验证 这将会打开多因素身份验证页。
<li>查找您想要管理和名称旁边的框中打勾的用户。 您可能需要更改在顶部的视图。</li>
<li>此操作将显示在右侧的**管理用户设置**链接。 单击该按钮。</li> 
<li>**删除由所选用户的所有现有应用程序密码**中打勾。</li>
![删除应用程序密码](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
<li>单击保存。</li>
<li>单击关闭。</li>





## 在用户的所有挂起的设备上恢复 MFA

管理员可以重置的设备和浏览器的多因素身份验证。 这是恢复多因素身份验证的用户的设备和浏览器。 当执行此操作，这将从所有用户的设备和浏览器都删除挂起。 

### 如何在用户的所有挂起的设备上还原 MFA

<ol>
<li>登录到 Azure 的管理门户。</li>
<li>在左边，单击撤销。</li>
<li>在下，目录单击想要还原 mfa 在用户的目录。</li>
<li>在顶部，单击用户。</li>
<li>在页面的底部，单击管理多元验证 这将会打开多因素身份验证页。
<li>查找您想要管理和名称旁边的框中打勾的用户。 您可能需要更改在顶部的视图。</li>
<li>此操作将显示在右侧的**管理用户设置**链接。 单击该按钮。</li> 
<li>还原所有挂起的设备上的多因素身份验证中打勾。</li>
![删除应用程序密码](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
<li>单击保存。</li>
<li>单击关闭。</li>


