---
ms.openlocfilehash: ece69820b10c0fa25736b1d7ea738f8d35a1859a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure 的 Linux 虚拟机上使用超级用户权限" 
    description="了解如何使用 Azure 中的 Linux 虚拟机上的根用户权限。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/29/2015" 
    ms.author="szark"/>




# 在 Azure 的 Linux 虚拟机上使用超级用户权限

默认情况下，`root`用户禁用在 Azure 的 Linux 虚拟机上。 用户可以通过使用提升的权限运行命令`sudo`命令。 但是，经验会因系统设置如何。

1. **SSH 密钥和密码或密码只**-虚拟机设置的任何证书 (`.CER`文件) 或 SSH 密钥，以及密码，或只是用户名和密码。 在这种情况下`sudo`在执行命令之前将提示输入用户的密码。

2. **仅 SSH 密钥**— 虚拟机设置的证书 (`.cer`或`.pem`文件) 或 SSH 密钥，但没有密码。  在这种情况下`sudo`**将不会**执行该命令之前的用户的密码提示。


## SSH 密钥和密码，或者密码只

登录到 Linux 虚拟机使用 SSH 密钥或密码身份验证，然后再运行使用命令`sudo`，例如︰

    # sudo <command>
    [sudo] password for azureuser:

在这种情况下用户将被提示输入密码。 输入密码后`sudo`将运行的命令`root`的特权。

您还可以通过编辑启用 passwordless sudo`/etc/sudoers.d/waagent`文件，例如︰

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

此更改将允许用户"azureuser"passwordless sudo。

## SSH 密钥只

登录到 Linux 虚拟机使用 SSH 密钥身份验证，然后再运行使用命令`sudo`，例如︰

    # sudo <command>

在这种情况下用户将**不**会提示您输入密码。 之后按下`<enter>`，`sudo`将运行的命令`root`的特权。

 
