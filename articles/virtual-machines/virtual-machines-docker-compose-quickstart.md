---
ms.openlocfilehash: b1b5fd57bbbf1d3c93fd82b5b66ec47e6c5270b7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="开始使用 Docker 和撰写 Azure 的虚拟机上"
   description="使用撰写和在 Azure 上的 Docker 快速简介"
   services="virtual-machines"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/07/2015"
   ms.author="danlep"/>

# 开始使用 Docker 和撰写 Azure 的虚拟机上

本文介绍了如何开始使用 Docker 和[撰写](http://github.com/docker/compose)定义并在 Azure 中的 Linux 虚拟机上运行复杂的应用程序。 与撰写 （*图*的后续产品），使用一个简单的文本文件包含多个 Docker 容器应用程序。 然后启动一个命令执行一切就让它在 VM 上运行您的应用程序。 作为示例，本文介绍如何快速设置与后端 MariaDB SQL 数据库的 WordPress 博客但撰写还可用来设置更复杂的应用程序。

如果您是新手 Docker 和容器，看到[Docker 高白板](http://azure.microsoft.com/documentation/videos/docker-high-level-whiteboard/)。

## 步骤 1︰ 设置 Linux 虚拟机作为 Docker 主机

可用于各种 Azure 过程和可用的图像在 Azure Markeplace 中创建一个 Linux 虚拟机并将其设置为 Docker 主机。 例如，请参阅[使用 Docker VM 扩展从 Azure 命令行界面](virtual-machines-docker-with-xplat-cli.md)快速过程创建 Ubuntu VM Docker VM 扩展名。 当您使用 Docker VM 扩展时，VM 自动设置为 Docker 主机。 这篇文章中的示例演示如何使用[Mac、 Linux、 和 Windows Azure 的命令行界面](../xplat-cli.md)(Azure CLI) 服务管理模式来创建虚拟机。

## 步骤 2︰ 安装撰写

Linux 虚拟机正在运行带 Docker 之后，请从客户端计算机使用 SSH 连接到它。 如果需要请通过运行以下两个命令安装[撰写](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md)。

>[AZURE.TIP] 如果 Docker VM 扩展用于创建您的 VM，撰写已经为您安装。 跳过这些命令，请转到步骤 3。 您只需要安装撰写，如果您自己安装的 Docker 在 VM 上。

```
$ curl -L https://github.com/docker/compose/releases/download/1.1.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

$ chmod +x /usr/local/bin/docker-compose
```
>[AZURE.NOTE]如果收到"权限被拒绝"错误，您在虚拟机上的 /usr/local/bin 目录可能不是可写，您需要以超级用户身份安装撰写。 运行`sudo -i`，然后两个命令上面，然后`exit`。

若要测试您的安装的撰写，运行下面的命令。

```
$ docker-compose --version
```

您将看到类似的输出
```
docker-compose 1.3.2
```


## 步骤 3︰ 创建 docker compose.yml 配置文件

接下来，您将创建`docker-compose.yml`文件，只是文本配置文件，以定义 Docker 容器，可以在虚拟机上运行。  该文件指定图像在每个容器上运行 （或可能是 Dockerfile 的生成），容器，等等之间的必要的环境变量和相关性，端口链接。 Yml 文件语法的详细信息，请参阅[docker compose.yml 参考](http://docs.docker.com/compose/yml/)。

在 VM，创建工作目录，并使用您喜爱的文本编辑器来创建`docker-compose.yml`。 尝试一个简单的示例，请对文件复制下面的文本。 这种配置使用图像从[DockerHub 注册表](https://registry.hub.docker.com/_/wordpress/)安装 WordPress （开源博客和内容管理系统） 和链接的端 MariaDB SQL 数据库。

 ```
 wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 8080:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>

```

## 第 4 步︰ 开始撰写容器

在 VM 上的工作目录，只需运行下面的命令。

```
$ docker-compose up -d

```

这将启动 Docker 容器中指定`docker-compose.yml`。 您将看到类似的输出︰

```
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
```

>[AZURE.NOTE] 请务必使用**-d**选项启动时，以便容器连续运行在后台。

若要验证容器组成，请键入`docker-compose ps`。 您应该看到类似于︰

```
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:8080->80
ess_1              apache2-for ...                       /tcp
```

您现在可以连接到 WordPress 在虚拟机上直接浏览到`http://localhost:8080`。 如果希望通过 Internet 连接到虚拟机，第一次配置 HTTP 终结点映射到专用端口 8080 的公用端口 80 的 VM 上。 例如，在 Azure 服务管理部署中，运行下面的 Azure CLI 命令︰

```
$ azure vm endpoint create <machine-name> 80 8080

```

现在，您应该看到 WordPress 开始屏幕上，您可以完成安装并开始使用该应用程序。

![WordPress 启动屏幕][wordpress_start]


## 下一步行动

* 签出[撰写 CLI 引用](http://docs.docker.com/compose/cli/)和[用户指南](http://docs.docker.com/compose/)》 的构建和部署多容器应用程序的更多示例。
* 使用 Azure 资源管理器模板，或者您自己或一个贡献[社区](http://azure.microsoft.com/documentation/templates/)，从部署 Docker 和应用程序设置与撰写 Azure VM。 例如，[部署与 Docker WordPress 博客](https://azure.microsoft.com/documentation/templates/docker-wordpress-mysql/)模板使用 Docker 和撰写快速与 MySQL 后端在 Ubuntu VM 部署 WordPress。
* 请尝试[Docker 群](virtual-machines-docker-swarm.md)群集集成 Docker 撰写。 请参阅[Docker 撰写/蜂群的集成](https://github.com/docker/compose/blob/master/SWARM.md)方案。

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-docker-compose-quickstart/WordPress.png
