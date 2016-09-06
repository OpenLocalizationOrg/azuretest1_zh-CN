---
ms.openlocfilehash: bd0a3b7c7b093bb5963a437b5edb629170dfaf8f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties title="Deploying Your Own Private Docker Registry on Azure"
  pageTitle="部署在 Azure 上的自己专用 Docker 注册表"
  description="描述如何使用 Docker 注册表来承载容器图像对 Azure Blob 存储服务。"
  services="virtual-machines"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="" 
  tags="" />

<tags
  ms.service="virtual-machines"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="06/17/2015" 
  ms.author="ahmetb" />

# 部署在 Azure 上的自己专用 Docker 注册表

本文档介绍什么 Docker 专用注册信息并显示如何部署 Docker 注册表 2.0 容器图像到 Microsoft Azure 使用 Azure Blob 存储在专用 Docker 注册表。

本文档假定︰

1. 您知道如何使用 Docker 并让 Docker 图像存储。 （您不？ [请查阅 Docker](https://www.docker.com)）
2. 您必须具有 Docker 引擎已安装的服务器上。 （您不？ [在 Azure 上快速完成。](http://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## 什么是专用 Docker 注册表？

为了运送到云应用程序容器化，构造 Docker 容器图像，并将其存储在某个位置，以便通过自己和其他人可以使用它。 

轻松创建容器图像和它传送到云时，就很难可靠地存储生成的图像。 由于这个原因，Docker 提供集中的服务调用[Docker 集线器][docker 集线器]上云存储容器图像，并允许您创建任何时候使用这些图像的容器。

虽然[Docker 集线器][docker 集线器]是付费的服务，用于存储私有应用程序容器图像，Docker 尊重开发人员的需要和提供开源的工具集，用于在防火墙后面自己专用 Docker 注册表中存储图像或内部没有碰到公用的 Internet。
由于 Azure Blob 存储可以很容易地安全，快速可以使用创建和使用自己控制的 Azure 中的专用 Docker 注册表

## 您为什么应承载在 Azure 上 Docker 注册表？

通过承载在 Microsoft Azure 上您 Docker 注册表的实例并在 Azure Blob 存储上存储您的图像，您可以有以下几个优点︰

**安全︰**Docker 图像不要 Azure 数据中心，因此，如果您在使用 Docker 集线器一样不会公用互联网。
  
**的性能︰**图像存储您 Docker 作为您的应用程序存储在同一个数据中心或地区。 这意味着将更快、 更可靠地比作 Docker 中心抽取图像。

**的可靠性︰**通过使用 Microsoft Azure Blob 存储，您可以使用的高可用性，冗余，高级存储 (SSDs)，像很多存储属性等等。

## 配置 Docker 注册表使用 Azure Blob 存储

（建议将读取[Docker 注册表 2.0 文档][注册表文档]之前。）

您可以[配置][注册表配置]Docker 注册表中两种不同方式。
您可以：

1. 使用`config.yml`文件。 在这种情况下您将需要创建单独的 Docker 图像的顶部`registry`图像。
2. 重写默认配置文件通过环境变量︰ 获取完成而无需创建和维护一个单独的 Docker 图像工作。

为了简单起见，本主题后面选项 2，使用环境变量。

为了运行 Docker 注册表实例的︰
* 用于存储图像使用 Azure 存储帐户
* 在虚拟机的端口 5000 上侦听
* 已配置的任何身份验证 （不建议这样做，请参阅下面的注释）

您需要在您的 bash 终端中运行以下 Docker 命令 (替换`<storage-account>`，`<storage-key>`与您的凭据):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

一旦退出该命令，可以看到容器承载您的私有 Docker 注册表实例通过运行`docker ps`命令您的主机上︰

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] 本文档中未涉及 Docker 注册表配置安全性，如果您打开虚拟机终结点上的注册表端口到端口或负载平衡器，如果您使用上面的部署命令将没有默认的身份验证的任何用户都可以访问注册表。
>
> 请阅读[配置 Docker 注册表][注册表配置]文档，以了解如何保护注册表实例和图像。

## 下一步行动

注册表设置之后，就去使用它多一些。 Docker[注册表文档]的开头。 

[docker 集线器]: https://hub.docker.com/
[注册表]: https://github.com/docker/distribution
[注册表文档]: http://docs.docker.com/registry/
[注册表配置]: http://docs.docker.com/registry/configuration/
 