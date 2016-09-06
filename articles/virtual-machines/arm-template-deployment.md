---
ms.openlocfilehash: 9a50032a981c57397d8f9b0b9ebca74267cc37b2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="通过使用模板部署 Azure 的资源"
    description="了解如何使用 Azure 资源管理库中一些可用的客户端部署的虚拟机、 虚拟网络和存储帐户"
    services="virtual-machines,virtual-networks,storage"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2015"
    ms.author="davidmu"/>

# 部署使用.NET 库和模板的 Azure 资源

通过使用资源组和模板，您就能够管理所有的资源，同时支持您的应用程序。 本教程展示了如何使用 Azure 资源管理库中的某些可用的客户端以及如何生成要部署虚拟机、 虚拟网络和存储帐户的模板。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

若要完成本教程，您还需要︰

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Azure 存储帐户](../storage-create-storage-account.md)
- [Windows 管理框架 3.0](http://www.microsoft.com/en-us/download/details.aspx?id=34595)或[Windows 管理框架 4.0](http://www.microsoft.com/en-us/download/details.aspx?id=40855)
- [Azure PowerShell](../powershell-install-configure.md)

大约需要 30 分钟来执行这些步骤。

## 步骤 1︰ 添加到 Azure AD 的应用程序并设置权限

若要使用 Azure AD 身份验证请求到 Azure 资源管理器，必须将应用程序添加到默认目录。 执行下列操作来添加应用程序︰

1. 打开 Azure PowerShell 命令提示符下，然后运行以下命令︰

        Switch-AzureMode –Name AzureResourceManager

2. 设置您要使用在本教程的 Azure 帐户。 运行此命令，您的订阅时提示您输入凭据︰

        Add-AzureAccount

3. 在以下命令中的 {密码} 替换您要使用，然后运行以创建应用程序的一个︰

        New-AzureADApplication -DisplayName "My AD Application 1" -HomePage "https://myapp1.com" -IdentifierUris "https://myapp1.com"  -Password "{password}"

4. 在上一步的响应中记录 ApplicationId 值的值。 您将需要它本教程的后面︰

    ![创建 AD 应用程序](./media/arm-template-deployment/azureapplicationid.png)

    >[AZURE.NOTE] 您还可以管理门户中应用程序的客户端 id 字段中查找应用程序标识符。

5. {应用程序 id} 替换刚才录制的标识符，然后创建该应用程序的服务主体︰

        New-AzureADServicePrincipal -ApplicationId {application-id}

6. 设置使用应用程序的权限︰

        New-AzureRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName "https://myapp1.com"

## 步骤 2︰ 创建 Visual Studio 项目模板文件、 参数文件

###创建模板文件

Azure 资源管理器模板使您可以部署和管理 Azure 的资源一起使用 JSON 资源和关联的部署参数的说明。 在 Visual Studio 中，执行下列操作︰

1. 单击**文件** > **新** > **项目**。

2. 在**模板** > **C#**，选择**控制台应用程序**、 输入的名称和位置的项目，然后单击**确定**。

3. 用鼠标右键单击解决方案资源管理器中的项目名称，然后单击**添加** > **新项**。

4.  在添加新项窗口中，选择**文本文件**，名称，请输入*VirtualMachineTemplate.json* ，然后单击**添加**。

5.  打开 VirtualMachineTemplate.json 文件，然后添加左和右方括号、 所需的架构元素和必需的 contentVersion 元素︰

        {
            "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
            "contentVersion": "1.0.0.0",
        }

6. [参数](../resource-group-authoring-templates.md#parameters)并不总是必需的但它们使模板管理更容易。 他们描述的类型的值，默认值，如果需要甚至允许该参数的值。 对于本教程，用于创建一个虚拟机、 存储帐户和虚拟网络的参数添加到模板中。

    ContentVersion 元素的后面添加参数元素和子元素︰

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "location": { "type": "String", "defaultValue" : "West US", "allowedValues": [ "West US", "East US" ] },
            "newStorageAccountName": { "type": "string" },
            "storageAccountType": { "type": "string", "defaultValue": "Standard_LRS", "allowedValues": [ "Standard_LRS", "Standard_GRS" ] },
            "publicIPAddressName": { "type": "string" },
            "publicIPAddressType" : { "type" : "string", "defaultValue" : "Dynamic", "allowedValues" : [ "Dynamic" ] },
            "vmStorageAccountContainerName": { "type": "string", "defaultValue": "vhds" },
            "vmName": { "type": "string" },
            "vmSize": { "type": "string", "defaultValue": "Standard_A0", "allowedValues" : [ "Standard_A0", "Standard_A1" ] },
            "vmSourceImageName": { "type": "string" },
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" },
            "virtualNetworkName":{ "type" : "string" },
            "addressPrefix": { "type" : "string", "defaultValue" : "10.0.0.0/16" },
            "subnet1Name": { "type" : "string", "defaultValue" : "Subnet-1" },
            "subnet2Name": { "type" : "string", "defaultValue" : "Subnet-2" },
            "subnet1Prefix" : { "type" : "string", "defaultValue" : "10.0.0.0/24" },
            "subnet2Prefix" : { "type" : "string", "defaultValue" : "10.0.1.0/24" },
            "dnsName" : { "type" : "string" },
            "subscriptionId": { "type" : "string" },
            "nicName": { "type" : "string" }
          },
        }

