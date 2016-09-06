---
ms.openlocfilehash: 373f68599eae56c2f192095ac157b6e07fa75ec4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="单一登录使用 Azure 活动目录 (PHP)" 
    description="了解如何创建单一登录使用 Azure Active Directory 的 PHP web 应用。" 
    services="active-directory" 
    documentationCenter="php" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="07/17/2015" 
    ms.author="tomfitz"/>

# Web 单一登录使用 PHP 和 Azure 的活动目录

##<a name="introduction"></a>简介

本教程将说明如何利用 Azure Active Directory 以启用单一登录的用户的 Office 365 提供客户 PHP 开发人员。 您将学习如何︰

* 提供在客户的组织的 web 应用程序
* 保护使用 WS 联合身份验证的应用程序

###先决条件
下面的开发环境条件所需的本演练︰

* [Azure 的 Active Directory 的 PHP 示例代码]
* [日蚀式 PDT 3.0.x 中的所有]
* PHP 5.3.1 （通过 Web 平台安装程序）
* Internet Information Services (IIS) 7.5 与启用 SSL
* Windows PowerShell
* [Office 365 PowerShell Commandlets]

## 步骤 1︰ 创建一个 PHP 应用程序
此步骤描述如何创建一个简单的 PHP 应用程序表示受保护的资源。 通过联合身份验证由该公司的 STS，文中所述教程，将被授予访问此资源。

1. 打开 Eclipse 的一个新实例。
2. 从**文件**菜单上，单击**新建**，然后单击**新 PHP 项目**。 
3. 在**新 PHP 项目**对话框中，名称*phpSample*项目，然后单击**完成。**
4. 从左侧的**PHP 资源管理器**菜单，用鼠标右键单击*phpProject*，单击**新建**，然后单击**PHP 文件**。
5. 在**新的 PHP 文件**对话框中，名称**index.php**文件，然后单击**完成。**
6. 生成的标记替换为以下内容，然后保存**index.php**:

        <!DOCTYPE>
        <html>
        <head>
            <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
            <title>Index</title>
        </head>
        <body>
            ##Index Page
        </body>
        </html> 

7. 打开**Internet Information Services (IIS) 管理**器的运行提示符下键入*inetmgr* ，然后按 Enter。

8. 在 IIS 管理器中，展开**网站**文件夹左侧的菜单中，右键单击**默认网站**，然后单击**添加应用程序...**。

9. 在**添加应用程序**对话框中，设置**别名**值为*phpSample*和文件路径的**物理路径**创建 PHP 项目的位置。

10. 在 Eclipse 中，从**运行**菜单上，单击**运行**。

11. 在**运行 PHP Web 应用程序**菜单上，单击**确定**。

12. **Index.php**页将打开 Eclipse 在新选项卡。 页面应该只显示文本︰*索引页*。 

## 步骤 2︰ 设置的应用程序中的公司目录租户
此步骤描述如何 Azure Active Directory 客户管理员规定其租户的 PHP 应用程序，并配置单一登录。 该步骤完成之后，公司的员工可以向 web 应用程序使用其 Office 365 的帐户进行身份验证。

通过创建新的应用程序服务主体开始设置过程。 Azure Active Directory 使用服务主体注册和验证应用程序的目录。

1. 下载并安装 Office 365 PowerShell Commandlets，如果您还没有尝试过。
2. 从**开始**菜单运行**Azure 活动目录模块用于 Windows PowerShell**控制台。 该控制台提供了用于配置有关您 Office 365 租户，如创建和修改服务主体属性的命令行环境。
3. 若要导入的必需的**MSOnlineExtended**模块，请键入以下命令并按 Enter:

        Import-Module MSOnlineExtended -Force
4. 要连接到您的 Office 365 目录，您需要提供该公司的管理员凭据。 键入以下命令并按 enter 键，然后输入您的凭据的提示︰

        Connect-MsolService
5. 现在您将创建新的应用程序服务主体。 键入以下命令并按 enter 键︰

        New-MsolServicePrincipal -ServicePrincipalNames @("phpSample/localhost") -DisplayName "Federation Sample Website" -Type Symmetric -Usage Verify -StartDate "12/01/2012" -EndDate "12/01/2013" 
此步骤将输出类似于以下的信息︰

        The following symmetric key was created as one was not supplied qY+Drf20Zz+A4t2w e3PebCopoCugO76My+JMVsqNBFc=
        DisplayName           : Federation Sample PHP Website
        ServicePrincipalNames : {phpSample/localhost}
        ObjectId              : 59cab09a-3f5d-4e86-999c-2e69f682d90d
        AppPrincipalId        : 7829c758-2bef-43df-a685-717089474505
        TrustedForDelegation  : False
        AccountEnabled        : True
        KeyType               : Symmetric
        KeyId                 : f1735cbe-aa46-421b-8a1c-03b8f9bb3565
        StartDate             : 12/01/2012 08:00:00 a.m.
        EndDate               : 12/01/2013 08:00:00 a.m.
        Usage                 : Verify 
> [AZURE.NOTE] 
> 您应保存此输出，尤其是生成对称密钥。 此项仅显示给您服务主体创建的过程，您将不能在以后对其进行检索。 其他值所需的图形 API 用于读取和写入目录中的信息。

