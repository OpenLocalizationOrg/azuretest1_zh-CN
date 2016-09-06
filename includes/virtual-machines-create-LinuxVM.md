---
ms.openlocfilehash: d1936ca7705cdd7e7bb2af35bd1b3892a3dceeca
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 登录到 Azure 订阅使用[连接到 Azure CLI 从 Azure](../articles/xplat-cli-connect.md)中列出的步骤。

2. 确保您的服务管理模式中使用︰

        azure config mode asm

3. 找出您想要从可用图像加载 Linux 映像︰

        azure vm image list | grep "Linux"

4. 使用`azure vm create`与 Linux 映像从上面的列表中创建新的虚拟机。 此步骤会创建一个新的云服务，以及一个新的存储帐户。 则可以将此虚拟机连接到现有的云服务与`-c`选项。 它还将创建 SSH 登录到 Linux 虚拟机与终结点`-e`选项。

        ~$ azure vm create "MyTestVM" b4590d9e3ed742e4a1d46e5424aa335e__suse-opensuse-13.1-20141216-x86-64 "adminUser" -z "Small" -e -l "West US"
        info:    Executing command vm create
        + Looking up image b4590d9e3ed742e4a1d46e5424aa335e__suse-opensuse-13.1-20141216-x86-64
        Enter VM 'adminUser' password:*********
        Confirm password: *********
        + Looking up cloud service
        info:    cloud service MyTestVM not found.
        + Creating cloud service
        + Retrieving storage accounts
        + Creating a new storage account 'mytestvm1437604756125'
        + Creating VM
        info:    vm create command OK

    >[AZURE.NOTE] 对于 Linux 虚拟机，您必须提供`-e`选项`vm create`;不能创建虚拟机后启用 SSH。 有关 SSH 的更多详细信息，请阅读[如何使用 SSH 使用 Linux 在 Azure 上](../articles/virtual-machines/virtual-machines-linux-use-ssh-key.md)。

    请注意，图像*b4590d9e3ed742e4a1d46e5424aa335e__suse-opensuse-13.1-20141216-x86-64*是我们在上述步骤中的图像列表中选择的一个。 *MyTestVM*是我们新的虚拟机的名称， *adminUser*是我们将使用 SSH 到虚拟机中的用户名。 您可以根据需要替换这些变量。 此命令的详细信息，请访问[使用 Azure CLI 使用 Azure 服务管理](../articles/virtual-machines/virtual-machines-command-line-tools.md)。

5. 新创建的 Linux 虚拟机将由给定的列表中显示︰

        azure vm list

6. 可以使用命令来验证该虚拟机的属性︰

        azure vm show MyTestVM

7. 新创建的虚拟机已准备就绪，开始`azure vm start`命令。

所有这些 Azure CLI 虚拟机命令的详细信息，请阅读[使用 Azure CLI 使用服务管理 API](../articles/virtual-machines/virtual-machines-command-line-tools.md)。
