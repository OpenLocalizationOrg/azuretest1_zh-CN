---
ms.openlocfilehash: a02690c4108592dfe800714a9eeaf579bd6c3390
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 移动接洽 Android SDK 集成" 
    description="最新的更新和 Android SDK Azure 移动服务的步骤"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="piyushjo" />


#升级过程

如果已具有集成我们 SDK 的较旧版本到您的应用程序，您需要升级 SDK 时要考虑以下几点。

您可能需要执行几个步骤，如果错过了几个版本的 SDK。 例如，如果您将从迁移 1.4.0 版到 1.6.0，您必须首先按照"发件人为 1.5.0 1.4.0 版"步骤再"发件人为 1.6.0 1.5.0"过程。

任何版本升级，您必须更换`mobile-engagement-VERSION.jar`用新。

##从 4.1.0 到 4.0.0

SDK 现在句柄新权限的模型从 Android M。

如果您使用位置功能或大图片的通知，请阅读[本节](mobile-engagement-android-integrate-engagement.md#android-m-permissions)。

除了新的权限模型中，我们现在支持在运行时的配置位置功能。
我们仍与位置的清单参数，但现已被否决。 要使用运行时配置，请删除以下各节从您``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

并阅读[此更新的过程](mobile-engagement-android-integrate-engagement.md#location-reporting)改为使用运行时的配置。

##从到 4.0.0 3.0.0

### 本机的推

本机的推 (GCM/ADM) 现在也可以用来在应用程序通知因此必须配置推入市场的任何类型的本机推凭据。

如果尚未完成请按照[此过程](mobile-engagement-android-integrate-engagement-reach.md#native-push)。

### AndroidManifest.xml

已修改范围集成``AndroidManifest.xml``。

将以下内容︰

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

通过

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

有可能是加载屏幕现在当您单击 （与文本/web 内容) 公告或投票。
您必须添加此为这些活动在 4.0.0 工作︰

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### 资源

嵌入的新`res/layout/engagement_loading.xml`文件到您的项目。

##从 2.4.0 为 3.0.0

下面介绍如何从 Azure 移动合作由应用程序提供的 Capptain SA Capptain 服务迁移 SDK 集成。 如果要从早期版本迁移，请参考 Capptain 网站，将第一次迁移到 2.4.0，然后再执行以下步骤。

>[AZURE.IMPORTANT] Capptain 和移动签约不是相同的服务，并给出下面的过程只重点介绍如何将客户端应用程序的迁移。 迁移的 SDK 应用程序中不会迁移数据从 Capptain 服务器到移动服务的服务器。

### JAR 文件

更换`capptain.jar`的`mobile-engagement-VERSION.jar`在您`libs`文件夹。

### 资源文件

我们提供的每个资源文件 (以前缀`capptain_`) 已被替换为新的 (带有前缀`engagement_`)。

如果您自定义这些文件，您必须重新应用您的新文件，**也已重命名的资源文件中的所有标识符**的自定义。

### 应用程序 ID

现在签约使用连接字符串配置的 SDK 标识符，如应用程序标识符。

您必须使用`EngagementAgent.init`启动此类活动中的方法︰

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

在 Azure 门户网站上显示您的应用程序的连接字符串。

请删除任何调用`CapptainAgent.configure`与`EngagementAgent.init`替换该方法。

`appId`不再可以使用配置`AndroidManifest.xml`。

请删除此部分从您`AndroidManifest.xml`如果您有它︰

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### Java API

每次我们 SDK 的任何 Java 类的调用必须被重命名。例如，`CapptainAgent.getInstance(this)`必须重命名`EngagementAgent.getInstance(this)`，`extends CapptainActivity`必须重命名`extends EngagementActivity`等等...

如果您已使用默认代理首选项文件集成，现在是默认的文件名`engagement.agent`键是`engagement:agent`。

在创建 web 发布时，Javascript 联编程序现在是`engagementReachContent`。

### AndroidManifest.xml

那里发生大量的更改、 服务不共享的和大量的接收器再也不可导出。

在服务声明现在是更简单;目的筛选器和内，所有元数据中删除和添加`exportable=false`。

加上所有内容重命名使用合约。

它现在看起来像︰

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

当您想要启用测试日志时，元数据已被移动到应用程序标记，并已被重命名︰

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

只是已重命名所有其他元数据，这是 （的课程重命名只有您使用的那些） 的完整列表︰

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google 播放和 SmartAd 跟踪已经从 SDK，您只需删除此不更换︰

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

范围活动现在声明如下︰

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
如果您有自定义范围活动，则只需更改以匹配任何一个目的操作`com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT`或`com.microsoft.azure.engagement.reach.intent.action.POLL`。

广播的接收器已被重命名，再加上我们现在添加`exported=false`。 下面是用新的规范 （的课程重命名只有您使用） 接收器的完整列表︰

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

跟踪收件人已被删除，因此您必须删除此部分︰

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

请注意， **EngagementMessageReceiver**的广播接收器实现的声明中的更改`AndroidManifest.xml`。 这是因为要发送和从任意 XMPP 实体删除任意 XMPP 消息的 API 和 API 来发送和接收设备之间的邮件已被删除。 因此，您必须也从**EngagementMessageReceiver**实现删除下列回调︰

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

 和 

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

然后删除在**EngagementAgent**的任何调用︰

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

 和 

            sendXMPPMessage(android.os.Bundle msg)

### Proguard

Proguard 的配置可能会影响通过重新塑造其品牌、 规则现在正如︰

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
测试
