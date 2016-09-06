---
ms.openlocfilehash: 71b5bc4bafdc00096e61fe54de6accf40469d502
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties services="virtual-machines" title="How to Log on to a Virtual Machine Running Windows Server" authors="KBDAzure" solutions="" manager="timlt" editor="tysonn" />

1. 如果您还没有登录到[Azure 门户](http://manage.windowsazure.com)的这么做。

2. 单击**虚拟机**，然后选择相应的虚拟机。

3. 在命令栏上，单击**连接**。

    ![登录到虚拟机](./media/virtual-machines-log-on-win-server/connectwindows.png)

4. 单击**打开**要用于虚拟机的远程桌面协议文件，将自动创建。

5. 单击**连接**以继续。

    ![继续进行连接](./media/virtual-machines-log-on-win-server/connectpublisher.png)

6. 在虚拟机上，键入管理员帐户的凭据，然后单击**确定**。

 >[AZURE.TIP] 在大多数情况下，您将使用的用户名和密码时创建的虚拟机指定。 请检查以确保其具有正确的域信息的用户名︰

>- 如果 VM 属于您组织中的域，请确保用户名称包含该域的名称。
- 如果 VM 不属于某个域，或者移除任何域的信息通过启动在行的\'或使用虚拟机名称与域的名称。 例如，`\MyUserName`或`MyTestVM\MyUserName`。
- 如果虚拟机是一个域控制器，请键入用户名和域的域管理员帐户的密码。

单击**是**以验证虚拟机器的身份。

![验证计算机的身份](./media/virtual-machines-log-on-win-server/connectverify.png)

您现在可以使用虚拟机远程。