7.  可以在模板中使用[变量](../resource-group-authoring-templates.md#variables)，以指定的值可能会经常更改或需要创建的参数值组合的值。

    在参数部分后添加变量元素︰

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "location": { "type": "String", "defaultValue" : "West US", "allowedValues": [ "West US", "East US" ] },
            "newStorageAccountName": { "type": "string" },
            "storageAccountType": { "type": "string", "defaultValue": "Standard_LRS", "allowedValues": [ "Standard_LRS", "Standard_GRS" ] },
            "publicIPAddressName": { "type": "string" },
            "publicIPAddressType" : { "type" : "string", "defaultValue" : "Dynamic", "allowedValues" : [ "Dynamic" ] },
            "vmStorageAccountContainerName": { "type": "string", "defaultValue": "vhds" },
            "vmName": { "type": "string" },
            "vmSize": { "type": "string", "defaultValue": "Standard_A0", "allowedValues" : [ "Standard_A0", "Standard_A1" ] },
            "vmSourceImageName": { "type": "string" },
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" },
            "virtualNetworkName":{ "type" : "string" },
            "addressPrefix": { "type" : "string", "defaultValue" : "10.0.0.0/16" },
            "subnet1Name": { "type" : "string", "defaultValue" : "Subnet-1" },
            "subnet2Name": { "type" : "string", "defaultValue" : "Subnet-2" },
            "subnet1Prefix" : { "type" : "string", "defaultValue" : "10.0.0.0/24" },
            "subnet2Prefix" : { "type" : "string", "defaultValue" : "10.0.1.0/24" },
            "dnsName" : { "type" : "string" },
            "subscriptionId": { "type" : "string" },
            "nicName": { "type" : "string" }
          },
          "variables": {
            "sourceImageName" : "[concat('/',parameters('subscriptionId'),'/services/images/',parameters('vmSourceImageName'))]",
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
            "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',parameters('subnet1Name'))]"
          },
        }

