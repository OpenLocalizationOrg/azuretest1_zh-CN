---
ms.openlocfilehash: 551885c9e255a8618ae1d9a47a1d724bcdbc3161
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties title="Configuring Oracle Data Guard for Azure" pageTitle="配置 Oracle Data Guard Azure 的" description="逐步建立和实施 Oracle Data Guard Azure 虚拟机的高可用性和灾难恢复的教程。" services="virtual-machines" authors="bbenz" documentationCenter=""/>
<tags ms.service="virtual-machines" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="infrastructure-services" ms.date="06/22/2015" ms.author="bbenz" />
#配置 Oracle Data Guard Azure 的
本教程演示如何设置和 Azure 的虚拟机的高可用性和灾难恢复的环境中实现 Oracle Data Guard。 本教程重点介绍 RAC Oracle 数据库的复制的一种方法。

Oracle Data Guard 支持 Oracle 数据库的数据保护和灾难恢复。 它是简单、 高性能、 顺便为灾难恢复、 数据保护和高可用性的整个 Oracle 数据库的解决方案。

本教程假设您已经在 Oracle 数据库高可用性和灾难恢复的概念上的理论和实践知识。 有关信息，请参阅[Oracle 网站](http://www.oracle.com/technetwork/database/features/availability/index.html)以及[Oracle 数据保护概念和管理指南 》](http://docs.oracle.com/cd/E11882_01/server.112/e17022/create_ps.htm)。

此外，本指南假定您已经实现了以下系统必备组件︰

- 您已审查[Oracle 虚拟机映像的其他考虑因素](virtual-machines-miscellaneous-considerations-oracle-virtual-machine-images.md)主题中的高可用性和灾难恢复考虑事项部分。 请注意 Azure 支持独立 Oracle 数据库实例，但不是 Oracle 真实应用程序群集 (Oracle RAC) 当前。

- 在 Azure 使用相同的平台提供 Oracle 企业版图像在 Windows 服务器上创建两个虚拟机 (Vm)。 有关信息，请参阅[创建 Oracle 数据库 12 c Azure 中的虚拟机](virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine.md)和[Azure 的虚拟机](http://azure.microsoft.com/documentation/services/virtual-machines/)。 确保虚拟机是在[同一个云服务](virtual-machines-load-balance.md)，在同一[虚拟网络](azure.microsoft.com/documentation/services/virtual-network/)以确保可以访问彼此的持久的专用 IP 地址上。 此外，它建议将虚拟机放置在同一[可用性设置](virtual-machines-manage-availability.md)以允许 Azure 将它们放置到单独的故障域和升级域。 请注意，Oracle Data Guard 只可用于 Oracle 数据库企业版。 每台计算机必须有至少 2 GB 的内存和 5 GB 的磁盘空间。 提供虚拟机大小的平台上的最新信息，请参阅[Azure 的虚拟机大小](http://msdn.microsoft.com/en-us/library/dn197896.aspx)。 如果您需要额外的磁盘卷用于虚拟机，您可以附加其他磁盘。 有关信息，请参阅[如何将附加到虚拟机的数据磁盘](storage-windows-attach-disk.md)。

- 虚拟机名称"Machine1"作为主要的 VM 和"Machine2"为设置备用 VM 在 Azure 门户网站。

- 已设置**ORACLE_HOME**环境变量指向主服务器和备用虚拟机中的同一个 oracle 根安装路径如`C:\OracleDatabase\product\11.2.0\dbhome_1\database`。

- 您登录到 Windows server 作为成员的**管理员组**或**ORA_DBA**组的成员。

在本教程中，您将能够︰

实现物理备用数据库环境

1. 创建一个主数据库

2. 主数据库准备备用数据库创建

    1. 启用强制日志记录

    2. 创建密码文件

    3. 配置备用的重做日志

    4. 启用存档

    5. 设置主数据库的初始化参数

创建一个物理的备用数据库

1. 准备一个备用数据库的初始化参数文件

2. 配置侦听器和 tnsnames 支持主和备用计算机上的数据库

    1. Listener.ora 服务器上配置这两个可容纳两个数据库的条目

    2. 用于保存项的主和备用数据库的主要和备用虚拟机上配置 tnsnames.ora

    3. 启动监听器，并检查这两个服务这两个虚拟机上的 tnsping。

3. 启动备用实例处于 nomount 状态

4. 使用 RMAN 克隆数据库并创建一个备用数据库

5. 在托管的恢复模式下启动物理备用数据库

6. 验证物理备用数据库

> [AZURE.IMPORTANT] 本教程已安装并根据以下硬件和软件配置进行测试︰
>
>|                      | **主数据库**                      | **备用数据库**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Oracle 版本**   | Oracle11g 企业版 (11.2.0.4.0) | Oracle11g 企业版 (11.2.0.4.0) |
>| **计算机名称**     | Machine1                                  | Machine2                                  |
>| **操作系统** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle 的 SID**       | 测试                                      | 测试\_STBY                                |
>| **Memory**           | 最小值 2 GB                                  | 最小值 2 GB                                  |
>| **磁盘空间**       | 最小值 5 GB                                  | 最小值 5 GB                                  |

对于 Oracle 数据库和 Oracle Data Guard 的后续版本，可能需要执行一些其他更改。 有最新版本特定的信息，请参阅 Oracle web 站点上的[数据保护](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html)和[Oracle 数据库](http://www.oracle.com/us/corporate/features/database-12c/index.html)文档。

##实现物理备用数据库环境
### 1.创建一个主数据库

- 主数据库"测试"中创建主虚拟机。 有关信息，请参阅创建和配置 Oracle 数据库。
- 作为具有 SYSDBA 角色在 SQL SYS 用户连接到您的数据库 * 加上命令提示符下，运行下面的语句以查看您的数据库的名称︰

        SQL> select name from v$database;
        
        The result will display like the following:
        
        NAME
        ---------
        TEST
- 接下来，查询 dba_data_files 系统视图中的数据库文件的名称︰

        SQL> select file_name from dba_data_files; 
        FILE_NAME 
        ------------------------------------------------------------------------------- 
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### 2.准备主数据库备用数据库创建

备用数据库之前，建议您确保正确配置主数据库。 以下是需要执行的步骤的列表︰

1. 启用强制日志记录
2. 创建密码文件
3. 配置备用的重做日志
4. 启用存档
5. 设置主数据库的初始化参数

#### 启用强制日志记录

为了实现备用数据库，我们需要在主数据库中启用强制日志。 此选项确保即使的 nologging 操作过程中，强制日志记录优先，且所有操作都记录到重做日志。 因此，我们确保主数据库中的所有内容将记录复制到备用服务器与主数据库中包括的所有操作。 运行 alter database 语句，以启用强制日志记录︰

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### 创建密码文件

若要能够交付和归档的日志从主服务器应用到备用服务器，sys 密码必须相同主和备用服务器上。 这就是为什么您创建主数据库上的密码文件，并将其复制到备用服务器。

>[AZURE.IMPORTANT] 当使用 Oracle 数据库 12 c，没有一个新用户， **SYSDG**，您可以使用它来管理 Oracle Data Guard。 有关详细信息，请参阅[Oracle 数据库 12 c 版本中的更改](http://docs.oracle.com/cd/E16655_01/server.121/e10638/release_changes.htm)。

此外，还要确保 ORACLE\_Machine1 中已经定义了家庭环境。 如果不是，则将其定义为一个环境变量，使用环境变量对话框。 若要访问此对话框，请启动**系统**实用程序，通过双击**控制面板**中; 在系统图标单击**高级**选项卡，然后选择**环境变量**。 单击要设置的环境变量的**系统变量**下的**新建**按钮。 设置环境变量后, 关闭现有的 Windows 命令提示符处，打开一个新。

运行下面的语句以切换到 Oracle\_Home 目录中的，如 c:\\OracleDatabase\\产品\\11.2.0\\dbhome\_1\\数据库。

    cd %ORACLE_HOME%\database

然后，创建一个使用密码文件创建实用程序[ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm)的密码文件。 在 Machine1 中的同一个 Windows 命令提示符，通过将密码值设置为**SYS**密码中运行以下命令︰

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

此命令将创建一个密码文件，命名为 PWDTEST.ora，在 ORACLE\_主页\\数据库目录。 应将此文件复制到 %oracle\_家庭 %\\手动数据库 Machine2 中的目录。

#### 配置备用的重做日志

然后，您需要配置备用的重做日志，以便主可正确接收重做时备用。 预先在此处创建还允许在备用服务器上自动创建备用的重做日志。 请务必配置待机恢复日志 (SRL) 具有相同的大小，如联机重做日志。 当前的备用的重做日志文件的大小必须与当前的主数据库联机重做日志文件的大小完全匹配。

在 SQL 中运行下面的语句\*以及 Machine1 中的命令提示符。 V$ 日志文件是包含信息的重做日志文件系统视图。

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file. 
    SQL> select bytes from v$log; 
    BYTES
    ----------
    52428800
    52428800
    52428800


请注意，52428800 50 兆字节。

然后，在 SQL\*加上窗口，运行下面的语句将新的备用恢复日志文件组添加到备用数据库，并指定标识使用 GROUP 子句组的编号。 使用组数字，可以使管理备用恢复日志文件组更容易︰

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

接下来，运行下面的系统视图列表信息的重做日志文件。 此操作还会验证待机恢复日志文件组被创建︰

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### 启用存档

然后，启用存档通过运行下面的语句将主数据库放在 ARCHIVELOG 模式下，启用自动存档。 正在装载数据库，然后执行 archivelog 命令，您可以启用存档日志模式。

首先，以 sysdba 登录。 在 Windows 命令提示符处，运行︰

    sqlplus /nolog
    
    connect / as sysdba

然后，关闭 SQL 数据库\*Plus 命令提示符︰

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

然后，执行启动 mount 命令来装载数据库。 这将确保 Oracle 将与指定的数据库关联的实例。

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

然后，运行︰

    SQL> alter database archivelog; 
    Database altered.

然后，运行 Alter database 语句打开的子句，以便使数据库可供正常使用︰

    SQL> alter database open;
    
    Database altered.

#### 设置主数据库的初始化参数

若要配置数据保护，您需要创建并配置备用参数在常规 pfile （文本初始化参数文件中） 第一。 Pfile 准备就绪后，您需要将其转换为服务器参数文件 (SPFILE)。

您可以控制在 INIT 中使用的参数的数据保护环境。ORA 文件。 当遵循本教程，您需要更新主数据库初始化。ORA 以便它可以容纳这两种角色︰ 主或待机状态。

    SQL> create pfile from spfile;
    File created.

接下来，您需要编辑 pfile 添加备用的参数。 若要执行此操作，请打开 INITTEST。ORA %oracle 的位置在文件\_家庭 %\\数据库。 接下来，将下面的语句附加到 INITTEST.ora 文件。 请注意，您初始化的命名约定。ORA 文件是 INIT\<YourDatabaseName\>。ORA。
    
    db_name='TEST' 
    db_unique_name='TEST' 
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE 
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE 
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc 
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


前一个语句块中包含三个重要的安装程序项目︰
-   **LOG_ARCHIVE_CONFIG...:**定义使用此语句的数据库的唯一 id。
-   **LOG_ARCHIVE_DEST_1...:**定义本地存档文件夹位置使用此语句。 我们建议您创建一个新目录数据库的存档需求并指定本地存档位置显式使用此语句，而不是使用 Oracle 的默认文件夹 %oracle_home%\database\archive。
-   **LOG_ARCHIVE_DEST_2...LGWR 异步...:**定义异步日志编写器进程 (LGWR) 收集事务恢复数据，并传送到备用目标。 在这里，DB_UNIQUE_NAME 指定目标备用服务器上的数据库的唯一名称。

一旦新的参数文件准备就绪后，您需要从它创建 spfile。

首先，关闭数据库︰

    SQL> shutdown immediate;
    
    Database closed.
    
    Database dismounted.
    
    ORACLE instance shut down.

接下来，按照以下方式运行启动 nomount 命令︰

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

现在，创建 spfile:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

然后，为关闭数据库︰

    SQL> shutdown immediate;
    
    ORA-01507: database not mounted

然后，使用启动命令启动一个实例︰

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##创建一个物理的备用数据库
本节重点介绍在 Machine2 准备物理备用数据库必须执行的步骤。

首先，您需要通过 Azure 门户 Machine2 到远程桌面。

然后，在备用服务器 (Machine2) 上，创建所有必要的文件夹的备用数据库，如 c:\\\<YourLocalFolder\>\\测试。 同时遵循本教程，确保文件夹结构匹配 Machine1 保留所有必要文件，如控制文件、 数据文件、 redologfiles、 udump、 bdump 和 cdump 文件的文件夹结构。 此外，定义将 ORACLE\_家庭和 ORACLE\_Machine2 中的基本环境变量。 如果不是，则将其定义为一个环境变量，使用环境变量对话框。 若要访问此对话框，请启动**系统**实用程序，通过双击**控制面板**中; 在系统图标单击**高级**选项卡，然后选择**环境变量**。 单击要设置的环境变量的**系统变量**下的**新建**按钮。 设置环境变量之后, 您需要关闭现有的 Windows 命令提示符窗口并打开一个新以查看更改。

接下来，请执行以下步骤︰

1. 准备一个备用数据库的初始化参数文件

2. 配置侦听器和 tnsnames 支持主和备用计算机上的数据库

    1. Listener.ora 服务器上配置这两个可容纳两个数据库的条目

    2. 用于保存项的主和备用数据库的主要和备用虚拟机上配置 tnsnames.ora

    3. 启动监听器，并检查这两个服务这两个虚拟机上的 tnsping。

3. 启动备用实例处于 nomount 状态

4. 使用 RMAN 克隆数据库并创建一个备用数据库

5. 在托管的恢复模式下启动物理备用数据库

6. 验证物理备用数据库

### 1.准备备用数据库的初始化参数文件

本部分介绍如何准备一个备用数据库的初始化参数文件。 要做到这一点，首先将 INITTEST 复制。ORA 文件从计算机 1 到 Machine2 手动。 您应该能够看到 INITTEST。ORA 文件 %oracle\_家庭 %\\在两台计算机中的数据库文件夹。 然后，修改 Machine2 以将其设置为如下所示的备用角色中的 INITTEST.ora 文件︰
    
    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'
    
    
    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) 
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


前一个语句块中包含两个重要的安装程序项目︰

-   **\*.LOG_ARCHIVE_DEST_1:**您需要手动在 Machine2 中创建的 c:\OracleDatabase\TEST_STBY\archives 文件夹。
-   **\*.LOG_ARCHIVE_DEST_2:**这是一个可选的步骤。 您设置与主计算机处于维护和备用计算机成为主数据库时，可能需要它。

然后，您需要启动备用实例。 在备用数据库服务器中，输入在 Windows 命令提示符处执行以下命令，通过创建新的 Windows 服务创建 Oracle 实例︰

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

请注意**Oradim**命令创建 Oracle 实例，但不启动它。 您可以找到它在 c:\\OracleDatabase\\产品\\11.2.0\\dbhome\_1\\BIN 目录中。

##配置侦听器和 tnsnames 支持主和备用计算机上的数据库
创建备用数据库之前，您需要确保您的配置中的主和备用数据库可以互相谈话。 若要执行此操作，您需要配置的侦听器和 TNSNames 手动或通过使用网络配置实用程序 NETCA。 使用恢复管理器实用程序 (RMAN)，这是一个强制性的任务。

### Listener.ora 服务器上配置这两个可容纳两个数据库的条目

Machine1 和编辑 listener.ora 文件作为下面指定的远程桌面。 在编辑 listener.ora 文件时，应始终确保，左和右括号对齐在同一列中。 您可以在以下文件夹中找到 listener.ora 文件︰ c:\\OracleDatabase\\产品\\11.2.0\\dbhome\_1\\网络\\管理\\。

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora
    
    # Generated by Oracle configuration tools.
    
    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )
    
    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

对 Machine2 和 listener.ora 文件，如下所示编辑的下一步的远程桌面: # listener.ora 网络配置文件︰ C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora
    
    # Generated by Oracle configuration tools.
    
    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )
    
    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### 用于保存项的主和备用数据库的主要和备用虚拟机上配置 tnsnames.ora

Machine1 和编辑 tnsnames.ora 文件作为下面指定的远程桌面。 您可以在以下文件夹中找到 tnsnames.ora 文件︰ c:\\OracleDatabase\\产品\\11.2.0\\dbhome\_1\\网络\\管理\\。

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )
    
    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Machine2 和编辑远程桌面 tnsnames.ora 文件，如下所示︰
    
    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )
    
    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### 启动监听器，并检查这两个服务这两个虚拟机上的 tnsping。

