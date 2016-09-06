---
ms.openlocfilehash: 8d8fe7e161dd3697b4faa26857214c5df43477b7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure 的 Linux 虚拟机优化 MySQL 性能" 
    description="了解如何优化 MySQL 运行 Linux Azure 虚拟机 (VM) 上运行。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="NingKuang" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/21/2015" 
    ms.author="ningk"/>

#在 Azure 的 Linux 虚拟机优化 MySQL 性能 

有很多影响 Azure，MySQL 性能虚拟硬件的选择和软件配置中的两个因素。 本文侧重于通过存储、 系统和数据库配置的优化性能。

##利用 RAID Azure 的虚拟机上 
存储是会影响在云环境中的数据库性能的关键因素。  相比于单个磁盘，RAID 可提供通过并发的访问速度更快。  更多详细信息，请参阅[标准 RAID 级别](http://en.wikipedia.org/wiki/Standard_RAID_levels)。   

可通过 RAID 中显著提高磁盘 I/O 吞吐量和 Azure 中的 I/O 响应时间。 我们的实验室测试显示磁盘 I/O 吞吐量翻了一番，I/O 响应时间将缩短一半平均时的 RAID 磁盘数量增加了一倍 （从 2 到 4，4 至 8 等）。 有关详细信息，请参阅[附录 A](#AppendixA) 。  

除了磁盘 I/O，MySQL 提高性能提高的 RAID 级别。  有关详细信息，请参阅[附录 B](#AppendixB) 。  

您可能还需要考虑块区大小。 一般情况下时有较大的区块大小，则会降低开销，尤其是对于大的写操作。 但是，当块大小太大，可能将添加额外的系统开销，则不能利用 RAID。 当前的默认大小为 512 KB，这被证明是最佳的最一般的生产环境。 有关详细信息，请参阅[附录 C](#AppendixC) 。   

请注意您可以为不同的虚拟机类型添加多少磁盘上没有限制。 这些限制中[的虚拟机并为 Azure 的云服务规模](http://msdn.microsoft.com/library/azure/dn197896.aspx)详细。 尽管您可以选择使用更少的磁盘设置 RAID，您将需要 4 个附加的数据磁盘来执行在本文中，RAID 示例。  

本文假定您已经创建了一个 Linux 虚拟机并 MYSQL 安装和配置。 有关入门的更多信息请参考如何在 Azure 上安装 MySQL。  
  
###在 Azure 上 RAID 设置
以下步骤显示如何创建 RAID 在 Azure 上的使用 Windows Azure 管理门户。 您还可以设置 RAID 使用 Windows PowerShell 脚本。 在本例中我们将配置 4 个磁盘 RAID 0。  

####步骤 1︰ 将数据磁盘添加到您的虚拟机  

在 Azure 管理门户网站虚拟机页中，单击想要将数据磁盘添加到虚拟机。 在此示例中，虚拟机是 mysqlnode1。  

![][1]

在虚拟机页上单击**仪表板**。  

![][2]
 

在任务栏中，单击**附加**。
 
![][3]

然后单击**附加的空磁盘**。  

![][4]
 
对于数据磁盘，**主机缓存首选项**应设置为**None**。  

这将添加一个空白磁盘插入您的虚拟机。 重复此步骤三次，这样就存在 4 个数据磁盘的 RAID。  

通过查看内核消息日志，您可以查看虚拟机中的添加的驱动器。 例如，要在 Ubuntu 上看到此内容，请使用下面的命令︰  

    sudo grep SCSI /var/log/dmesg

####步骤 2︰ 创建具有更多磁盘 RAID
请按照本文详细 RAID 安装步骤︰  

[http://azure.microsoft.com/documentation/articles/virtual-machines-linux-configure-RAID/](http://azure.microsoft.com/documentation/articles/virtual-machines-linux-configure-RAID/)

>[AZURE.NOTE] 如果您使用的 XFS 文件系统，则按照下面的步骤创建 RAID 后。

在 Debian 安装 XFS，Ubuntu 或 Linux 薄荷，请使用下面的命令︰  

    apt-get -y install xfsprogs  

要安装 XFS 在 Fedora，CentOS，或者 RHEL，请使用下面的命令︰  

    yum -y install xfsprogs  xfsdump 


####步骤 3︰ 设置一个新的存储路径
使用下面的命令︰  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####步骤 4︰ 将原始数据复制到新的存储路径
使用下面的命令︰  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####步骤 5︰ 修改权限，这样，MySQL 可以访问 （读和写） 数据磁盘
使用下面的命令︰  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##调整磁盘 I/O 调度算法
Linux 可以实现四种类型的 I/O 调度算法︰  

-   NOOP 算法 （无操作）
-   截止日期算法 （截止日期）
-   完全公平队列算法 (CFQ)
-   预算期间算法 (Anticipatory)  

您可以选择不同 I/O 调度程序在不同的情形下，以优化性能。 在完全随机访问环境中，没有很大区别性能的 CFQ 和截止时间算法。 通常建议将 MySQL 数据库环境设置为稳定性的截止日期。 如果没有大量的顺序 I/O，CFQ 可能会减少磁盘 I/O 性能。   

对于 SSD 和其它设备，使用 NOOP 或截止日期可以获得性能优于默认计划程序。   

从 2.5 内核，默认 I/O 调度算法是最后期限。 从内核 2.6.18 开始，CFQ 将成为默认 I/O 调度算法。  您可以指定在内核引导时此设置或在系统运行时动态修改此设置。  

下面的示例演示如何检查和 NOOP 算法设置默认计划程序。  

Debian 分布族中︰

###步骤 1.查看当前的 I/O 调度程序
使用下面的命令︰  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler 

您将看到以下输出，它指示当前计划程序。  

    noop [deadline] cfq 


###第 2 步。 更改当前设备 (/ 开发/sda) 的 I/O 调度算法
使用以下命令︰  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] 将此值设置为 /dev/sda 单独不是有用的。 它需要将所有数据磁盘上数据库所在。  

您应看到下面的输出，指示已成功重新生成该 grub.cfg 和默认计划程序已经被更新为 NOOP。  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

对于 Redhat 分发系列，您只需要下面的命令︰   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

##配置系统文件的操作设置
最佳做法之一是禁用文件系统上的执行非常日志记录功能。 执行非常是最后一个文件的访问时间。 无论何时访问某个文件时，文件系统会在日志中记录的时间戳。 但是，很少使用此信息。 如果不需要它，这将降低总体磁盘访问时间，您可以禁用它。  
 
若要禁用执行非常日志记录，您需要修改文件系统配置文件 /etc/ fstab 并添加**noatime**选项。  

例如，编辑 vim /etc/fstab 文件中，添加如下所示的 noatime。  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

然后，重新安装文件系统使用以下命令︰  

    mount -o remount /RAID0

测试修改的结果。 请注意，当您修改测试文件，访问时间不更新。  

在之前的示例︰     

![][5]
 
之后的示例︰

![][6]

##增加最大值的高并发系统句柄
MySQL 是高并发数据库。 默认的并发的句柄数是 1024 linux，它并不总是足够。 **使用以下步骤来增加系统以支持高并发的 MySQL 的最大并发控点**。

###步骤 1︰ 修改 limits.conf 文件
增加最大允许的并发处理的 /etc/security/limits.conf 文件中添加以下四行。 请注意，65536 系统可以支持的最大数量。   

    * soft nofile 65536
    * hard nofile 65536
    * soft nproc 65536
    * hard nproc 65536

###步骤 2︰ 更新系统的新限制
运行以下命令︰  

    ulimit -SHn 65536
    ulimit -SHu 65536 

###第 3 步︰ 确保限制更新在系统启动时
在 /etc/rc.local 文件中作出下面的启动命令，以便它将会在每次启动时生效。  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##MySQL 数据库优化 
相同的性能调优策略可用于在本地计算机上作为 Azure 配置 MySQL。  

主 I/O 优化规则如下︰   

-   增加高速缓存大小。
-   减少了 I/O 响应时间。  

若要优化 MySQL 服务器设置，您可以更新 my.cnf 文件，则服务器和客户端计算机的默认配置文件。  

下面的配置项是影响 MySQL 性能的主要因素︰  

-   **innodb_buffer_pool_size**︰ 缓冲池中包含缓冲的数据和索引。 这通常设置为物理内存的 70%。
-   **innodb_log_file_size**︰ 这是重做日志的大小。 您可以使用重做日志以确保写操作快速、 可靠且可恢复瘫痪。 这是设置为 512 MB，将为您提供足够的空间用于记录的写操作。
-   **max_connections**︰ 有时应用程序不关闭连接正确。 更大的值可以使服务器回收 idled 的连接更多的时间。 最大连接数是 10000，但建议最大值为 5000。
-   **Innodb_file_per_table**︰ 此设置启用或禁用 InnoDB 存储在单独的文件中的表的能力。 启用选项将确保高级的管理的几个操作，可以有效地应用。 从性能角度来看，它可以提高表空间传输的速度和优化碎屑管理性能。 因此，这样的推荐的设置为 ON。</br>
    从 MySQL 5.6，默认设置为 ON。 因此，不不需要任何操作。 有关其他的版本，低于 5.6，默认设置为 OFF。 打开此是必需的。 并应将其应用之前加载数据时，因为只有新创建的表才会受到影响。
-   **innodb_flush_log_at_trx_commit**︰ 默认值为 1，使用范围设置为 0 ~ 2。 默认值是独立 MySQL 数据库为最适合的选项。 2 的设置使大多数的数据完整性和适合 MySQL 群集中的主机。 设置为 0 可允许数据丢失，这可能会影响可靠性，在某些情况下，具有更好的性能，适用于从 MySQL 群集中。
-   **Innodb_log_buffer_size**︰ 日志文件缓冲区允许事务，而无需刷新到磁盘的日志，事务提交之前运行。 但是，如果没有二进制大对象或文本字段，缓存将会很快耗尽，将触发频繁磁盘 I/O。 它是更好地增加缓冲区的大小，如果 Innodb_log_waits 状态变量不是 0。
-   **query_cache_size**︰ 最好从一开始就将其禁用。 将 query_cache_size 设置为 0 （这是默认设置 MySQL 5.6），并使用其他方法来提高查询速度。  
  
优化后的性能比较，请参见[附录 D](#AppendixD) 。


##打开用于分析性能瓶颈的 MySQL 慢速查询日志
MySQL 慢速查询日志可以帮助您识别为 MySQL 的慢速查询。 启用后的 MySQL 慢速查询日志，可以使用像**mysqldumpslow**这样的 MySQL 工具来识别性能瓶颈。  

请注意，默认情况下不会启用此。 启用慢速查询日志可能会占用一些 CPU 资源。 因此，建议您启用此暂时解决性能瓶颈。

###步骤 1︰ 修改 my.cnf 文件末尾加上以下行   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###步骤 2︰ 重新启动 mysql 服务器
    service  mysql  restart

###步骤 3︰ 检查是否正在使用"显示"命令的效果设置
 
![][7]   
   
![][8]
 
在此示例中，您可以看到的慢速查询功能已打开。 然后，可以使用**mysqldumpslow**工具来确定性能瓶颈和优化性能，如添加索引。





##附录

下面是示例性能测试数据生成目标的实验室环境中，它们提供一般背景上的性能数据趋势具有不同的性能调优的方法，但是结果可能会有所不同，在不同的环境或产品版本。 

<a name="AppendixA"></a>附录 a:  
**磁盘性能 (IOPS) 使用不同的 RAID 级别** 


![][9]
 
**测试命令︰**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE。注︰ 此测试工作负荷使用 64 个线程，试图到达 RAID 的上限值。

<a name="AppendixB"></a>附录 b:  
**MySQL 的性能 （吞吐量） 比较，具有不同的 RAID 级别**   
（XFS 文件系统）

 
![][10]  
![][11]

**测试命令︰**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**使用不同的 RAID 级别 MySQL (OLTP) 性能比较**  
![][12]

**测试命令︰**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>附录 c:   
**不同的区块大小的的磁盘 (IOPS) 的性能比较**  
（XFS 文件系统）

 
![][13]

**测试命令︰**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

注意︰ 用于此测试的文件大小分别为 30 GB 和 1 GB，使用 RAID 0(4 disks) XFS fie 系统。


<a name="AppendixD"></a>附录 d:  
**MySQL 在优化前后的性能 （吞吐量） 比较**  
（XFS 文件系统）

  
![][14]

**测试命令︰**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**默认值和 optmization 的配置设置如下所示︰**

|参数 |默认值    |optmization
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |无   |7 G
|**innodb_log_file_size**   |5 米 |512 M
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8 米 |128 米
|**query_cache_size**   |16 M    |0


更详细的优化配置参数，请参考 mysql 的官方说明。  

[http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**测试环境**  

|硬件   |详细信息
|-----------|-------
|Cpu    |AMD Opteron(tm) 处理器 4171 他 / 4 内核
|Memory |14 G
|磁盘   |10g/磁盘
|操作系统 |Ubuntu 14.04.1 LTS



[1]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-optimize-mysql-perf/virtual-machines-linux-optimize-mysql-perf-14.png
 