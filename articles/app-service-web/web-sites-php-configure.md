---
ms.openlocfilehash: d0ce282c4f075c14fddf28ba8b8739497a3670da
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Azure 应用程序服务 Web 应用程序中配置 PHP"
    description="了解如何配置默认的 PHP 安装或在 Azure 应用程序服务 Web 应用程序中添加自定义的 PHP 安装。"
    services="app-service\web"
    documentationCenter="php"
    authors="tfitzmac"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/24/2015"
    ms.author="tomfitz"/>

#在 Azure 应用程序服务 Web 应用程序中配置 PHP

## 简介

本指南将向您介绍如何在[Azure 应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)Web 应用程序的配置内置 PHP 运行时，提供自定义的 PHP 运行时，并启用扩展。 若要使用应用程序服务，注册[免费试用版]。 要充分利用本指南，应首先应用程序服务中创建一个 PHP web 应用程序。

## 如何︰ 更改内置 PHP 版本
默认情况下，PHP 5.4 已安装并立即可供使用时创建的应用程序服务 web 应用程序。 若要查看可用的发行版本、 默认的配置，并启用的扩展的最佳方法是部署脚本中调用[phpinfo()]函数。

PHP 5.5 和 PHP 5.6 版本也是可用的但默认情况下不启用。 要更新的 PHP 版本，请按照以下方法之一︰

### Azure 门户

