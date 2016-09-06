---
ms.openlocfilehash: 56598261ae55f6255a8fc387e1255ae1f09e7332
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 请确保它当前正在运行 IIS Express 中时停止移动的服务。 右键单击 IIS Express 任务栏图标，然后单击移动服务**停止**。

    ![](./media/mobile-services-how-to-configure-iis-express/iis-express-tray-stop-site.png)


2. 在命令提示符窗口中，运行**ipconfig**命令来查找您的工作站的有效本地 IP 地址。

    ![](./media/mobile-services-how-to-configure-iis-express/ipconfig.png)


3. 在 Visual Studio，为 IIS Express 中打开 applicationhost.config 文件。 此文件位于以下用户配置文件目录的子目录中。

        C:\Users\<your profile name>\Documents\IISExpress\config\applicationhost.config

4. 配置 IIS Express 允许远程连接到该服务的请求。 为此，请在 applicationhost.config 文件中，查找您的移动服务站点元素并添加一个新`binding`元素，为端口使用上面提到的 IP 地址。 然后将保存的 applicationhost.config 文件。 

    更新的站点元素应类似于如下︰

        <site name="todolist_Service(1)" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="C:\Archive\GetStartedDataWP8\C#\todolist_Service" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:58203:localhost" />
                <binding protocol="http" bindingInformation="*:58203:192.168.137.72" />
            </bindings>
        </site>

5. 打开 Windows 防火墙控制台，创建新的端口规则，以允许连接到该端口。 创建新的 Windows 防火墙端口规则的详细信息，请参阅[如何添加一个新的 Windows 防火墙端口规则]。

    >[AZURE.NOTE] 如果您测试的计算机加入到域中，域策略可能控制防火墙例外。 在这种情况下，您需要与您的域管理员联系，以获取您计算机上的端口例外。

    您现在应该配置为使用 IIS Express 承载您的移动服务进行测试。 

    >[AZURE.NOTE] 一旦您完成您本地的服务的测试，则应删除您创建的 Windows 防火墙规则。 


<!-- URLs. -->
[如何添加新的 Windows 防火墙端口规则]:  http://go.microsoft.com/fwlink/?LinkId=392240
