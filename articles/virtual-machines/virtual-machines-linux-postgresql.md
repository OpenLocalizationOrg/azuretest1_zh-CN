---
ms.openlocfilehash: 299f6507ac1371b08b272901f88f1e5bf4c0d7a0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="安装并运行 Linux 的 Microsoft Azure 虚拟机上配置 PostgreSQL |Microsoft Azure"
    description="了解如何安装和配置 PostgreSQL 在 Azure 中的 Linux 虚拟机上。"
    services="virtual-machines"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
  tags=""/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="linux"
    ms.workload="infrastructure-services"
    ms.date="08/10/2015"
    ms.author="mingzhan"/>


#安装和配置 PostgreSQL 在 Azure 上

PostgreSQL 是类似于 Oracle 和 DB2 高级的开源数据库。 它包括如完全 ACID 的法规遵从性、 可靠的事务处理和多版本并发控制的企业就绪功能。 它还支持 ANSI SQL 和 SQL/MED （包括 Oracle、 MySQL，MongoDB，和其他许多的外部数据包装） 等标准。 它具有高度可扩展性支持 JSON 或基于密钥值的应用程序超过 12 种过程语言、 GIN 和梗概索引、 空间数据的支持和多个 NoSQL 类似功能。

在本文中，您将学习如何安装和配置 PostgreSQL 运行 Linux Azure 的虚拟机上。

> [AZURE.NOTE] 您必须已经为了完成本教程运行 Linux Azure 的虚拟机。 若要创建和设置 Linux 虚拟机之前，请参阅[Azure Linux 虚拟机教程](virtual-machines-linux-tutorial.md)。

在这种情况下，用作 PostgreSQL 端口端口 1999年。  

## 安装 PostgreSQL

连接到 Linux 通过 PuTTY 创建的虚拟机。 如果这是首次使用 Azure Linux 虚拟机，请参阅[如何使用 SSH 使用 Linux 在 Azure 上](virtual-machines-linux-use-ssh-key.md)，若要了解如何使用 PuTTY 连接到 Linux 虚拟机。

1. 运行以下命令来切换到根 （管理员）︰

        # sudo su -

2. 一些分布都有，PostgreSQL 在安装之前必须安装这些依赖项。 检查您在此列表中的 distro，并运行适当的命令︰

    - Red Hat 基 Linux:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Debian 基 Linux:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. 下载到根目录，PostgreSQL，然后解压缩软件包︰

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    上述是一个例子。 [的 /pub/源/索引](https://ftp.postgresql.org/pub/source/)中，您可以找到更详细的下载地址。

4. 若要启动生成，运行以下命令︰

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. 如果您想要生成可以生成，包括文档 （HTML 文件和手册页） 和附加模块 (contrib) 的所有内容，改为运行以下命令︰

        # gmake install-world

    您应该收到以下确认消息︰

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## 配置 PostgreSQL

1. （可选）创建符号链接来缩短 PostgreSQL 的引用不包含版本号︰

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. 创建数据库的目录︰

        # mkdir -p /opt/pgsql_data

3. 创建非根用户和修改该用户的配置文件。 然后，切换到该新用户 （在本例中称为*postgres* ）︰

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] 出于安全考虑，PostgreSQL 使用非根用户初始化、 启动或关闭数据库。


4. 通过输入下面的命令来编辑*bash_profile*文件。 这些线将被添加到*bash_profile*文件的末尾︰

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. 执行*bash_profile*文件︰

        $ source .bash_profile

6. 通过使用下面的命令来验证您的安装︰

        $ which psql

    如果安装不成功，您将看到以下消息︰

        /opt/pgsql/bin/psql

7. 您还可以检查 PostgreSQL 版本︰

        $ psql -V

8. 初始化数据库︰

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    您应该收到以下输出︰

![image](./media/virtual-machines-linux-postgresql/no1.png)

## 建立 PostgreSQL

<!--    [postgres@ test ~]$ exit -->

运行以下命令︰

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

修改 /etc/init.d/postgresql 文件中的两个变量。 前缀设置 PostgreSQL 的安装路径为︰ **/opt/pgsql**。 PGDATA 设置 PostgreSQL 的数据存储路径为︰ **/opt/pgsql_data**。

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![image](./media/virtual-machines-linux-postgresql/no2.png)

更改文件以使其可执行文件︰

    # chmod +x /etc/init.d/postgresql

启动 PostgreSQL:

    # /etc/init.d/postgresql start

检查是否 PostgreSQL 的终结点上︰

    # netstat -tunlp|grep 1999

您应该看到以下输出︰

![image](./media/virtual-machines-linux-postgresql/no3.png)

## 连接到 Postgres 数据库

再次切换到 postgres 的用户︰

    # su - postgres

创建 Postgres 数据库︰

    $ createdb events

连接到您刚才创建的事件数据库︰

    $ psql -d events

## 创建和删除 Postgres 表

现在您已连接到数据库，可以创建表中。

例如，使用下面的命令创建一个新示例 Postgres 表︰

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

现在已经设置下面的列名称和限制与一个四列的表格︰

1. "名称"列已限制在 20 个字符长的 VARCHAR 命令。
2. "食物"列指示将每个人的食物项目。 VARCHAR 限制此文本将在 30 个字符。
3. "已确认"列会记录此人是否具有 RSVP'd 到家宴。 可接受的值为"Y"和"N"。
4. "日期"列显示当它们注册的事件。 Postgres 要求，日期写为 yyyy-毫米-dd.

如果您的表创建成功，您应看到以下︰

![image](./media/virtual-machines-linux-postgresql/no4.png)

您还可以通过使用以下命令检查表结构︰

![image](./media/virtual-machines-linux-postgresql/no5.png)

### 向表中添加数据

首先，将信息插入行︰

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

您应看到以下输出︰

![image](./media/virtual-machines-linux-postgresql/no6.png)

同样适用于表中可以添加一些更多的人。 以下是一些选项，也可以创建您自己︰

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### 显示表

使用以下命令来显示表︰

    select * from potluck;

输出为︰

![image](./media/virtual-machines-linux-postgresql/no7.png)

### 删除表中的数据

使用下面的命令删除表中的数据︰

    delete from potluck where name=’John’;

这将删除"John"行中的所有信息。 输出为︰

![image](./media/virtual-machines-linux-postgresql/no8.png)

### 更新表中的数据

使用下面的命令来更新表中的数据。 对于此，Sandy 已经确认，她正在参加，因此我们将更改为"Y""N"从她 RSVP:

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##获取有关 PostgreSQL 详细信息
现在，您已完成的 PostgreSQL 在 Azure Linux 虚拟机中安装，您可以享受在 Azure 中使用。 若要了解有关 PostgreSQL 的详细信息，请访问[PostgreSQL 网站](http://www.postgresql.org/)。
