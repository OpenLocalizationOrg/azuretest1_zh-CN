---
ms.openlocfilehash: 2f4b955828a49ab9f6d2e2641196731882891601
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="通知集线器爆炸性新闻教程-Android"
    description="了解如何使用 Azure 服务总线通知集线器将重大新闻通知发送到 Android 设备。"
    services="notification-hubs"
    documentationCenter="android"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="wesmc"/>


# 使用通知集线器发送的新闻

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##概述

本主题演示如何使用 Azure 通知集线器广播重大新闻通知到 Android 的应用程序。 完成后，将能够注册的重大新闻类别感兴趣，并在收到这些类别只推式通知。 这种情况下是许多应用程序的常见模式通知需要发送到以前已声明它们，例如 RSS 阅读器、 音乐风扇等应用程序感兴趣的用户组。

通过包含一个或多个_标记_，通知中心中创建登记时启用了广播的方案。 当通知被发送到一个标记时，所有已注册标记的设备将接收到通告。 标记是简单的字符串，因为它们不需要预先设置。 有关标记的详细信息，请参阅[通知集线器指南]。


##先决条件

本主题基于[入门通知集线器][入门]在创建应用程序。 之前开始学习本教程，您必须已完成[通知集线器开始][入门的]。

##向应用程序添加类别选择

第一步是将 UI 元素添加到您现有的主要活动，使用户能够选择要注册的类别。 由用户选择的类别存储在该设备中。 当应用程序启动时，设备注册中创建与所选的类别通知网络集线器作为标记。

1. 打开您的 res/layout/activity_main.xml 文件，并替换为以下内容︰

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. 打开 res/values/strings.xml 文件，然后添加以下行︰

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    Main_activity.xml 图形布局现在应该如下所示︰

    ![][A1]

3. 现在在与**MainActivity**类相同的包中创建类**的通知**。

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId) {
                this.context = context;
                this.senderId = senderId;

                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(<hub name>, <connection string with listen access>, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
                            hub.register(regid, categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }

                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    此类使用本地存储区来存储此设备具有接收新闻的类别。 它还包含用于注册这些类别的方法。

4. 在上面的代码中，替换`<hub name>`，`<connection string with listen access>`与您通知中心名称和之前获得的*DefaultListenSharedAccessSignature*的连接字符串的占位符。

    > [AZURE.NOTE] 分布式客户端应用程序使用的凭据不是通常的安全，因为您只应该与您的客户端应用程序分发听访问的键。 倾听 access 将启用不能修改您的应用程序的通知，但现有登记注册并不能发送通知。 完全访问键用于发送通知和更改现有登记的受保护的后端服务。

4. 在**MainActivity**类中为**NotificationHub**和**GoogleCloudMessaging**，删除您的私有字段并将字段添加为**通知**︰

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. 然后，在**onCreate**方法中，删除初始化**集线器**域和**registerWithNotificationHubs**方法。 然后添加以下各行的初始化**通知**类的一个实例。 该方法应包含以下行︰

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID);
        }

6. 然后，添加下面的方法︰

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    此方法创建一个类别列表，并使用**通知**类来存储本地存储区中的列表并通知中心注册相应的标记。 更改类别，注册新类别重新创建。

您的应用程序现在就可以在设备上的本地存储区中存储一组类别和注册通知中心，每当用户更改所选内容的类别。

##要通过注册获取通知

这些步骤注册通知中心在启动时将使用已存储本地存储区中的类别。

> [AZURE.NOTE] 因为随时可以更改分配由 Google 云消息 (GCM) registrationId，您应注册通知经常以避免通知失败。 本示例注册通知每个应用程序启动的时间。 为经常运行的应用程序，不止一次，每天可以可能跳过注册以节省带宽，如果以前的注册起已过了不少于一天。

1. 将以下代码添加到**通知**类︰

        public Set<String> retrieveCategories() {
            SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
            return settings.getStringSet("categories", new HashSet<String>());
        }

    此方法返回的类中定义的类别。

2. 现在在**MainActivity**类中的**onCreate**方法末尾添加以下代码︰

        notifications.subscribeToCategories(notifications.retrieveCategories());

    这将确保，每次启动应用程序时，它从本地存储区中检索类别，并请求这些类别的注册。 **InitNotificationsAsync**方法创建的一部分 [入门通知集线器] 教程，但它不需要本主题中。

3. 然后将下面的方法添加到**MainActivity**中︰

        @Override
        protected void onStart() {
            super.onStart();

            Set<String> categories = notifications.retrieveCategories();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            world.setChecked(categories.contains("world"));
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            politics.setChecked(categories.contains("politics"));
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            business.setChecked(categories.contains("business"));
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            technology.setChecked(categories.contains("technology"));
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            science.setChecked(categories.contains("science"));
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            sports.setChecked(categories.contains("sports"));
        }

    这将更新基于状态的以前保存的类别的主要活动。

应用程序现已完成，并可以用于每当用户更改的类别选择注册通知中心设备本地存储区中存储一组类别。 接下来，我们将定义后端，可以将类别通知发送到这个应用程序。

##从后端发送通知

[AZURE.INCLUDE [notification-hubs-back-end](../../includes/notification-hubs-back-end.md)]

##运行应用程序并生成通知

1. 在 Eclipse 中，生成应用程序并在设备或仿真程序上启动它。

    注意应用程序用户界面提供了一套切换，您可以选择要订阅的类别。

2. 启用一个或多个类别切换，然后单击**订阅**。

    应用程序转换为标记的所选的类别和从通知中心请求为选定的标记新的设备注册。 注册的类别返回并显示在一个对话框中。

4. 通过以下方式之一从后端发送新的通知︰

    + **.NET 控制台应用程序︰**启动控制台应用程序。

    + **Java/PHP:**运行您的应用程序/脚本。

    通知所选的类别的好象 toast 通知。

##下一步行动

在本教程中我们学习了如何按类别广播的新闻。 完成下面的教程，突出显示其他高级的通知集线器方案之一，请考虑︰

+ [使用通知集线器以本地化的新闻广播]

    了解如何展开重大新闻应用程序启用本地化的发送通知。

+ [通知用户使用通知集线器]

    了解如何对特定的已验证身份用户推式通知。 这是一个不错的解决方案，只能向特定用户发送通知。




<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[入门-]: notification-hubs-android-get-started.md
[使用通知集线器以本地化的新闻广播]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[通知用户使用通知集线器]: /manage/services/notification-hubs/notify-users
[移动服务]: /develop/mobile/tutorials/get-started/
[通知集线器指南]: http://msdn.microsoft.com/library/jj927170.aspx
[为 Windows 应用商店的通知集线器操作方法]: http://msdn.microsoft.com/library/jj927172.aspx
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Azure 的管理门户]: https://manage.windowsazure.com/
[wns 对象]: http://go.microsoft.com/fwlink/p/?LinkId=260591
