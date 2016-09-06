---
ms.openlocfilehash: ee0973b9b779d178bdbd6a5e2a3298fb8a56dae5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
### 通过作曲家安装

1. [安装 Git][安装 git]。 

 
    > [AZURE.NOTE] 在 Windows 上，您还需要添加到 PATH 环境变量的可执行 git 中获取。

2. 创建名为**composer.json**的项目根目录中的文件并向其中添加下面的代码︰

        {
            "repositories": [
                {
                    "type": "pear",
                    "url": "http://pear.php.net"
                }
            ],
            "require": {
                "pear-pear.php.net/mail_mime" : "*",
                "pear-pear.php.net/http_request2" : "*",
                "pear-pear.php.net/mail_mimedecode" : "*",
                "microsoft/windowsazure": "*"
            }
        }


3. 下载**[composer.phar][作曲家 phar]**中的项目根。

4. 打开命令提示符窗口，并在项目根目录中执行以下命令

        php composer.phar install

### 手动安装

要下载并手动安装 Azure PHP 客户端库，请执行以下步骤︰

1. 下载.zip 存档包含从[GitHub][github 的 sdk 的 php]库。 或者，分叉存储库并克隆它到您的本地计算机。 第二种选项要求 GitHub 帐户并让 Git 安装在本地。

    
    > [AZURE.NOTE] Azure PHP 客户端库的[HTTP_Request2](http://pear.php.net/package/HTTP_Request2)、 [Mail_mime](http://pear.php.net/package/Mail_mime)和[Mail_mimeDecode](http://pear.php.net/package/Mail_mimeDecode)梨树软件包具有相关性。 解决这些依赖项的推荐的方式是安装这些软件包使用[梨树程序包管理器](http://pear.php.net/manual/en/installation.php)。


2. 复制`WindowsAzure`下载存档应用程序目录结构的目录。

有关安装 （包括为梨树包安装有关的信息） 的 Azure PHP 客户端库的详细信息，请参阅[下载 php Azure SDK][下载 SDK PHP]。


[php 的 sdk github]: http://go.microsoft.com/fwlink/?LinkId=252719
[安装 git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[下载 SDK PHP]: ../articles/php-download-sdk.md
[作曲家 phar]: http://getcomposer.org/composer.phar
