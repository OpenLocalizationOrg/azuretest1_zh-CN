---
ms.openlocfilehash: ddcb0c1b5c78d6f13f095f5966eb7d8f85465983
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="从 runbook 开始在 Azure 自动化"
   description="总结了不同的方法，可用来启动 runbook Azure 自动化中使用 Azure 门户和 Windows PowerShell 提供了详细信息。"
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2015"
   ms.author="bwren" />

# 从 runbook 开始在 Azure 自动化

下表将帮助您确定要启动 runbook Azure 自动化中最适用于您的特定情况的方法。    这篇文章包括与 Azure 的门户和 Windows PowerShell 启动 runbook 的详细信息。  您可以从下面的链接访问其他文档中提供了其他方法的详细信息。

<table>
 <tr>
  <td>方法</td>
  <td>特征</td>
 </tr>
 <tr>           
  <td><a href="#starting-a-runbook-with-the-azure-portal">Azure 门户</a></td>
  <td>
   <ul>
    <li>交互式用户界面的最简单方法。</li>
    <li>窗体中提供简单的参数值。</li>
    <li>轻松跟踪作业状态。</li>
    <li>使用 Azure 的登录身份验证的访问。</li>
   </ul>
  </td>
 </tr>
 <tr>
  <td><a href="https://msdn.microsoft.com/library/dn690259.aspx">Windows PowerShell</a></td>
  <td>
   <ul>
     <li>从命令行中使用 Windows PowerShell cmdlet 调用。</li>
     <li>可以包含多个步骤的自动化解决方案中。</li>
     <li>请求进行身份验证的证书或 OAuth 用户主体 / 服务主体。</li>
     <li>提供简单和复杂的参数值。</li>
     <li>跟踪工作状态。</li>
     <li>支持 PowerShell cmdlet 所需的客户端。</li>
   </ul>
  </td>
 </tr>
 <tr>
 <tr>
  <td><a href="http://msdn.microsoft.com/library/azure/mt163849.aspx">Azure 自动化 API</a></td>
  <td>
   <ul>
    <li>最灵活的方法，但也最复杂。</li>
    <li>从可以发出 HTTP 请求的任何自定义代码中调用。</li>
    <li>经过身份验证的证书，或 Oauth 用户主体 / 服务请求主体。</li>
    <li>提供简单和复杂的参数值。</li>
    <li>跟踪工作状态。</li>
   </ul>
  </td>
 </tr>
 <tr>
 <tr>
  <td><a href="http://azure.microsoft.com/documentation/articles/automation-webhooks/">Webhook</a></td>
  <td>
   <ul>
    <li>从 runbook 开始从单一的 HTTP 请求。</li>
    <li>使用 URL 中的安全令牌身份验证。</li>
    <li>客户端无法重写 webhook 创建时指定的参数值。  Runbook 可以定义包含 HTTP 请求的详细信息的单个参数。</li>
    <li>跟踪工作状态通过 webhook URL 不能。</li>
   </ul>
  </td>
 </tr>
 <tr>
 <tr>
  <td><a href="http://azure.microsoft.com/documentation/articles/automation-scheduling-a-runbook">时间表</a></td>
  <td>
   <ul>
    <li>自动启动 runbook 每小时、 每日或每周的时间表。</li>
    <li>操作通过 Azure 的门户，PowerShell 的 cmdlet 或 Azure API 的计划。</li>
    <li>提供参数值用于日程安排。</li>
   </ul>
  </td>
 </tr>
 <tr>
  <td><a href="http://msdn.microsoft.com/library/azure/dn857355.aspx">从另一个 runbook</a></td>
  <td>
   <ul>
    <li>使用 runbook 作为另一个 runbook 中的活动</li>
    <li>对于由多个运行手册的功能非常有用。</li>
    <li>提供子 runbook 的参数值并使用父 runbook 中的输出。</li>
   </ul>
  </td>
 </tr>
</table>
<br>



## 从 runbook 开始的 Azure 门户

