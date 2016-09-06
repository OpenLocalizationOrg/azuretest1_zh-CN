---
ms.openlocfilehash: b01a01cf754f6f8b2572090fa5f4300f655d186a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure 数据工厂事件创建通知" 
    description="了解在 Azure 数据工厂操作引发事件，您就可以创建警报。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/21/2015" 
    ms.author="spelluru"/>

# 在 Azure 事件创建通知
Azure 事件提供了有用的见解在 Azure 资源发生了什么。 Azure 记录用户事件时创建、 更新或删除的 Azure 的资源 （例如数据工厂）。 当使用 Azure 数据工厂时，会生成事件时︰
 
1.  Azure 数据工厂是创建/更新/删除。
2.  数据处理 （运行时称为） 已开始/完成。
3.  当按需 HDInsight 群集创建和删除。

可以创建警报，这些用户事件上，并将其配置为向管理员和订阅的联管理员发送电子邮件通知。 此外，您还可以指定其他电子邮件地址的用户需要在满足条件时收到电子邮件通知。

## 指定警报定义
若要指定警报定义，您创建一个描述您想要得到预警上的操作的 JSON 文件。 在下面的示例中，警报将发送电子邮件通知**RunFinished**操作。 要具体，发送电子邮件通知是在完成数据工厂中的运行并且运行失败 (状态 = FailedExecution)。

    {
        "contentVersion": "1.0.0.0",
         "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters": {},
        "resources": 
        [
            {
                "name": "ADFAlertsSlice",
                "type": "microsoft.insights/alertrules",
                "apiVersion": "2014-04-01",
                "location": "East US",
                "properties": 
                {
                    "name": "ADFAlertsSlice",
                    "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                    "isEnabled": true,
                    "condition": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                        "dataSource": 
                        {
                            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                            "operationName": "RunFinished",
                            "status": "Failed",
                                "subStatus": "FailedExecution"   
                        }
                    },
                    "action": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails": [ "<your alias>@contoso.com" ]
                    }
                }
            }
        ]
    }

从上面的 JSON 定义，如果不想在特定的故障发出警报，则可以删除**子状态**。

操作和状态 （和子状态） 列表，请参阅[可用操作和状态](#AvailableOperationsStatuses)。 

## 部署该警报
若要部署此警报，请使用 Azure PowerShell cmdlet:**新建 AzureResourceGroupDeployment**，如下面的示例所示︰

    New-AzureResourceGroupDeployment -ResourceGroupName adf     -TemplateFile .\ADFAlertFailedSlice.json -StorageAccountName testmetricsabc

StorageAccountName 指定用于存储部署的警报 JSON 文件存储帐户。

已成功完成资源组部署后，您将看到以下消息︰
    
    VERBOSE: 7:00:48 PM - Template is valid.
    WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
    Please update scripts to remove this parameter.
    VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
    VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded

    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :

## 正在检索列表的 Azure 资源组部署
若要检索已部署的 Azure 资源组部署的列表，请使用 cmdlet:**获取 AzureResourceGroupDeployment**，如下面的示例所示︰
    
    Get-AzureResourceGroupDeployment -ResourceGroupName adf
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :

## <a name="AvailableOperationsStatuses"></a>可用的操作的名称和状态的值

| 操作名称 | 。 | 子状态 |
| -------------- | ------ | ---------- |
| RunStarted | 启动 | 启动 |
| RunFinished | 失败成功 |  <p>FailedResourceAllocation </p><p>成功</p><p>FailedExecution</p><p>超时</p><p>Canceled</p><p>FailedValidation</p><p>放弃</p> | 
| SliceOnTime | 正在进行中 | Ontime |
| SliceDelayed | 正在进行中 | 最晚 |
| OnDemandClusterCreateStarted | 启动 | |
| OnDemandClusterCreateSuccessful | 成功 | | 
| OnDemandClusterDeleted | 成功 | |


## 用户事件的故障排除
运行下面的命令以查看所生成的事件︰

    Get-AzureResourceGroupLog –Name $ResourceGroup -All | Where-Object EventSource -eq "Microsoft.DataFactory"
 
测试
