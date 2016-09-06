---
ms.openlocfilehash: acf10ec7fd77daa64e07eb659f7de85204041766
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 在 Azure[管理门户](http://manage.windowsazure.com)中，单击**虚拟机**，然后选择您刚创建的 (**testlinuxvm**) 的虚拟机。

2. 在命令栏上单击**附加**，然后单击**附加空白磁盘**。

    显示**附加空白磁盘**对话框。


3. 已经为您定义的**虚拟机的名称**、**存储位置**和**文件名称**。 所有您需要做的是输入所需的磁盘的大小。 在**大小**字段中输入**5** 。

    ![附加的空磁盘][Image2]

    **注意︰**在 Azure 存储.vhd 文件中创建所有的磁盘。 您可以添加到存储.vhd 文件的名称，但 Azure 将自动生成磁盘的名称。

4. 单击复选标记以将数据磁盘连接到虚拟机。

5. 单击以显示仪表板，因此您可以验证数据磁盘已成功附加到虚拟机的虚拟机的名称。 附加的磁盘的**磁盘**表中列出。

    附加数据磁盘时，它才可以使用登录以完成安装。

##连接到虚拟机使用 SSH 或 PuTTY 并完成安装程序
因此可以使用它来存储数据，请登录以完成安装磁盘的虚拟机。

1. 配置虚拟机之后，连接作为**新**使用 SSH 或 PuTTY 和登录 （如上面步骤中所述）。 


2. 在 SSH 或 PuTTY 窗口中键入以下命令，然后输入帐户密码︰

    `$ sudo grep SCSI /var/log/messages`

    您可以查找已添加的邮件中的最后一个数据磁盘的标识符显示 (**sdc**，在本例中)。

    ![GREP][Image4]


3. 在 SSH 或 PuTTY 窗口中，输入以下命令以分区磁盘**/dev/sdc**:

    `$ sudo fdisk /dev/sdc`


4. 输入**n**以创建新的分区。

    ![FDISK][Image5]


5. 要建立主磁盘分区类型**1**分区，以使它的第一个分区，然后键入的类型**p**输入以接受默认值 (1) 圆柱。

    ![FDISK][Image6]


6. 键入**p**有关进行分区的磁盘的详细信息，请参阅。

    ![FDISK][Image7]


7. 键入**w**要写入磁盘的设置。

    ![FDISK][Image8]


8. 格式化新磁盘使用**mkfs**命令︰

    `$ sudo mkfs -t ext4 /dev/sdc1`

9. 接下来，您必须具有可用于安装新的文件系统的目录。 例如，键入以下命令来创建一个新目录装入驱动器，然后输入帐户密码︰

    `sudo mkdir /datadrive`


10. 键入以下命令以装载驱动器︰

    `sudo mount /dev/sdc1 /datadrive`

    现在可以使用**/datadrive**作为数据磁盘了。


11. 将新驱动器添加到 /etc/fstab:

    确保该驱动器已重新装入自动重新启动后它必须添加到 /etc/fstab 文件。 此外，强烈建议的 UUID （通用唯一标识符） 用于在 /etc/fstab 引用驱动器，而不是只是设备名称 (例如 /dev/sdc1)。 若要查找新驱动器的 UUID 可以使用**blkid**实用程序︰
    
        `sudo -i blkid`

    输出将类似于以下形式︰

        `/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"`
        `/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"`
        `/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"`

    >[AZURE.NOTE] blkid 可能不需要在所有情况下的 sudo 访问，但是，它可能更容易使用运行`sudo -i`上如果 /sbin 或/usr/sbin 不在某些分配您`$PATH`。

    **注意︰**不正确地编辑 /etc/fstab 文件中，可能会导致系统无法启动。 如果不确定，请参阅有关如何正确编辑此文件的信息的分发文档。 此外建议，在编辑之前创建 /etc/fstab 文件的备份。

    使用文本编辑器，输入新的文件系统在 /etc/fstab 文件末尾的信息。  在本例中我们将使用 UUID 值在前面的步骤，并装载点**/datadrive**中创建的新**/dev/sdc1**设备︰

        `UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2`

    如果创建附加的数据驱动器或分区将需要输入/等/fstab 分别也。

    现在可以测试，只需卸载，然后即重新挂载文件系统中，通过正确装载文件系统 该示例使用装入点`/datadrive`在前面步骤中创建︰ 

        `sudo umount /datadrive`
        `sudo mount /datadrive`

    如果第二个命令将产生一个错误，请检查 /etc/fstab 文件中的，语法正确。


    >[AZURE.NOTE] 随后删除数据磁盘，而无需编辑 fstab 可能会导致虚拟机无法启动。 如果这是很常见，那么大多数版本提供以下任一`nofail`和/或`nobootwait`fstab 选项，将允许系统引导即使磁盘不存在。 请有关这些参数的详细信息，参阅您的发行版的文档。


[Image2]: ./media/attach-data-disk-centos-vm-in-portal/AttachDataDiskLinuxVM2.png
[Image4]: ./media/attach-data-disk-centos-vm-in-portal/GrepScsiMessages.png
[Image5]: ./media/attach-data-disk-centos-vm-in-portal/fdisk1.png
[Image6]: ./media/attach-data-disk-centos-vm-in-portal/fdisk2.png
[Image7]: ./media/attach-data-disk-centos-vm-in-portal/fdisk3.png
[Image8]: ./media/attach-data-disk-centos-vm-in-portal/fdisk4.png
[Image9]: ./media/attach-data-disk-centos-vm-in-portal/mkfs.png

