---
ms.openlocfilehash: a46356886b82c1545a44d5ff0aa25bc26d90e171
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
请按照这些步骤来安装和运行在虚拟机运行 CentOS Linux MongoDB。

> [AZURE.WARNING] 默认情况下未启用 MongoDB 的安全功能，如身份验证和 IP 地址绑定。 将 MongoDB 部署到生产环境之前，应启用安全功能。  有关详细信息，请参见[安全性和身份验证](http://www.mongodb.org/display/DOCS/Security+and+Authentication)。

1. 这样您可以安装 MongoDB 配置包管理系统 (YUM)。 创建一个*/etc/yum.repos.d/10gen.repo*文件，有关您的存储库中保存的信息，并添加下列代码︰

        [10gen]
        name=10gen Repository
        baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64
        gpgcheck=0
        enabled=1

2. Repo 文件保存，然后运行下面的命令以更新本地软件包数据库︰

        $ sudo yum update

3. 要安装程序包，请运行以下命令以安装 MongoDB 和相关的工具的最新稳定版本︰

        $ sudo yum install mongo-10gen mongo-10gen-server

    请稍候 MongoDB 下载并安装。

4. 创建一个数据目录。 默认情况下 MongoDB 将数据存储在*/data/db*目录中，但您必须创建该目录。 若要创建它，请运行︰

        $ sudo mkdir -p /srv/datadrive/data
        $ sudo chown `id -u` /srv/datadrive/data

    在 Linux 上安装 MongoDB 的详细信息，请参阅[快速入门 Unix][QuickstartUnix]。

5. 要启动数据库，请运行︰

        $ mongod --dbpath /srv/datadrive/data --logpath /srv/datadrive/data/mongod.log

    如 MongoDB 服务器启动并预先分配日志文件的所有日志消息将被重都定向到*/srv/datadrive/data/mongod.log*文件。 它可能需要几分钟的 MongoDB 预分配日志文件并启动侦听连接。

6. 若要启动 MongoDB 的管理外壳程序，打开一个单独的 SSH 或 PuTTY 窗口并运行︰

        $ mongo
        > db.foo.save ( { a:1 } )
        > db.foo.find()
        { _id : ..., a : 1 }
        > show dbs  
        ...
        > show collections  
        ...  
        > help  

    插入创建数据库。

7. 一旦安装了 MongoDB 必须这样，MongoDB 可以远程访问配置终结点。 在管理门户中，单击**虚拟机**，然后单击您的新虚拟机的名称然后单击**终结点**。
    
    ![终结点][Image7]

8. 单击页面底部的**添加终结点**。
    
    ![终结点][Image8]

9. 添加终结点具有以下设置︰

 - **名称**︰ Mongo
 - **协议**︰ TCP
 - **公用端口**︰ 27017
 - **专用端口**︰ 27017
 
 这将允许远程访问的 MongoDB。



[QuickStartUnix]: http://www.mongodb.org/display/DOCS/Quickstart+Unix


[Image7]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint2.png
