---
ms.openlocfilehash: 513996271f9f669814f8b9ad2cd5c31c01a93dee
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="要开始使用 Azure AD 报告 API"
   description="如何开始使用 Azure 活动目录报告 API"
   services="active-directory"
   documentationCenter=""
   authors="kenhoff"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/17/2015"
   ms.author="kenhoff;yossib"/>


# 要开始使用 Azure AD 报告 API

Azure 的 Active Directory 提供了多种活动、 安全和审计报告。 这些数据可以通过 Azure 的门户，消耗，但也可以在许多其他应用程序，如 SIEM 系统、 审核及商业智能工具中非常有用。

Azure AD 报告 Api 提供编程访问这些数据，通过基于 REST 的 api，可以从各种不同调用一组编程语言和工具。

这篇文章将引导您通过调用 Azure 的 AD 报告 Api 使用 PowerShell 的过程。 根据您的情况的需要，可以修改访问的数据从任何 JSON、 XML 或文本格式中的可用报表的示例 PowerShell 脚本。

若要使用此示例，您需要一个[Azure Active Directory](active-directory-whatis.md)

## 创建 AD Azure 应用程序访问 API

报告 API 使用[OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx)授权访问 web Api。 若要访问您的目录信息，必须在活动目录中，创建应用程序并授予相应的权限才能访问 AAD 的数据。


### 创建应用程序
- 导航到[Azure 的管理门户](https://manage.windowsazure.com/)。
- 导航到目录中。
- 导航到应用程序中。
- 在底部栏上，单击"添加"。
    - 单击"添加我的公司正在开发的应用程序"。
    - **名称**︰ 任何名称很好。 "报告 API 应用程序"类似建议。
    - **类型**︰ 选择"Web 应用程序和/或 Web API"。
    - 单击箭头移动到下一个页面。
    - **登录 URL**: ```http://localhost```。
    - **应用程序 ID URI**: ```http://localhost```。
    - 单击选中标记完成添加应用程序。

### 授予您使用 API 的应用程序权限
- 导航到应用程序选项卡。
- 导航到您新创建的应用程序。
- 单击**配置**选项卡。
- 在"权限与其他应用程序"部分中︰
    - Microsoft 在 Azure Active Directory > 应用程序权限，选择**读取目录数据**。
- 底部栏上，单击**保存**。


### 获取您的目录 ID、 客户端 ID 和客户端密钥

下面的步骤将引导您完成获取应用程序的客户机 ID 和客户的机密。  您还需要知道您的组织名称，它可以是您 *。 onmicrosoft.com 或自定义的域的名称。  复制这些内容到不同的地方;您将使用它们来修改脚本。

#### 应用程序客户端 ID
- 导航到应用程序选项卡。
- 导航到您新创建的应用程序。
- 导航到**配置**选项卡。
- 应用程序的客户端 ID 将列在**客户 ID**字段。

#### 应用程序客户端密码
- 导航到应用程序选项卡。
- 导航到您新创建的应用程序。
- 导航到配置选项卡。
- 通过在"密钥"部分中选择一个持续时间生成新的密钥，您的应用程序。
- 保存时将显示该密钥。 一定要将其复制并粘贴到一个安全的地方，因为没有办法以后对其进行检索。


## 修改脚本
编辑下面的脚本来使用您的目录通过 $ClientID、 $ClientSecret 和 $tenantdomain 替换为正确的值从"Azure 广告中的委派访问"。

### PowerShell 脚本

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID      = "<<YOUR CLIENT ID HERE>>"                # Should be a ~35 character string insert your info here
    $ClientSecret  = "<<YOUR CLIENT SECRET HERE>>"          # Should be a ~44 character string insert your info here
    $loginURL      = "https://login.windows.net"
    $tenantdomain  = "<<YOUR TENANT NAME HERE>>"            # For example, contoso.onmicrosoft.com

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body          = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth         = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
        $headerParams  = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        # Returns a list of all the available reports
        Write-host List of available reports
        Write-host =========================
        $allReports = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports?api-version=beta")
        Write-host $allReports.Content

        Write-host
        Write-host Data from the AccountProvisioningEvents report
        Write-host ====================================================
        Write-host
        # Returns a JSON document for the "accountProvisioningEvents" report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/accountProvisioningEvents?api-version=beta")
        Write-host $myReport.Content

        Write-host
        Write-host Data from the AuditEvents report with datetime filter
        Write-host ====================================================
        Write-host
        # Returns a JSON document for the "auditEvents" report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/auditEvents?api-version=beta&$filter=eventTime gt 2015-05-20")
        Write-host $myReport.Content

        # Options for other output formats

        # to output the JSON use following line
        $myReport.Content | Out-File -FilePath accountProvisioningEvents.json -Force

        # to output the content to a name value list
        ($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath accountProvisioningEvents.txt -Force

        # to output the content in XML use the following line
        (($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath accountProvisioningEvents.xml -Force

    } else {
        Write-Host "ERROR: No Access Token"
        }

### Bash 脚本

    #!/bin/bash

    # Author: Ken Hoff (kenhoff@microsoft.com)
    # Date: 2015.08.20
    # NOTE: This script requires jq (https://stedolan.github.io/jq/)

    CLIENT_ID="<<YOUR CLIENT ID HERE>>"         # Should be a ~35 character string insert your info here
    CLIENT_SECRET="<<YOUR CLIENT SECRET HERE>>" # Should be a ~44 character string insert your info here
    LOGIN_URL="https://login.windows.net"
    TENANT_DOMAIN="<<YOUR TENANT NAME HERE>>"    # For example, contoso.onmicrosoft.com

    TOKEN_INFO=$(curl -s --data-urlencode "grant_type=client_credentials" --data-urlencode "client_id=$CLIENT_ID" --data-urlencode "client_secret=$CLIENT_SECRET" "$LOGIN_URL/$TENANT_DOMAIN/oauth2/token?api-version=1.0")

    TOKEN_TYPE=$(echo $TOKEN_INFO | jq -r '.token_type')
    ACCESS_TOKEN=$(echo $TOKEN_INFO | jq -r '.access_token')

    REPORT=$(curl -s --header "Authorization: $TOKEN_TYPE $ACCESS_TOKEN" https://graph.windows.net/$TENANT_DOMAIN/reports/auditEvents?api-version=beta)

    echo $REPORT | jq -r '.value' | jq -r ".[]"




## 执行脚本
一旦您完成编辑脚本，运行它，并验证 AuditEvents 报告中的预期的数据，则返回。

该脚本返回列出所有可用的报告，并从 AccountProvisioningEvents 报告在 PowerShell 窗口中以 JSON 格式返回的输出。 它还会创建与 JSON、 文本和 XML 中相同的输出文件。 可以修改该脚本以从其他报告中，返回的数据进行试验和注释掉的输出格式不需要注释。


## 下一步行动
- 想了解可以使用哪些安全、 审核及活动报表？ 签出[Azure AD 安全、 审核及活动报告](active-directory-view-access-usage-reports.md)
- 在审计报告上的更多详细信息，请参阅[事件 Azure 广告审核报告](active-directory-reporting-audit-events.md)
- 有关图形 API REST 服务的更多详细信息，请参见[Azure AD 报告和事件 （预览）](https://msdn.microsoft.com/library/azure/mt126081.aspx)

测试
