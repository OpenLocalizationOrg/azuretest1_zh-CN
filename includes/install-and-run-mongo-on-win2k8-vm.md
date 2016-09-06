---
ms.openlocfilehash: a488093882bf9a1a94a9bca7ee24c2f36602e6d8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
按照以下步骤安装并运行 Windows 服务器的虚拟机上运行 MongoDB。

> [AZURE.IMPORTANT] 默认情况下未启用 MongoDB 的安全功能，如身份验证和 IP 地址绑定。 将 MongoDB 部署到生产环境之前，应启用安全功能。  有关详细信息，请参见[安全性和身份验证](http://www.mongodb.org/display/DOCS/Security+and+Authentication)。

1. 您已经连接到虚拟机使用远程桌面之后，从虚拟机上的**开始**菜单打开 Internet Explorer。

2. 选择 [**工具**] 按钮，在右上角。  在**Internet 选项**，选择**安全**选项卡，然后选择的**受信任的站点**图标，最后单击**站点**按钮。 添加_http://\*。 mongodb.org_到受信任的站点的列表。

3. 请转至[下载-MongoDB] [MongoDownloads]。

4. 查找**当前稳定发行版**、 Windows 列中选择最新的**64 位**版本，下载并运行 MSI 安装程序。

5. MongoDB 通常被安装在 C:\Program Files\MongoDB 上。 桌面搜索环境变量并将 MongoDB 二进制文件路径添加到 PATH 变量。 例如，可能会发现在 C:\Program Files\MongoDB\Server\3.0\bin 二进制文件在您的计算机上。

6. 创建数据磁盘 （例如驱动器**f:**，） 在 MongoDB 数据和日志目录，在上述步骤中创建。 从**开始**，选择**命令提示符下**打开命令提示符窗口。  类型：

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs

7. 若要运行该数据库，请运行︰

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    当 mongod.exe 服务器启动和预先分配日志文件的所有日志消息将被重都定向到*F:\MongoLogs\mongolog.log*文件。 它可能需要几分钟的 MongoDB 预分配日志文件并启动侦听连接。

8. 若要启动 MongoDB 的管理外壳程序，从**启动**打开另一个命令窗口并键入以下命令︰

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    插入创建数据库。

9. 或者，您可以安装 mongod.exe 作为一种服务︰

        C:\mongodb\bin>mongod --logpath F:\MongoLogs\mongolog.log --logappend --dbpath F:\MongoData\ --install

    这将创建名为"Mongo 数据库"的说明，说明的 MongoDB 的服务。 必须使用**-logpath**选项来指定日志文件，因为正在运行的服务不会命令窗口来显示输出。  **-Logappend**选项指定服务的重新启动将导致输出追加到现有的日志文件。  **-Dbpath**选项指定数据目录的位置。 更多有关服务的命令行选项，请参阅[有关服务的命令行选项] [MongoWindowsSvcOptions]。

    要启动服务，请运行以下命令︰

        C:\mongodb\bin>net start MongoDB

10. 现在，MongoDB 已安装并运行，您将需要在 Windows 防火墙中打开端口，因此您可以远程连接到 MongoDB。  从**开始**菜单中，选择**管理员工具**，然后选择**具有高级安全性的 Windows 防火墙**。

11. 在左窗格中，选择**入站规则**。  在右侧的**操作**窗格中，选择**新建规则...**。

    ![Windows 防火墙][Image1]

    在**新建入站规则向导**中，选择**端口**，然后单击**下一步**。

    ![Windows 防火墙][Image2]

    选择**TCP** ，然后选择**特定的本地端口**。  指定"27017"（默认端口侦听 MongoDB） 的端口，然后单击**下一步**。

    ![Windows 防火墙][Image3]

    选择**允许该连接**，然后单击**下一步**。

    ![Windows 防火墙][Image4]

    再次单击**下一步**。

    ![Windows 防火墙][Image5]

    指定规则，如"MongoPort"的名称，然后单击**完成**。

    ![Windows 防火墙][Image6]

12. 如果没有为 MongoDB 配置终结点时创建虚拟机，您可以做到现在。 您需要的防火墙规则，以便能够远程连接到 MongoDB 的终结点。 在管理门户中，单击**虚拟机**，单击您的新虚拟机的名称，然后单击**终结点**。

    ![终结点][Image7]

13. 单击页面底部的**添加**。 选择**添加一个独立的终结点**，然后单击**下一步**。

    ![终结点][Image8]

14. 添加终结点名称"Mongo"、 协议**TCP**和**公用**和**专用**的端口设置为"27017"。 这将允许远程访问的 MongoDB。

    ![终结点][Image9]

> [AZURE.NOTE] 端口 27017 是 MongoDB 所使用的默认端口。 您可以更改此通过_-端口_子命令来启动 mongod.exe 服务器时。 请确保向提供同一防火墙中的端口号以及在上述说明中的"Mongo"终结点。


[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint3.png
