---
ms.openlocfilehash: d544c72351e2cb03dbf8806cb460dffe1d1162d8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure 应用程序服务创建一个 PHP MySQL web 应用程序，并使用 FTP 部署" 
    description="演示如何创建 PHP 教程 web 应用程序将数据存储在 MySQL 和使用 FTP 部署到 Azure。" 
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


#在 Azure 应用程序服务创建一个 PHP MySQL web 应用程序，并使用 FTP 部署

本教程展示了如何创建一个 PHP MySQL web 应用程序以及如何将其使用 FTP 部署。 本教程假设您有[PHP][安装 php]， [MySQL][安装 mysql]、 web 服务器和 FTP 客户端计算机上安装。 在本教程中的说明进行操作的后面可以在任何操作系统，包括 Windows、 Mac 和 Linux 上。 在完成本指南，您必须在 Azure 上运行 PHP/MySQL web 应用程序。
 
您将了解︰

* 如何创建 web 应用程序和使用 Azure 门户的 MySQL 数据库。 因为 PHP Web 应用程序中默认启用的没有任何特殊需要运行您的 PHP 代码。
* 如何发布您的应用程序到 Azure 使用 FTP。
 
通过遵循本教程中，您将生成 PHP 中一个简单的注册 web 应用程序。 将 Web 应用程序中承载的应用程序。 下面是完整的应用程序的屏幕快照︰

![Azure 的 PHP 网站][running-app]

>[AZURE.NOTE] 如果您想要开始使用 Azure 应用程序服务帐户的注册之前，转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 不需要信用卡，没有承诺。 


##创建 web 应用程序和设置 FTP 发布

请按照下列步骤来创建一个 web 应用程序和 MySQL 数据库︰

1. 登录到[Azure 门户][管理门户]。
2. 请单击左下角的门户**+ 新建**图标。

    ![创建新的 Azure 网站][new-website]

3. 请单击**Web + 移动**，然后**Web 应用程序 + MySQL**。

    ![自定义创建新的 Web 站点][custom-create]

4. 输入资源组的有效名称。

    ![设置资源组的名称][resource-group]

5. 输入您的新 web 应用程序的值。

     ![创建 web 应用程序][new-web-app]

6. 输入新的数据库，其中包括同意的法律条款的值。

    ![创建新的 MySQL 数据库][new-mysql-db]
    
7. 创建 web 应用程序后，您将看到新的资源组。 单击要配置其设置的 web 应用程序的名称。

    ![打开 web 应用程序][go-to-webapp]

6. 向下滚动，直到您找到**设置部署凭据**。 

    ![集中部署凭据][set-deployment-credentials]

7. 要启用 FTP 发布，您必须提供用户名和密码。 保存的凭据，并记下用户名称和密码创建。

    ![创建发布的凭据][portal-ftp-username-password]

##生成和测试您的应用程序本地

注册应用程序是一个简单的 PHP 应用程序，允许您注册为事件提供您的姓名和电子邮件地址。 以前登记的信息将显示在表中。 注册信息存储在一个 MySQL 数据库中。 该应用程序由两个文件组成︰

* **index.php**︰ 显示注册和包含注册人信息表的窗体。
* **createtable.php**︰ 创建应用程序的 MySQL 表。 此文件将只使用一次。

要生成并在本地运行该应用程序，请按照下面的步骤。 请注意，这些步骤都假定有 PHP，MySQL，并安装在您的本地计算机，以及您的 web 服务器已启用了[MySQL 的 PDO 扩展][pdo mysql]。

1. 创建一个 MySQL 数据库，名为`registration`。 您可以从 MySQL 命令提示符处使用以下命令来执行此操作︰

        mysql> create database registration;

2. 在您的 web 服务器的根目录下，创建一个名为文件夹`registration`和创建两个文件，在其中的一个名为`createtable.php`，一个名为`index.php`。

3. 打开`createtable.php`文件中的文本编辑器或 IDE 并添加下面的代码。 此代码用于创建`registration_tbl`表中`registration`数据库。

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > 您将需要更新的值<code>$user</code>，<code>$pwd</code>与您本地 MySQL 用户名称和密码。

