---
ms.openlocfilehash: 7dc02faf051430b421cd73a7fefd2dbfc0fb959f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 上运行的 MariaDB (MySQL) 群集"
    description="创建 MariaDB + Galera MySQL 群集在 Azure 的虚拟机"
    services="virtual-machines"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# MariaDB (MySQL) 群集-Azure 的教程

<p>我们创建的[MariaDBs](https://mariadb.org/en/about/)，MySQL 在 Azure 虚拟机的高可用性环境中工作的可靠、 可伸缩且可靠的顺便替代多主机[Galera](http://galeracluster.com/products/)群集。</p>

## 体系结构概述

本主题将执行以下步骤︰

1. 创建一个 3 节点的群集
2. OS 磁盘中分隔数据磁盘
3. 创建数据磁盘分条 RAID 0/设置，以提高 IOPS
4. 使用 Azure 负载平衡器 3 个节点的负载平衡
5. 要尽量减少重复工作，创建包含 MariaDB + Galera VM 映像并使用它来创建群集虚拟机。

![体系结构](./media/virtual-machines-mariadb-cluster/Setup.png)

> [AZURE.NOTE]  本主题使用[Azure CLI]工具，因此，请确保下载它们并将它们连接到 Azure 订阅根据的说明。 如果您需要在 Azure CLI 命令参考，签出的[Azure CLI 命令参考]此链接。 您还需要[创建 SSH 密钥进行身份验证]并记下的**.pem 文件的位置**。


## 创建模板

### 基础结构

1. 创建要在一起持有这些资源关系组

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. 创建虚拟网络

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. 创建存储帐户来承载我们的所有磁盘。 请注意，您不应该将放置超过 40 很大程度上相同的存储帐户使用磁盘以避免达到 20000 IOPS 存储帐户限制。 在这种情况下，我们很远，从这个数字，因此我们会将所有内容存储在同一帐户为简单起见

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. 查找 CentOS 7 虚拟机映像的名称

        azure vm image list | findstr CentOS
这将输出类似于`5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`。 在以下步骤中使用的名称。

4. 创建虚拟机模板**/path/to/key.pem**替换生成的.pem SSH 密钥存储的路径

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. 4 x 500 GB 数据磁盘附着在 RAID 配置中使用虚拟机

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. 在**mariadbhatemplate.cloudapp.net:22**上创建和使用您的私钥进行连接的 VM 模板到 SSH。

### 软件

1. 获取根

        sudo su

2. 安装 RAID 支持︰

     - 安装 mdadm

                yum install mdadm

     - EXT4 文件系统创建条带 RAID0/配置

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - 创建装载点目录

                mkdir /mnt/data

     - 检索新创建的 RAID 设备 UUID

                blkid | grep /dev/md0

     - 编辑 /etc/fstab

                vi /etc/fstab

     - 若要启用自动重新启动时替换 UUID 值从之前的**blkid**命令获得的 mouting 中添加设备

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - 装入新的分区

                mount /mnt/data

3. 安装 MariaDB:

     - 创建 MariaDB.repo 文件︰

                vi /etc/yum.repos.d/MariaDB.repo

     - 填充它与下面的内容

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - 删除现有后缀和 mariadb-库以避免冲突

            yum remove postfix mariadb-libs-*

     - 安装 Galera MariaDB

            yum install MariaDB-Galera-server MariaDB-client galera

4. 将 MySQL 数据目录移动到 RAID 块设备

     - 将当前的 MySQL 目录复制到新位置，并移除旧的目录

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - 在新目录中相应地设置权限

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - 创建 RAID 分区上的新位置中指向旧的目录的符号链接

            ln -s /mnt/data/mysql /var/lib/mysql

5. 因为[SELinux 会影响群集操作](http://galeracluster.com/documentation-webpages/configuration.html#selinux)，就需要为当前会话 （直到出现兼容版本） 将其禁用。 编辑`/etc/selinux/config`为后续重新启动禁用它︰

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. 验证 MySQL 运行

    - 启动 MySQL

            service mysql start

    - 保护 MySQL 安装、 设置 root 密码、 删除匿名用户，禁用远程根用户登录，并删除测试数据库

            mysql_secure_installation

    - 在群集操作和 （可选） 您的应用程序的数据库中创建用户

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - 停止 MySQL

            service mysql stop

7. 创建配置占位符

    - 编辑 MySQL 配置创建的群集设置的占位符。 不要替换**`<Vairables>`**或立即取消注释。 我们从该模板创建虚拟机后会发生。

            vi /etc/my.cnf.d/server.cnf

    - 编辑**[galera]**部分并清除它

    - 编辑**[mariadb]**部分

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. 打开所需的端口上 （使用 CentOS 7 FirewallD） 防火墙

    - MySQL 中︰ `firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - GALERA IST: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - 重新加载防火墙︰ `firewall-cmd --reload`

9.  优化系统的性能。 请参考本文[性能优化策略]的更多详细信息

    - 重新编辑 MySQL 配置文件

            vi /etc/my.cnf.d/server.cnf

    - 编辑**[mariadb]**部分并追加下面

    > [AZURE.NOTE] 所以建议**innodb\_缓冲区\_pool_size**的虚拟机的内存的 70%。 它已设置为 2.45 gb 此处用于中等 Azure VM 3.5 gb 的 RAM。

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. 停止 MySQL，MySQL 从禁用服务在启动时为避免会弄乱群集添加一个新节点时运行并取消设置机器。

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. 捕获通过门户 VM。 （目前，[问题在 Azure CLI 中的 # 1268年]工具描述事实 Azure CLI 工具捕获的图像不会捕获这些附加的数据磁盘。）

    - 通过门户关机
    - 单击捕获和图像名称指定为**mariadb galera 图像**提供说明并检查"已运行 waagent"。
    ![获取虚拟机](./media/virtual-machines-mariadb-cluster/Capture.png)
    ![捕获虚拟机](./media/virtual-machines-mariadb-cluster/Capture2.PNG)

## 创建群集

创建 3 Vm 模板，您只需创建配置并启动群集之外。

1. 创建第一个 CentOS 7 VM 从您创建的**mariadb galera 图像**映像，提供虚拟网络名称**mariadbvnet**和**mariadb**的子网，加工尺寸**中等**，在云服务中传递命名为**mariadbha** （或想要通过 mariadbha.cloudapp.net 来访问任意名称），设置此名称加工**mariadb1**和用户名为**azureuser**，并启用 SSH 访问并传递 SSH 证书.pem 文件并替换**/path/to/key.pem**的路径，您存储中生成的.pem 的 SSH 密钥。

    > [AZURE.NOTE] 划分到多行，为了清楚起见，下面的命令，但您应该输入每个作为一条线。

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. 通过_连接_创建 2 更多虚拟机给当前创建**mariadbha**云服务，将**虚拟机名称**以及**SSH 端口**更改为一个唯一的端口与同一个云服务中的其他虚拟机不冲突。

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
并对 MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. 您需要为下一步获取每 3 虚拟机的内部 IP 地址︰

    ![获取 IP 地址](./media/virtual-machines-mariadb-cluster/IP.png)

4. SSH 到 3 虚拟机并在每个配置文件并进行编辑

        sudo vi /etc/my.cnf.d/server.cnf

    注释**`wsrep_cluster_name`**和**`wsrep_cluster_address`**通过删除**#**开头和验证它们确实属于您的希望。
    此外，替换**`<ServerIP>`**在**`wsrep_node_address`**，**`<NodeName>`**在**`wsrep_node_name`**与虚拟机的 IP 地址和名称分别并取消这些行也。

5. 在 MariaDB1 上启动群集并使其在启动时运行

        sudo service mysql bootstrap
        chkconfig mysql on

6. 启动 MySQL MariaDB2 和 MariaDB3，并让它在启动时运行

        sudo service mysql start
        chkconfig mysql on

## 负载平衡群集
当您创建群集虚拟机时，您可以将其添加到可用性设置称为**clusteravset**以确保它们对不同的故障并更新域和，Azure 永远不会不会在所有计算机上的维护一次。 这种配置可以满足必须支持该 Azure 服务级别协议 (SLA) 的要求。

现在您可以使用 Azure 负载平衡器来平衡我们 3 个节点之间的请求。

运行下面的命令使用 Azure CLI 的计算机上。
命令参数结构如下︰ `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

最后，因为 CLI 将负载平衡器探测间隔设置为 15 秒 （这可能有点太长），将其更改**终结点**在门户中任何虚拟机

![编辑终结点](./media/virtual-machines-mariadb-cluster/Endpoint.PNG)

再单击 Reconfigure The Load-Balanced 设置并转到下一步

![重新配置负载平衡的设置](./media/virtual-machines-mariadb-cluster/Endpoint2.PNG)

将探测间隔更改为 5 秒，然后保存

![更改探测间隔](./media/virtual-machines-mariadb-cluster/Endpoint3.PNG)

## 验证群集

努力工作是完成的。 群集现在应该可以访问`mariadbha.cloudapp.net:3306`将命中负载平衡器和平稳有效地传送 3 虚拟机之间的请求。

使用您最喜爱的 MySQL 客户端来连接或只是连接从一个虚拟机来验证工作该簇。

     mysql -u cluster -h mariadbha.cloudapp.net -p

然后创建一个新数据库并填充某些数据

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

将下表中

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动

在本文中，您创建了 3 节点 MariaDB + Galera 高可用性群集在 Azure 的虚拟机运行 CentOS 7。 虚拟机的负载平衡与 Azure 负载平衡器。

您可能需要看看[到群集 MySQL 在 Linux 上的另一种方法]以及如何[优化和测试 MySQL Azure Linux 虚拟机的性能]。

<!--Anchors-->
[体系结构概述]: #architecture-overview
[创建模板]: #creating-the-template
[创建群集]: #creating-the-cluster
[负载平衡群集]: #load-balancing-the-cluster
[验证群集]: #validating-the-cluster
[下一步行动]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[Azure CLI]: http://azure.microsoft.com/documentation/articles/xplat-cli/
[Azure 的 CLI 命令参考]: http://azure.microsoft.com/documentation/articles/virtual-machines-command-line-tools/
[创建 SSH 密钥作身份验证]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[性能优化策略]: http://azure.microsoft.com/documentation/articles/virtual-machines-linux-optimize-mysql-perf/
[优化和测试 MySQL Azure Linux 虚拟机的性能]:http://azure.microsoft.com/documentation/articles/virtual-machines-linux-optimize-mysql-perf/
[问题 #1268 Azure CLI 中]:https://github.com/Azure/azure-xplat-cli/issues/1268
[向群集 MySQL 在 Linux 上的另一种方法]: http://azure.microsoft.com/documentation/articles/virtual-machines-linux-mysql-cluster/
 