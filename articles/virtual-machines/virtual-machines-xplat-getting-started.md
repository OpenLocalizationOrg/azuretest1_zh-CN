---
ms.openlocfilehash: 83a1e42a7a58bc237ddcc4d73b0b88a87c9655fb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="如何使用 Azure CLI 创建 Azure 的虚拟机 |Microsoft Azure"
   description="本主题介绍如何安装在任何平台上的 Azure CLI、 如何使用它来连接到 Azure 帐户，以及如何从 Azure CLI 创建虚拟机。"
   services="virtual-machines"
   documentationCenter="virtual-machines"
   authors="dlepow"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="command-line-interface"
   ms.workload="infrastructure-services"
   ms.date="06/09/2015"
   ms.author="danlep"/>

# 通过使用 Azure 命令行界面 (Azure CLI) 来创建虚拟机
Azure CLI 可从任何平台管理 Azure 基础的一个好办法。

仅安装 Azure CLI 和无 Azure 的订阅将阻止您立即创建一个虚拟机，因此，让我们小心这些步骤。 如果您没有[转获取一个空闲](http://azure.microsoft.com/pricing/free-trial/)的 Azure 帐户。

## 安装 Azure CLI

按照说明[安装 Azure CLI](../xplat-cli.md#install)。

## 通过使用 Azure CLI 连接到 Azure

用个人 Azure 帐户登录，或与工作或学校的 Azure 帐户，您可以连接 Azure CLI 安装。 了解这些差异，并选择，如何[连接到 Azure 订购](../xplat-cli.md#configure)。

## 创建和连接到一个 Azure 中的虚拟机

选择创建一个虚拟机启动 （或上传） 图像，并使用`azure vm create`命令。

1. 若要从命令行中选择一个图像，可以列出可用 VM 映像使用命令`azure vm image list`。 因为有如此之多的图像，您需要通过使用页结果`more`或筛选器通过使用`grep`(Linux) 或`findstr`(Windows)。 例如，如果您正在寻找 Ubuntu Linux 上的图像，使用如下命令︰

        azure vm image list | grep Ubuntu

    这仍会导致图像，您可以进一步缩小版本的长列表︰

        azure vm image list | grep Ubuntu-14_10

    从这里您可以选择图像并使用`show`命令来更详细地查看它的属性︰

        azure vm image show b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_10-amd64-server-20150202-en-us-30GB

2. 一旦您选择了虚拟机映像，使用`vm create`命令来创建图像。 此命令有很多选项，您可以通过使用列出的`help`命令︰

        vm create --help

    与步骤 1 中的图像，则需要创建一个虚拟机的主要参数是位置、 DNS 名称和用户名。

    进行身份验证，您可以选择指定的密码 （在命令行上或以交互方式），或使用证书进行身份验证。 如果您选择的密码，则需要至少 8 个字符，包含大写和小写字母，而包含特殊字符 (例如之一 ！ @# $%^ & + =)。 最好将密码放入引号和转义特殊字符，如果在命令行上传递它。

    若要选择一个位置，您可以使用`vm location list`命令以选择离您最近的区域。

  您选择的 DNS 名称必须是唯一的 (它将映射到`dnsname.cloudapp.net`)，并将会与计算机名称相同，如果不单独在命令行上指定的计算机的名称。  

    下面的 Linux 示例中西部美国创建一个虚拟机，打开默认 SSH 端口 22 （-e 参数），并创建一个名为用户`myadminuser`:

        azure vm create -e -l "West US"  my-new-cli-vm b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_10-amd64-server-20150202-en-us-30GB "myadminuser" "myAdm1n@passwd"

## 下一步行动

让我们转到做某事的 VM。

上面的示例中打开默认 SSH 端口，因为一旦连接到虚拟机并运行十分简单。 从 Linux 命令行︰

    ssh myadminuser@my-new-cli-vm.cloudapp.net

查看更多示例使用 Azure CLI 管理 Azure 基础的主要场所是[Azure CLI 命令参考页](../virtual-machines-command-line-tools.md)。

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
