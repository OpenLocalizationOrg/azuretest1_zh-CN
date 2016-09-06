---
ms.openlocfilehash: 50e24c47c5f9d4f8a7261353b2eb4dddd8c1ef64
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 通知集线器 |Microsoft Azure"
    description="在本教程中，您将学习如何使用 Azure 通知集线器到 Kindle 应用程序发送推式通知。"
    services="notification-hubs"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

# 开始使用通知集线器

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##概述

本教程展示如何使用 Azure 通知集线器到 Kindle 应用程序发送推式通知。
您将创建一个空白的 Kindle 应用程序使用 Amazon 设备消息 (ADM) 接收推式通知。

##先决条件

本教程要求如下︰

+ 从获取 Android SDK （我们假定您将使用 Eclipse） <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android 的网站</a>。
+ 按照中的步骤<a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">设置开发环境</a>为 Kindle 设置开发环境。

##添加新应用程序开发人员门户

1. 首先， [Amazon 开发人员门户]中创建的应用程序。

    ![][0]

2. 复制**应用程序注册表项**。

    ![][1]

3. 在门户中，单击您的应用程序的名称，然后单击**设备消息**选项卡。

    ![][2]

4. 单击**创建新的安全配置文件**，然后创建新的安全配置文件 （例如， **TestAdm 的安全配置文件**）。 然后单击**保存**。

    ![][3]

5. 单击以查看您刚刚创建的安全模板的**安全配置文件**。 复制**客户机 ID**和**客户端密钥**值以供将来使用。

    ![][4]

## 创建一个 API 键

1. 使用管理员权限打开命令提示符。
2. 导航到 Android SDK 文件夹。
3. 输入以下命令︰

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  为**密钥库**口令中，键入**android**。

5.  将复制的**MD5**指纹。
6.  回到中开发人员门户，在**邮件**选项卡上单击**Android/Kindle**和输入包的名称为您的应用程序 (例如， **com.sample.notificationhubtest**) 和**MD5**值，然后单击**生成 API 密钥**。

## 添加到该中心的凭据

在门户中，将添加到通知中心的**配置**选项卡上的客户端机密和客户机 ID。

## 应用程序设置

> [AZURE.NOTE] 在创建应用程序时，至少要使用 API 级别 17。

ADM 库添加到 Eclipse 项目︰

1. 若要获得 ADM 库，[下载 SDK]。 提取的 SDK zip 文件。
2. 在 Eclipse 中，用鼠标右键单击您的项目，然后单击**属性**。 在左边，选择**Java 构建路径**，然后选择**库**选项卡的顶部。 单击**添加外部 jar/文件夹**，然后选择文件`\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar`从 Amazon SDK 的解压的目录。
3. 下载 NotificationHubs Android SDK （链接）。
4. 解压缩该程序包，并将该文件`notification-hubs-sdk.jar`到`libs`在 Eclipse 中的文件夹。

编辑您的应用程序清单，以支持 ADM:

1. 在根元素中清单添加 Amazon 的命名空间︰


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. 作为清单元素下的第一个元素添加权限。 与用于创建您的应用程序的程序包替换**[您的软件包名称]** 。

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. 作为应用程序元素的第一个子级插入下面的元素。 请记住要在下一个部分 （包括包） 中，创建您 ADM 消息处理程序的名称替换**[您的服务名称]**和**[您的软件包名称]**替换创建您的应用程序使用的程序包名称。

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver"

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## 创建 ADM 消息处理程序

1. 创建新类继承自`com.amazon.device.messaging.ADMMessageHandlerBase`，并将它命名为`MyADMMessageHandler`，如下图中所示︰

    ![][6]

2. 添加以下`import`语句︰

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. 在您创建的类中添加以下代码。 请记住，替换集线器名称和连接字符串 （听）︰

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.drawable.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. 添加以下代码为`OnMessage()`方法︰

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. 添加以下代码为`OnRegistered`方法︰

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  添加以下代码为`OnUnregistered`方法︰

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. 在`MainActivity`方法中，添加下面的导入语句︰

        import com.amazon.device.messaging.ADM;

8. 在末尾添加以下代码`OnCreate`方法︰

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## 将您的 API 密钥添加到您的应用程序

1. 在 Eclipse 中，创建一个名为**api_key.txt**在您的项目目录资产中的新文件。
2. 打开该文件并复制在 Amazon 开发人员门户中生成的 API 密钥。

## 运行应用程序

1. 启动仿真程序。
2. 在仿真器中，刷卡从顶部和单击**设置**然后单击**我的帐户**并有效的 Amazon 帐户注册。
3. 日蚀式，在运行应用程序。

> [AZURE.NOTE] 如果出现问题，请检查的模拟器 （或设备） 的时间。 时间值必须是准确的。 若要更改 Kindle 仿真程序的时间，可以从 Android SDK 平台工具目录运行以下命令︰

        adb shell  date -s "yyyymmdd.hhmmss"

## 发送消息

若要使用.NET 中发送一条消息︰

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon 开发人员门户]: https://developer.amazon.com/home.html
[下载 SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
