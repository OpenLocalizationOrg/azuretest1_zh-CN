---
ms.openlocfilehash: cc4489a6703fe5ca9edd86e0c23f14d6c1c03fed
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
 pageTitle="计划程序的出站身份验证" 
 description="" 
 services="scheduler" 
 documentationCenter=".NET" 
 authors="krisragh" 
 manager="dwrede" 
 editor=""/>
<tags 
 ms.service="scheduler" 
 ms.workload="infrastructure-services" 
 ms.tgt_pltfrm="na" 
 ms.devlang="dotnet" 
 ms.topic="article" 
 ms.date="08/04/2015" 
 ms.author="krisragh"/>
 
# 计划程序的出站身份验证

调度程序作业可能需要动员到要求身份验证的服务。 这样一来，被调用的服务可以确定是否调度作业可以访问它的资源。 其中一些服务包括其他 Azure 服务、 Salesforce.com、 Facebook 和安全的自定义网站。

## 添加和删除身份验证

添加到计划程序作业的身份验证很简单 – 添加一个 JSON 子元素`authentication`到`request`元素时创建或更新作业。 机密信息传递给传、 修补或 POST 请求 – 中计划服务的一部分`authentication`对象--永远不会在响应中返回。 在响应中，机密信息被设置为 null 或已表示经过验证的实体的公钥标记。

若要删除身份验证，放置或修补作业显式地设置`authentication`为空值的对象。 您不会看到在响应中的任何身份验证属性。

目前，唯一受支持的身份验证类型是`ClientCertificate`（对于使用 SSL/TLS 客户端证书），模型`Basic`模型 （用于基本身份验证），和`ActiveDirectoryOAuth`模型 （对于当前目录 OAuth 验证）。

## 提出身份验证请求正文

当添加身份验证使用`ClientCertificate`模型，则在请求正文中指定下面的其他元素。  

|元素|说明|
|:---|:---|
|_身份验证 （父元素）_|使用 SSL 客户端证书身份验证对象。|
|_类型_|必需。 身份验证类型。对于 SSL 客户端证书，值必须为`ClientCertificate`。|
|_pfx_|必需。 PFX 文件的 Base64 编码的内容。|
|_password_|必需。 要访问该 PFX 文件密码。|


## 提出身份验证响应正文

当使用身份验证信息发送一个请求时，响应将包含下列身份验证相关的元素。

|元素 |说明 |
|:--|:--|
|_身份验证 （父元素）_ |使用 SSL 客户端证书身份验证对象。|
|_类型_ |身份验证类型。 对于 SSL 客户端证书，则值是`ClientCertificate`。|
|_certificateThumbprint_ |证书的指纹。|
|_certificateSubjectName_ |证书的主题可分辨的名称。|
|_certificateExpiration_ |证书的到期日期。|

## 请求和响应提出验证示例

下面的示例请求发出 PUT 请求合并了`ClientCertificate`身份验证。 该请求是，如下所示︰


    PUT https://management.core.windows.net/7e2dffb5-45b5-475a-91be-d3d9973c82d5/cloudservices/cs-brazilsouth-scheduler/resources/scheduler/~/JobCollections/testScheduler/jobs/testScheduler 
    x-ms-version: 2013-03-01
    User-Agent: Microsoft.WindowsAzure.Scheduler.SchedulerClient/3.0.0.0 AzurePowershell/v0.8.10
    Content-Type: application/json; charset=utf-8
    Host: management.core.windows.net
    Content-Length: 4013
    Expect: 100-continue

    {
      "action": {
        "type": "http",
        "request": {
          "uri": "https://management.core.windows.net/7e2dffb5-45b5-475a-91be-d3d9973c82d5/cloudservices/CS-NorthCentralUS-scheduler/resources/scheduler/~/JobCollections/testScheduler/jobs/test",
          "method": "GET",
          "headers": {
            "x-ms-version": "2013-03-01"
          },
          "authentication": {
            "type": "clientcertificate",
            "password": "test",
            "pfx": "long-pfx-key”
          }
        }
      },
      "recurrence": {
        "frequency": "minute",
        "interval": 1
      }
    }