8.  接下来模板中定义[的资源](../resource-group-authoring-templates.md#resources)，如虚拟机、 虚拟的网络，以及存储帐户。

    后面的变量部分中添加资源部分︰

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "location": { "type": "String", "defaultValue" : "West US", "allowedValues": [ "West US", "East US" ] },
            "newStorageAccountName": { "type": "string" },
            "storageAccountType": { "type": "string", "defaultValue": "Standard_LRS", "allowedValues": [ "Standard_LRS", "Standard_GRS" ] },
            "publicIPAddressName": { "type": "string" },
            "publicIPAddressType" : { "type" : "string", "defaultValue" : "Dynamic", "allowedValues" : [ "Dynamic" ] },
            "vmStorageAccountContainerName": { "type": "string", "defaultValue": "vhds" },
            "vmName": { "type": "string" },
            "vmSize": { "type": "string", "defaultValue": "Standard_A0", "allowedValues" : [ "Standard_A0", "Standard_A1" ] },
            "vmSourceImageName": { "type": "string" },
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" },
            "virtualNetworkName":{ "type" : "string" },
            "addressPrefix": { "type" : "string", "defaultValue" : "10.0.0.0/16" },
            "subnet1Name": { "type" : "string", "defaultValue" : "Subnet-1" },
            "subnet2Name": { "type" : "string", "defaultValue" : "Subnet-2" },
            "subnet1Prefix" : { "type" : "string", "defaultValue" : "10.0.0.0/24" },
            "subnet2Prefix" : { "type" : "string", "defaultValue" : "10.0.1.0/24" },
            "dnsName" : { "type" : "string" },
            "subscriptionId": { "type" : "string" },
            "nicName": { "type" : "string" }
          },
          "variables": {
            "sourceImageName" : "[concat('/',parameters('subscriptionId'),'/services/images/',parameters('vmSourceImageName'))]",
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
            "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',parameters('subnet1Name'))]"
          },
          "resources": [ {
            "apiVersion": "2014-12-01-preview",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('newStorageAccountName')]",
            "location": "[parameters('location')]",
            "properties": { "accountType": "[parameters('storageAccountType')]" }
          },
          {
            "apiVersion": "2014-12-01-preview",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
              "publicIPAllocationMethod": "[parameters('publicIPAddressType')]",
              "dnsSettings": { "domainNameLabel": "[parameters('dnsName')]" }
            }
          },
          {
            "apiVersion": "2014-12-01-preview",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetworkName')]",
            "location": "[parameters('location')]",
            "properties": {
              "addressSpace": { "addressPrefixes": [ "[parameters('addressPrefix')]" ] },
              "subnets": [ {
                "name": "[parameters('subnet1Name')]",
                "properties": { "addressPrefix": "[parameters('subnet1Prefix')]" }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties": { "addressPrefix": "[parameters('subnet2Prefix')]" }
              } ]
            }
          },
          {
            "apiVersion": "2014-12-01-preview",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[parameters('nicName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
              "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
              "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
            ],
            "properties": {
              "ipConfigurations": [ {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddressName'))]"
                  },
                  "subnet": { "id": "[variables('subnet1Ref')]" }
                }
              } ]
            }
          },
          {
            "apiVersion": "2014-12-01-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[parameters('vmName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
              "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
              "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
            ],
            "properties": {
              "hardwareProfile": { "vmSize": "[parameters('vmSize')]" },
              "osProfile": {
                "computername": "[parameters('vmName')]",
                "adminUsername": "[parameters('adminUsername')]",
                "adminPassword": "[parameters('adminPassword')]",
                "windowsProfile": { "provisionVMAgent": "true" }
              },
              "storageProfile": {
                "sourceImage": { "id": "[variables('sourceImageName')]" },
                "destinationVhdsContainer" : "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',parameters('vmStorageAccountContainerName'),'/')]"
              },
              "networkProfile": {
                "networkInterfaces" : [ {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                } ]
              }
            }
          } ]
        }

9.  将保存您创建的模板文件。

###创建参数文件

若要指定在模板中定义的资源参数的值，则创建参数文件包含的值，并将其提交到资源管理器使用该模板。 在 Visual Studio 中，执行下列操作︰

1.  用鼠标右键单击解决方案资源管理器中的项目名称，然后单击**添加**，然后选择**新建项目**。

2.  在添加新项窗口中选择**文本文件**， *Parameters.json*输入名称，然后单击**添加**。