4. 打开 web 浏览器并定位到[http://localhost/registration/createtable.php][本地主机 createtable]。 这将创建`registration_tbl`数据库中的表。

5. 在文本编辑器或 IDE 打开**index.php**文件并添加 （将在后续步骤中添加 PHP 代码） 页面的基本 HTML 和 CSS 代码。

        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php

        ?>
        </body>
        </html>

6. 在 PHP 标记，将添加用于连接到数据库的 PHP 代码。

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > 再次，需要更新的值<code>$user</code>，<code>$pwd</code>与您本地 MySQL 用户名称和密码。

7. 数据库连接的代码中，添加代码，以向数据库插入注册信息。

        if(!empty($_POST)) {
        try {
            $name = $_POST['name'];
            $email = $_POST['email'];
            $date = date("Y-m-d");
            // Insert data
            $sql_insert = "INSERT INTO registration_tbl (name, email, date) 
                           VALUES (?,?,?)";
            $stmt = $conn->prepare($sql_insert);
            $stmt->bindValue(1, $name);
            $stmt->bindValue(2, $email);
            $stmt->bindValue(3, $date);
            $stmt->execute();
        }
        catch(Exception $e) {
            die(var_dump($e));
        }
        echo "<h3>Your're registered!</h3>";
        }

8. 最后，上面的代码中，添加代码，用于从数据库中检索数据。

        $sql_select = "SELECT * FROM registration_tbl";
        $stmt = $conn->query($sql_select);
        $registrants = $stmt->fetchAll(); 
        if(count($registrants) > 0) {
            echo "<h2>People who are registered:</h2>";
            echo "<table>";
            echo "<tr><th>Name</th>";
            echo "<th>Email</th>";
            echo "<th>Date</th></tr>";
            foreach($registrants as $registrant) {
                echo "<tr><td>".$registrant['name']."</td>";
                echo "<td>".$registrant['email']."</td>";
                echo "<td>".$registrant['date']."</td></tr>";
            }
            echo "</table>";
        } else {
            echo "<h3>No one is currently registered.</h3>";
        }

现在可以浏览到[http://localhost/registration/index.php][localhost 索引]来测试该应用程序。

##获得 MySQL 和 FTP 连接信息

若要连接到 MySQL 数据库的 Web 应用程序，您将在运行时需要的连接信息。 若要获取 MySQL 连接信息，请执行以下步骤︰

1. 从资源组中，单击数据库:

    ![选择数据库][select-database]

2. 从数据库 sumamry 中，选择**属性**。

    ![选择属性][select-properties]
    
2. 请记下的值`Database`， `Host`， `User Id`，和`Password`。

    ![注释属性][note-properties]

3. 从 web 应用程序，请单击位于页面的右下角的**下载发布配置文件**链接︰

    ![下载发布配置文件][download-publish-profile]

4. 打开`.publishsettings`在 XML 编辑器中的文件。 

3. 查找`<publishProfile >`元素与`publishMethod="FTP"`的类似于下图︰

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
请记下的`publishUrl`， `userName`，和`userPWD`属性。

##发布您的应用程序

在测试您的应用程序本地后，可以将其发布到您的 web 应用程序使用 FTP。 但是，您首先需要更新应用程序中的数据库连接信息。 使用前面 （在**获得 MySQL 和 FTP 连接信息**部分中），您获得的数据库连接信息更新的以下信息**同时**在`createdatabase.php`和`index.php`文件用适当的值︰

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

现在，您就可以发布您的应用程序使用 FTP。

1. 打开 FTP 客户端选择。

2. 输入的*主机名部分*从`publishUrl`您的 FTP 客户端到上面提到的特性。

3. 输入`userName`，`userPWD`到您的 FTP 客户端的上面提到的属性保持不变。

4. 建立连接。

连接后将能够上载和下载文件，根据需要。 请确保您将文件上载到根区目录，这是`/site/wwwroot`。

同时上载后`index.php`， `createtable.php`，浏览到**http://[site name].azurewebsites.net/createtable.php**创建的 MySQL 表的应用程序，然后浏览到**http://[site name].azurewebsites.net/index.php** ，开始使用该应用程序。
 

[安装 php]: http://www.php.net/manual/en/install.php
[安装 mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[本地主机 createtable]: http://localhost/tasklist/createtable.php
[本地主机索引]: http://localhost/tasklist/index.php
[运行应用程序]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[新网站]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[创建自定义的]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[网站详细信息]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[新 mysql 数据库]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[转到 webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[部署的一组凭据]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[门户网站 ftp 用户名密码]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[资源组]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[新 web 应用]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[选择数据库]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[选择属性]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[注释属性]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[字符串的连接信息]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[管理门户]: https://portal.azure.com
[下载发布配置]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 

测试
