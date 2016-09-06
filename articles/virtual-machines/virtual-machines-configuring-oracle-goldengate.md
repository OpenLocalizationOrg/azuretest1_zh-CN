---
ms.openlocfilehash: ecc0a306443b039627c85e8b816a2a35ae6c0f0d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties title="Configuring Oracle GoldenGate for Azure" pageTitle="为 Azure 配置 Oracle GoldenGate" description="逐步建立和实施 Oracle GoldenGate Azure 虚拟机的高可用性和灾难恢复的教程。" services="virtual-machines" authors="bbenz" documentationCenter=""/>
<tags ms.service="virtual-machines" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="infrastructure-services" ms.date="06/22/2015" ms.author="bbenz" />
#为 Azure 配置 Oracle GoldenGate
本教程演示如何安装 Oracle GoldenGate Azure 虚拟机环境的高可用性和灾难恢复。 本教程侧重 RAC Oracle 数据库的[双向复制](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm)，并要求这两个站点处于活动状态。

Oracle GoldenGate 支持数据分发和数据集成。 它使您能够设置数据分布和数据同步解决方案通过 Oracle Oracle 复制配置，并提供灵活的高可用性解决方案。 Oracle GoldenGate 补充 Oracle Data Guard 使用复制功能来实现企业范围的信息分发和零停机时间升级和迁移。 有关详细信息，请参阅[使用 Oracle Data Guard 使用 Oracle GoldenGate](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm)。

