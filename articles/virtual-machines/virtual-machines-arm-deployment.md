---
ms.openlocfilehash: 2699d8c757807ca968b285ce478f12c44c95e5eb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="部署使用计算、 网络和存储.NET 库的 Azure 资源"
    description="了解如何使用计算、 存储和网络.NET 库中一些可用的客户端可以创建和删除 Microsoft Azure 中的资源"
    services="virtual-machines,virtual-network,storage"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2015"
    ms.author="davidmu"/>

# 部署使用计算、 网络和存储.NET 库的 Azure 资源

本教程展示如何使用一些可用的客户端计算、 存储和网络.NET 库中创建和删除 Microsoft Azure 中的资源。 它还演示如何使用 Azure Active Directory 到 Azure 资源管理器的请求进行身份验证。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

若要完成本教程，您还需要︰

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Azure 存储帐户](../storage-create-storage-account.md)
- [Windows 管理框架 3.0](http://www.microsoft.com/en-us/download/details.aspx?id=34595)或[Windows 管理框架 4.0](http://www.microsoft.com/en-us/download/details.aspx?id=40855)
- [Azure PowerShell](../install-configure-powershell.md)

大约需要 30 分钟来执行这些步骤。

## 步骤 1︰ 添加到 Azure AD 的应用程序并设置权限

若要使用 Azure AD 身份验证请求到 Azure 资源管理器，必须将应用程序添加到默认目录。 执行下列操作来添加应用程序︰

1. 打开 Azure PowerShell 提示符下，然后运行以下命令︰

        Switch-AzureMode –Name AzureResourceManager

2. 设置您要使用在本教程的 Azure 帐户。 运行此命令，您的订阅时提示您输入凭据︰

        Add-AzureAccount

3. 在以下命令中的 {密码} 替换您要使用，然后运行以创建应用程序的一个︰

        New-AzureADApplication -DisplayName "My AD Application 1" -HomePage "https://myapp1.com" -IdentifierUris "https://myapp1.com"  -Password "{password}"

4. 在上一步的响应中的 ApplicationId 值的记录。 您将需要它本教程的后面︰

    ![创建 AD 应用程序](./media/virtual-machines-arm-deployment/azureapplicationid.png)

    >[AZURE.NOTE] 您还可以管理门户中应用程序的客户端 id 字段中查找应用程序标识符。

5. {应用程序 id} 替换刚才录制的标识符，然后创建该应用程序的服务主体︰

        New-AzureADServicePrincipal -ApplicationId {application-id}

6. 设置使用应用程序的权限︰

        New-AzureRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName "https://myapp1.com"

## 步骤 2︰ 创建一个 Visual Studio 项目并安装库

NuGet 程序包将安装库，您需要完成本教程的最简便方法。 您必须安装 Azure 资源管理库、 Azure 活动目录身份验证库和计算机资源提供程序库。 若要获取这些库在 Visual Studio 中的，执行下列操作︰

1. 单击**文件** > **新** > **项目**。

2. 在**模板** > **C#**，选择**控制台应用程序**、 输入的名称和位置的项目，然后单击**确定**。

3. 用鼠标右键单击解决方案资源管理器中的项目名称，然后单击**管理 NuGet 程序包**。

4. 类型*Active Directory*中的搜索框中，为活动目录身份验证库包，单击**安装**，然后按照说明进行安装。

5. 在页面的顶部，选择**包括预发布版**。 类型*Azure 计算*中的搜索框中，为计算.NET 库中，单击**安装**，然后按照说明进行安装。

6. 类型*Azure 网络*中的搜索框中，对于网络.NET 库中，单击**安装**，然后按照说明进行安装。

7. 类型*Azure 存储*中的搜索框中，为网络.NET 库中，单击**安装**，然后按照说明进行安装。

8. 在搜索框中键入*Azure 资源管理*、 资源管理库中单击**安装**。

现在您可以开始使用这些库来创建您的应用程序。

## 步骤 3︰ 创建用于对请求进行身份验证的凭据

Azure Active Directory 应用程序创建并安装了身份验证库时，您设置的格式使用 Azure 资源管理器对请求进行身份验证的凭据的应用程序信息。 请执行以下操作︰

1.  打开您创建，项目的 Program.cs 文件，然后添加以下使用语句到文件的顶部︰

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Resources;
        using Microsoft.Azure.Management.Resources.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;


2. 将下面的方法添加到程序类，以获取创建凭据所需的标记︰

        private static string GetAuthorizationHeader()
        {
          ClientCredential cc = new ClientCredential("{application-id}", "{password}");
            var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
            var result = context.AcquireToken("https://management.azure.com/", cc);

          if (result == null)
          {
            throw new InvalidOperationException("Failed to obtain the JWT token");
          }

          string token = result.AccessToken;

          return token;
        }

    使用应用程序标识符，您早，记录 {密码} {应用程序 id} 替换选择密码的 AD 应用程序中，和 {租户 id} 租户标识符与您的订阅。 您可以通过运行 Get AzureSubscription 找到租户 id。

3.  将以下代码添加到 Main 方法中的 Program.cs 文件来创建凭据︰

        var token = GetAuthorizationHeader();
        var credential = new TokenCloudCredentials("{subscription-id}", token);

    您的订阅标识符替换 {订阅 id}。

4.  将 Program.cs 文件保存。

## 步骤 4︰ 添加代码以注册提供程序并创建资源

### 注册提供程序并创建资源组

资源总是部署向资源组中。 使用[ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx)和[ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx)类来创建到部署资源的资源组。

1.  将下面的方法添加到程序类以创建资源组︰

        public async static void CreateResourceGroup(TokenCloudCredentials credential)
        {
          Console.WriteLine("Creating the resource group...");

          using (var resourceManagementClient = new ResourceManagementClient(credential))
          {
            var rgResult = await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync("mytestrg1", new ResourceGroup { Location = "West US" });
            Console.WriteLine(rgResult.StatusCode);
            var rpResult = await resourceManagementClient.Providers.RegisterAsync("Microsoft.Storage");
            Console.WriteLine(rpResult.StatusCode);
            rpResult = await resourceManagementClient.Providers.RegisterAsync("Microsoft.Network");
            Console.WriteLine(rpResult.StatusCode);
            rpResult = await resourceManagementClient.Providers.RegisterAsync("Microsoft.Compute");
            Console.WriteLine(rpResult.StatusCode);
          }
        }

2.  将以下代码添加到刚添加的方法调用的 Main 方法︰

        CreateResourceGroup(credential);
        Console.ReadLine();

###创建存储帐户。

存储帐户需要存储的虚拟机创建虚拟硬盘文件。

1.  将下面的方法添加到要创建存储帐户的程序类︰

        public async static void CreateStorageAccount(TokenCloudCredentials credential)
        {
          Console.WriteLine("Creating the storage account...");

          using (var storageManagementClient = new StorageManagementClient(credential))
          {
            var saResult = await storageManagementClient.StorageAccounts.CreateAsync(
              "mytestrg1",
              "mytestsa1",
              new StorageAccountCreateParameters()
              { AccountType = AccountType.StandardLRS, Location = "West US" } );
            Console.WriteLine(saResult.StatusCode);
          }
        }

3.  将以下代码添加到刚添加的方法调用的 Main 方法︰

        CreateStorageAccount(credential);
        Console.ReadLine();

###创建虚拟网络

添加到虚拟网络时，虚拟机是最有效的。

1.  将下面的方法添加到程序类来创建一个子网、 公用 IP 地址和虚拟网络︰

        public async static void CreateNetwork(TokenCloudCredentials credential)
        {
          Console.WriteLine("Creating the virtual network...");
          using (var networkClient = new NetworkResourceProviderClient(credential))
          {
            var subnet = new Subnet
            {
              Name = "mytestsn1",
              AddressPrefix = "10.0.0.0/24"
            };

            var vnResult = await networkClient.VirtualNetworks.CreateOrUpdateAsync(
              "mytestrg1",
              "mytestvn1",
              new VirtualNetwork
              {
                Location = "West US",
                AddressSpace = new AddressSpace { AddressPrefixes = new List<string> { "10.0.0.0/16" } },
                Subnets = new List<Subnet> { subnet }
              });
            Console.WriteLine(vnResult.StatusCode);

            var subnetResponse = await networkClient.Subnets.GetAsync(
              "mytestrg1",
              "mytestvn1",
              "mytestsn1"
            );

            Console.WriteLine("Creating the public ip...");
            vnResult = await networkClient.PublicIpAddresses.CreateOrUpdateAsync(
              "mytestrg1",
              "mytestip1",
              new PublicIpAddress
              {
                Location = "West US",
                PublicIpAllocationMethod = "Dynamic"
              });
            Console.WriteLine(vnResult.StatusCode);

            var pubipResponse = await networkClient.PublicIpAddresses.GetAsync("mytestrg1", "mytestip1");

            Console.WriteLine("Updating the network with the nic...");
            vnResult = await networkClient.NetworkInterfaces.CreateOrUpdateAsync(
              "mytestrg1",
              "mytestnic1",
              new NetworkInterface
              {
                Name = "mytestnic1",
                Location = "West US",
                IpConfigurations = new List<NetworkInterfaceIpConfiguration>
                {
                  new NetworkInterfaceIpConfiguration
                  {
                    Name = "nicconfig1",
                    PublicIpAddress = new ResourceId
                    {
                      Id = pubipResponse.PublicIpAddress.Id
                    },
                    Subnet = new ResourceId
                    {
                      Id = subnetResponse.Subnet.Id
                    }
                  }
                }
              });
            Console.WriteLine(vnResult.StatusCode);
          }
        }

2.  将以下代码添加到刚添加的方法调用的 Main 方法︰

        CreateNetwork(credential);
        Console.ReadLine();

###创建虚拟机

现在，您将创建所有起支持作用的资源，可以创建一个虚拟机。

1.  将下面的方法添加到要创建虚拟机的程序类︰

        public async static void CreateVirtualMachine(TokenCloudCredentials credential)
        {
          using (var computeClient = new ComputeManagementClient(credential))
          {
            Console.WriteLine("Creating the availability set...");
            var avSetResponse = await computeClient.AvailabilitySets.CreateOrUpdateAsync(
              "mytestrg1",
              new AvailabilitySet
              {
                Name = "mytestav1",
                Location = "West US"
              } );
            Console.WriteLine(avSetResponse.StatusCode);

            var networkClient = new NetworkResourceProviderClient(credential);
            var nicResponse = await networkClient.NetworkInterfaces.GetAsync("mytestrg1", "mytestnic1");

            Console.WriteLine("Creating the virtual machine...");
            var putVMResponse = await computeClient.VirtualMachines.CreateOrUpdateAsync(
              "mytestrg1",
              new VirtualMachine
              {
                Name = "mytestvm1",
                Location = "West US",
                AvailabilitySetReference = new AvailabilitySetReference
                {
                  ReferenceUri = avSetResponse.AvailabilitySet.Id
                },
                HardwareProfile = new HardwareProfile
                {
                  VirtualMachineSize = "Standard_A0"
                },
                OSProfile = new OSProfile
                {
                  AdminUsername = "mytestuser1",
                  AdminPassword = "Mytestpass1",
                  ComputerName = "mytestsv1",
                  WindowsConfiguration = new WindowsConfiguration
                  {
                    ProvisionVMAgent = true
                  }
                },
                NetworkProfile = new NetworkProfile
                {
                  NetworkInterfaces = new List<NetworkInterfaceReference>
                  {
                    new NetworkInterfaceReference
                    {
                      ReferenceUri = nicResponse.NetworkInterface.Id
                    }
                  }
                },
                StorageProfile = new StorageProfile
                {
                  SourceImage = new SourceImageReference
                  {
                    ReferenceUri = "/{subscription-id}/services/images/a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201502.01-en.us-127GB.vhd"
                  },
                  OSDisk = new OSDisk
                  {
                    Name = "myosdisk1",
                    CreateOption = "FromImage",
                    VirtualHardDisk = new VirtualHardDisk
                    {
                      Uri = "http://mytestsa1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    }
                  }
                }
              } );
            Console.WriteLine(putVMResponse.StatusCode);
          }
        }

    >[AZURE.NOTE] 图像 vhd 名称会更改定期在图像库中，因此您需要获取部署虚拟机的当前图像名称。 若要执行此操作，请参阅[使用 Windows PowerShell 管理图像窗口](https://msdn.microsoft.com/library/azure/dn790330.aspx)中，和 {源映像名称} 然后替换 vhd 文件要使用的名称。 例如，"a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201411.01-en.us-127GB.vhd"。

    {订阅 id} 替换您的订阅的标识符。

2.  将以下代码添加到刚添加的方法调用的 Main 方法︰

        CreateVirtualMachine(credential);
        Console.ReadLine();

##步骤 5︰ 添加代码以删除资源

收取在 Azure 中使用的资源，因为它始终是最好删除不再需要的资源。 如果您想要删除的虚拟机和所有起支持作用的资源，所有您需要做是删除该资源组。

1.  将下面的方法添加到要删除该资源组的程序类︰

        public async static void DeleteResourceGroup(TokenCloudCredentials credential)
        {
          Console.WriteLine("Deleting resource group...");
          using (var resourceGroupClient = new ResourceManagementClient(credential))
          {
            var rgResult = await resourceGroupClient.ResourceGroups.DeleteAsync("mytestrg1");
            Console.WriteLine(rgResult.StatusCode);
          }
        }

2.  将以下代码添加到刚添加的方法调用的 Main 方法︰

        DeleteResourceGroup(credential);
        Console.ReadLine();

##第 6 步︰ 运行控制台应用程序

1.  要运行控制台应用程序，请在单击**启动**Visual Studio，然后登录到 Azure 广告使用相同的用户名和密码，使用您的订购。

2.  每个状态代码返回要创建的每个资源后，则按**enter 键**。 创建虚拟机后，按 Enter 键删除所有资源之前做下一步。

    需要大约 5 分钟，从开始完全运行此控制台应用程序来完成。 按 Enter 键删除资源之前，可能需要几分钟的时间来验证所创建的 Azure 预览门户网站中的资源之前将其删除。

3. 浏览到 Azure 预览门户中审核日志，以查看资源的状态︰

    ![创建 AD 应用程序](./media/virtual-machines-arm-deployment/crpportal.png)