打开新的 Windows 命令提示符中主要的和备用的虚拟机并运行下列语句︰

    C:\Users\DBAdmin>tnsping test
    
    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)
    

    C:\Users\DBAdmin>tnsping test_stby
    
    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##启动备用实例处于 nomount 状态
您需要将环境设置为待机状态的虚拟机 (MACHINE2) 支持备用数据库。

首先，密码文件从主计算机 (Machine1) 到备用机 (Machine2) 将手动复制。 这是必要的**sys**密码必须在两台计算机上相同。

然后，在 Machine2，打开 Windows 命令提示符设置环境变量来指向备用数据库，如下所示︰

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

接下来，在 nomount 状态下启动备用数据库，然后生成 spfile。

启动数据库︰

    SQL>shutdown immediate;
    
    SQL>startup nomount
    ORACLE instance started.
    
    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##使用 RMAN 克隆数据库并创建一个备用数据库
恢复管理器实用程序 (RMAN) 可用于主数据库以创建物理备用数据库的任何备份副本。

远程桌面的备用虚拟机 (MACHINE2) 和运行 RMAN 实用程序通过指定完整的连接字符串为这两个目标 （主数据库，Machine1） 和辅助 （备用数据库，Machine2） 实例。

>[AZURE.IMPORTANT] 不要因为没有数据库备用服务器计算机中尚未使用操作系统身份验证。

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY
    
    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##在托管的恢复模式下启动物理备用数据库
