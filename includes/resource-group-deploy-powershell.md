---
ms.openlocfilehash: 70493f24e81d416f4f1fbda4df9d6f6812976649
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 如何使用 PowerShell 部署

1. 登录到您的 Azure 帐户。

          Add-AzureAccount

   在让您的凭据后，该命令将返回有关您的帐户信息。

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. 如果您有多个订阅，提供您想要用于部署的订阅 id。 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. 切换到 Azure 资源管理器模块。

          Switch-AzureMode AzureResourceManager

4. 如果您没有现有资源组，创建新的资源组。 提供的资源组，您需要为您的解决方案的位置的名称。

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

   返回新的资源组的摘要。

        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. 要创建新部署的资源组，请运行**新建 AzureResourceGroupDeployment**命令并提供必需的参数。 参数将包含一个名称为您的部署、 资源组、 路径或 URL 为您创建的模板的名称和适合您的方案所需的任何其他参数。 
   
   您有下列选项提供的参数值︰ 
   
   - 使用内联参数。

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - 使用参数对象。

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - 使用参数文件。

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  在已部署的资源组中，您将看到部署的摘要。

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. 若要获取有关部署失败的信息。

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. 若要获取有关部署失败的详细的信息。

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
