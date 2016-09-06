---
ms.openlocfilehash: 5c3c91a48d2deb5bcfde66e9b387d8c6b30fd138
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="操作的见解日志搜索 API"
   description="提供操作经验搜索 API 的概述，包括示例，向您展示如何使用它。"
   services="operational-insights"
   documentationCenter=""
   authors="bandersmsft"
   manager="jwhit"
   editor="" />
<tags
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="07/21/2015"
   ms.author="banders" />



# 操作的见解搜索 API 概述和参考

本指南提供了基本的教程来介绍如何您可以使用该操作的见解日志搜索 API 并提供示例，向您展示如何使用这些命令。

## 日志搜索 API 的概述

操作的见解日志搜索 API 是 RESTful 和可通过 Azure 资源管理器 API 来访问。 在本文中您将找到示例 API 的访问[ARMClient](https://github.com/projectkudu/ARMClient)，开源命令行工具，可以简化调用 Azure 资源管理器 API。 使用 ARMClient 和 PowerShell 是访问操作的见解日志搜索 API 的许多选项之一。 使用这些工具，您可以利用 rest 风格的 Azure 资源管理器 API 调用的操作的见解工作区并执行其中的搜索命令。 API 将输出搜索结果仅限于您以 JSON 格式允许您以编程方式在许多不同的方式使用搜索结果。

通过[.net 库](https://msdn.microsoft.com/library/azure/dn910477.aspx)，以及一个[REST API，](https://msdn.microsoft.com/library/azure/mt163658.aspx)可以使用 Azure 资源管理器。 查看喜欢的 web 页，以了解详细信息。

## 基本搜索 API 教程

### 若要使用 ARM 的客户端

1. 安装[Chocolatey](https://chocolatey.org/)，它是为 Windows 打开源计算机程序包管理器。
2. 打开管理员 PowerShell 窗口，然后运行以下命令︰

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

3. 安装 ARMClient 通过打开新的命令提示符处，运行以下命令︰

    ```
    choco install armclient
    ```

### 要执行简单搜索使用 ARMClient

1. 登录到您的 Microsoft 或 OrgID 帐户︰

    ```
    armclient login
```
  成功登录列表绑定到给定帐户的所有订阅。 例如︰

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. 获取操作管理套件工作区。 例如︰

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2014-10-10
    ```

    成功的 Get 调用会输出与订阅相关的所有工作区。 例如︰

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. 创建您的搜索变量。 例如︰

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}”;
    ```
4. 使用新的搜索变量进行搜索。 例如︰

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2014-10-10 $mySearch
    ```

## 搜索 API 引用示例
下面的示例演示如何使用搜索 API。

### 搜索-操作/读取

**示例 Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2014-10-10
```

**请求︰**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2014-10-10 $searchParametersJson
```
下表描述了可用的属性。

|**属性**|**说明**|
|---|---|
|top|要返回的结果的最大数目。|
|突出显示|Pre 和 post 参数，通常用于突出显示匹配的字段包含|
|前|给定的字符串匹配的字段的前缀。|
|发布|将给定的字符串追加到匹配的字段。|
|查询|用于收集并返回结果的搜索查询。|
|start|要找的结果的时间窗口的开始处。|
|结束|结束的时间窗口您想要找的结果。|


**响应︰**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### 搜索 / {ID}-操作/读取

**请求内容的已保存的搜索︰**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2014-10-10
```

>[AZURE.NOTE] 如果搜索结果返回的挂起状态，则可以通过该 API 完成然后轮询更新后的结果。 6 分钟之后, 将从缓存中删除搜索的结果，将返回 Http 消失。 如果初始搜索请求立即返回成功状态，它将不会添加到导致此 API 返回 Http 消失如果查询缓存。 在初始的搜索请求只是使用更新后的值与相同的格式将 Http 200 结果的内容。

### 已保存的搜索的其余部分只

**请求已保存的搜索的列表︰**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2014-10-10
```

支持的方法︰ 获取将删除

支持收集方法︰ 获取

下表描述了可用的属性。

|属性|说明|
|---|---|
|Id|唯一的标识符。|
|Etag|**所需的修补程序**。 在每次写入更新服务器。 该值必须等于当前存储的值或 * 来更新。 对于每个无效的旧值返回 409。|
|properties.query|被**必需**。 搜索查询中。|
|properties.displayName|被**必需**。 查询的用户定义的显示名称。 如果作为 Azure 的资源建模，这是一个标记。|
|properties.category|被**必需**。 用户定义的类别的查询。 如果建模为 Azure 的资源将为某个标记。|

>[AZURE.NOTE]操作的见解搜索 API 目前返回用户创建已保存的搜索时轮询的工作区中保存的搜索条件。 API 将不会返回以前保存的搜索，这次提供的解决方案。 此功能将在以后添加。

### 删除已保存的搜索

**请求︰**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2014-10-10
```

### 更新保存的搜索

 **请求︰**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2014-10-10 $savedSearchParametersJson
```

### 元数据的 JSON 只

下面是查看收集的数据在您的工作区中的所有日志类型的字段的方法。 例如，如果您想知道是否事件类型有一个名为计算机，则查找和确认的一种方法。

**请求的域︰**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2014-10-10
```

**响应︰**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

下表描述了可用的属性。

|**属性**|**说明**|
|---|---|
|name|字段名称。|
|显示名称|该字段的显示名称。|
|类型|字段值的类型。|
|facetable|结合当前索引的存储和小平面属性。|
|显示|当前的显示属性。 如果字段中搜索可见，则为 true。|
|所有者类型|减少到只有属于 onboarded IP 类型。|


## 可选参数
以下信息描述了可用的可选参数。

### 突出显示

"突出显示"参数是可选参数可用于请求搜索子系统在其响应中包含一套标记。

这些标记指示开始和结束突出显示匹配项时搜索查询中提供的文本。
您可以指定搜索将使用自动换行的突出显示的术语的开始和结束标记。

**示例搜索查询**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2014-10-10 $searchParametersJson
```

**结果示例︰**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

请注意，上面的结果包含前缀和追加一条错误消息。