6. 最后一步设置回复 URL 为您的应用程序。 回复 URL 是以下身份验证尝试的响应发送至何处。 键入以下命令并按 enter 键︰

        $replyUrl = New-MsolServicePrincipalAddresses –Address "https://localhost/phpSample" 

        Set-MsolServicePrincipal –AppPrincipalId "7829c758-2bef-43df-a685-717089474505" –Addresses $replyUrl 
    
现在已在目录中设置的 web 应用程序，它可用于 web 单一登录由公司员工。

## 步骤 3︰ 保护的雇员号使用 WS 联合身份验证的应用程序
此步骤演示了如何添加对联合登录系统必备组件中的代码示例使用 Windows 标识基础 (WIF) 和您下载的 simpleSAML.php 库的支持。 此外将添加一个登录页面，并配置应用程序和目录租户之间的信任。

1. 在 Eclipse 中，右击**phpSample**项目，单击**新建**，然后单击**文件**。 

2. 在**新建文件**对话框中，名称文件**federation.ini**中，然后单击**完成。**

3. 在新的**federation.ini**文件中，请输入以下信息，提供的值与创建服务主体时，在第 2 步中保存的信息︰

        federation.trustedissuers.issuer=https://accounts.accesscontrol.windows.net/v2/wsfederation
        federation.trustedissuers.thumbprint=qY+Drf20Zz+A4t2we3PebCopoCugO76My+JMVsqNBFc=
        federation.trustedissuers.friendlyname=Fabrikam
        federation.audienceuris=spn:7829c758-2bef-43df-a685-717089474505
        federation.realm=spn:7829c758-2bef-43df-a685-717089474505
        federation.reply=https://localhost/phpSample/index.php 


    > [AZURE.NOTE] **Audienceuris**和**领域**值必须的前面加上"spn:"。

4. 在 Eclipse 中，右击**phpSample**项目，单击**新建**，然后单击**PHP 文件**。 

5. 在**新的 PHP 文件**对话框中，名称文件**secureResource.php**中，然后单击**完成。**

6. 在新的**secureResource.php**文件中，请输入以下代码中， **c:\phpLibraries**路径替换为您下载的示例代码的根位置。 根位置应包含**simpleSAML.php**文件和**联盟**文件夹︰

        <?php
        ini_set('include_path', ini_get('include_path').';c:\phpLibraries\;');
        require_once ('federation/FederatedLoginManager.php');
        session_start();
        $token = $_POST['wresult'];
        $loginManager = new FederatedLoginManager();
        if (!$loginManager->isAuthenticated()) {
            if (isset ($token)) {
                try {
                    $loginManager->authenticate($token);
                } catch (Exception $e) {
                    print_r($e->getMessage());
                }  
            } else {
                $returnUrl = "https://" . $_SERVER['HTTP_HOST'] . $_SERVER['PHP_SELF'];
                header('Pragma: no-cache');
                header('Cache-Control: no-cache, must-revalidate');
                header("Location: " . FederatedLoginManager :: getFederatedLoginUrl($returnUrl), true, 302);
                exit();
            }
        }
        ?> 

7. 打开**index.php**页面并更新其内容，以安全页，然后将其保存︰

        <?php
        require_once (dirname(__FILE__) . '/secureResource.php');
        ?>
        <!DOCTYPE html>
        <html>
        <head>
            <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
            <title>Index Page</title>
        </head>
        <body>
            <h2>Index Page</h2>
            <h3>Welcome <strong><?php print_r($loginManager->getPrincipal()->getName()); ?></strong>!</h3>
            <h4>Claim list:</h4>
            <ul>
                <?php
                foreach ($loginManager->getClaims() as $claim) {
                    print_r('<li>' . $claim->toString() . '</li>');
                }
                ?>  
            </ul>
        </body>
        </html> 

8. 从**运行**菜单上，单击**运行**。 您应自动重定向到 Office 365 身份标识提供程序页上，您可以记录中使用目录租户的凭据。 例如， *john.doe@fabrikam.onmicrosoft.com*。

## 摘要
本教程向您展示了如何创建和配置一个租户使用 Azure Active Directory 的单一登录功能的 PHP 应用程序。

<Https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/WAAD.WebSSO.PHP>提供了一个示例，演示如何使用 PHP 网站 Azure Active Directory 和单一登录。


[步骤 1︰ 创建一个 PHP 应用程序]: #createapp
[步骤 2︰ 设置的应用程序中的公司目录租户]: #provisionapp
[步骤 3︰ 保护的雇员号使用 WS 联合身份验证的应用程序]: #protectapp
[摘要]: #summary
[简介]: #introduction
[开发多租户云应用程序使用 Azure 的活动目录]: http://g.microsoftonline.com/0AX00en/121
[Windows 标识基础 3.5 SDK]: http://www.microsoft.com/download/details.aspx?id=4451
[Windows 标识 1.0 的基础运行时]: http://www.microsoft.com/download/details.aspx?id=17331
[Office 365 Powershell Commandlets]: http://msdn.microsoft.com/library/azure/jj151815.aspx
[ASP.NET 3 MVC]: http://www.microsoft.com/download/details.aspx?id=4211
[日蚀式 PDT 3.0.x 中的所有]: http://www.eclipse.org/pdt/downloads/
[Azure 的 Active Directory 的 PHP 示例代码]: https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/WAAD.WebSSO.PHP 
 
测试
