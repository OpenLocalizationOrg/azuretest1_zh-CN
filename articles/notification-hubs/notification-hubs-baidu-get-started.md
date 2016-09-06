---
ms.openlocfilehash: 2a86a663cebb431e4835ec6843ac48542924723e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 通知集线器 |Microsoft Azure"
    description="在本教程中，您将学习如何使用 Azure 通知集线器到 Android 设备的推式通知。"
    services="notification-hubs"
    documentationCenter="android"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

# 开始使用通知集线器

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##概述

Baidu 云推是中国式的云服务可用于向移动设备发送推式通知。 此服务是非常有用的在中国，其中交付给 Android 的推式通知是复杂由于存在不同的应用程序商店和推送服务，除了没有通常连接到 GCM （Google 云消息） 的 Android 设备的可用性。

##先决条件

本教程要求如下︰

+ Android SDK （我们假定您将使用 Eclipse），您可以从下载<a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android 的站点</a>
+ [移动服务 Android SDK]
+ [Baidu 推 Android SDK]

>[AZURE.NOTE] 若要完成本教程，您必须为活动 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F)。


##创建 Baidu 帐户

若要使用 Baidu，必须具有 Baidu 帐户。 如果您已经有一个登录[Baidu 门户]，请跳到下一步。 否则，如何创建 Baidu 帐户中看到下面的说明进行操作。  

1. 转到[Baidu 门户]并单击**登录**（**登录**） 链接。 单击**立即注册**启动帐户注册过程。

    ![][1]

2. 输入所需的详细信息 — — 电话/电子邮件地址、 密码和验证代码，然后单击**注册**。

    ![][2]

3. 您将您输入的链接来激活 Baidu 帐户的电子邮件地址发送一封电子邮件。

    ![][3]

4. 登录到您的电子邮件帐户、 打开 Baidu 激活邮件，并单击以激活 Baidu 帐户激活链接。

    ![][4]

一旦激活的 Baidu 帐户，登录到[Baidu 门户]。

##作为开发人员，Baidu 注册

1. 一旦您已登录到[Baidu 门户]，单击**更多 >>** （**更多**）。

    ![][5]

2. 在**站长与开发者服务 （站点管理员和开发人员服务）**部分向下滚动并单击**百度开放云平台**（**Baidu 打开云平台**）。

    ![][6]

3. 在下一页上，单击**开发者服务**（**开发商服务**） 上的右上角。

    ![][7]

4. 在下一页中，从右上角的菜单中单击**注册开发者**（**注册开发人员**）。

    ![][8]

5. 用于接收确认文本信息，输入您的名称、 说明和移动电话号码，然后单击**送验证码**（**发送验证代码**）。 请注意，对于国际电话号码，您都需要用括号括起来的国家/地区代码。 例如，对于一个美国的数字，它将是**(1) 1234567890**。

    ![][9]

6. 下面的示例中所示，然后应带有大量验证收到文本消息︰

    ![][10]

7. 输入的验证编号**验证码**（**确认代码**） 中的消息。

8. 最后，通过接受 Baidu 协议并单击**提交**（**提交**） 完成开发人员注册。 成功完成注册，您将看到以下页面︰

    ![][11]

##创建 Baidu 云推项目

当创建 Baidu 云推项目时，您收到您的应用程序 ID，API 密钥和密钥。

1. 一旦您已登录到[Baidu 门户]，单击**更多 >>** （**更多**）。

    ![][5]

2. 在**站长与开发者服务**（**站点管理员和开发人员服务**） 部分向下滚动并单击**百度开放云平台**（**Baidu 打开云平台**）。

    ![][6]

3. 在下一页上，单击**开发者服务**（**开发商服务**） 上的右上角。

    ![][7]

4. 在下一页上，单击**云推送**（**云推**）**云服务**（**云服务**） 部分。

    ![][12]

5. 一旦您注册开发人员，您将看到**管理控制台**（**管理控制台**） 在顶部的菜单。 单击**开发者服务管理**（**开发人员服务管理**）。

    ![][13]

6. 在下一页上，单击**创建工程**（**创建项目**）。

    ![][14]

7. 输入应用程序名称，然后单击**创建**（**创建**）。

    ![][15]

8. 在 Baidu 云推项目的成功创建，您会看到页面**应用程序标识**、 **API 密钥**和**密钥**。 记下的 API 密钥和私钥，我们将稍后使用。

    ![][16]

9. 通过单击**云推送**（**云推**） 左侧窗格中配置推式通知的项目。

    ![][31]

10. 在下一页上，单击**推送设置**（**推设置**） 按钮。

    ![][32]  

11. 在配置页中，添加包名，您将能使用 Android 项目**应用包名**（**应用程序包**） 字段中，然后单击**保存设置**（**保存**）。  

    ![][33]

您将看到**保存成功 ！** (**已成功保存 ！**) 消息。

##配置通知中心

1. 登录到[Azure 门户]，然后单击**+ 新建**在屏幕的底部。

2. 单击**服务应用程序**，单击**服务总线**，**通知集线器**中，请单击，然后单击**快速创建**。