Oracle GoldenGate 包含下列主要组件︰ 提取，数据抽取，建议您使用，追踪或提取文件，检查点、 管理和回收。 要有两个站点之间的双向复制，您需要创建并开始在两个站点上的所有组件。 Oracle GoldenGate 体系结构的详细信息，请参阅[Oracle GoldenGate 指南](http://docs.oracle.com/goldengate/1212/gg-winux/index.html)。

本教程假设您已经在 Oracle 数据库高可用性和灾难恢复的概念以及[Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html)的理论和实践知识。 有关详细信息，请参阅[Oracle 网站](http://www.oracle.com/technetwork/database/features/availability/index.html)。

此外，本指南假定您已经实现了以下系统必备组件︰

- 您已审查[Oracle 虚拟机映像的其他考虑因素](virtual-machines-miscellaneous-considerations-oracle-virtual-machine-images.md)主题中的高可用性和灾难恢复考虑事项部分。 请注意 Azure 支持独立 Oracle 数据库实例，但不是 Oracle 真实应用程序群集 (Oracle RAC) 当前。

- 您已经从[Oracle 下载](http://www.oracle.com/us/downloads/index.html)网站下载 Oracle GoldenGate 软件。 您已经选择产品包 Oracle 融合中间件中 – 数据集成。 然后，选择了 Oracle GoldenGate Oracle v11.2.1 媒体包在 Microsoft windows x64 （64 位） 针对 Oracle 11g 数据库。 接下来，下载适用于 Oracle 的 Oracle GoldenGate V11.2.1.0.3 11g 64 位 Windows 2008 年 （64 位）。

- 在使用 Windows 服务器上提供 Oracle 企业版图像平台 Azure 创建两个虚拟机 (Vm)。 有关信息，请参阅[创建 Oracle 数据库 12 c Azure 中的虚拟机](#z3dc8d3c097cf414e9048f7a89c026f80)和[Azure 的虚拟机](http://azure.microsoft.com/documentation/services/virtual-machines/)。 确保虚拟机是在[同一个云服务](virtual-machines-load-balance.md)，在同一[虚拟网络](http://azure.microsoft.com/documentation/services/virtual-network/)以确保可以访问彼此的持久的专用 IP 地址上。

- 设置虚拟机名称为"MachineGG1"网站和"MachineGG2"网站 b 在 Azure 门户网站。

- 在站点 A 和站点 B 上的"TestGG2"，已经在"TestGG1"创建测试数据库

- 您登录到 Windows server 作为成员的管理员组或**ORA_DBA**组的成员。

在本教程中，您将能够︰

1. 安装在站点 A 和站点 B 的数据库  

    1. 执行初始数据装载
    
2. 准备站点 A 和站点 B 的数据库复制

3. 创建所有必需的对象以支持 DDL 复制

4. 在站点 A 和站点 B 上配置 GoldenGate 管理器

5. 站点 A 和站点 B 上创建提取组和数据抽取进程

    1. 创建站点 A 上提取和数据抽取进程

    2. 在站点 B 上创建一个 GoldenGate 检查点表

    3. 在站点 B 上添加建议您使用

    4. 在站点 B 上创建提取和数据抽取过程

    5. 在站点 A 中创建 GoldenGate 检查点表

    6. 在站点上添加建议您使用

    7. 添加站点 A 和站点 B 上的 trandata

    8. 在站点 A 开始提取和数据抽取过程

    9. 在站点 B 上开始提取和数据抽取过程

    10. 在站点 A 上启动建议您使用过程

    11. 在站点 B 上开始建议您使用过程

6. 验证双向复制过程

>[AZURE.IMPORTANT] 本教程已安装并根据下面的软件配置进行测试︰
>
>|                        | **站点数据库**              | **站点 B 的数据库**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Oracle 版本**     | Oracle11g 发布 2-(11.2.0.1) | Oracle11g 发布 2-(11.2.0.1) |
>| **计算机名称**       | MachineGG1                       | MachineGG2                       |
>| **操作系统**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle 的 SID**         | TESTGG1                          | TESTGG2                          |
>| **复制架构** | SCOTT                            | SCOTT                            |

对于 Oracle 数据库和 Oracle GoldenGate 的后续版本，可能需要执行一些其他更改。 有最新版本特定的信息，请参阅在 Oracle 网站上的[Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html)和[Oracle 数据库](http://www.oracle.com/us/corporate/features/database-12c/index.html)文档。 例如，版本 11.2.0.4 源数据库及更高版本，捕获 DDL 的由 logmining 服务器以异步方式执行并要求没有特殊触发器、 表或安装其他数据库对象。 而无需停止应用程序用户可以执行 oracle GoldenGate 升级。 在集成模式下使用早于版本 11.2.0.4 Oracle 11g 源数据库提取时，使用 DDL 触发器和支持对象是必需的。 详细的指南，请参阅[安装和配置针对 Oracle 数据库的 Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf)。

##1.安装在站点 A 和站点 B 的数据库
此部分说明如何在站点 A 和站点 B 上执行数据库系统必备组件您必须在两个站点上执行的此部分中的所有步骤︰ 站点 A 和站点 b。

首先，远程桌面到站点 A 和站点 B 通过管理门户。 打开 Windows 命令提示符，然后创建 Oracle GoldenGate 安装程序文件的主目录︰

    mkdir C:\OracleGG

然后，解压缩，然后在此文件夹中安装 Oracle GoldenGate 软件。 此步骤之后，您可以通过执行下面的命令启动 GoldenGate 软件命令解释程序 (GGSCI):

    C:\OracleGG\.\ggsci

您可以使用[GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm)来运行多个命令的配置、 控制和监视 Oracle GoldenGate。

下一步，运行以下命令以创建安装程序包中的所有子文件夹︰

    GGSCI (Hostname) 1> CREATE SUBDIRS

运行下面的命令以退出 GGSCI 命令提示符︰

    GGSCI (Hostname) 1> EXIT

然后，您需要创建数据库用户，这将由 Oracle GoldenGate 管理器、 提取和复制进程。 请注意，您可以创建每个进程的单个用户或只有一个公共的用户配置。 在本教程中，我们创建一个用户，称为 ggate。 然后，我们授予该用户所需的权限。 请注意，您必须执行以下操作在站点 A 和站点 b。

打开 SQL\*加站点 A 和站点 B 上具有使用**SYSDBA**，如数据库管理员权限的命令窗口︰

    Enter username: / as sysdba

然后运行︰

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m; 
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

下一步，找到初始化\<DatabaseSID\>。ORA 文件 %oracle\_家庭 %\\数据库文件夹在站点 A 和站点 B 并将下面的数据库参数追加到 INITTEST.ora:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

所有 Oracle GoldenGate GGSCI 命令的完整列表，请参阅[参考 Oracle GoldenGate for Windows 的](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm)。

### 执行初始数据装载

通过以下几种方法，您可以在数据库中执行初始数据加载。 例如，使用[Oracle GoldenGate 直接负载](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm)或定期导出和导入实用程序将从站点 A 的表格数据导出到站点 b。

为了演示 Oracle GoldenGate 复制过程，本教程演示站点 A 和站点 B 上创建表，使用以下命令。

首先，打开 SQL\*加上命令窗口并运行以下命令以在站点 A 和站点 B 的数据库上创建库存表︰
    
    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

接下来，将约束添加到新创建的表在站点 A 和站点 B 的数据库︰

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

然后，将授予给用户 ggate 站点 A 和站点 b︰ 新的库存表上的所有权限

    grant all on scott.inventory to ggate;

接下来，创建并启用数据库中的触发器，INVENTORY_CDR_TRG，新创建的表，以确保向新表的所有事务将都记录，如果该用户不是 ggate。 执行此操作的站点 A 和站点 b。

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    / 


##2.准备站点 A 和站点 B 数据库复制
此部分说明了如何准备站点 A 和站点 B 的数据库复制。 您必须在两个站点上执行的此部分中的所有步骤︰ 站点 A 和站点 b。

首先，远程桌面到站点 A 和站点 B 通过 Azure 门户。 切换到 archivelog 模式下使用 SQL 的数据库 * 加上命令窗口︰ 
    
    sql>shutdown immediate 
    sql>startup mount 
    sql>alter database archivelog; 
    sql>alter database open;


然后，按如下所述启用最小[附加的日志记录](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm)︰

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

接下来，准备该数据库以支持 DDL （数据定义语言） 复制︰

    SQL> alter system set recyclebin=off scope=spfile;

然后，关闭并重新启动数据库︰

    sql>shutdown immediate 
    sql>startup


##3.创建所有必需的对象以支持 DDL 复制
本部分列出了您需要用于创建所有必需的对象以支持 DDL 复制的脚本。 您需要运行在站点 A 和站点 B 上的此部分中指定的脚本

Windows 命令提示符处打开，然后定位到 Oracle GoldenGate 文件夹，如 c:\\OracleGG。 启动 SQL\*加上具有数据库管理员权限，例如，在站点 A 和站点 B 上使用**SYSDBA**命令提示符

然后，运行以下脚本︰
    
    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql 
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Oracle GoldenGate 工具需要表级别登录 DDL （数据定义语言） 的支持。 这就是为什么，补充通过添加 TRANDATA 命令记录在表级别启用。 Oracle GoldenGate 命令解释程序窗口，请登录到数据库，打开，然后运行添加 TRANDATA 命令︰

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##4.在站点 A 和站点 B 上配置 GoldenGate 管理器
Oracle GoldenGate 管理器执行一些功能，如启动其他 GoldenGate 进程，跟踪日志文件管理和报告。

您需要在站点 A 和站点 B 上配置 Oracle GoldenGate 管理器进程若要执行此操作，请执行以下步骤在站点 A 和站点 b。

打开的窗口命令窗口并启动 Oracle GoldenGate 命令解释程序︰

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


这样，您可以执行命令来对数据库的影响，GGSCI 会话登录到数据库︰

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

显示状态和系统上的所有管理器，提取，建议您使用进程滞后 （如果相关）︰

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

打开使用编辑参数化命令参数文件，然后再追加以下信息︰

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

显示状态和系统上的所有管理器，提取，建议您使用进程滞后 （如果相关）︰

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

这样，您可以执行命令来对数据库的影响，GGSCI 会话登录到数据库︰

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

启动管理器进程︰

    GGSCI (HostName) 48> start manager
    Manager started.

##5.制定提取站点 A 和站点 B 上的组和数据抽取进程

###创建站点 A 上提取和数据抽取进程

您需要创建站点 A 和站点 A 和站点 B 通过管理门户网站 B.远程桌面的提取和数据抽取过程。 GGSCI 命令解释程序窗口打开。 答︰ 站点上运行下面的命令

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

打开使用编辑参数化命令参数文件，然后将附加的以下信息︰ GGSCI (MachineGG1) 18 > 编辑 params ext1 提取 ext1 用户 ID ggate，密码 ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER ggate 表 scott.inventory，GETBEFORECOLS （在更新 KEYINCLUDING （prod_category、 qty_in_stock、 last_dml），在删除 KEYINCLUDING （prod_category、 qty_in_stock、 last_dml））;

打开使用编辑参数化命令参数文件，然后再追加以下信息︰

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###在站点 B 上创建一个 GoldenGate 检查点表

接下来，您需要添加站点 B 上的检查点表若要执行此操作，您需要一个 GoldenGate 命令解释器窗口打开并运行︰

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

然后，然后，将检查点表添加到数据库，其中 ggate 是所有者︰
    
    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

将检查点表的名称添加到目标服务器，则在此步骤中的站点 B 上的全局文件。 编辑全局文件在站点 b:

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

然后，将 CHECKPOINTTABLE 参数追加到现有的全局文件︰

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

作为最后一步，保存并关闭全局参数文件。


###在站点 B 上添加建议您使用
本节介绍如何添加建议您使用进程"REP2"在站点 b。
 
添加建议您使用命令用于在站点 b︰ 创建建议您使用组

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

打开使用编辑参数化命令参数文件，然后再追加以下信息︰

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###在站点 B 上创建提取和数据抽取过程

本部分介绍如何在站点 b︰ 创建一个新的提取过程"EXT2"和新的数据抽取过程"DPUMP2"

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

打开使用编辑参数化命令参数文件，然后再追加以下信息︰

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

打开使用编辑参数化命令参数文件，然后再追加以下信息︰

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###在站点 A 中创建 GoldenGate 检查点表

Oracle GoldenGate 命令解释程序窗口打开，然后创建检查点︰

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

您还需要添加到 A 站点上的全局文件检查点表的名称 

Oracle GoldenGate 命令解释程序窗口中打开和编辑站点 a︰ 上的全局文件

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

保存并关闭全局参数文件。

###在站点上添加建议您使用

本部分介绍如何将建议您使用进程"REP1"添加站点 a。 

下面的命令创建与名称相关联的 checkpointtable 以及轨迹，建议您使用组 rep1。

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

打开使用编辑参数化命令参数文件，然后再追加以下信息︰

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### 添加站点 A 和站点 B 上的 trandata

使用添加 TRANDATA 命令来启用补充表级别的日志记录。 打开 Oracle GoldenGate 命令解释程序窗口，登录到数据库，然后再运行添加 TRANDATA 命令。

远程桌面到 MachineGG1，Oracle GoldenGate 命令解释程序，打开并运行︰

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.
        
远程桌面到 MachineGG2，Oracle GoldenGate 命令解释程序，打开并运行︰

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

补充表级别的状态显示信息日志记录︰

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###添加站点 A 和站点 B 上的 trandata

使用添加 TRANDATA 命令来启用补充表级别的日志记录。 打开 Oracle GoldenGate 命令解释程序窗口，登录到数据库，然后再运行添加 TRANDATA 命令。 

远程桌面到 MachineGG1，Oracle GoldenGate 命令解释程序，打开并运行︰

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

远程桌面到 MachineGG2，Oracle GoldenGate 命令解释程序，打开并运行︰

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###在站点 A 开始提取和数据抽取过程

答︰ 网站上开始提取过程 ext1

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

答︰ 站点上启动数据抽取过程 dpump1

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
显示信息提取组 ext1: GGSCI (MachineGG1) 32 > 信息提取 ext1 提取 EXT1 上次启动 2013年-11-25 08:03 状态运行检查点延隔时间 00:00:00 (更新 00:00:02 年前) 日志的读取已由检查点 Oracle 恢复的日志 2013年-11-25 08:03:18 Seqno 6，RBA 3230720 SCN 0.1074371 (1074371)

显示状态和系统上的所有管理器，提取，建议您使用进程滞后 （如果相关）︰

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt
    
    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###在站点 B 上开始提取和数据抽取过程

在站点 b︰ 开始提取过程 ext2

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

在站点 b︰ 上启动数据抽取过程 dpump2

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

显示状态和系统上的所有管理器，提取，建议您使用进程滞后 （如果相关）︰

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt
    
    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### 在站点 A 上启动建议您使用过程

本部分介绍如何开始建议您使用"REP1"在站点 a。 

答︰ 网站上开始建议您使用过程

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

显示建议您使用组的状态︰

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###在站点 B 上开始建议您使用过程

本部分介绍如何启动建议您使用过程"REP2"在站点 b。 

在站点 b︰ 启动建议您使用过程

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

显示建议您使用组的状态︰

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##6.确认双向复制过程

要验证的 Oracle GoldenGate 配置，请将行插入数据库的站点到站点 a。 打开 SQL 答︰ 远程桌面 * 和命令窗口并运行︰ SQL > 选择名称从 v$ 数据库;
    
    NAME
    ———
    TESTGG
    
    SQL> insert into inventory values  (100,’TV’,100,sysdate);
    
    1 row created.
    
    SQL> commit;
    
    Commit complete.

然后，检查是否该行复制到站点 b。为此，到站点 b。 打开远程桌面 SQL 以及窗口和运行︰

    SQL> select name from v$database;
    
    NAME
    ———
    TESTGG
    
    SQL> select * from inventory;
    
    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

在站点 b︰ 插入新记录

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.
    
    SQL> commit;
    
    Commit complete.

到站点 A，并检查如果复制发生的远程桌面︰

    SQL> select * from inventory;
    
    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##其他资源
[Oracle 的 Azure 的虚拟机映像](virtual-machines-oracle-list-oracle-virtual-machine-images.md)
