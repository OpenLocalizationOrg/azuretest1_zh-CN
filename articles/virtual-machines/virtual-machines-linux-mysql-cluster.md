---
ms.openlocfilehash: 015e5717cd066e31233fbc0bd1340f9167969a23
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用负载平衡设置 clusterize MySQL 在 Linux 上"
    description="一篇文章，阐释模式安装 Linux 的负载平衡、 高可用性群集作为示例使用 MySQL 的 Azure 上"
    services="virtual-machines"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>

# 使用负载平衡设置 clusterize MySQL 在 Linux 上

* [做好准备](#getting-ready)
* [设置群集](#setting-up-the-cluster)
* [设置 MySQL](#setting-up-mysql)
* [设置 Corosync](#setting-up-corosync)
* [设置 Pacemaker](#setting-up-pacemaker)
* [测试](#testing)
* [STONITH](#stonith)
* [限制](#limitations)

## 简介

本文的目的是探索并说明可用于部署高可用性基于 Linux 的服务在 Microsoft Azure，探索作为入门的 MySQL 服务器高可用性上的不同方法。 视频演示了这种方法是提供[第 9 频道](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL)上。

我们列出了一个共享型两个节点单主机 MySQL 高可用性解决方案基于 DRBD、 Corosync 和 Pacemaker。 只有一个节点在每次运行 MySQL。 读取和写入 DRBD 资源一次还是限制为只有一个节点。

由于我们使用 Microsoft Azure 负载平衡设置可提供循环功能和端点检测、 删除和 VIP 平稳地恢复，则无需像 LVS 的 VIP 解决方案。 VIP 是我们第一次创建云服务时由 Microsoft Azure 指定全局路由 IPv4 地址。

作为虚拟机[虚拟机安装盘](http://vmdepot.msopentech.com)上有可用其他可能的体系结构，包括 5x9 下一工作日群集、 Percona Galera 以及几个中间件解决方案，其中包括至少一个 MySQL 的。 只要这些解决方案可以复制到单播与多播或广播上，并且不依赖于共享的存储位置或多个网络接口，这些方案应该很容易在 Microsoft Azure 上部署。

当然这些群集体系结构可以扩展到其他产品，如 PostgreSQL 和形式上类似的方式。 例如，使用多主机二进制，成功测试此负载平衡与无共享的过程和观看我们频道 9 在博客上。

## 做好准备

您将需要能够创建至少两 （2） 虚拟机有效订购的 Microsoft Azure 帐户 （X 是本示例中使用），网络和子网、 相似性组和可用性设置，以及在同一区域在云服务，创建新的 Vhd 并将它们附加到 Linux 虚拟机的能力。

### 测试的环境

- Ubuntu 13.10
  - DRBD
  - MySQL 服务器
  - Corosync 和 Pacemaker

### 仿射性组

通过登录到设置滚动 Azure 门户并创建新的关联组创建解决方案关联组。 以后创建的已分配的资源将分配给该组的相似性。

### 网络

创建一个新的网络，并在网络内创建一个子网。 我们选择了一个/24 子网内的 10.10.10.0/24 网络。

### 虚拟机

使用 Endorsed Ubuntu 库图像，创建第一个 Ubuntu 13.10 VM 并将其称为`hadb01`。 在名为 hadb 的进程中创建新的云服务。 我们称之为这种方式来说明服务会有当我们添加更多的资源的共享、 负载平衡特性。 创建的`hadb01`是一般的和已完成使用门户。 自动创建 SSH 的终结点，并且我们创建的网络中被选中。 我们还选择创建新的虚拟机设置的可用性。

（从技术上讲，云服务创建时） 创建第一个 VM 之后我们继续创建第二个虚拟机， `hadb02`。 对于第二个 VM 我们将还使用 Ubuntu 13.10 VM 库中使用门户，但我们会选择使用现有的云服务， `hadb.cloudapp.net`，而不是创建一个新。 应为我们自动选择网络和可用性设置。 SSH 创建终结点将太。

在创建两个虚拟机之后，我们将注意的 SSH 端口的`hadb01`(TCP 22) 和`hadb02`（自动分配的 Azure）

### 连接的存储

我们将新磁盘连接到这两个虚拟机，并在进程中创建新的 5 GB 磁盘。 磁盘将承载我们的主操作系统磁盘使用 VHD 容器中。 一旦创建和附加磁盘没有必要为我们重新启动 Linux 内核将会看到新设备 (通常`/dev/sdc`，您可以检查`dmesg`的输出)

在每个虚拟机上，我们继续创建新的分区使用`cfdisk`（主，Linux 分区） 和写入新的分区表。 **不会创建该分区的文件系统**。

## 设置群集

在这两种 Ubuntu Vm 中，我们需要使用 APT 安装 Corosync Pacemaker，DRBD。 使用`apt-get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**不要安装 MySQL 这次**。 Debian 和 Ubuntu 安装脚本会在初始化 MySQL 数据目录`/var/lib/mysql`，但 DRBD 文件系统将会取代目录，因为我们需要以后再执行此操作。

此时我们还应该验证 (使用`/sbin/ifconfig`) 这两个虚拟机都使用 10.10.10.0/24 子网中的地址和，他们可以通过名称 ping 对方。 如果需要也可以使用`ssh-keygen`， `ssh-copy-id` ，以确保这两个虚拟机可以通过 SSH 通信无需密码。

### DRBD 设置

我们将创建使用基础的 DRBD 资源`/dev/sdc1`分区以产生`/dev/drbd1`资源能够被使用 ext3 格式和用于主要和次要节点。 若要执行此操作，请打开`/etc/drbd.d/r0.res`和复制下面的资源定义。 在这两个虚拟机中执行此操作︰

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

这样后, 初始化资源使用`drbdadm`在这两个虚拟机︰

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

最后，在上主 (`hadb01`) 强制 DRBD 资源的所有权 （主）︰

    sudo drbdadm primary --force r0

如果您检查 drbd/过程/内容 (`sudo cat /proc/drbd`) 在这两个虚拟机，您应看到`Primary/Secondary`上`hadb01`和`Secondary/Primary`在`hadb02`、 一致与此时的解决方案。 5 GB 磁盘将通过免费向客户 10.10.10.0/24 网络同步。

一旦同步磁盘上，您可以创建文件系统`hadb01`。 出于测试目的，我们使用 ext2，但下面的指令将创建一个 ext3 文件系统︰

    mkfs.ext3 /dev/drbd1

### 挂载 DRBD 资源

在`hadb01`我们现在可以挂载 DRBD 资源。 Debian 和衍生品使用`/var/lib/mysql`为 MySQL 的数据目录。 由于我们还没有安装 MySQL，我们将创建目录，并装载 DRBD 资源。 On `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## 设置 MySQL

现在，您就可以在安装 MySQL `hadb01`:

    sudo apt-get install mysql-server

为`hadb02`，有两种选择。 您可以安装 mysql 服务器，以创建 /var/lib/mysql 和填充新数据目录，然后继续操作，删除的内容。 On `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

第二个选项是故障转移到`hadb02`，然后安装那里 mysql 服务器 （安装脚本会注意到现有的安装并不会动它）

On `hadb01`:

    sudo drbdadm secondary –force r0

On `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

如果不立即计划故障转移 DRBD，第一个选项是虽然可以说更少的优雅更容易。 对此进行设置后，您可以开始处理 MySQL 数据库。 在`hadb02`（或任何一种服务器处于活动状态，根据 DRBD）︰

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**警告**︰ 此最后一条语句将有效地禁用此表中的根用户的身份验证。 这应由生产级授予语句并仅用于说明目的。

您还需要启用网络为 MySQL 如果您想要查询从外部虚拟机，这是本指南的目的。 打开这两个虚拟机， `/etc/mysql/my.cnf` ，然后浏览到`bind-address`，来自 127.0.0.1 将其更改为 0.0.0.0。 保存该文件之后, 发出`sudo service mysql restart`您当前在主计算机上。

### 创建 MySQL 负载平衡设置

我们将回到 Azure 门户网站，并浏览到`hadb01`虚拟机，然后终结点。 我们将创建一个新的终结点，选择 MySQL (TCP 3306) 下拉列表和*创建新的负载平衡组*框上的刻度线。 我们将其称为我们负载平衡的端点`lb-mysql`。 我们将离开的大多数选项仅除了时间，我们将减少到 5 （秒，最小值）

创建终结点后我们转到`hadb02`，终结点和创建新的终结点，但我们将选择`lb-mysql`，然后从下拉菜单中选择 MySQL。 您还可以为此步骤使用 Azure CLI。

目前我们拥有一切我们需要为群集中的手动操作。

### 测试负载平衡设置

从外部的计算机可以执行测试，这种情况下使用任何 MySQL 客户端，以及应用程序 (例如，运行与 Azure 网站 phpMyAdmin) 我们使用 MySQL 的命令行工具在另一个 Linux 中︰

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### 手动故障转移

您可以通过关闭 MySQL，切换 DRBD 的主，并重新启动 MySQL 现在模拟故障转移。

在 hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

然后，在 hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

一旦您故障切换手动可以重复执行远程查询和它应该能够完美运行。

## 设置 Corosync

Corosync 是 Pacemaker 工作所需的基础群集结构。 心跳 v1 和 v2 的用户 （以及其他方法，如 Ultramonkey） Corosync 是 CRM 的功能，一个拆分 Pacemaker 保持更类似于检测信号，在功能中。

在 Azure 上 Corosync 的主要约束是 Corosync 喜欢多播广播通过单播通信，但 Microsoft Azure 网络只支持单址广播。

幸运的是，Corosync 已经使用单址广播模式，唯一真正的约束，因为所有节点都不能都通信之间互相*自动*，您需要在您的配置文件，包括其 IP 地址定义节点。 我们可以使用单址广播和只需更改绑定地址、 节点列表和日志记录目录 Corosync 示例文件 (使用 Ubuntu`/var/log/corosync`示例文件使用`/var/log/cluster`) 和启用仲裁工具。

**注意`transport: udpu`下面的指令，并手动定义的节点的 IP 地址**。

在`/etc/corosync/corosync.conf`两个节点︰

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

我们在这两个虚拟机中复制此配置文件并启动 Corosync 两个节点中︰

    sudo service start corosync

不久之后启动服务群集应该建立在当前环，应构成仲裁。 我们可以通过查看日志来检查此功能或︰

    sudo corosync-quorumtool –l

应按照类似于下图所示的输出︰

![corosync quorumtool-l 示例输出](media/virtual-machines-linux-mysql-cluster/image001.png)

## 设置 Pacemaker

Pacemaker 使用群集资源的监视，定义原色下移并切换到次映像的这些资源。 从一组可用的脚本或 LSB （类似的 init） 脚本，其他选项，则可以定义资源。

我们希望 Pacemaker 要"拥有"DRBD 资源、 装入点和 MySQL 服务。 如果 Pacemaker 可以打开和关闭 DRBD，将其装载/卸载它，启动/停止 MySQL 按正确的顺序当东西坏反应与主，我们的设置已完成。

首次安装 Pacemaker 时，您的配置应该是简单，如下所示︰

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

通过运行中检查`sudo crm configure show`。 现在，创建一个文件 (说， `/tmp/cluster.conf`) 与下列资源︰

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"

    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"

    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"

    primitive mysqld lsb:mysql

    group mysql fs_mysql mysqld

    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master

    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start

    property stonith-enabled=false

    property no-quorum-policy=ignore

并立即将其加载到配置 （您只需在一个节点执行此操作）︰

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

另外，请确保两个节点中启动时，启动 Pacemaker:

    sudo update-rc.d pacemaker defaults

几秒钟后，和使用`sudo crm_mon –L`，请您的节点之一已成为该群集的主，并且正在运行的所有资源。 您可以使用装载和 ps 检查资源正在运行。

下面的屏幕快照演示`crm_mon`在一个节点停止 （退出使用控件-C）

![crm_mon 节点停止](media/virtual-machines-linux-mysql-cluster/image002.png)

然后，此屏幕抓图显示了具有一个主机和一个从属的两个节点︰

![crm_mon 操作主设备/从设备](media/virtual-machines-linux-mysql-cluster/image003.png)

## 测试

我们可进行自动故障切换功能模拟。 这样的两种方法︰ 软和硬。 软的方法使用群集的关机功能︰ ``crm_standby -U `uname -n` -v on``。 使用此母版上，从属服务器将接管。 请记住这将重新设置为 off （crm_mon 将告诉您一个节点否则是待机状态）

硬方式是关闭主虚拟机 (hadb01) 通过该门户或更改 VM （即暂停、 关机） 上的运行级别，然后我们通过发送信号下的主控形状的日常帮助 Corosync 和 Pacemaker。 我们可以对此进行测试 （适用于维护窗口），但我们也可以通过只冻结 VM 强制方案。

## STONITH

它应能发出通过 Azure CLI 而不是一个 STONITH 脚本，控制物理设备的 VM 关机。 您可以使用`/usr/lib/stonith/plugins/external/ssh`为基础并在群集配置中的启用 STONITH。 Azure CLI 应全局安装和发布设置/配置文件应加载群集的用户。

在[GitHub](https://github.com/bureado/aztonith)上可用资源的示例代码。 您需要添加到以下更改群集配置`sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**注意︰**检查上下，不会执行该脚本。 原始的 SSH 资源有 15 ping 检查但 Azure VM 的恢复时间可能更多变量。

## 限制

以下限制适用︰

- 作为 Pacemaker 使用中的资源管理 DRBD linbit DRBD 资源脚本`drbdadm down`关闭节点，即使节点只会待机时。 由于从属服务器将不会同步 DRBD 资源主机获取写入操作时，这并不理想。 如果在母版不会 graciously 失败，从属服务器可以花较旧的文件系统状态。 有两种可能的方法来解决这︰
  - 强制执行`drbdadm up r0`在所有群集节点，通过一个本地 (不是 clusterized) 的监视程序，或者，
  - 编辑 linbit DRBD 编写脚本以确保`down`未调用，在`/usr/lib/ocf/resource.d/linbit/drbd`。
- 负载平衡器需要至少 5 秒钟，以响应，因此应用程序应支持群集并将更能容忍超时;其他体系结构还可以帮助，例如在应用程序队列、 查询 middlewares 等。
- MySQL 调整是有必要确保书写完成清醒的步调和高速缓存刷新到磁盘经常尽可能最小化内存丢失
- 写性能将会更依赖于虚拟机互连虚拟交换机中，因为这是使用 DRBD 复制设备的机制
 