1. 浏览到在[Azure 门户](http://go.microsoft.com/fwlink/?LinkId=529715)web 应用程序并单击**设置**按钮上。

    ![Web 应用程序设置][settings-button]

2. 从**设置**刀片式服务器选择**应用程序设置**并选择新的 PHP 版本。

    ![应用程序设置][application-settings]

3. 单击**Web 应用程序设置**刀片式服务器顶部的**保存**按钮。

    ![保存配置设置][save-button]

### Azure PowerShell (Windows)

1. 打开 Windows PowerShell
2. 类型`Set-AzureWebsite -PhpVersion [5.4 | 5.5 | 5.6] -Name <site-name>`，然后按 enter 键。
3. 现在设置 PHP 版本。

    ![设置使用 Azure PowerShell 的 PHP 版本][SETPHPVERPS]
4. 您可以通过键入来确认这些设置`Get-AzureWebiste -Name <site-name>`，然后按 enter 键。

    ![验证使用 Azure PowerShell 的 PHP 版本][GETPHPVERPS]

### Azure 的命令行界面 (Linux，Mac，Windows)

若要使用 Azure 命令行界面，您必须**Node.js**安装在您的计算机上。

1. 打开终端。
2. 类型`azure site set --php-version [5.4 | 5.5] [site-name]`，然后按 enter 键。
3. 现在设置 PHP 版本。

    ![设置 PHP 版本使用 Azure 的命令行界面][SETPHPVERCLI]
4. 您可以通过键入来确认这些设置`azure site show [site-name]`，然后按 enter 键。

    ![验证的 PHP 版本使用 Azure 的命令行界面][GETPHPVERCLI]

## 如何︰ 更改内置 PHP 配置

对于任何内置 PHP 运行时，您可以更改配置选项中的任何按照下面的步骤。 （php.ini 指令有关的信息，请参阅[php.ini 指令的列表]）。

### 更改 PHP\_INI\_用户，PHP\_INI\_PERDIR、 PHP\_INI\_的所有配置设置

1. 添加[。 user.ini]到根目录下的文件。
2. 添加配置设置应用于`.user.ini`文件中使用相同的语法中使用`php.ini`文件。 例如，如果您希望将`display_errors`设置并设置`upload_max_filesize`设置为 10 分钟，您`.user.ini`文件将包含此文本︰

        ; Example Settings
        display_errors=On
        upload_max_filesize=10M

3. 部署 web 应用程序。
4. 重新启动 web 应用程序。 (重新启动是必要的因为哪个 PHP 的频率读取`.user.ini`文件由`user_ini.cache_ttl`设置，它是系统级别设置，默认为 300 秒 （5 分钟）。 重新启动 web 应用程序会强制 PHP 读取中的新设置`.user.ini`文件。)

除了使用`.user.ini`文件，您可以使用[ini_set()]函数在脚本中设置配置选项不是系统级指令的。

### 更改 PHP\_INI\_系统配置设置

1. 使用密钥添加到您的 Web 应用程序的应用程序设置`PHP_INI_SCAN_DIR`和值 `d:\home\site\ini`
2. 创建`settings.ini`文件中使用 Kudu 控制台 (http://&lt;网站名称&gt;。 scm.azurewebsite.net) 在`d:\home\site\ini`目录。
3. 添加配置设置应用于`settings.ini`文件 php.ini 文件中使用相同的语法，请使用。 例如，如果要指出`curl.cainfo`设置为`*.crt`文件，并将 wincache.maxfilesize 设置配置为 512k，您`settings.ini`文件将包含此文本︰

        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. 请重新启动您的 Web 应用程序以加载所做的更改。

## 如何︰ 启用默认 PHP 运行时中的扩展
如前一节中所述，请参阅默认 PHP 版本，默认的配置，并启用的扩展的最佳方法是部署脚本中调用[phpinfo()]。 若要启用其他扩展，请按照下面的步骤︰

### 通过 ini 设置配置

1. 添加`ext`目录`d:\home\site`目录。
2. 放入`.dll`扩展名的文件`ext`目录 (例如， `php_mongo.dll` ， `php_xdebug.dll`)。 确保与 PHP （这是本书截稿时，PHP 5.4） 是 VC9 和非线程安全 (nts) 兼容的默认版本兼容扩展。
3. 使用密钥添加到您的 Web 应用程序的应用程序设置`PHP_INI_SCAN_DIR`和值 `d:\home\site\ini`
4. 创建`ini`文件以`d:\home\site\ini`名为`extensions.ini`。
5. 添加配置设置应用于`extensions.ini`文件 php.ini 文件中使用相同的语法，请使用。 例如，如果您想要启用了 MongoDB，XDebug 扩展您`extensions.ini`文件将包含此文本︰

        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. 请重新启动您的 Web 应用程序以加载所做的更改。

### 通过应用程序设置配置

1. 添加`bin`目录的根目录。
2. 放入`.dll`中的文件扩展名`bin`目录 (例如， `php_mongo.dll`)。 确保与 PHP （这是本书截稿时，PHP 5.4） 是 VC9 和非线程安全 (nts) 兼容的默认版本兼容扩展。
3. 部署 web 应用程序。
4. 浏览到在 Azure 门户 web 应用程序并单击**设置**按钮上。

    ![Web 应用程序设置][settings-button]

5. **设置**从刀片式服务器中选择**应用程序设置**和滚动到**应用程序设置**部分。
6. 在**应用程序设置**部分中创建的**PHP_EXTENSIONS**键。 该关键字的值是相对于网站根目录的路径︰ **bin\your 扩展文件**。

    ![启用扩展的应用程序设置中][php-extensions]

7. 单击**Web 应用程序设置**刀片式服务器顶部的**保存**按钮。

    ![保存配置设置][save-button]

Zend 扩展还支持通过使用**PHP_ZENDEXTENSIONS**项。 若要启用多个扩展名，请包含一个以逗号分隔的列表`.dll`文件的应用程序设置值。


## 如何︰ 使用自定义的 PHP 运行时
而不是默认的 PHP 运行时，应用程序服务 Web 应用程序可以使用 PHP 运行时提供的可执行 PHP 脚本。 可以通过配置运行时提供的`php.ini`还提供的文件。 要自定义的 PHP 运行时使用 Web 应用程序，请按照下面的步骤。

1. 获取一个非线程安全、 VC9 或 VC11 兼容版本的 Windows 的 PHP。 可以在此处找到的 PHP 的 Windows 最新版本︰ [http://windows.php.net/download/]。 这里的档案中找不到旧版本︰ [http://windows.php.net/downloads/releases/archives/]。
2. 修改`php.ini`为您运行库文件。 请注意 Web 应用程序将忽略任何配置设置是系统级别仅指令。 （仅限系统级别指令有关的信息，请参阅[php.ini 指令的列表]）。
3. （可选） 添加到您的 PHP 运行时扩展，使他们在`php.ini`文件。
4. 添加`bin`目录到根目录下，并将包含您的 PHP 运行时在它的目录 (例如， `bin\php`)。
5. 部署 web 应用程序。
4. 浏览到在 Azure 门户 web 应用程序并单击**设置**按钮上。

    ![Web 应用程序设置][settings-button]

7. **设置**从刀片式服务器中选择**应用程序设置**和滚动到**处理程序映射**部分。 添加`*.php`给扩展字段并添加到路径`php-cgi.exe`可执行文件。 如果您将您的 PHP 运行时放入`bin`目录中应用程序的根目录中，该路径将是`D:\home\site\wwwroot\bin\php\php-cgi.exe`。

    ![在处理程序映射中指定处理程序][handler-mappings]

8. 单击**Web 应用程序设置**刀片式服务器顶部的**保存**按钮。

    ![保存配置设置][save-button]

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 旧的门户与新门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)

[免费试用版]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php
[选择 php 版本]: ./media/web-sites-php-configure/select-php-version.png
[Php.ini 指令的列表]: http://www.php.net/manual/en/ini.list.php
[。 user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php
[ini_set()]: http://www.php.net/manual/en/function.ini-set.php
[应用程序设置]: ./media/web-sites-php-configure/application-settings.png
[设置按钮]: ./media/web-sites-php-configure/settings-button.png
[保存按钮]: ./media/web-sites-php-configure/save-button.png
[php 扩展]: ./media/web-sites-php-configure/php-extensions.png
[处理程序映射]: ./media/web-sites-php-configure/handler-mappings.png
[http://windows.php.net/download/]: http://windows.php.net/download/
[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png
 

测试
