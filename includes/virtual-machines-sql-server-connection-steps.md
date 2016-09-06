---
ms.openlocfilehash: 5019b6a5717cf4201837f4ed674cab8881de3e77
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
以下步骤演示如何使用 SQL Server 管理 Studio (SSMS) 通过 internet 连接到 SQL Server 实例。 但是，同样的步骤适用于使您的 SQL Server 虚拟机可访问您运行内部部署的应用程序并在 Azure。

您可以从另一个虚拟机或互联网连接到 SQL Server 的实例之前，您必须完成以下任务，如以下各节中所述︰

- [创建虚拟机的 TCP 终结点](#create-a-tcp-endpoint-for-the-virtual-machine)
- [在 Windows 防火墙中打开 TCP 端口](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [配置 SQL Server 侦听 TCP 协议](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [将 SQL Server 配置的混合的模式身份验证](#configure-sql-server-for-mixed-mode-authentication)
- [创建 SQL Server 身份验证登录](#create-sql-server-authentication-logins)
- [确定虚拟机的 DNS 名称](#determine-the-dns-name-of-the-virtual-machine)
- [从另一台计算机连接到数据库引擎](#connect-to-the-database-engine-from-another-computer)

下图概述连接路径︰

![连接到 SQL Server 虚拟机](./media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

### 创建虚拟机的 TCP 终结点

要从 internet 访问 SQL Server，虚拟机必须侦听传入的 TCP 通信的终结点。 此 Azure 的配置步骤中，将定向到虚拟机可以访问 TCP 端口的入站 TCP 端口通讯。

>[AZURE.NOTE] 如果要在同一个云服务或虚拟网络连接时，您不必创建公开可访问终结点。 在这种情况下，您可以继续下一步。 有关详细信息，请参阅[连接方案](../articles/virtual-machines/virtual-machines-sql-server-connectivity.md#connection-scenarios)。

1. 在 Azure 管理门户网站，单击**虚拟**机。
    
2. 单击您新创建的虚拟机上。 介绍了虚拟机有关的信息。
    
3. 靠近顶部的页上，选择**终结点**，然后在页面的底部，单击**添加**。
    
4. 在**添加到虚拟机的终结点**页上，单击**添加一个独立的终结点**，然后单击继续下一步箭头。
    
5. 在**指定终结点的详细信息**页中，提供以下信息。

    - 在**名称**框中，提供方终结点的名称。
    - 在**协议**框中，选择**TCP**。 您可以在**公共端口**框中键入**57500** 。 同样，您可以键入 SQL Server 侦听端口**1433年****专用端口**框中的默认值。 请注意，许多组织选择不同端口号，以避免恶意的安全攻击。 

6. 单击复选标记以继续。 创建终结点。

### 数据库引擎的默认实例在 Windows 防火墙中打开 TCP 端口

1. 连接到虚拟机通过 Windows 远程桌面。 一旦登录，在开始屏幕中，键入**WF.msc**，，然后按 enter 键。 

    ![启动防火墙程序](./media/virtual-machines-sql-server-connection-steps/12Open-WF.png)
2. **具有高级安全性的 Windows 防火墙**，在左窗格中，右键单击**入站规则**，然后在操作窗格中单击**新建规则**。

    ![新规则](./media/virtual-machines-sql-server-connection-steps/13New-FW-Rule.png)

3. 在**新的 Inbount 规则向导**对话框中的**规则类型**下,，选择**端口**，然后单击**下一步**。

4. 在**协议和端口**对话框中，使用默认**TCP**。 在**特定的本地端口**框中，然后键入端口号 (默认实例的**1433年**) 或专用端口在终结点步骤中选择的数据库引擎的实例。 

    ![TCP 端口 1433](./media/virtual-machines-sql-server-connection-steps/14Port-1433.png)

5. 单击**下一步**。

6. 在**操作**对话框中，选择**允许连接**，然后单击**下一步**。

    **安全说明︰**选择**允许安全连接**可以提供更高的安全性。 如果您想要在您的环境中配置附加的安全选项，请选择此选项。

    ![允许连接](./media/virtual-machines-sql-server-connection-steps/15Allow-Connection.png)

7. 在**配置文件**对话框中，选择**公用**、**专用**和**域**。 然后单击**下一步**。 

    **安全说明︰** **公共**选择允许通过互联网访问。 只要有可能，请选择更严格的配置文件。

    ![公共配置文件](./media/virtual-machines-sql-server-connection-steps/16Public-Private-Domain-Profile.png)

8. 在**名称**对话框中，键入一个名称和描述为该规则中，，然后单击**完成**。

    ![规则名称](./media/virtual-machines-sql-server-connection-steps/17Rule-Name.png)

根据需要打开其他端口用于其他组件。 有关详细信息，请参阅[配置 Windows 防火墙允许 SQL Server 访问](http://msdn.microsoft.com/library/cc646023.aspx)。


### 配置 SQL Server 侦听 TCP 协议

1. 同时连接到虚拟机中，在开始页中，键入**SQL Server 配置管理器**，然后按 enter 键。
    
    ![打开 SSCM](./media/virtual-machines-sql-server-connection-steps/9Click-SSCM.png)

2. 在 SQL Server 配置管理器中，在控制台窗格中，展开**SQL Server 网络配置**。

3. 在控制台窗格中，单击**用于 MSSQLSERVER 协议**（他默认实例名称）。在详细信息窗格中，右键单击 TCP，它应是库图像的默认启用的。 您的自定义图像，请单击**启用**（如果其状态为禁用。）

    ![启用 TCP](./media/virtual-machines-sql-server-connection-steps/10Enable-TCP.png)

5. 在控制台窗格中，单击**SQL Server 服务**。 在详细信息窗格中，右键单击**SQL Server （_实例名称_）** （默认实例是**SQL Server (MSSQLSERVER)**），然后单击**重新启动**，停止并重新启动 SQL Server 的实例。 

    ![重新启动数据库引擎](./media/virtual-machines-sql-server-connection-steps/11Restart.png)

7. 关闭 SQL Server 配置管理器。

有关启用协议的 SQL Server 数据库引擎的详细信息，请参阅[启用或禁用服务器网络协议](http://msdn.microsoft.com/library/ms191294.aspx)。

### 将 SQL Server 配置的混合的模式身份验证

SQL Server 数据库引擎不能没有域环境中使用 Windows 身份验证。 若要从另一台计算机连接到数据库引擎，请为混合的模式身份验证配置 SQL Server。 混合的模式身份验证允许 SQL Server 身份验证和 Windows 身份验证。

>[AZURE.NOTE] 配置混合的模式身份验证可能不是有必要的如果您配置了配置的域环境中使用 Azure 虚拟网络。

1. 在连接到虚拟机，在开始页上，键入**SQL Server 2014年管理 Studio** ，单击将选定的图标。

    ![启动 SSMS](./media/virtual-machines-sql-server-connection-steps/18Start-SSMS.png)

    第一次打开管理 Studio 它必须创建用户管理 Studio 环境。 这可能需要几分钟时间。

2. 管理 Studio 将显示**连接到服务器**对话框。 在**服务器名称**框中，键入要连接到数据库引擎使用对象资源管理器的虚拟机的名称。 （而不是虚拟机的名称您可以使用**（本地）**或一段时间内作为**服务器名称**。 选择**Windows 身份验证**，并在**用户名称**框中将**_your_VM_name_\your_local_administrator** 。 单击**连接**。

    ![连接到服务器](./media/virtual-machines-sql-server-connection-steps/19Connect-to-Server.png)

3. SQL Server 管理 Studio 对象资源管理器中，用鼠标右键单击 SQL Server （虚拟机名称），该实例的名称，然后单击**属性**。

    ![服务器属性](./media/virtual-machines-sql-server-connection-steps/20Server-Properties.png)

4. 在**服务器身份验证**下**安全**页上选择**SQL Server 和 Windows 身份验证模式**，然后单击**确定**。

    ![选择身份验证模式](./media/virtual-machines-sql-server-connection-steps/21Mixed-Mode.png)

5. 在 SQL Server 管理 Studio 对话框中，单击**确定**以确认要重新启动 SQL Server 的要求。

6. 在对象资源管理器，右键单击您的服务器，然后单击**重新启动**。 （如果正在运行 SQL Server 代理程序，它必须也重新启动。）

    ![重新启动](./media/virtual-machines-sql-server-connection-steps/22Restart2.png)

7. 在 SQL Server 管理 Studio 对话框中，单击**是**同意要重新启动 SQL Server。

### 创建 SQL Server 身份验证登录

要从另一台计算机连接到数据库引擎，您必须创建至少一个 SQL Server 身份验证登录。

1. SQL Server 管理 Studio 对象资源管理器中，展开要在其中创建新登录的服务器实例的文件夹。

2. 用鼠标右键单击**安全性**文件夹，指向**新建**，然后选择**登录...**。

    ![新建登录](./media/virtual-machines-sql-server-connection-steps/23New-Login.png)

3. 在**新登录**对话框中的在**常规**页上，输入新用户的**登录名**框中的名称。

4. 选择**SQL Server 身份验证**。

5. 在**密码**框中，输入新用户的密码。 在**确认密码**框中再次输入该密码。

6. 要实施的复杂性和强制实施密码策略选项，请选择**强制密码策略**（推荐）。 选择 SQL Server 身份验证时，这是默认选项。

7. 要强制密码的过期策略选项，请选择**强制密码过期**（推荐）。 强制实施密码策略必须选择要启用此复选框。 选择 SQL Server 身份验证时，这是默认选项。

8. 要强制用户登录将使用在第一次创建一个新密码，请选择**用户必须更改在下一次登录的密码**（建议为其他人使用此登录时。 如果登录名是供您自己使用，请不要不选择此选项。）强制实施密码过期，必须选择要启用此复选框。 选择 SQL Server 身份验证时，这是默认选项。 

9. 从**默认数据库**列表中，选择登录名的默认数据库。 **主**是此选项的默认值。 如果尚未创建的用户数据库，请将此设置保留为**母版**。

10. 在**默认语言**列表中，保留作为值的**默认值**。
    
    ![登录属性](./media/virtual-machines-sql-server-connection-steps/24Test-Login.png)

11. 如果这是您要创建的第一次登录时，可能要指定此登录作为 SQL Server 管理员。 如果是这样，在**服务器角色**页上，检查**系统管理员**。 

    **安全说明︰**Sysadmin 固定的服务器角色的成员可以完全控制数据库引擎。 小心地应当限制在此角色中的成员资格。

    ![系统管理员](./media/virtual-machines-sql-server-connection-steps/25sysadmin.png)

12. 单击确定。

有关 SQL Server 登录的详细信息，请参阅[创建登录名](http://msdn.microsoft.com/library/aa337562.aspx)。

### 确定虚拟机的 DNS 名称

要从另一台计算机连接到 SQL Server 数据库引擎，必须知道该虚拟机的域名系统 (DNS) 名称。 （这是互联网用来标识该虚拟机的名称。 可以使用 IP 地址，但 IP 地址可能会更改当 Azure 将冗余或维护资源。 DNS 名称将稳定因为可以被重定向到一个新的 IP 地址。  

1. Azure 管理门户 （或上一步） 中，选择**虚拟机**。 

2. 在**虚拟机实例**页上，下的**快速概览**列中，找到并复制虚拟机的 DNS 名称。

    ![DNS 名称](./media/virtual-machines-sql-server-connection-steps/sql-vm-dns-name.png)
    

### 从另一台计算机连接到数据库引擎
 
1. 在连接到 internet 的计算机上打开 SQL Server 管理 Studio。
2. 在**连接到服务器**或**连接到数据库引擎**对话框中的**服务器名称**框中，输入虚拟机 （确定在前一任务中） 和公共端点端口号格式的*DNSName，端口号*如**tutorialtestVM.cloudapp.net,57500**的 DNS 名称。
要获取的端口号，请登录到 Azure 管理门户中，找到该虚拟机。 在仪表板，请单击**终结点**和使用**公用端口**分配到**MSSQL**。
    ![公用端口](./media/virtual-machines-sql-server-connection-steps/sql-vm-port-number.png)
3. 在**身份验证**框中，选择**SQL Server 身份验证**。
5. 在**登录**框中，键入您在前一任务中创建的登录名。
6. 在**密码**框中，键入您在前一任务中创建的登录密码。
7. 单击**连接**。

    ![使用 SSMS 进行连接](./media/virtual-machines-sql-server-connection-steps/33Connect-SSMS.png)