3. 为您的**通知中心**提供一个名称，然后选择**区域**和**Namespace**将创建此通知中心的位置，然后单击**创建新的通知中心**。  

    ![][17]

4. 单击在其中创建通知网络集线器，命名空间，然后在顶部单击**通知集线器**。

    ![][18]

5. 选择通知中心创建，然后再从顶部的菜单中单击**配置**。

    ![][19]

6. 向下滚动到**baidu 通知设置**部分，输入 API 密钥和密钥从获得 Baidu 控制台以前 Baidu 云推项目。 单击**保存**。

    ![][20]

7. 单击通知中心，中心的顶部的**仪表板**选项卡，然后单击**视图连接字符串**。

    ![][21]

8. 记下的**DefaultListenSharedAccessSignature**和**DefaultFullSharedAccessSignature**从**访问连接信息**窗口。

    ![][22]

##将您的应用程序连接到通知中心

1. 日蚀式 ADT 在创建一个新的 Android 项目 (**文件** > **New** > **Android 应用程序项目**)。

    ![][23]

2. 输入**应用程序名称**并确保，**最少需要 SDK**版本设置为**API 16: Android 4.1**。

    ![][24]

3. 单击**下一步**，继续按照向导，直到**创建活动**窗口出现。 请确保选择了**空的活动**，并最后选择**完成**以创建新的 Android 应用程序。

    ![][25]

4. 请确保已正确设置了**项目生成目标**。

    ![][26]

5. 下载和解压缩[移动服务 Android SDK]、 打开**notificationhubs**文件夹，将**通知-集线器-x.y.jar**文件复制到您的 Eclipse 项目的**库**文件夹和刷新*库*文件夹。

6. 下载和解压缩[Baidu 推 Android SDK]，打开**库**文件夹，然后复制**pushservice x.y.z** jar 文件和**armeabi** & **mips** Android 应用程序的**库**文件夹中的文件夹。

7. 打开您的 Android 项目的**AndroidManifest.xml**文件并添加 Baidu SDK 所需的权限。

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. 将**android: name**属性添加到您的**应用程序**元素在**AndroidManifest.xml**，替换*yourprojectname* (例如， **com.example.BaiduTest**)。 请确保此项目名称与您在 Baidu 控制台中配置一个。

        <application android:name="yourprojectname.DemoApplication"

9. 添加以下配置应用程序元素内的后**。MainActivity**活动元素，替换*yourprojectname* (例如， **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. 添加到项目名为**ConfigurationSettings.java**的新类。

    ![][28]

    ![][29]

10. 将以下代码添加到它︰

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    设置**API_KEY**的值与您从 Baidu 云项目以前，您从 Azure 门户的通知中心名称与**NotificationHubName**和**NotificationHubConnectionString**与 DefaultListenSharedAccessSignature 从 Azure 门户的检索。

11. 添加一个新类，称为**DemoApplication.java**，并给它添加以下代码︰

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. 添加另一个新类，称为**MyPushMessageReceiver.java**，并将下面的代码添加到它。 这是处理从 Baidu 强制服务器接收到推式通知的类。

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. 打开**MainActivity.java**，并将以下内容添加到**onCreate**方法︰

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. 打开顶部的以下导入语句︰

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##将通知发送到您的应用程序

您可以从使用任何后端使用 Azure 通知集线器发送通知我们<a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST 接口</a>。 在本教程中，我们展示它使用.NET 控制台应用程序。

1. 创建一个新 C# 控制台应用程序︰

    ![][30]

2. 将引用添加到带有 Azure 服务总线 SDK <a href="http://nuget.org/packages/WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet 程序包</a>。 在 Visual Studio 主菜单中，单击**工具**，单击**库程序包管理器**，然后单击**程序包管理器控制台**。 然后，在控制台窗口中，键入以下命令并按 enter 键︰

        Install-Package WindowsAzure.ServiceBus

3. 打开**Program.cs**文件，然后添加以下 using 语句︰

        using Microsoft.ServiceBus.Notifications;

4. 在您`Program`类中添加以下方法， *DefaultFullSharedAccessSignatureSASConnectionString*和*NotificationHubName*替换为您所具有的值。

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. 在**Main**方法中添加以下行︰

         SendNotificationAsync();
         Console.ReadLine();

##测试您的应用程序

若要测试此应用程序中使用实际的电话，只需将手机通过 USB 电缆连接到计算机中。 这样会加载您的应用程序传递到连接的电话。

若要测试此应用程序的模拟器，在 Eclipse 顶部的工具栏上，单击**运行**，然后选择应用程序。 此启动仿真程序，然后加载和运行该应用程序。

该应用程序从 Baidu 推送通知服务中检索用户 Id 和 channelId，并通知中心注册。

使用.NET 控制台应用程序时，将发送测试通知，只是在 Visual Studio 以运行该应用程序中按 F5 键。 该应用程序将发送您的设备或仿真程序上的通知区域中将显示一个通知。


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[移动服务 Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu 推 Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure 门户]: https://manage.windowsazure.com/
[Baidu 门户]: http://www.baidu.com/