本教程演示如何创建一个物理的备用数据库。 有关创建逻辑的备用数据库的信息，请参阅 Oracle 文档。

打开 SQL\*加上命令提示符并启用备用虚拟机或服务器 (MACHINE2) 上的数据保护，如下所示︰

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

备用数据库中**装载**模式中，存档日志打开时运输将继续并管理的恢复过程将继续在备用数据库上应用的日志。 这将确保备用数据库保持最新的主数据库。 请注意待机数据库无法访问用于报告在这段时间。

当您在**只读**模式下打开备用数据库时，存档日志传送仍然存在。 但管理的恢复过程停止。 这会使备用数据库变得越来越过时，继续管理的恢复过程之前。 您也可以在这段时间访问出于报告目的的备用数据库，但数据可能没有反映最新的更改。

一般情况下，我们建议您保持备用数据库中**装载**模式将数据保存在备用数据库保持最新的主数据库出现故障时。 但是，您可以出于报告目的具体取决于您的应用程序要求在**只读**模式下保留备用数据库。 下面的步骤演示如何启用数据保安在只读模式下使用 SQL\*加︰

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##验证物理备用数据库
这一节演示如何验证高可用性配置作为管理员。

打开 SQL\*Plus 命令提示符窗口并检查已存档重做日志上等待虚拟机 (Machine2):

    SQL> show parameters db_unique_name;
    
    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY
    
    SQL> SELECT NAME FROM V$DATABASE
    
    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;
    
    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

打开 SQL * 加上命令提示符窗口并切换日志文件 (Machine1) 的主计算机上︰

    SQL> alter system switch logfile; 
    System altered.
    
    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

检查已存档重做日志上等待虚拟机 (Machine2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;
    
    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES
    
    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

检查有任何间隙在待机状态的虚拟机 (Machine2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

另一种验证方法无法将故障转移到备用数据库，然后测试是否可以回切到主数据库。 要激活主数据库作为备用数据库，请使用下面的语句︰

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

如果您没有启用原始的主数据库上的闪回，建议您删除原始的主数据库并重新创建为备用数据库。

我们建议您启用闪回主服务器上的数据库和备用数据库。 在故障转移情况下，可以进行故障转移之前的时间重新刷新和快速转换到备用数据库主数据库。

##其他资源
[Oracle 的 Azure 的虚拟机映像](virtual-machines-oracle-list-oracle-virtual-machine-images.md)