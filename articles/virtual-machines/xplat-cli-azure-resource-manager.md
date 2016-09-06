---
ms.openlocfilehash: 8aba00df60aa795adac315c141c7925e584fe9f3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用 Microsoft Azure CLI Mac、 Linux 和 Windows Azure 资源管理 |Microsoft Azure"
    description="使用 Microsoft Azure CLI Mac、 Linux、 Windows 使用 Azure 资源管理器。"
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="virtual-machines"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services""
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/09/2015"
    ms.author="danlep"/>

# Azure CLI 用于 Mac、 Linux、 Windows 使用 Azure 资源管理器

> [AZURE.SELECTOR]
- [Azure PowerShell](../powershell-azure-resource-manager.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)


本文介绍如何创建、 管理和删除 Azure 资源和 Vm 使用 Mac、 Linux、 和使用 Azure 资源管理器模式的 Windows Azure CLI。  

>[AZURE.NOTE] 要创建和管理命令行上的 Azure 资源，您将需要 Azure 帐户 （[免费试用版在此处](http://azure.microsoft.com/pricing/free-trial/)）。 您还需要对[安装 Azure CLI](../xplat-cli-install.md)，和[登录使用 Azure 与您的帐户相关联的资源](../xplat-cli-connect.md)。 如果所做的这些事情，你就可以运行。

## Azure 的资源

使用 Azure 资源管理器_资源_（用户管理的实体，如虚拟机、 数据库服务器、 数据库或网站） 的一组作为单个逻辑单元或_资源组_来管理。 可以创建、 管理和删除命令的方式在命令行中，这些资源就像在 asm 模式中一样。

使用 Azure 资源管理器模式时，可以通过描述的结构和关系的 JSON*模板*中的资源的可部署组还管理 Azure 资源以_声明性_方式。 该模板描述可以填写任何内联时运行的命令或存储在一个单独的 JSON azuredeploy parameters.json 文件中的参数。 这使您可以轻松地创建新的资源使用相同的模板，只是通过不同的参数。 例如，创建一个网站的模板具有网站的名称，将位于该网站中，该区域和其他常用参数的参数。

模板用于修改或创建一个组，将创建_部署_，其中然后应用于组。 在 Azure 资源管理器的详细信息，请访问[Azure 资源管理器的概述](../resource-group-overview.md)。

## 身份验证

工作与 Azure 资源管理器通过 Azure CLI 要求您验证到 Microsoft Azure 使用工作或学校的帐户。 通过.publishsettings 文件安装的证书进行身份验证将不起作用。

使用工作或学校的帐户进行验证的详细信息，请参阅[连接到 Azure CLI 从 Azure 订阅](../xplat-cli-connect.md)。

> [AZURE.NOTE] 因为您使用的工作或学校帐户-由 Azure Active Directory 管理 — 您可以使用 Azure Role-Based 的访问控制 (RBAC) 来管理访问和 Azure 资源的使用情况。 有关详细信息，请参阅[管理和审核对资源的访问](../resource-group-rbac.md)。

## 设置 Azure 资源管理器模式

因为默认情况下未启用 Azure 资源管理器模式，您必须使用以下命令启用 Azure CLI 资源管理器命令。

    azure config mode arm

>[AZURE.NOTE] Azure 资源管理器模式和 Azure 服务管理模式是互斥的。 也就是说，在一个模式中创建的资源不能从另一种模式进行管理。

## 查找位置

大部分的 Azure 资源管理器命令需要有效的位置，以创建或查找资源。 您可以通过使用以下命令找到所有可用的位置。

    azure location list

它将列出位置特定于等地区，"西美国"、"东部美国"，等等。

## 创建资源组

资源组是网络、 存储和其他资源的逻辑分组。 在 Azure 资源管理器模式下几乎所有的命令需要一个资源组。 您可以创建资源组，命名为_testrg_，例如，使用下面的命令。

    azure group create -n "testrg" -l "West US"

您可以启动之后，将资源添加到该组，并使用它来配置新的虚拟机。

## 创建虚拟机

有两种方法可以在 Azure 资源管理器模式中创建虚拟机︰

1. 使用各个 Azure CLI 命令。
2. 使用资源组模板。

请务必使用下列任一方法启动之前创建至少一个资源组。

### 使用各个 Azure CLI 命令

这是基本的方法来配置和创建虚拟机根据您的需要。 在 Azure 资源管理器模式中，将需要配置一些必需的资源，如网络之前，您可以使用**虚拟机创建**命令。

>[AZURE.NOTE] 如果您第一次在命令行上为您的订阅创建资源，可能提示您注册为某些资源提供程序。
> 如果发生这种情况，很容易以注册该提供程序，然后重试命令失败，接下来的示例中所示。
>
> `azure provider register Microsoft.Storage`
>
> 您可以找出提供程序通过运行下面的命令来注册您的订阅列表。
>
> `azure provider list`


#### 创建公用 IP 资源

您将需要创建公用 IP，这样您可以将 SSH 到您新的虚拟机的任何有意义的工作。 创建一个公用 IP 是简单的。 该命令需要资源组、 公用 IP 资源，并且该订单中的某个位置的名称。

    azure network public-ip create "testrg" "testip" "westus"

#### 创建网络接口卡资源

网络接口卡或 NIC 需要子网和第一次创建虚拟网络。 通过使用**网络 vnet create**命令特定的位置和资源组中创建虚拟网络。

    azure network vnet create "testrg" "testvnet" "westus"

然后可以通过使用**vnet 子网创建**命令，在这个虚拟的网络中创建子网。

    azure network vnet subnet create "testrg" "testvnet" "testsubnet"

您应该能够创建 NIC 的**网络 nic 创建**命令使用这些资源。

    azure network nic create "testrg" "testnic" "westus" -k "testsubnet" -m "testvnet" -p "testip"

>[AZURE.NOTE] 虽然可选的它是非常重要的公共 IP 名称作为参数进行传递给**网络 nic 创建**与此命令绑定到此 IP，这将是以后 NIC 用于 SSH 到虚拟机中创建使用此网卡。

**网络**命令的更多信息，请参阅命令行帮助或[使用 Azure CLI 使用 Azure 资源管理](azure-cli-arm-commands.md)。

#### 查找操作系统映像

目前，您只能找到基于图像的发布服务器上的操作系统。 换句话说，您必须运行以下命令，以在所需的位置中查找 OS 映像发布者的列表。

    azure vm image list-publishers "westus"

然后从列表中选择发布服务器，并通过运行以下命令找到由该发行者的图像列表。

    azure vm image list "westus" "CoreOS"

最后，从下面的示例类似于下面的列表选择 OS 映像。

    info:    Executing command **vm image list**
    warn:    The parameters --offer and --sku if specified will be ignored
    + Getting virtual machine image offers (Publisher: "Canonical" Location: "westus")
    data:    Publisher  Offer        Sku          Version          Location  Urn

    data:    ---------  -----------  -----------  ---------------  --------- ----------------------------------------
    data:    CoreOS     CoreOS       Alpha        475.1.0          westus    CoreOS:CoreOS:Alpha:475.1.0
    data:    CoreOS     CoreOS       Alpha        490.0.0          westus    CoreOS:CoreOS:Alpha:490.0.0

保存要在虚拟机加载的图像的 URN 名。 在文章的后面，您将使用它。

#### 创建虚拟机

现在您可以通过运行**vm 创建**命令和传递所需的信息创建一个虚拟机。 它是可选的在此阶段，传递公用 IP，因为网卡已出现此信息。 您的命令可能类似于以下示例中， _testvm_所在的_testrg_资源组中创建的虚拟机的名称。

    azure-cli@0.8.0:/# azure vm create "testrg" "testvm" "westus" "Linux" -Q "CoreOS:CoreOS:Alpha:660.0.0" -u "azureuser" -p "Pass1234!" -N "testnic"
    info:    Executing command vm create
    + Looking up the VM "testvm"
    info:    Using the VM Size "Standard_A1"
    info:    The [OS, Data] Disk or image configuration requires storage account
    + Retrieving storage accounts
    info:    Could not find any storage accounts in the region "westus", trying to create new one
    + Creating storage account "cli02f696bbfda6d83414300" in "westus"
    + Looking up the storage account cli02f696bbfda6d83414300
    + Looking up the NIC "testnic"
    + Creating VM "testvm"
    info:    vm create command OK

您应该能够通过运行下面的命令启动此虚拟机。

    azure vm start "testrg" "testvm"

然后 SSH 到它通过使用**ssh username@ipaddress**命令。 若要快速查找公用 IP 资源的 IP 地址，请使用下面的命令。

    azure network public-ip show "testrg" "testip"

管理此虚拟机可以很容易与**虚拟机**的命令。 有关详细信息，请访问[使用 Azure CLI 使用 Azure 资源管理](azure-cli-arm-commands.md)。

### 虚拟机快速创建快捷方式

新的**虚拟机快速创建**快捷方式减少大部分 VM 创建命令性方法的步骤。 这非常方便，当您想要尝试创建简单的虚拟机，或您不关心的网络配置。 它是一个交互式命令，并且需要在运行它之前找出只有 OS 映像 URN。

    azure-cli@0.8.0:/# azure vm quick-create
    info:  Executing command vm quick-create
    Resource group name: CLIRG
    Virtual machine name: myqvm
    Location name: westus
    Operating system Type [Windows, Linux]: Linux
    ImageURN (format: "publisherName:offer:skus:version"): CoreOS:CoreOS:Alpha:660.0.0
    User name: azureuser
    Password: ********
    Confirm password: ********

Azure CLI 将使用默认虚拟机大小创建虚拟机。 它还将创建存储帐户、 NIC、 虚拟网络和子网和公用 IP。 您可以将虚拟机启动后，请使用公用 IP 到 SSH。

### 使用资源组模板

#### 查找并配置一个资源组模板

1. 当使用模板，可以创建您自己的或使用从模板库中，或在[GitHub](https://github.com/azurermtemplates/azurermtemplates)上使用可用的模板。 首先，，我们将使用名为 CoreOS.CoreOSStable.0.2.40 的预览模板库中的模板。 若要列出库中可用的模板，请使用下面的命令。 因为有成千上万的可分页结果或使用**grep**或**findstr** （Windows) 上或您喜爱的字符串搜索命令来查找感兴趣的模板，可用的模板。 或者，可以使用**json —**选项并下载整个列表以 JSON 格式更容易搜索。

        azure group template list

    响应将列出的发布服务器和模板名称，并将显示类似于下面的示例 （尽管会有更多）。

        data:    Publisher               Name
        data:    ----------------------------------------------------------------------------
        data:    CoreOS                  CoreOS.CoreOSStable.0.2.40-preview
        data:    CoreOS                  CoreOS.CoreOSAlpha.0.2.39-preview
        data:    SUSE                    SUSE.SUSELinuxEnterpriseServer12.2.0.36-preview
        data:    SUSE                    SUSE.SUSELinuxEnterpriseServer11SP3PremiumImage0.2.54-preview

2. 若要查看模板的详细信息，请使用下面的命令。

        azure group template show CoreOS.CoreOSStable.0.2.40-preview

    这将返回有关模板的描述性信息。 我们正在使用的模板将创建一个 Linux 虚拟机。

3. 一旦选择了一个模板，您可以使用以下命令来进行下载。

        azure group template download CoreOS.CoreOSStable.0.2.40-preview

    下载模板允许您自定义它更好的套件您的要求。 例如，向模板中添加其他的资源。

    >[AZURE.NOTE] 如果修改的模板，使用`azure group template validate`命令以使用它来创建或修改现有资源组之前验证该模板。

4. 若要配置以供您使用的资源组模板，请在文本编辑器中打开模板文件。 注意**参数**JSON 集合顶部附近。 此文件中包含此模板创建模板所介绍的资源需要的参数的列表。 而其他指定允许值的范围或值的类型的某些参数可能有默认值。

    在使用模板时，您可以要么作为命令行参数，或者通过指定包含参数值的文件的一部分提供参数。 您还可以编写您直接在**参数**部分内的**值**字段在模板中，尽管那样会使模板紧密地绑定到特定的部署和轻松地不是是可重复使用。 无论如何，这些参数必须是以 JSON 格式，您必须提供您自己的值不具有默认值的键。

    例如，若要创建包含 CoreOS.CoreOSStable.0.2.40 预览模板参数的文件，使用下面的数据创建名为 params.json 的文件。 替换为您自己的值与此示例中使用的值。 **位置**应该指定附近，如**北美欧洲**或**美国南部中部**Azure 地区。 （本示例使用**西美国**）。

        {
          "newStorageAccountName": {
            "value": "testStorage"
          },
          "newDomainName": {
            "value": "testDomain"
          },
          "newVirtualNetworkName": {
            "value": "testVNet"
          },
          "vnetAddressSpace": {
            "value": "10.0.0.0/11"
          },
          "hostName": {
            "value": "testHost"
          },
          "userName": {
            "value": "azureUser"
          },
          "password": {
            "value": "Pass1234!"
          },
          "location": {
            "value": "West US"
          },
          "hardwareSize": {
            "value": "Medium"
          }
        }

5. 保存后的 params.json 文件，请使用下面的命令来创建基于该模板的新资源组。 `-e`参数指定在上一步中创建的 params.json 文件。 您想要使用，组名和**testDeploy**与您部署的名称替换**testRG** 。 该位置应与 params.json 模板参数文件中指定的相同。

        azure group create "testRG" "West US" -f CoreOS.CoreOSStable.0.2.40-preview.json -d "testDeploy" -e params.json

    一旦上载部署，但在部署应用于组中的资源之前，该命令将返回确定。 若要检查部署状态，请使用下面的命令。

        azure group deployment show "testRG" "testDeploy"

    **ProvisioningState**显示部署的状态。

    如果您的部署是否成功，您将看到与以下示例类似的输出。

        azure-cli@0.8.0:/# azure group deployment show testRG testDeploy
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : testDeploy
        data:    ResourceGroupName  : testRG
        data:    ProvisioningState  : Running
        data:    Timestamp          : 2015-04-27T07:49:18.5237635Z
        data:    Mode               : Incremental
        data:    Name                   Type          Value
        data:    ---------------------  ------------  ----------------
        data:    newStorageAccountName  String        testStorage
        data:    newDomainName          String        testDomain
        data:    newVirtualNetworkName  String        testVNet
        data:    vnetAddressSpace       String        10.0.0.0/11
        data:    hostName               String        testHost
        data:    userName               String        azureUser
        data:    password               SecureString  undefined
        data:    location               String        West US
        data:    hardwareSize           String        Medium
        info:    group deployment show command OK

    >[AZURE.NOTE] 如果您意识到您的配置不正确，并且需要停止长时间运行的部署，请使用下面的命令。
    >
    > `azure group deployment stop "testRG" "testDeploy"`
    >
    > 如果您不提供部署名称，将创建一个自动根据模板文件的名称。 它将返回作为输出的一部分`azure group create`命令。

6. 若要查看组，请使用下面的命令。

        azure group show "testRG"

    此命令返回组中的资源的信息。 如果您有多个组，您可以使用`azure group list`命令以检索组名称的列表，然后使用`azure group show`来查看特定组中的详细信息。

7. 此外可以使用直接从 GitHub，最新模板而不是从模板库中下载。 打开[GitHub.com](http://www.github.com)和 AzureRmTemplates 的搜索。 选择 AzureRmTemplates 存储库，然后查找任何模板，您感兴趣，例如， _101-简单的虚拟机-从-映像_。 如果您单击模板时，您将看到它包含在其他文件中的 azuredeploy.json。 这是您想要使用您的命令中使用的模板**-模板 url**选项。 以_原始_模式，打开，复制的 URL，将出现在浏览器的地址栏中。 然后可以使用此 URL 直接以创建部署，而不是从模板库，下载使用与以下示例类似的命令。

        azure group deployment create "testDeploy" -g "testResourceGroup" --template-uri https://raw/githubusercontent.com/azurermtemplates/azurermtemplates/master/101-simple-vm-from-image/azuredeploy.json

    > [AZURE.NOTE] 请务必在_原始_模式打开 json 模板。 浏览器的地址栏中显示的 URL 是不同于常规模式中显示。 要打开_raw_模式时查看右上角在 GitHub，该文件中的文件，请单击**原始**。

#### 使用资源

模板使您可以声明组范围配置中的更改，而有时您需要使用特定的资源。 你可以使用`azure resource`命令。

> [AZURE.NOTE] 当使用`azure resource`以外的其他命令`list`命令，您必须指定您正在使用的资源的 API 版本`-o`参数。 如果您不确定要使用的 API 版本，请参考模板文件和资源中查找**apiVersion**字段。

1. 若要列出组中的所有资源，请使用下面的命令。

        azure resource list "testRG"

2. 若要查看单个资源，在组中，请使用下面的命令。

        azure resource show "testRG" "testHost" Microsoft.ClassicCompute/virtualMachines -o "2014-06-01"

    请注意**Microsoft.ClassicCompute/virtualMachines**参数。 这表明在所请求的信息资源的类型。 如果您看一下前面下载的模板文件，您会注意到此相同的值用于定义资源类型的虚拟机模板中所述。

    此命令会返回与虚拟机相关的信息。

3. 查看有关资源的详细信息，时，通常使用`--json`参数，为此使输出更具可读性，因为一些值是嵌套的结构或集合。 下面的示例演示如何返回作为 JSON 文档的**显示**命令的结果。

        azure resource show "testRG" "testHost" Microsoft.ClassicCompute/virtualMachines -o "2014-06-01" --json

    >[AZURE.NOTE] 您可以通过使用 JSON 数据保存到文件&gt;字符将输出传送到文件。 例如︰
    >
    > `azure resource show "testRG" "testHost" Microsoft.ClassicCompute/virtualMachines -o "2014-06-01" --json > myfile.json`

4. 若要删除现有的资源，请使用下面的命令。

        azure resource delete "testRG" "testHost" Microsoft.ClassicCompute/virtualMachines -o "2014-06-01"

## 日志记录

若要查看记录的信息对组执行的操作，请使用`azure group log show`命令。 默认情况下，将列出上次对组执行的操作。 若要查看所有的操作，请使用可选的`--all`参数。 对于最后一次部署中，使用`--last-deployment`。 对于特定的部署，使用`--deployment`，并指定部署名称。 下面的示例返回的对照组 MyGroup 执行的所有操作的日志。

    azure group log show mygroup --all

## 下一步行动

* 有关使用 Azure 命令行界面 (Azure CLI) 的信息，请参阅[安装和配置 Azure CLI][clisetup]。
* 有关使用 Azure 资源管理器使用 Azure PowerShell 的信息，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器中](../powershell-azure-resource-manager.md)。
* 有关使用 Azure 资源管理器从 Azure 门户网站的信息，请参阅[使用资源组来管理 Azure 资源][psrm]。

[signuporg]: http://www.windowsazure.com/documentation/articles/sign-up-organization/
[adtenant]: http://technet.microsoft.com/library/jj573650#createAzureTenant
[门户网站]: https://manage.windowsazure.com/
[clisetup]: ../xplat-cli.md
[psrm]: http://go.microsoft.com/fwlink/?LinkId=394760