3.  打开 parameters.json 文件，然后添加下面的 JSON 内容︰

        {
          "contentVersion": "1.0.0.0",
          "parameters": {
            "vmName": { "value": "mytestvm1" },
            "newStorageAccountName": { "value": "mytestsa1" },
            "storageAccountType": { "value": "Standard_LRS" },
            "publicIPaddressName": { "value": "mytestip1" },
            "location": { "value": "West US" },
            "vmStorageAccountContainerName": { "value": "vhds" },
            "vmSize": { "value": "Standard_A1" },
            "subscriptionId": { "value": "{subscription-id}" },
            "vmSourceImageName": { "value": "{source-image-name}" },
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" },
            "virtualNetworkName": { "value": "mytestvn1" },
            "dnsName": { "value": "mytestdns1" },
            "nicName": { "value": "mytestnic1" }
          }
        }

    >[AZURE.NOTE] 图像 vhd 名称会更改定期在图像库中，因此您需要获取部署虚拟机的当前图像名称。 若要执行此操作，请参阅[使用 Windows PowerShell 管理图像窗口](https://msdn.microsoft.com/library/azure/dn790330.aspx)中，和 {源映像名称} 然后替换 vhd 文件要使用的名称。 例如，"a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201412.01-en.us-127GB.vhd"。 {订阅 id} 替换您的订阅的标识符。


4.  将保存您创建的参数文件。

模板文件和参数文件从 Azure 存储帐户访问通过 Azure 资源管理器中。 若要将文件放置在存储区中，执行下列操作︰

1.  打开服务器资源管理器，然后定位到您想要放置该文件的存储帐户中的容器。 对于本教程，模板的容器被命名模板。

2.  在模板容器窗格的右上角，单击上载 Blob 图标，浏览到 VirtualMachineTemplate.json 您创建文件，并单击**打开**。

3. 再次单击上载 Blob 图标，浏览到 Parameters.json 您创建文件，并单击**打开**。

##步骤 3︰ 安装库

NuGet 程序包将安装库，您需要完成本教程的最简便方法。 您必须安装 Azure 资源管理库和 Azure 活动目录身份验证库。 若要获取这些库在 Visual Studio 中的，执行下列操作︰

1.  用鼠标右键单击解决方案资源管理器中的项目名称，然后单击**管理 NuGet 程序包**。

2.  类型*Active Directory*中的搜索框中，为活动目录身份验证库包，单击**安装**，然后按照说明进行安装。

3.  在页面的顶部，选择**包括预发布版**。 类型*Azure 资源管理*中的搜索框中，对于 Microsoft Azure 的资源管理库中，单击**安装**，然后按照说明进行安装。

现在您可以开始使用这些库来创建您的应用程序。

##步骤 4︰ 创建用于对请求进行身份验证的凭据

既然 Azure Active Directory 应用程序被创建并已安装身份验证库，您设置格式用于请求到 Azure 资源管理器进行身份验证的凭据的应用程序信息。 请执行以下操作︰

1.  打开您创建，项目的 Program.cs 文件，然后添加以下使用语句到文件的顶部︰

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Resources;
        using Microsoft.Azure.Management.Resources.Models;

2.  将下面的方法添加到程序类，以获取创建凭据所需的标记︰

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

4.  将 Program.cs 文件保存。


##步骤 5︰ 添加代码以将部署模板

始终部署资源从模板给资源组。 使用[ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx)和[ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx)类来创建到部署资源的资源组。

1.  将下面的方法添加到程序类以创建资源组︰

        public async static void CreateResourceGroup(TokenCloudCredentials credential)
        {
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = "West US" };
          using (var resourceManagementClient = new ResourceManagementClient(credential))
          {
            var rgResult = await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync("mytestrg1", resourceGroup);
            Console.WriteLine(rgResult.StatusCode);
          }
        }

2.  将以下代码添加到刚添加的方法调用的 Main 方法︰

        CreateResourceGroup(credential);
        Console.ReadLine();

3.  将下面的方法添加到要使用您定义的模板部署到该资源组的资源的程序类︰

        public async static void CreateTemplateDeployment(TokenCloudCredentials credential)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = new Uri("https://{storage-account-name}.blob.core.windows.net/templates/VirtualMachineTemplate.json")
            },
            ParametersLink = new ParametersLink
            {
              Uri = new Uri("https://{storage-account-name}.blob.core.windows.net/templates/Parameters.json")
            }
          };
          using (var templateDeploymentClient = new ResourceManagementClient(credential))
          {
            var dpResult = await templateDeploymentClient.Deployments.CreateOrUpdateAsync("mytestrg1", "mytestdp1", deployment);
            Console.WriteLine(dpResult.StatusCode);
          }
        }

    {存储帐户名称} 替换以前放置文件的帐户的名称。

4.  将以下代码添加到刚添加的方法调用的 Main 方法︰

        CreateTemplateDeployment(credential);
        Console.ReadLine();

##第 6 步︰ 添加代码以删除资源

收取在 Azure 中使用的资源，因为它始终是最好删除不再需要的资源。 您不需要分别删除每个资源的资源组。 您可以删除该资源组，并将自动删除其所有资源。

1.  将下面的方法添加到要删除该资源组的程序类︰

        public async static void DeleteResourceGroup(TokenCloudCredentials credential)
        {
          using (var resourceGroupClient = new ResourceManagementClient(credential))
          {
            var rgResult = await resourceGroupClient.ResourceGroups.DeleteAsync("mytestrg1");
            Console.WriteLine(rgResult.StatusCode);
          }
        }

2.  将以下代码添加到刚添加的方法调用的 Main 方法︰

        DeleteResourceGroup(credential);
        Console.ReadLine();

##第 7 步︰ 运行控制台应用程序

1.  要运行控制台应用程序，请在单击**启动**Visual Studio，然后登录到 Azure 广告使用相同的用户名和密码，使用您的订购。

2.  每个状态代码返回要创建的每个资源后，则按**enter 键**。 创建虚拟机后，按 Enter 键删除所有资源之前做下一步。

    需要大约 5 分钟，从开始完全运行此控制台应用程序来完成。 按 Enter 键删除资源之前，可能需要几分钟的时间来验证所创建的 Azure 预览门户网站中的资源之前将其删除。

3. 浏览到 Azure 预览门户中审核日志，以查看资源的状态︰

    ![创建 AD 应用程序](./media/arm-template-deployment/crpportal.png)
