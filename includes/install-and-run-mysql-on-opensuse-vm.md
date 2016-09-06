---
ms.openlocfilehash: 53e921142a354eaf7378e842732f135c993035f6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 以提升的权限，请键入︰

        sudo -s

    请输入您的密码。

2. 若要安装 MySQL 社区服务器版本，请键入︰

        # zypper install mysql-community-server

    请稍候 MySQL 下载并安装。

3. 若要设置 MySQL 在系统引导时启动，请键入︰

        # insserv mysql

4. 使用此命令手动启动 MySQL 后台程序 (mysqld):

        # rcmysql start

    若要检查 MySQL 后台进程的状态，请键入︰

        # rcmysql status

    若要停止 MySQL 守护程序，请键入︰

        # rcmysql stop

    > [AZURE.IMPORTANT] 安装完成后，MySQL root 密码为空，默认情况。 我们建议您运行**mysql\_安全\_安装**，有助于安全 MySQL 的脚本。 该脚本会提示您更改 MySQL root 密码、 删除匿名用户帐户，禁用远程根用户登录、 删除测试数据库并重新加载权限表。 我们建议您回答是对所有这些选项，并更改超级用户口令。

5. 键入以下指令来运行 MySQL 安装脚本的脚本︰

        $ mysql_secure_installation

6. 运行后，您可以登录到 MySQL:

        $ mysql -u root -p

    输入 MySQL root 密码 （这一步中，您更改了），您将会看到提示的位置可以发出 SQL 语句与数据库进行交互。

7. 若要创建新的 MySQL 用户，运行以下命令在**mysql >**提示︰

        mysql> CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';

    请注意，以分号 （;） 结尾的行至关重要的终止命令。

8. 若要创建数据库并授予`mysqluser`用户权限，发出以下命令︰

        mysql> CREATE DATABASE testdatabase;
        mysql> GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';

    注意通过连接到数据库的脚本只使用数据库用户名称和密码。  数据库名称不一定代表实际的用户帐户在系统上的用户帐户。

9. 要从另一台计算机登录，请键入︰

        mysql> GRANT ALL ON testdatabase.* TO 'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';

    其中`ip-address`是将连接到 MySQL 的计算机的 IP 地址。

10. 若要退出 MySQL 数据库管理实用程序，请键入︰

        quit

11. MySQL 安装后，您将需要配置的终结点，远程访问 MySQL。 登录到[Azure 的门户网站][AzurePortal]。 单击**虚拟机**，请单击您的新虚拟机的名称，然后单击**终结点**。

12. 单击页面底部的**添加**。

13. 添加终结点命名为"MySQL"与**TCP**协议和**公用**和**专用**的端口设置为"3306"。

14. 若要从您的计算机远程连接到虚拟机，请键入︰

        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net

    例如，使用我们在本教程中创建的虚拟计算机，键入以下命令︰

        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
