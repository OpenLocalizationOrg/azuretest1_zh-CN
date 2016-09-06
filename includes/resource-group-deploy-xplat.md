---
ms.openlocfilehash: 02e58aaae9d0bf49db0416513610adeb7d4c5ca0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 如何使用 Azure CLI 部署

1. 登录到您的 Azure 帐户。

        azure login

  在让您的凭据后，该命令返回您的登录名的结果。

        ...
        info:    login command OK

2. 如果您有多个订阅，提供您想要用于部署的订阅 id。

        azure account set <YourSubscriptionNameOrId>

3. 切换到 Azure 资源管理器模块

        azure config mode arm

   您将收到确认的新的模式。

        info:     New mode is arm

4. 如果您没有现有资源组，创建新的资源组。 提供的资源组，您需要为您的解决方案的位置的名称。

        azure group create -n ExampleResourceGroup -l "West US"

   返回新的资源组的摘要。

        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. 要创建新部署的资源组，请运行下面的命令并提供必要的参数。 参数将包含一个名称为您的部署、 资源组、 路径或 URL 为您创建的模板的名称和适合您的方案所需的任何其他参数。

   您有下列选项提供的参数值︰

   - 使用内联参数和本地模板。

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - 使用内联参数和模板的链接。

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - 使用参数文件。

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  在已部署的资源组中，您将看到部署的摘要。

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. 若要获取有关最新部署的信息。

         azure group log show -l ExampleResourceGroup

7. 若要获取有关部署失败的详细的信息。

         azure group log show -l -v ExampleResourceGroup
