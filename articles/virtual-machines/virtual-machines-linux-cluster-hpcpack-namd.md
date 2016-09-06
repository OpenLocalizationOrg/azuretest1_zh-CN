---
ms.openlocfilehash: f0c52f38244baf478b56a3b8cb430485ba9ed96e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
 pageTitle="与 Linux 虚拟机上的 Microsoft HPC 包 NAMD |Microsoft Azure"
 description="部署 Microsoft HPC 包群集在 Azure 上的并在多个 Linux 计算节点上运行 charmrun NAMD 模拟。"
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/02/2015"
 ms.author="danlep"/>

# 在 Azure 中的 Linux 计算节点上运行 Microsoft HPC 包 NAMD

本文介绍如何部署 Microsoft HPC 包群集在 Azure 上的并运行[NAMD](http://www.ks.uiuc.edu/Research/namd/) **charmrun**多个 Linux 上的作业计算虚拟群集网络计算和可视化的一个大型 biomolecular 系统结构中的节点。

NAMD （Nanoscale 分子动态程序） 的是专为高性能模拟的大型 biomolecular 系统包含多达数以百万计的原子，如病毒、 单元结构和大型 proteins 设计一个并行的分子动态包。 NAMD 可以扩展到数百个典型模拟的内核和 500000 多个内核的最大的模拟。

Microsoft HPC 包提供了运行各种大规模 HPC 和并行应用程序，包括 Microsoft Azure 虚拟机群集上的 MPI 应用程序的功能。 从 Microsoft HPC 包 2012 R2 开始，HPC 软件包还支持运行 Linux HPC 应用程序在 Linux 上的计算 Vm 部署包 HPC 群集中的节点。 有关的简介与 HPC 包一起使用 Linux 计算节点，请参阅[开始使用 Linux 计算在 Azure 中包 HPC 群集中的节点](virtual-machines-linux-cluster-hpcpack.md)。


## 先决条件

* **HPC 包群集使用 Linux 计算节点**-请参阅[开始使用 Linux 计算在 Azure 中包 HPC 群集中的节点](virtual-machines-linux-cluster-hpcpack.md)的前提条件和步骤在 Azure 市场使用 Azure PowerShell 脚本和 HPC 包映像部署 HPC 包节点的群集，Linux 计算在 Azure 上。

    下面是示例 XML 配置文件可用于脚本部署 Windows Server 2012 R2 头节点包含一个基于 Azure 的 HPC 包群集和 4 尺寸大 (A3) CentOS 6.6 计算节点。 替换为相应订阅和服务名称的值。

    ```
    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionName>Subscription-1</SubscriptionName>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>West US</Location>  
      <VNet>
        <VNetName>MyVNet</VNetName>
        <SubnetName>Subnet-1</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpclab.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>CentOS66HN</VMName>
        <ServiceName>MyHPCService</ServiceName>
        <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
    <VMSize>Large</VMSize>
    <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```


* **NAMD 软件和教程文件**-linux 从[NAMD](http://www.ks.uiuc.edu/Research/namd/)网站下载 NAMD 软件。 这篇文章基于 NAMD 版本 2.10，并使用[Linux x86_64 (64 位英特尔/AMD 与以太网)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310)存档，您将使用在多个 Linux 计算节点中的群集网络上运行 NAMD。 下载[NAMD 教程文件](http://www.ks.uiuc.edu/Training/Tutorials/#namd)。 请按照本文后面的说明进行操作，以提取存档和群集的头节点上的教程示例。

* **VMD** （可选）-查看 NAMD 作业的结果下载并在您选择的计算机上安装[VMD](http://www.ks.uiuc.edu/Research/vmd/)的分子可视化程序。 当前版本是 1.9.2。 请参阅下载站点着手 VMD。  


## 设置计算节点之间的相互信任
在 Linux 中的多个节点上运行跨节点作业要求相互信任 （通过**rsh**或**ssh**） 的节点。 HPC 包群集创建与 Microsoft HPC 包 IaaS 部署脚本时，脚本将自动设置为管理员帐户指定的永久相互信任。 对于群集的域中创建非管理员用户，您必须设置临时节点之间的相互信任时作业分配给它们，并销毁作业完成后的关系。 若要为每个用户执行此操作，提供到群集 HPC 包用来建立信任关系的 RSA 密钥对。

### 生成 RSA 密钥对
很容易地生成 RSA 密钥对，它通过运行 Linux **ssh keygen**命令包含一个公钥和一个私钥。

1.  登录到 Linux 计算机上。

2.  运行以下命令。

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] 按**enter 键**，以使用默认设置，直到完成该命令。 请不要输入密码当系统提示您输入密码，只需按**enter 键**。

    ![生成 RSA 密钥对][keygen]

3.  将目录更改到 ~/.ssh 目录。 私钥存储在 id_rsa 和 id_rsa.pub 中的公钥。

    ![私钥和公钥][keys]

### 添加到包 HPC 群集密钥对
1.  头节点与 HPC 包管理员帐户 （运行部署脚本时，您设置了管理员帐户） 进行远程桌面连接。

2. 使用标准 Windows Server 过程群集的 Active Directory 域中创建一个域用户帐户。 例如，在头节点上使用活动目录用户和计算机工具。 本文中的示例假定您创建了一个名为 hpclab\hpcuser 的域用户。

2.  创建名为 C:\cred.xml 的文件并将 RSA 密钥数据复制到其中。 您可以在本文末尾的附录中找到此文件的一个示例。

    ```
    <ExtendedData>
      <PrivateKey>Copy the contents of private key here</PrivateKey>
      <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  打开一个命令窗口并输入以下命令以设置为 hpclab\hpcuser 帐户凭据数据。 **Extendeddata**参数用于传递 C:\cred.xml 您创建密钥数据文件的名称。

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    不输出的情况下，该命令已成功完成。 之后设置您需要运行作业的用户帐户的凭据，将 cred.xml 文件存储在安全的位置，或将其删除。

5.  如果您在一个 Linux 节点上生成 RSA 密钥对，请记住要使用它们完成后删除注册表项。 如果发现现有的 id_rsa 或 id_rsa.pub 文件，HPC 包不向上相互信任设置。

>[AZURE.IMPORTANT] 我们不建议上共享群集，群集管理员运行 Linux 作业因为作业提交由管理员在使用超级用户帐户下运行 Linux 节点上。 由非管理员用户提交的作业运行本地的 Linux 用户帐户具有相同的名称作为作业的用户，并在 HPC 包设置为此 Linux 用户的相互信任，在分配给作业的所有节点。 您可以设置运行该作业之前, 在 Linux 节点上手动的 Linux 用户或 HPC 包创建用户自动提交作业时。 如果 HPC 包创建用户，HPC 包在作业完成后删除它。 以减少安全威胁的节点上的作业完成后删除键。

## 设置文件共享的 Linux 节点

现在设置在头节点上的某个文件夹的 SMB 共享的标准和允许 Linux 节点访问的常见路径的 NAMD 文件的所有 Linux 节点上装载的共享的文件夹。 文件共享选项，[开始使用 Linux 计算节点在 Azure 中包 HPC 群集](virtual-machines-linux-cluster-hpcpack.md)中的步骤，请参阅。 （我们建议在这篇文章中的 head 节点上装载的共享的文件夹，因为 CentOS 6.6 Linux 节点当前不支持 Azure 文件服务，这可提供类似的功能。 有关装载 Azure 文件共享的详细信息，请参阅[Persisting 连接到 Microsoft Azure 文件](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)。）

1.  在头节点上创建一个文件夹和共享它的所有人都通过设置读/写权限。 在此示例中， \\\\CentOS66HN\Namd 是 CentOS66HN 后的头节点的主机名的文件夹的名称。

2. 将文件夹中的 NAMD 文件提取.tar 存档文件上使用**tar**或另一个 Windows 实用工具，用于运行的 Windows 版本。 解压缩到 NAMD tar 存档文件\\\\CentOS66HN\Namd\namd2 和提取教程文件下\\\\CentOS66HN\Namd\namd2\namdsample。

2.  打开 Windows PowerShell 窗口并运行以下命令以装载的共享的文件夹。

    ```
    PS > clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    PS > clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

第一个命令创建一个名为 LinuxNodes 组中的所有节点上的 /namd2 文件夹。 第二个命令装载到 dir_mode 文件夹共享的文件夹 //CentOS66HN/Namd/namd2 和 file_mode 位设置为 777。 *用户名*和*密码*在命令应在头节点上的用户的凭据。

>[AZURE.NOTE]"\`"中的第二个命令符号是 PowerShell 转义符号。 "\`，"表示"，"（逗号） 是该命令的一部分。


## 运行 NAMD 作业作准备

 NAMD 作业需要**charmrun**知道启动 NAMD 进程时要使用的节点数的*列表*文件。 您将编写 Bash 脚本来生成列表文件并使用该列表文件运行**charmrun** 。 然后，您可以提交 NAMD 作业在 HPC 群集管理器中调用此脚本。

### 环境变量和列表文件
有关节点和核心信息是当作业被激活时自动设置 HPC 包头节点的 $CCP_NODES_CORES 环境变量中。 $CCP_NODES_CORES 变量的格式如下所示︰

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
```

此处列出的节点、 节点名称和分配给作业的每个节点上的内核数的总数。 例如，如果作业需要 10 个内核运行，$CCP_NODES_CORES 的值将类似于︰

```
3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
```

以下是该脚本将生成列表文件中的信息︰

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

例如︰

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```
### Bash 脚本，以创建列表文件

使用您所选的文本编辑器，包含 NAMD 程序文件的文件夹中创建下面的 Bash 脚本并将其命名为 hpccharmrun.sh。 该文件的完整示例是本文的附录中。 此 bash 脚本将执行以下操作。

>[AZURE.TIP] 保存您的脚本视为文本文件使用 Linux 的行尾 （仅换行符，不 CR LF）。 这将确保它正确运行 Linux 节点上。

1.  定义一些变量。

    ```
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  从环境变量中获取节点的信息。 $NODESCORES 存储从 $CCP_NODES_CORES 的拆分单词的列表。 $COUNT 是 $NODESCORES 的大小。

    ```
    # Get node information from the environment variables
    # CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```

3.  如果未设置 $CCP_NODES_CORES 变量，只需直接启动**charmrun** 。 （这应该只出现在您直接在您的 Linux 节点上运行此脚本时。）

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  或创建列表文件的**charmrun**。

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  **Charmrun**列表文件，获取其返回的状态和运行删除结尾处的列表文件。

    ${CCP_NUMCPUS} 是 HPC 包头节点来设置的另一个环境变量。 它将存储分配给此作业的总内核数。 我们可以使用它来指定为 charmrun 的进程数。

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  以**charmrun**返回状态退出。

    ```
    exit ${RTNSTS}
    ```

## 提交 NAMD 作业

现在您已准备提交 NAMD 作业在 HPC 群集管理器中。

1.  连接到群集的头节点并启动 HPC 群集管理器。

2.  在**节点管理**确保 Linux 计算节点都处于**联机**状态。 如果不是，请将其选定并单击**联机**。

2.  在**作业管理**中，单击**新建作业**。

3.  输入如*hpccharmrun*的作业的名称。

    ![新的 HPC 作业][namd_job]

4.  在**作业详细信息**页上，在**作业资源**，作为**节点**选择的资源的类型和所需的**最小值**设置为 3。 在此示例中我们将在 3 个 Linux 节点上运行作业，每个节点有 4 内核。

    ![作业的资源][job_resources]

5.  在**任务详细信息和 I/O 重定向**页上，将新任务添加到作业并设置以下值。

    * **命令行** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

    * **工作目录**-/namd2

    * **最小值**-3

    ![任务详细信息][task_details]

    >[AZURE.NOTE] 因为**charmrun**尝试导航到每个节点上的相同工作目录在此处设置的工作目录。 如果未设置工作目录，则 HPC 包在一个 Linux 节点上创建一个随机命名文件夹中启动命令。 这在其他节点上将导致以下错误︰`/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.`若要避免此问题，请指定一个文件夹路径，可以在工作目录的所有节点进行访问。

5.  单击**提交**以运行此作业。

    默认情况下，HPC 包提交的作业作为您的当前登录的用户帐户。 一个对话框可能会提示您输入用户名和密码后单击**提交**。

    ![作业的凭据][creds]

    在某些情况下 HPC 包会记住用户信息输入之前，将不会显示此对话框。 如果要再次显示它的 HPC 包，请在命令窗口中输入以下，然后提交作业。

    ```
    hpccred delcreds
    ```

6.  该作业需要几分钟才能完成。

7.  查找在作业日志\\<headnodeName>\Namd\namd2\namd2_hpccharmrun.log 文件和输出文件中的\\<headnode>\Namd\namd2\namdsample\1-2-sphere\.

8.  （可选） 启动 VMD 查看作业结果。 直观显示 NAMD 的步骤输出文件 （在此例中 ubiquitin 蛋白分子在水球体） 都是超出了本文的范围。 有关详细信息，请参阅[NAMD 教程](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf)。

    ![作业结果][vmd_view]

## 附录

### Hpccharmrun.sh 脚本示例

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 
### Cred.xml 文件示例

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```




<!--Image references-->
[keygen]: ./media/virtual-machines-linux-cluster-hpcpack-namd/keygen.png
[密钥]: ./media/virtual-machines-linux-cluster-hpcpack-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-cluster-hpcpack-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-cluster-hpcpack-namd/job_resources.png
[凭据设置]: ./media/virtual-machines-linux-cluster-hpcpack-namd/creds.png
[task_details]: ./media/virtual-machines-linux-cluster-hpcpack-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-cluster-hpcpack-namd/vmd_view.png
