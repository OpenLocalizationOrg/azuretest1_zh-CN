---
ms.openlocfilehash: d39055c38204ce36b87e12bff0ec4c8de7ef1c23
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Linux 和开源在 Azure 上计算"
    description="本主题包含 Linux 和开源计算 Azure，包括基本的 Linux 使用情况，对一些基本概念，关于运行或者上传 Azure 和其他有关的特定技术和优化的内容上的 Linux 映像的列表。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/02/2015"
    ms.author="rasquill"/>



# Linux 和开源在 Azure 上计算

本文尝试在一个地方列出由 Microsoft 编写的所有主题和其合作伙伴之间有关运行基于 Linux 的虚拟机，以及其他的开源计算环境和 Microsoft Azure 上的应用程序。 因为 Azure 和开源的计算环境是快速移动的目标，它是几乎可以肯定，此文档已过时，*尽管*我们应该做我们最大努力不断地添加新标题并删除已过时的事实。 如果我们已经错过了其中一个，请让我们知道在注释，或提交到我们[GitHub repo](https://github.com/Azure/azure-content/)拉请求。

## 一般说明
在该页的右侧分解部分。 （链接可能会出现在多个部分，主题可以是关于多个概念、 distro 或技术一样。）此外，有介绍各种 Linux 选项、 图像存储库、 案例研究和帮助主题传您自己的自定义图像的几个主题︰

- [Azure 的市场](http://azure.microsoft.com/marketplace/virtual-machines/)
- [MSOpenTech 虚拟机厂](https://vmdepot.msopentech.com/List/Index)
- [事件和演示︰ Microsoft 开放性 CEE](http://www.opennessatcee.com/)
- [方法︰ 上传您自己的 Distro 图像](virtual-machines-linux-create-upload-vhd.md)（并说明还使用[Azure-Endorsed 分发](virtual-machines-linux-endorsed-distributions.md)）
- [备注︰ 常规 Linux 要求到 Azure 中运行](virtual-machines-linux-create-upload-vhd-generic.md)
- [Linux 在 Azure 上说明︰ 一般性介绍](virtual-machines-linux-introduction.md)

<!--
- [Distros](#distros) &mdash; Topics to do with a specific distro.
- [The Basics](#basics) &mdash; A lot of the basic things to do that you either know or need to know.
- [Community Images and Repositories](#images) &mdash; Other places for very useful information, repositories, and binaries.
- [Languages and Platforms](#langsandplats)
- [Samples and Scripts](#samples)
- [Auth and Encryption](#security) &mdash; Important security-related topics, not necessarily specific to Azure.
- [Devops, Management, and Optimization](#devops) &mdash; A big category, changing rapidly.
- [Support, Troubleshooting, and "It Just Doesn't Work"](#supportdebug) &mdash; Really.
-->

## Distros

有吨的 Linux 版本，通常按包管理系统︰ 一些基于 dpkg，像 Debian 和 Ubuntu，和其他人都基于 rpm，CentOS、 SUSE 和 RedHat 等。 一些公司为 Microsoft 的正式伙伴提供 distro 的图像和被认可。 其他人是由社区提供的。 Distros 本节中的有正式文章，即使只使用示例中的其他技术。

### [Ubuntu](http://azure.microsoft.com/marketplace/partners/Canonical/)

Ubuntu 是非常流行和 Azure 认可 Linux 分布基于 dpkg 和 apt 获取软件包管理。

1. [如何︰ 上载自己 Ubuntu 的图像](virtual-machines-linux-create-upload-vhd-ubuntu.md)
2. [如何︰ Ubuntu 灯堆栈](virtual-machines-linux-install-lamp-stack.md)
2. [图像︰ LAPP 堆栈](http://azure.microsoft.com/marketplace/partners/bitnami/lappstack54310ubuntu1404/)
3. [如何︰ MySQL 群集](virtual-machines-linux-mysql-cluster.md)
4. [如何︰ Node.js 和卡桑德拉](virtual-machines-linux-nodejs-running-cassandra.md)
5. [如何︰ IPython 笔记本](virtual-machines-python-ipython-notebook.md)
6. [出的 geeking︰ 运行 Linux 使用 Docker 容器上的 ASP.NET 5](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
7. [图像︰ Redis 服务器](http://azure.microsoft.com/marketplace/partners/cognosys/redisserver269ubuntu1204lts/)
8. [图像︰ Minecraft 服务器](http://azure.microsoft.com/marketplace/partners/bitnami/craftbukkitminecraft179r030ubuntu1210/)
9. [图像︰ Moodle](http://azure.microsoft.com/marketplace/partners/bitnami/moodle270ubuntu1404/)
11. [作为一种服务的图像︰ 单声道](http://azure.microsoft.com/marketplace/partners/aegis/monoasaserviceubuntu1204/)


### [Debian](https://vmdepot.msopentech.com/List/Index?sort=Featured&search=Debian)

Debian 是 Linux 和开源世界基于 dpkg 和 apt 获取软件包管理重要分布。 MSOpenTech 虚拟机厂有几个要使用的图像。

### CentOS

CentOS Linux 分发是一个稳定、 可预测性、 可管理和 reproduceable 平台，源自源 Red Hat 企业 Linux (RHEL)。

1. [MSOpenTech 虚拟机厂](https://vmdepot.msopentech.com/List/Index?sort=Featured&search=centos)
2. [图像库](http://azure.microsoft.com/en-in/marketplace/partners/OpenLogic/)
3. [如何︰ 为 Azure 准备自定义基于 CentOS 的虚拟机](virtual-machines-linux-create-upload-vhd-centos.md)
4. [博客︰ 如何将从 OpenLogic CentOS VM 映像部署](http://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
6. [如何︰ 安装 Apache Qpid 质子-C AMQP 和服务总线](http://msdn.microsoft.com/library/azure/dn235560.aspx)
7. [图像︰ Apache 2.2.15 上 OpenLogic CentOS 6.3](http://azure.microsoft.com/marketplace/partners/cognosys/apache2215onopenlogiccentos63/)
8. [图像︰ Drupal 7.2，灯泡 OpenLogic CentOS 6.3 服务器](http://azure.microsoft.com/marketplace/partners/cognosys/drupal720lampserveronopenlogiccentos63/)

### SUSE Linux 企业服务器和 openSUSE

9. [MSOpenTech 虚拟机厂](https://vmdepot.msopentech.com/List/Index?sort=Featured&search=OpenSUSE)
11. [如何︰ 安装和运行 MySQL](virtual-machines-linux-mysql-use-opensuse.md)
12. [如何︰ 准备自定义 SLES 或 openSUSE VM](virtual-machines-linux-create-upload-vhd-suse.md)  
13. [[SUSE 论坛]如何︰ 将移动到新的修补程序服务器](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
14. [图像︰ SAP 云装置库的 SUSE Linux 企业服务器](http://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)

### CoreOS

CoreOS 是小型、 优化 distro 纯计算范围，为自定义控件的高度。

10. [图像库](http://azure.microsoft.com/en-in/marketplace/partners/coreos/)  
11. [如何︰ 在 Azure 上使用 CoreOS](virtual-machines-linux-coreos-how-to.md)
12. [如何︰ 五星和 CoreOS 在 Azure 上的 Docker 入门](virtual-machines-linux-coreos-fleet-get-started.md)
13. [博客︰ TechEd 欧洲 — Docker 的 Windows 客户端和 Linux 容器](http://azure.microsoft.com/blog/2014/10/28/new-docker-coreos-topics-linux-on-azure/)
14. [博客︰ Azure 的获得更大、 速度更快，而且更开放](http://azure.microsoft.com/blog/2014/10/20/azures-getting-bigger-faster-and-more-open/)
15. [部署在 Azure 上的 CoreOS GitHub︰ 快速入门](https://github.com/timfpark/coreos-azure)
16. [GitHub︰ 部署春季启动，MongoDB，和 CoreOS 的 Java 应用程序](https://github.com/chanezon/azure-linux/tree/master/coreos/cloud-init)

#### [Oracle Linux](http://azure.microsoft.com/marketplace/?term=Oracle+Linux)
  2. [准备 Azure 的 Oracle Linux 虚拟机](virtual-machines-linux-create-upload-vhd-oracle.md)

### FreeBSD

12. [MSOpenTech 虚拟机厂](https://vmdepot.msopentech.com/List/Index?sort=Date&search=FreeBSD)
13. [博客︰ 运行 FreeBSD 在 Azure 中](http://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
14. [博客︰ 轻松部署 FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
15. [博客︰ 部署自定义的 FreeBSD 映像](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
17. [如何︰ 安装 Azure 的 Linux 代理](virtual-machines-linux-agent-user-guide.md)
18. [市场︰ Kaspersky AV Linux 文件服务器](http://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

## 基础知识

1. [基础知识︰ Azure 的命令行界面 (Azure CLI)](../xplat-cli.md)
4. [基础知识︰ 证书使用和管理](http://msdn.microsoft.com/library/azure/gg981929.aspx)
5. [基本操作︰ 选择 Linux 用户名](virtual-machines-linux-usernames.md)
6. [基础知识︰ 登录到 Linux 虚拟机使用 Azure 门户](virtual-machines-linux-how-to-log-on.md)
7. [基础知识︰ SSH](virtual-machines-linux-use-ssh-key.md)
8. [基础知识︰ 如何对 Linux 重置密码或 SSH 属性](virtual-machines-linux-use-vmaccess-reset-password-or-ssh.md)
9. [基础知识︰ 使用根](virtual-machines-linux-use-root-privileges.md)
10. [基础知识︰ 附加到 Linux 虚拟机的数据磁盘](virtual-machines-linux-how-to-attach-disk.md)
11. [基础知识︰ 分离从 Linux 虚拟机数据磁盘](virtual-machines-linux-how-to-detach-disk.md)
12. [撰写网络日志基础知识︰ 优化存储、 磁盘和使用 Linux 和 Azure 的性能](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
13. [基础知识︰ RAID](virtual-machines-linux-configure-raid.md)
14. [基础知识︰ 捕获 Linux 虚拟机来创建模板](virtual-machines-linux-capture-image.md)
15. [基础知识︰ Azure Linux 代理](virtual-machines-linux-agent-user-guide.md)
16. [基础知识︰ Azure VM 扩展和功能](http://msdn.microsoft.com/library/azure/dn606311.aspx)
17. [基础知识︰ 注入到 VM 使用云初始化自定义数据](virtual-machines-how-to-inject-custom-data.md)
18. [撰写网络日志基础知识︰ 构建高度可用 Linux 在 Azure 中 12 个步骤](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
19. [撰写网络日志基础知识︰ 自动化资源调配的 Linux 使用 Azure CLI，node.js，jhawk Azure 上](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
19. [创建多个 VM 部署使用 Azure CLI](virtual-machines-create-multi-vm-deployment-../xplat-cli.md)
20. [基础知识︰ Azure Docker VM 扩展](virtual-machines-docker-vm-extension.md)
23. [Azure 服务管理 REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)参考
24. [在 Azure 上 GlusterFS](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

## 社区的图像和资料库
3. [MSOpenTech 虚拟机厂](https://vmdepot.msopentech.com/List/Index)&mdash;社区提供虚拟机映像。
4. [GitHub](https://github.com/Azure/)&mdash; Azure CLI 中，和许多其他工具和项目。
5. [Docker 集线器注册表](https://registry.hub.docker.com/)&mdash; Docker 容器图像注册表。

## 语言和平台
### [Azure 的 Java 开发人员中心](http://azure.microsoft.com/develop/java/)

1. [图片](https://vmdepot.msopentech.com/List/Index?sort=Featured&search=java)
2. [如何︰ 从 Java 服务总线使用 AMQP 1.0](http://msdn.microsoft.com/library/azure/jj841073.aspx)
3. [如何︰ 设置 Tomcat7 Linux 使用 Azure 门户](virtual-machines-linux-setup-tomcat7-linux.md)
4. [视频︰ Azure Java SDK 服务管理](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-157-The-Java-SDK-for-Azure-Management-with-Brady-Gaster)
5. [日志︰ 开始使用 Java 的 Azure 管理库](http://azure.microsoft.com/blog/2014/09/15/getting-started-with-the-azure-java-management-libraries/)
5. [GitHub repo: Eclipse 使用 Java 的 Azure Toolkit](https://github.com/MSOpenTech/WindowsAzureToolkitForEclipseWithJava)
6. [引用︰ 对 Eclipse 使用 Java 的 Azure Toolkit](http://msdn.microsoft.com/library/azure/hh694271.aspx)
7. [GitHub repo: IntelliJ 的想法和 Android Studio MS 开放的技术工具插件](https://github.com/MSOpenTech/msopentech-tools-for-intellij)
7. [博客︰ MSOpenTech 分配给 OpenJDK](http://msopentech.com/blog/2014/10/21/ms-open-techs-first-contribution-openjdk/)
8. [图像︰ WebSphere](http://azure.microsoft.com/marketplace/partners/msopentech/was-8-5-was-8-5-5-3/)
9. [图像︰ WebLogic](http://azure.microsoft.com/marketplace/?term=weblogic)
10. [在 Windows 上的图像︰ JDK6](http://azure.microsoft.com/marketplace/partners/msopentech/jdk6onwindowsserver2012/)
11. [在 Windows 上的图像︰ JDK7](http://azure.microsoft.com/marketplace/partners/msopentech/jdk7onwindowsserver2012/)
12. [在 Windows 上的图像︰ JDK8](http://azure.microsoft.com/marketplace/partners/msopentech/jdk8onwindowsserver2012r2/)

### JVM 语言

1. [在 Azure 的云服务的 Scala︰ 运行播放框架应用程序](http://msopentech.com/blog/2014/09/25/tutorial-running-play-framework-applications-microsoft-azure-cloud-services-2/)

### SDK 类型，安装升级
4. [Azure 服务管理 SDK: Java](http://dl.windowsazure.com/javadoc/)
5. [Azure 服务管理 SDK︰ 转](https://github.com/MSOpenTech/azure-sdk-for-go)
5. [Azure 服务管理 SDK︰ 拼音](https://github.com/MSOpenTech/azure-sdk-for-ruby)
    - [如何︰ 安装在导轨上的拼音](virtual-machines-ruby-rails-web-app-linux.md)
6. [Azure 服务管理 SDK: Python](https://github.com/Azure/azure-sdk-for-python)
    - [如何︰ Django Hello World Web 应用程序 (Mac Linux)](virtual-machines-python-django-web-app-linux.md)
7. [Azure 服务管理 SDK: Node.js](https://github.com/MSOpenTech/azure-sdk-for-node)
8. [Azure 服务管理 SDK: PHP](https://github.com/MSOpenTech/azure-sdk-for-php)
    - [如何︰ 在 Azure 的虚拟机上安装灯泡堆栈](virtual-machines-linux-install-lamp-stack.md)
    - [视频︰ 在 Azure 的虚拟机上安装灯泡堆栈](http://channel9.msdn.com/Shows/Azure-Friday/LAMP-stack-on-Azure-VMs-with-Guy-Bowerman)
9. [Azure 服务管理 SDK:.NET](https://github.com/Azure/azure-sdk-for-net)
10. [博客︰ 单声道、 ASP.NET 5、 Linux 和 Docker](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)

## 示例和脚本

查找此节可迅速填满。 如果您有建议，向我们发送领料/收货或将其保留在评论，下面。

1. [创建多个 VM 部署使用 Azure CLI](virtual-machines-create-multi-vm-deployment-../xplat-cli.md)
2. [Patrick Chanezon Azure Linux GitHub 存储库](https://github.com/chanezon/azure-linux)
3. [视频︰ 如何使用**usbip** Azure 到 Linux 上移动内部 USB 数据](http://channel9.msdn.com/Blogs/Open/On-premises-USB-devices-on-Linux-on-Azure-via-usbip)
4. [视频︰ 访问在浏览器中使用 fernapp Azure 上基于 Linux 的 GUI](http://channel9.msdn.com/Blogs/Open/Accessing-Linux-based-GUI-on-Azure-over-browser-with-fernapp)
5. [视频︰ 共享存储上 Linux 使用 Azure 文件预览 — 第 1 部分](http://channel9.msdn.com/Blogs/Open/Shared-storage-on-Linux-via-Azure-Files-Preview-Part-1)
6. [视频︰ 接纳在 Azure 使用服务总线和 Web 站点上的 Linux 设备](http://channel9.msdn.com/Blogs/Open/Embracing-Linux-devices-on-Azure-via-Service-Bus-and-Web-Sites)
7. [视频︰ memcached 基于 Linux 本机应用程序连接到 Azure](http://channel9.msdn.com/Blogs/Open/Connecting-a-Linux-based-native-memcache-application-to-Windows-Azure)
8. [视频︰ 负载平衡在 Azure 上具有高可用性的 Linux 服务︰ 二进制和 MySQL](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL)


## Data

本节包含几个不同的存储方法和技术，包括 NoSQL、 关系和大数据有关的信息。

### NoSQL

1. [Azure 的博客︰ 8 开源 NoSql 数据库](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
2. Couchdb
    - [在 Azure 上 CouchDb Slideshare (MSOpenTech): 经验](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
    - [博客︰ 运行 CouchDB 作为-服务与 node.js、 CORS 和 Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)
3. MongoDB
    - [如何︰ 在使用 MongoDB 使用 MongoLab 加载项的 Azure 上创建 Node.js 应用程序](store-mongolab-web-sites-nodejs-store-data-mongodb.md)
4. 卡桑德拉
    - [如何︰ 使用 Azure 和 Node.js 从访问 Linux 运行卡桑德拉](virtual-machines-linux-nodejs-running-cassandra.md)
5. Redis
    - [在 Windows Azure Redis 缓存服务中 Redis 的博客︰](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
    - [博客︰ 宣布 Redis 预览版的 ASP.NET 会话状态提供程序](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)
6. RavenHQ
    - [博客︰ RavenHQ 现已提供在 Azure 的市场](http://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### 大数据
2. Hadoop/Cloudera  
    - [博客︰ 在 Azure 的 Linux 虚拟机上安装 Hadoop](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
    - [如何︰ 开始使用 Hadoop 并使用 HDInsight 配置单元](hdinsight-get-started.md)  
3. [Azure HDInsight](http://azure.microsoft.com/services/hdinsight/) — 在 Azure 上完全托管的 Hadoop 服务。

### 关系数据库
2. MySQL
    - [如何︰ 安装和运行 MySQL](virtual-machines-linux-mysql-use-opensuse.md)
    - [如何︰ 优化性能的 MySQL 在 Azure 上](virtual-machines-linux-optimize-mysql-perf.md)
    - [如何︰ MySQL 群集](virtual-machines-linux-mysql-cluster.md)
    - [如何︰ 创建使用市场上一个 MySQL 数据库](store-php-create-mysql-database.md)
    - [如何︰ Django 和 MySQL Python 和 Visual Studio Azure 网站上](web-sites-python-ptvs-django-mysql.md)
    - [如何︰ PHP 和 MySQL WebMatrix 与 Azure 网站上](web-sites-php-mysql-use-webmatrix.md)
    - [在 Microsoft Azure 的 MySQL 高可用性体系结构](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
7. MariaDB
    - [如何︰ 创建 MariaDbs 的多主机群集](virtual-machines-mariadb-cluster.md)
8. [安装 corosync，使用 ILB pg_bouncer Postgres](https://github.com/chgeuer/postgres-azure)


## 身份验证和加密

身份验证和加密是在软件开发中的关键主题和很多、 很多主题网站上，描述了如何学习和使用适当的安全技术都有。 我们将介绍一些启动并快速运行 Linux 和源代码开放的工作负载，以及指向工具用于重置或删除在 Azure 上的远程安全功能的基本用法。 这些都是基本的过程，我们将添加更复杂的方案很快。

4. [基础知识︰ 证书使用和管理](http://msdn.microsoft.com/library/azure/gg981929.aspx)
7. [基础知识︰ SSH](virtual-machines-linux-use-ssh-key.md)
8. [基础知识︰ 如何对 Linux 重置密码或 SSH 属性](virtual-machines-linux-use-vmaccess-reset-password-or-ssh.md)
9. [基础知识︰ 使用根](virtual-machines-linux-use-root-privileges.md)

## Linux 高性能计算 (HPC)

运行 Linux 虚拟机群集构建与开放源代码工具或 Microsoft HPC 包 HPC 的工作负载。

1.  [快速启动模板︰ SLURM 群集启动](http://azure.microsoft.com/documentation/templates/slurm/)（和[博客文章](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)）
2.  [快速启动模板︰ 运转扭矩群集](http://azure.microsoft.com/documentation/templates/torque-cluster/)
3.  [快速启动模板︰ 创建带有 Linux 计算节点的 HPC 群集](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)
4.  [教程︰ 开始使用 Linux 计算在 Azure 中包 HPC 群集中的节点](virtual-machines-linux-cluster-hpcpack.md)
5.  [教程︰ 在 Linux 上的运行 Microsoft HPC 包 NAMD 计算在 Azure 中的节点](virtual-machines-linux-cluster-hpcpack-namd.md)
6.  [教程︰ 设置 Linux RDMA 群集运行 MPI 应用程序](virtual-machines-linux-cluster-rdma.md)


## Devops、 管理和优化

本节开头包含一系列视频在博客[视频︰ Azure 虚拟机︰ 使用厨师的简历、 傀儡和管理 Linux 虚拟机 Docker](http://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)。 然而，devops、 管理和优化的世界是很广，而且不断变化速度非常快，所以应考虑下面的起始点的列表。

1. Docker
    - [在 Azure 上 linux docker VM 扩展](virtual-machines-docker-vm-extension.md)
    - [使用命令行 Azure Docker VM 扩展接口 (Azure CLI)](virtual-machines-docker-with-../xplat-cli.md)
    - [使用从 Azure 预览门户 Docker VM 扩展](virtual-machines-docker-with-portal.md)
    - [快速入门 Docker Azure 市场](virtual-machines-docker-ubuntu-quickstart.md)
    - [如何在 Azure 上使用 docker 机](virtual-machines-docker-machine.md)
    - [如何使用 Azure 上蜂群 docker](virtual-machines-docker-swarm.md)
    - [开始使用 Docker 和 Azure 上撰写](virtual-machines-docker-compose-quickstart.md)

2. [与 CoreOS 的车库](virtual-machines-linux-coreos-how-to.md)
3. Deis
    - [GitHub repo︰ 在 Azure 上 CoreOS 群集上安装 Deis](https://github.com/chanezon/azure-linux/tree/master/coreos/deis)
4. Kubernetes
    - [与 CoreOS 和编织的自动化 Kubernetes 群集部署完整指南](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
    - [Kubernetes 可视化工具](http://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure)
5. Jenkins 和编写
    - [博客︰ Jenkins 从 Azure 插件](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
    - [GitHub repo: Jenkins 存储用于 Azure 的插件](https://github.com/jenkinsci/windows-azure-storage-plugin)
    - [第三方︰ 编写从 Azure 插件](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
    - [第三方︰ 编写存储用于 Azure 的插件](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
10. 厨师的简历
    - [厨师的简历和虚拟机](virtual-machines-windows-install-chef-client.md)
    - [视频︰ 什么是厨师的简历，以及它是如何工作？](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

12. Azure 的自动化
    - [视频︰ 如何对 Linux 虚拟机使用 Azure 的自动化](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
13. 用于 Linux 的 DSC Powershell
    - [博客︰ 如何为 Linux 做 Powershell DSC](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
    - [GitHub: Docker 客户端 DSC](https://github.com/anweiss/DockerClientDSC)
13. [Ubuntu Juju](https://juju.ubuntu.com/docs/config-azure.html)
14. [Azure 的打包程序插件](https://github.com/msopentech/packer-azure)

## 支持、 故障排除和"它只是不起作用"

1. Microsoft 的支持文档
    - [在 Microsoft Azure 上 Linux 映像的支持︰ 支持](http://support2.microsoft.com/kb/2941892)

<!--Anchors-->
[Distros]: #distros
[基础知识]: #basics
[社区的图像和资料库]: #images
[语言和平台]: #langsandplats
[示例和脚本]: #samples
[身份验证和加密]: #security
[Devops、 管理和优化]: #devops
[支持、 故障排除和"它只是不起作用"]: #supportdebug

<!--Link references--In actual articles, you only need a single period before the slash. -->
[如何在 Azure 上使用 docker 机]: virtual-machines-docker-machine.md
[如何使用 Azure 上蜂群 docker]: virtual-machines-docker-swarm.md