一旦发送此请求，响应如下所示︰

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Pragma: no-cache
    Content-Length: 721
    Content-Type: application/json; charset=utf-8
    Expires: -1
    Server: 1.0.6198.153 (rd_rdfe_stable.141027-2149) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
     

    {
      "id": "testScheduler",
      "action": {
        "request": {
          "uri": "https:\/\/management.core.windows.net\/7e2dffb5-45b5-475a-91be-d3d9973c82d5\/cloudservices\/CS-NorthCentralUS-scheduler\/resources\/scheduler\/~\/JobCollections\/testScheduler\/jobs\/test",
          "method": "GET",
          "headers": {
            "x-ms-version": "2013-03-01"
          },
          "authentication": {
            "type": "ClientCertificate",
            "certificateThumbprint": "C1645E2AF6317D9FCF9C78FE23F9DE0DAFAD2AB5",
            "certificateExpiration": "2021-01-01T08:00:00Z",
            "certificateSubjectName": "CN=Scheduler Management"
          }
        },
        "type": "http"
      },
      "recurrence": {
        "frequency": "minute",
        "interval": 1
      },
      "state": "enabled",
      "status": {
        "nextExecutionTime": "2014-10-29T21:52:35.2108904Z",
        "executionCount": 0,
        "failureCount": 0,
        "faultedCount": 0
      }
    }
## 对于基本身份验证的请求正文

当添加身份验证使用`Basic`模型，则在请求正文中指定下面的其他元素。

|元素|说明|
|:--|:--|
|_身份验证 （父元素）_ |使用基本身份验证的身份验证对象。|
|_类型_ |必需。 身份验证类型。 该值必须是用于基本身份验证， `Basic`。|
|_用户名_ |必需。 进行身份验证的用户名。|
|_password_ |必需。 进行身份验证的密码。|

## 对于基本身份验证的响应正文

当使用身份验证信息发送一个请求时，响应将包含下列身份验证相关的元素。

|元素|说明|
|:--|:--|
|_身份验证 （父元素）_ |使用基本身份验证的身份验证对象。|
|_类型_ |身份验证类型。 值是用于基本身份验证， `Basic`。|
|_用户名_ |经过身份验证的用户名。|

## 请求和响应对于基本身份验证示例

下面的示例请求发出 PUT 请求合并了`Basic`身份验证。 该请求是，如下所示︰

    PUT https://management.core.windows.net/7e2dffb5-45b5-475a-91be-d3d9973c82d5/cloudservices/cs-brazilsouth-scheduler/resources/scheduler/~/JobCollections/testScheduler/jobs/testScheduler 
    x-ms-version: 2013-03-01
    User-Agent: Microsoft.WindowsAzure.Scheduler.SchedulerClient/3.0.0.0 AzurePowershell/v0.8.10
    Content-Type: application/json; charset=utf-8
    Host: management.core.windows.net
    Expect: 100-continue

    {
      "action": {
        "type": "http",
        "request": {
          "uri": "https://management.core.windows.net/7e2dffb5-45b5-475a-91be-d3d9973c82d5/cloudservices/CS-NorthCentralUS-scheduler/resources/scheduler/~/JobCollections/testScheduler/jobs/test",
          "method": "GET",
          "headers": {
            "x-ms-version": "2013-03-01"
          },
        "authentication":{  
          "username":"user1",
          "password":"password",
          "type":"basic"
          }           
        }
      },
      "recurrence": {
        "frequency": "minute",
        "interval": 1
      }
    }

一旦发送此请求，响应如下所示︰

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Pragma: no-cache
    Content-Length: 721
    Content-Type: application/json; charset=utf-8
    Expires: -1
    Server: 1.0.6198.153 (rd_rdfe_stable.141027-2149) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET

    {
      "id": "testScheduler",
      "action": {
        "request": {
          "uri": "https:\/\/management.core.windows.net\/7e2dffb5-45b5-475a-91be-d3d9973c82d5\/cloudservices\/CS-NorthCentralUS-scheduler\/resources\/scheduler\/~\/JobCollections\/testScheduler\/jobs\/test",
          "method": "GET",
          "headers": {
            "x-ms-version": "2013-03-01"
          },
          "authentication":{  
            "username":"user1",
            "type":"Basic"
          }
        },
        "type": "http"
      },
      "recurrence": {
        "frequency": "minute",
        "interval": 1
      },
      "state": "enabled",
      "status": {
        "nextExecutionTime": "2014-10-29T21:52:35.2108904Z",
        "executionCount": 0,
        "failureCount": 0,
        "faultedCount": 0
      }
    }

## 对于 ActiveDirectoryOAuth 身份验证请求正文

当添加身份验证使用`ActiveDirectoryOAuth`模型，则在请求正文中指定下面的其他元素。

|元素 |说明 |
|:--|:--|
|_身份验证 （父元素）_ |使用 ActiveDirectoryOAuth 身份验证身份验证对象。|
|_类型_ |必需。 身份验证类型。 ActiveDirectoryOAuth 验证的值必须为`ActiveDirectoryOAuth`。|
|_租户_ |必需。 租户标识符是用于标识广告租户 ID。|
|_访问群体_ |必需。 此选项设置为 https://management.core.windows.net/。|
|_客户机 Id_ |必需。 Azure AD 应用程序提供的客户端标识符。|
|_秘密_ |必需。 客户端请求令牌的机密。|

