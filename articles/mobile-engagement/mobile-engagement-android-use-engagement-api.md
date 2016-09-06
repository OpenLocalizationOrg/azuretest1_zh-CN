---
ms.openlocfilehash: cda6ca748be30021e845f4adab4358568afe463a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何在 Android 上使用服务 API" 
    description="最新的 Android SDK-如何在 Android 上使用服务 API"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="piyushjo" />

#如何在 Android 上使用服务 API

本文档是对文档[如何在 Android 上的集成服务](mobile-engagement-android-integrate-engagement.md)的附加补充。 它提供了有关如何使用服务 API 报告您的应用程序统计信息的深度详细信息中。

请记住，如果您只需要合作报告应用程序的会话、 活动、 系统崩溃和技术信息，则最简单的方法是使所有您`Activity`子类继承相应的`EngagementActivity`类。

如果您想要做更多，例如，如果您需要报表应用程序特定事件、 错误和作业，或如果您要以不同的方式报告应用程序的活动，不是在一个实现`EngagementActivity`类，则需要使用服务 API。

服务 API 提供的`EngagementAgent`类。 此类的实例可以通过调用来检索`EngagementAgent.getInstance(Context)`静态方法 (请注意，`EngagementAgent`返回的对象是一个单一实例)。

##交往概念

下列部件优化常见的[移动合作概念](mobile-engagement-concepts.md)，为 Android 平台。

### `Session`  和  `Activity`

如果用户保持不超过几秒闲置的两项*活动*，在两个不同*的会话*中拆分他一连串*的活动*。 这些几秒钟被称为"会话超时"。

*活动*是通常与一个屏幕的应用程序，也就是说在*活动*启动时屏幕显示和屏幕关闭时停止︰ 使用集成服务 SDK 时，这种情况`EngagementActivity`类。

但*活动*还能受手动使用服务 API。 这样就可以拆分给的屏幕中若干子部分，以获取更多详细信息 （例如对已知频率和多长时间在此屏幕内部使用对话） 此屏幕的用法。

##报告的活动

> [AZURE.IMPORTANT] 您无需的报告活动如本部分中介绍，如果您使用的`EngagementActivity`类，如 Android 文档所述如何集成服务及其变种。

### 用户启动一个新的活动

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

您需要调用`startActivity()`每次用户活动的更改。 第一次调用此函数启动一个新的用户会话。

调用此函数的最佳时机是在每个活动`onResume`回调。

### 用户结束他当前活动

            EngagementAgent.getInstance(this).endActivity();

您需要调用`endActivity()`至少一次当用户完成他最后一次活动。 这将通知该用户是当前处于空闲状态，并将用户会话需要关闭一次会话超时接洽 SDK (如果调用`startActivity()`会话超时时间到期之前，只需恢复会话)。

调用此函数的最佳时机是在每个活动`onPause`回调。

##报告事件

### 会话事件

会话事件通常用来报告他的会话期间由用户执行的操作。

**而无需额外数据的示例︰**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**包含额外数据的示例︰**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### 独立事件

会话事件与独立事件可以发生在一个会话上下文的外部。

**示例：**

假设您要报告事件发生时触发广播的接收器︰

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##报告错误

### 会话错误

会话错误通常用来报告他的会话过程中影响用户的错误。

**示例：**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### 独立的错误

会话错误与独立错误可能发生在一个会话上下文的外部。

**示例：**

下面的示例演示如何只要内存变得低在手机上运行您的应用程序进程时报告错误。

            public MyApplication extends EngagementApplication {
            
              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##报告作业

### 示例

假设您要报告您的登录过程的持续时间︰
            
            [...]
            public void signIn(Context context, ...) {
            
              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);
            
              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);
            
              [... sign in ...]
            
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### 在作业过程中报告错误

可以与正在运行的作业，而不是正相关的当前用户会话相关的错误。

**示例：**

假设您要在您错误登录进程的报告︰

[...]公共 void 登录 （上下文上下文，...）{

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);
            
              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);
            
              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);
            
                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### 在作业过程中报告的事件

可以与正在运行的作业，而不是正相关的当前用户会话相关的事件。

**示例：**

假设我们有一个社交网络，并且我们使用报告作业的总时间，在此期间用户连接到服务器。 用户可以保持连接在背景甚至他使用另一个应用程序时，或者当手机处于睡眠状态，所以没有会话。

用户可以接收来自他朋友的邮件，这是一个作业事件。
            
            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##额外的参数

可以将任意数据附加到事件、 错误、 活动和作业。

此数据可能结构化，它会使用 Android 的捆绑包类 （实际上，它的工作方式像 Android 的方法中的额外参数）。 请注意，一个包可以包含数组或另一个软件包实例。

> [AZURE.IMPORTANT] 如果放在 parcelable 或可序列化参数时，请确保其`toString()`方法实现返回的可读字符串。 可序列化包含非瞬态字段不是可序列化的类将使 Android 崩溃时将调用 `bundle.putSerializable("key",value);`

> [AZURE.WARNING] 不支持稀疏数组中额外的参数，即它不会被序列化数组形式。 您应该将它们转换为标准数组之前在额外的参数中使用它。

### 示例

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### 限制

#### 密钥

在每个键`Bundle`必须匹配以下正则表达式︰

`^[a-zA-Z][a-zA-Z_0-9]*`

这意味着至少一个字母后, 跟字母、 数字或下划线，必须从开始键 (\_)。

#### 大小

显示额外内容被限制为**1024年**个字符，每个调用 （一次编码在 JSON 中交往服务）。

在前面的示例中，发送到服务器的 JSON 是 58 个字符︰

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##报告应用程序信息

您可以手动报告跟踪信息 （或任何其他应用程序特定信息） 使用`sendAppInfo()`函数。

请注意，可以将这些信息发送增量︰ 只给定键的最新值将保存对某个给定设备。

类似事件显示额外内容，包类用于抽象应用程序的信息，请注意数组或子束会认为是简单的字符串 （使用 JSON 序列化）。

### 示例

下面是一个代码示例发送用户的性别和出生日期︰

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### 限制

#### 密钥

在每个键`Bundle`必须匹配以下正则表达式︰

`^[a-zA-Z][a-zA-Z_0-9]*`

这意味着至少一个字母后, 跟字母、 数字或下划线，必须从开始键 (\_)。

#### 大小

应用程序信息被限制为**1024年**个字符，每个调用 （一次编码在 JSON 中交往服务）。

在前面的示例中，发送到服务器的 JSON 是 44 个字符︰

            {"expiration":"2016-12-07","status":"premium"}
 

测试
