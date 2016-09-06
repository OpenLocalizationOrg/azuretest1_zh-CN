---
ms.openlocfilehash: fc3d36f962f0b5098b4cd00c5270314e8b2cc69b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="安全的 azure 通知集线器推"
    description="了解如何从 Azure 将安全推式通知发送给一个 Android 的应用程序。 在 Java 和 C# 中编写的代码示例。"
    documentationCenter="android"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/16/2015" 
    ms.author="wesmc"/>

#安全的 azure 通知集线器推

> [AZURE.SELECTOR]
- [Windows 世界](notification-hubs-aspnet-backend-windows-dotnet-secure-push.md)
- [iOS](notification-hubs-aspnet-backend-ios-secure-push.md)
- [Android](notification-hubs-aspnet-backend-android-secure-push.md)

##概述

在 Microsoft Azure 支持推送通知使您可以访问一个易于使用、 多平台和向外扩展的推的基础结构，极大地简化了使用者和企业应用程序的移动平台的推式通知的实现。

法规或安全约束，有时应用程序可能需要在无法通过标准推式通知基础架构传输通知中包括的内容。 本教程介绍了如何通过发送敏感信息通过安全、 经身份验证的客户端设备和应用程序的后端之间连接来获得相同的体验。

在较高级别流程如下所示︰

1. 应用程序后端︰
    - 在后端数据库中的存储安全有效负载。
    - 此通知的 ID 将发送到设备 （安全信息不会发送）。
2. 该应用程序在设备上，在接收到此通知时︰
    - 设备联系人后端请求安全有效负载。
    - 应用程序可以显示为通知设备上的负载。

值得注意的是，上述流中 （并在本教程中），我们假定用户登录后，设备将在本地存储中中, 存储身份验证令牌。 这可以保证完全无缝的体验，因为该设备可以检索通知使用此令牌的安全有效负载。 如果您的应用程序不会存储在设备上，身份验证令牌，或这些标记可以到期，设备应用程序中的，在接收到此通知时应显示一般通知提示用户启动该应用程序。 应用程序对用户进行身份验证，然后显示通知负载。

本安全推教程演示如何安全地发送推式通知。 本教程基于**通知用户**教程，所以应完成的步骤，该教程中第一次。

> [AZURE.NOTE] 本教程假设您已经创建并配置通知中心[入门通知集线器 (Android)](notification-hubs-android-get-started.md)中所述。

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## 修改 Android 项目

现在，您可以修改您的应用程序后端发送只是*id*的通知，您需要更改您的 Android 应用程序来处理该通知以及回拨您后端来检索要显示安全消息。
为了实现这一目标，必须确保您的 Android 应用程序知道如何验证自己的身份与后端接收推式通知时。

我们现在要共享您的应用程序的首选项中保存身份验证标头值修改*登录*流程。 类似的机制可以用于存储该应用程序将具有用户凭据的情况下使用任何身份验证令牌 （例如 OAuth 令牌）。

1. 在 Android 应用程序项目中， **MainActivity**类的顶部添加下列常量︰

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. 仍在**MainActivity**类中，更新`getAuthorizationHeader()`方法包含以下代码︰

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. 添加以下`import` **MainActivity**文件顶部的语句︰

        import android.content.SharedPreferences;

现在，我们将更改通知收到时，将调用该处理程序。

4. 在**MyHandler**类更改`OnReceive()`方法包含︰

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. 然后添加`retrieveNotification()`方法，替换占位符`{back-end endpoint}`与后端端点部署后端时获取︰

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


此方法调用您的应用程序后端检索使用凭据存储在共享首选项中的通知内容，并将其显示为正常的通知。 通知看起来完全像任何其他推式通知的应用程序用户。

请注意，最好由后端处理缺少身份验证标头属性或拒绝的情况。 这种情况下的特定处理大部分依赖于您的目标用户体验。 一个选项是显示一个通知，通知一般提示用户进行身份验证以检索实际的通知。

## 运行应用程序

要运行该应用程序，请执行以下操作︰

1. 请确保**AppBackend**被部署在 Azure。 如果使用 Visual Studio，运行**AppBackend** Web API 应用程序。 将显示 ASP.NET web 页。

2. 日蚀式，Android 的物理设备或仿真程序上运行应用程序。

3. 在 Android 应用程序用户界面中，输入用户名和密码。 这些可以是任何字符串，但它们必须是相同的值。

4. 在 Android 应用程序用户界面中，单击**登录**。 然后单击**发送推**。