## 对于 ActiveDirectoryOAuth 身份验证响应正文

当使用身份验证信息发送一个请求时，响应将包含下列身份验证相关的元素。

|元素 |说明 |
|:--|:--|
|_身份验证 （父元素）_ |使用 ActiveDirectoryOAuth 身份验证身份验证对象。|
|_类型_ |身份验证类型。 对于 ActiveDirectoryOAuth 身份验证，则值是`ActiveDirectoryOAuth`。|
|_租户_ |用来标识广告租户的租户标识符。|
|_访问群体_ |此选项设置为 https://management.core.windows.net/。|
|_客户机 Id_ |Azure AD 应用程序的客户端标识符。|

## 请求和响应进行 ActiveDirectoryOAuth 身份验证示例

下面的示例请求发出 PUT 请求合并了`ActiveDirectoryOAuth`身份验证。 该请求是，如下所示︰

    PUT https://management.core.windows.net/7e2dffb5-45b5-475a-91be-d3d9973c82d5/cloudservices/cs-brazilsouth-scheduler/resources/scheduler/~/JobCollections/testScheduler/jobs/testScheduler 
    x-ms-version: 2013-03-01
    User-Agent: Microsoft.WindowsAzure.Scheduler.SchedulerClient/3.0.0.0 AzurePowershell/v0.8.10
    Content-Type: application/json; charset=utf-8
    Host: management.core.windows.net
    Expect: 100-continue

    {
      "action": {
        "type": "http",
        "request": {
          "uri": "https://management.core.windows.net/7e2dffb5-45b5-475a-91be-d3d9973c82d5/cloudservices/CS-NorthCentralUS-scheduler/resources/scheduler/~/JobCollections/testScheduler/jobs/test",
          "method": "GET",
          "headers": {
            "x-ms-version": "2013-03-01"
          },
          "authentication":{  
            "tenant":"contoso.com",
            "audience":"https://management.core.windows.net/",
            "clientId":"8a14db88-4d1a-46c7-8429-20323727dfab",
            "secret": "&lt;secret-key&gt;",
            "type":"ActiveDirectoryOAuth"
          }                      
        }
      },
      "recurrence": {
        "frequency": "minute",
        "interval": 1
      }
    }

一旦发送此请求，响应如下所示︰

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Pragma: no-cache
    Content-Length: 721
    Content-Type: application/json; charset=utf-8
    Expires: -1
    Server: 1.0.6198.153 (rd_rdfe_stable.141027-2149) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET


    {
      "id": "testScheduler",
      "action": {
        "request": {
          "uri": "https:\/\/management.core.windows.net\/7e2dffb5-45b5-475a-91be-d3d9973c82d5\/cloudservices\/CS-NorthCentralUS-scheduler\/resources\/scheduler\/~\/JobCollections\/testScheduler\/jobs\/test",
          "method": "GET",
          "headers": {
            "x-ms-version": "2013-03-01"
          },
          "authentication":{  
            "tenant":"contoso.com",
            "audience":"https://management.core.windows.net/",
            "clientId":"8a14db88-4d1a-46c7-8429-20323727dfab",
            "type":"ActiveDirectoryOAuth"
          }
        },
        "type": "http"
      },
      "recurrence": {
        "frequency": "minute",
        "interval": 1
      },
      "state": "enabled",
      "status": {
        "nextExecutionTime": "2014-10-29T21:52:35.2108904Z",
        "executionCount": 0,
        "failureCount": 0,
        "faultedCount": 0
      }
    }

## 请参见
 
 [计划是什么？](scheduler-intro.md)
 
 [计划程序的概念、 术语和实体层次结构](scheduler-concepts-terms.md)
 
 [开始管理门户中使用计划程序](scheduler-get-started-portal.md)
 
 [计划和 Azure 的计划程序中的计费](scheduler-plans-billing.md)
 
 [如何构建复杂的调度和高级的定期使用 Azure 的调度程序](scheduler-advanced-complexity.md)
 
 [计划程序 REST API 参考](https://msdn.microsoft.com/library/dn528946)   
 
 [计划程序 PowerShell Cmdlet 引用](scheduler-powershell-reference.md)
 
 [计划程序的高可用性和可靠性](scheduler-high-availability-reliability.md)
 
 [计划程序限制、 默认值和错误代码](scheduler-limits-defaults-errors.md)
 
  
