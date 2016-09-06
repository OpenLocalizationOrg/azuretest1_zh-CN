---
ms.openlocfilehash: d363b89a4752275fe8db447ecc0b28d364044f70
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

本节说明了如何安装 SQL Server Express，启用 TCP/IP、 设置静态端口，并创建一个数据库，可以用混合连接。  

###安装 SQL Server 速成版

若要使用内部部署 SQL Server 数据库或 SQL Server Express 数据库混合连接，需要静态端口上启用 TCP/IP。 在 SQL Server 上的默认实例使用静态端口 1433，而命名的实例则不能。 正因为如此，我们将安装默认实例。 如果您已经有 SQL Server Express 安装默认实例，则可以跳过本节。

1. 要安装 SQL Server Express，请运行您下载的**SQLEXPRWT_x64_ENU.exe**或**SQLEXPR_x86_ENU.exe**文件。 SQL Server 安装中心向导出现。
    
2. 选择**新的 SQL Server 独立安装或将功能添加到现有安装**，按照说明进行操作，接受默认的选项和设置，直到您到达的**实例配置**页面。
    
3. 在**实例配置**页上，选择**默认实例**，然后接受**服务器配置**页上的默认设置。

    >[AZURE.NOTE]如果您已经安装的 SQL Server 默认实例，可以跳到下一节并与混合连接使用此实例。 
    
5. 在**数据库引擎配置**页上，在**身份验证模式**，选择**混合模式 （SQL Server 身份验证，Windows 身份验证）**，并为内置的**sa**管理员帐户提供一个安全的密码。
    
    在本教程中，您将使用 SQL Server 身份验证。 请务必记住您所提供的密码，因为您以后将需要。
    
6. 完成该向导以完成安装。

###启用 TCP/IP 和设置一个静态端口

本部分将使用 SQL Server 配置管理器，它安装在安装 SQL Server 的表达，来启用 TCP/IP 和设置静态 IP 地址时。 

1. 按照中启用 TCP/IP 访问实例[的 SQL Server 中启用 TCP/IP 网络协议](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx)的步骤。

2. （可选）如果您不能使用默认实例，必须遵循设置实例的静态端口[配置为侦听特定 TCP 端口服务器](https://msdn.microsoft.com/library/ms177440.aspx)中的步骤操作。 如果您完成此步骤，您将连接使用新的端口定义，而不端口 1433年。

3. （可选）如果需要请在防火墙以允许远程访问 SQL Server 进程 (sqlservr.exe) 中添加例外。

###内部部署 SQL Server 实例中创建一个新数据库

1. 在 SQL Server 管理 Studio 中，连接到您刚才安装 SQL Server。 （如果**连接到服务器**不自动显示对话框的左窗格中导航到**对象资源管理器**中，单击**连接**，然后单击**数据库引擎**。）     

    ![连接到服务器](./media/hybrid-connections-create-on-premises-database/A04SSMSConnectToServer.png)
    
    对于**服务器类型**，选择**数据库引擎**。 对于**服务器名称**中，可以使用**本地主机**或计算机的名称在安装 SQL Server。 选择**SQL Server 身份验证**，并提供有关您在前面创建的 sa 登录密码。 
    
2. 若要通过使用 SQL Server 管理 Studio 中创建一个新数据库，用鼠标右键单击在对象资源管理器中的**数据库**，然后单击**新建数据库**。
    
3. 在**新建数据库**对话框中，键入`OnPremisesDB`，然后单击**确定**。 
    
4. 在对象资源管理器中，展开**数据库**，如果您将看到创建的新数据库。

###创建新的 SQL Server 登录，并设置权限

最后，您将使用受限权限创建新的 SQL Server 登录名。 Azure 服务将连接到后端 SQL Server 使用此登录名而不内置的 sa 登录的服务器具有完全控制权限。

1. 在 SQL Server 管理 Studio 对象资源管理器中，右键单击**OnPremisesDB**数据库，然后单击**新建查询**。

2.  将下面的 TSQL 查询粘贴到查询窗口。

        USE [master]
        GO
        
        /* Replace the PASSWORD in the following statement with a secure password. 
           If you save this script, make sure that you secure the file to 
           securely maintain the password. */ 
        CREATE LOGIN [HybridConnectionLogin] WITH PASSWORD=N'<**secure_password**>', 
            DEFAULT_DATABASE=[OnPremisesDB], DEFAULT_LANGUAGE=[us_english], 
            CHECK_EXPIRATION=OFF, CHECK_POLICY=ON
        GO
    
        USE [OnPremisesDB]
        GO
    
        CREATE USER [HybridConnectionLogin] FOR LOGIN [HybridConnectionLogin] 
        WITH DEFAULT_SCHEMA=[dbo]
        GO

        GRANT CONNECT TO [HybridConnectionLogin]
        GRANT CREATE TABLE TO [HybridConnectionLogin]
        GRANT CREATE SCHEMA TO [HybridConnectionLogin]
        GO  
   
3. 在上面的脚本中，替换的字符串`<**secure_password**>`与新*HybridConnectionsLogin*的安全密码。

4. **执行**查询以创建新的登录名并授予在内部数据库中所需的权限。