1. 在 Azure 的门户中，选择**自动化**，然后再单击自动化帐户的名称。
1. 选择**运行手册**选项卡。
1. 选择 runbook，然后单击**开始**。
1. 如果 runbook 参数，系统会提示您用在文本框中为每个参数提供值。 更多有关参数的详细信息，请参阅下面的[Runbook 参数](#Runbook-parameters)。
1. 请选择**查看作业**下, 一步**启动**runbook 消息或选择 runbook 来查看 runbook 作业的状态为**作业**选项卡。

## 从 runbook 开始的 Azure 预览门户

1. 从自动化您的帐户，单击以打开**运行手册**刀片式服务器的**运行手册**部分。
1. 单击要打开其**Runbook**刀片式服务器 runbook。
2. 单击**开始**。
1. 如果 runbook 没有任何参数，将提示您确认是否要启动它。  如果 runbook 参数，则将会打开**启动 Runbook**刀片式服务器，使您可以提供参数值。 更多有关参数的详细信息，请参阅下面的[Runbook 参数](#Runbook-parameters)。
3. **作业**刀片式服务器已打开，以便您可以跟踪作业的状态。


## 从 runbook 开始 Windows PowerShell

可以使用[启动 AzureAutomationRunbook](http://msdn.microsoft.com/library/azure/dn690259.aspx)以 runbook 开头 Windows PowerShell。 下面的代码示例启动调用测试 Runbook runbook。

    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook"

开始 AzureAutomationRunbook 返回可用于启动 runbook 后跟踪其状态的作业对象。 然后可以使用[Get AzureAutomationJob](http://msdn.microsoft.com/library/azure/dn690263.aspx)与本作业对象来确定作业和[Get AzureAutomationJobOutput](http://msdn.microsoft.com/library/azure/dn690268.aspx)以获取其输出的状态。 下面的代码示例启动 runbook 调用测试 Runbook，等待它完成，并显示其输出。

    $job = Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook"
    
    $doLoop = $true
    While ($doLoop) {
       $job = Get-AzureAutomationJob –AutomationAccountName "MyAutomationAccount" -Id $job.Id
       $status = $job.Status
       $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped") 
    }
    
    Get-AzureAutomationJobOutput –AutomationAccountName "MyAutomationAccount" -Id $job.Id –Stream Output

如果 runbook 需要参数，则必须提供它们作为[哈希表](http://technet.microsoft.com/library/hh847780.aspx)在哈希表的键匹配的参数名和值是参数值。 下面的示例演示如何以 runbook 开头两个名为名字字段和姓氏、 名为 RepeatCount，一个整数和命名放映一个布尔型参数的字符串参数。 参数的其他信息，请参阅[Runbook 参数](#Runbook-parameters)下。

    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" –Parameters $params

## Runbook 参数

当您开始使用 Azure 管理门户或 Windows PowerShell runbook 时，通过 Azure 自动化 web 服务发送指令。 此服务不支持复杂数据类型的参数。 如果您需要一个复杂的参数提供的值，然后必须调用其内联从[开始从另一个 Runbook Runbook](http://msdn.microsoft.com/library/azure/dn857355.aspx)所述的另一个 runbook。

Azure 自动化 web 服务将提供使用以下各节中所述某些数据类型参数的特殊功能。

### 已命名的值

如果该参数是数据类型 [对象]，则可以使用下面的 JSON 格式发送该已命名的值的列表︰ *{"Name1": Value1、"Name2": 值 2，"Name3": Value3}*。 这些值必须是简单类型。 Runbook 会收到[PSCustomObject](http://msdn.microsoft.com/library/azure/system.management.automation.pscustomobject(v=vs.85).aspx)与属性对应于每个已命名的值作为参数。

请考虑下面的测试 runbook 接受名为 user 参数。

    Workflow Test-Parameters
    {
       param ( 
          [Parameter(Mandatory=$true)][object]$user
       )
        if ($user.Show) {
            foreach ($i in 1..$user.RepeatCount) {
                $user.FirstName
                $user.LastName
            }
        } 
    }

下面的文本无法用于用户参数。

    {"FirstName":"Joe","LastName":"Smith","RepeatCount":2,"Show":true}

这会导致下面的输出。

    Joe
    Smith
    Joe
    Smith

### 数组

如果参数是一个数组，如 [阵列] 或 [字符串 []]，则可以使用以下的 JSON 格式发送它的值的列表: *[值 1，值 2，Value3]*。 这些值必须是简单类型。

请考虑下面的测试 runbook 接受名为*user*参数。

    Workflow Test-Parameters
    {
       param ( 
          [Parameter(Mandatory=$true)][array]$user
       )
        if ($user[3]) {
            foreach ($i in 1..$user[2]) {
                $ user[0]
                $ user[1]
            }
        } 
    }

下面的文本无法用于用户参数。

    ["Joe","Smith",2,true]

这会导致下面的输出。

    Joe
    Smith
    Joe
    Smith

### Credentials

如果该参数是数据类型**PSCredential**，您可以提供 Azure 自动化[凭据资产](http://msdn.microsoft.com/library/azure/dn940015.aspx)的名称。 Runbook 将检索与您指定的名称，凭据。

请考虑以下测试 runbook 接受参数称为凭据。

    Workflow Test-Parameters
    {
       param ( 
          [Parameter(Mandatory=$true)][PSCredential]$credential
       )
       $credential.UserName
    }

下面的文本可能用于为用户参数假设，没有称为*我的凭据*的凭据资产。

    My Credential

这假定凭据中的用户名*jsmith*，在下面的输出结果。

    jsmith

## 相关的文章

- [开始从另一个 Runbook Runbook](http://msdn.microsoft.com/library/azure/dn857355.aspx)测试
