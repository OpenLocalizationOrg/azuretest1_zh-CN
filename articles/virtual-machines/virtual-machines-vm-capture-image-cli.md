---
ms.openlocfilehash: cb7c06196f69e4f7cf0c222d3bb56328634b639b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="捕获映像运行 Linux 使用 CLI 的虚拟机"
    description="了解如何捕获运行 Linux Azure 虚拟机 (VM) 映像。"
    services="virtual-machines"
    documentationCenter=""
    authors="karthmut"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/20/2015"
    ms.author="karthmut"/>




# 如何捕获 Linux 虚拟机作为模板使用 CLI 使用##



本文演示如何捕获 Azure 运行 Linux，因此可用它像一个模板来创建其他虚拟机的虚拟机。 此模板包括操作系统磁盘和数据中的任何磁盘连接虚拟机。 不包括网络配置，因此您将需要配置它，当您创建其他使用该模板的虚拟机。



Azure 将此模板作为图像处理，并将其存储在图像列表中。 这是您已上载的任何图像的存储位置。 有关映像的详细信息，请参阅[关于虚拟机映像在 Azure 中] []。



##开始之前##



这些步骤假定已经创建 Azure 的虚拟机并配置操作系统，包括附加任何数据磁盘。 如果尚未尚未执行此操作，请参阅以下说明︰



- [如何创建自定义的虚拟机] []

- [如何附加到虚拟机的数据磁盘] []



##获取虚拟机##



1. 连接到虚拟机。 有关详细信息，请参阅[如何登录到虚拟机运行 Linux][]。



2. 如果状态是**StoppedDeallocated**，可只捕获 VM。 在 SSH 窗口中，键入以下命令关闭您的虚拟机︰



        vm shutdown [options] <vm-name>



    记下的那个选项`vm shutdown`是`-p`，它将保留在崩溃时的计算资源。 Do**不**启用，作为 VM 必须为了 deprovisioned 捕获它。



3. 运行以下命令来捕捉您的 VM 映像︰



        vm capture <vm-name> <target-image-name> --deleted



    为了捕获映像，必须删除虚拟机。 您可以使用`--deleted`或`-t`若要这样做。



4. 您可以确认您的映像已被捕获通过检查您的 VM 映像列表︰



        vm image list | grep <target-image-name>



    `| grep <target-image-name>`部分是可选的但将帮助您更轻松地在列表中找到您的图像。



以下是捕获虚拟机包括终端输出的一个示例演练︰


    ~$ azure vm list

    info:    Executing command vm list

    + Getting virtual machines

    data:    Name         Status            Location  DNS Name                  IP Address

    data:    -----------  ----------------  --------  ------------------------  ------------

    data:    kmorig       RoleStateUnknown  West US   kmorig.cloudapp.net       100.92.56.16

    info:    vm list command OK

    ~$ azure vm shutdown kmorig

    info:    Executing command vm shutdown

    + Getting virtual machines

    + Shutting down VM

    info:    vm shutdown command OK

    ~$ azure vm list

    info:    Executing command vm list

    + Getting virtual machines

    data:    Name         Status              Location  DNS Name                  IP Address

    data:    -----------  ------------------  --------  ------------------------  ----------

    data:    kmorig       StoppedDeallocated  West US   kmorig.cloudapp.net

    info:    vm list command OK

    ~$ azure vm capture kmorig kmcaptured --deleted

    info:    Executing command vm capture

    + Getting virtual machines

    + Checking image with name kmcaptured exists

    + Capturing VM

    info:    vm capture command OK

    ~$ azure vm image list | grep kmcaptured

    data:    kmcaptured



访问[Azure CLI 文档页中找到][]有关更多详细信息和其他命令。


[Azure CLI 文档页]: ../virtual-machines-command-line-tools.md

[如何登录到运行 Linux 的虚拟机]: virtual-machines-linux-how-to-log-on.md

[有关在 Azure 中的虚拟机映像]: http://msdn.microsoft.com/library/azure/dn790290.aspx

[如何创建自定义的虚拟机]: virtual-machines-create-custom.md

[如何附加到虚拟机的数据磁盘]: storage-windows-attach-disk.md
 