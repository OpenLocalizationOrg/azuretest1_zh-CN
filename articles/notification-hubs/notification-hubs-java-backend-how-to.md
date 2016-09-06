---
ms.openlocfilehash: a1d9d060cffb29d3491d8e8425ed131f43e690bd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 Java 通知集线器" 
    description="了解如何从一个后端的 Java 使用 Azure 通知集线器。" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="java" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="07/17/2015" 
    ms.author="yuaxu"/>

# 如何使用 Java 从通知集线器
> [AZURE.SELECTOR] 
- [Java](notification-hubs-php-backend-how-to.md)
- [PHP](notification-hubs-python-backend-how-to.md)
- [Python](notification-hubs-nodejs-how-to-use-notification-hubs.md)
- [Node.js](notification-hubs-nodejs-how-to-use-notification-hubs.md)
        
本主题介绍新完全受支持官方 Azure 通知中心 Java SDK 的主要的功能。 这是一个开放源码项目，您可以查看整个 SDK 代码在[Java SDK]。 

一般情况下，您可以访问所有通知集线器功能从 Java/PHP/Python/拼音后端使用通知中心 REST 接口[通知集线器 REST Api](http://msdn.microsoft.com/library/dn223264.aspx)的 MSDN 主题中所述。 此 Java SDK 提供的瘦包装这些 Java 中的其他接口。 

当前支持的 SDK:

- 在通知集线器上的 CRUD 
- 对登记的 CRUD
- 安装管理
- 导入/导出登记
- 定期发送
- 定时的发送
- 通过 Java NIO 的异步操作
- 支持的平台︰ APNS (iOS)，GCM (Android)，WNS （Windows 应用商店应用程序）、 MPNS (Windows Phone)、 ADM （Amazon Kindle 火灾），Baidu (Android 没有 Google 服务) 

## SDK 用法

### 编译和构建

使用[Maven]

生成︰

    mvn package

## 代码

### 通知中心 CRUDs

**创建 NamespaceManager:**
    
    NamespaceManager namespaceManager = new NamespaceManager("connection string")

**创建通知中心︰**
    
    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);
    
 运算，在指定数据点中设置指定位数。

    hub = new NotificationHub("connection string", "hubname");

**获取通知中心︰**
    
    hub = namespaceManager.getNotificationHub("hubname");

**更新通知中心︰**
    
    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

**删除通知中心︰**
    
    namespaceManager.deleteNotificationHub("hubname");

### 注册 CRUDs
**创建通知中心客户机︰**

    hub = new NotificationHub("connection string", "hubname");

**创建 Windows 注册︰**

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

**创建 iOS 登记︰**

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

同样您可以登记为 Android (GCM)、 Windows Phone (MPNS) 和 Kindle 火灾 (ADM)。

**创建模板的登记︰**

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

**创建登记 registrationid + upsert 使用创建模式**

如果将注册 id 存储在该设备上，移除重复项由于任何丢失的响应︰

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

**更新注册︰**
    
    hub.updateRegistration(reg);

**删除登记︰**
    
    hub.deleteRegistration(regid);

**查询登记︰**

*   **获取单个登记︰**
    
        hub.getRegistration(regid);
    
*   **进入中心的所有登记︰**
    
        hub.getRegistrations();
    
*   **获取带标记的登记︰**
    
        hub.getRegistrationsByTag("myTag");
    
*   **获取通道登记︰**
    
        hub.getRegistrationsByChannel("devicetoken");

集合中的所有查询都支持 $top 和延续标记。

### 安装 API 的使用情况
安装 API 进行的注册管理替代机制。 而维护多个登记，这不是一件小事和错误的或效率低下可能太容易做到，不是现在可以使用单个安装对象。 安装包含您所需要的一切︰ 力推通道 （设备标记）、 标记、 模板、 辅助磁贴 （WNS 和 APNS）。 您不需要调用服务不再获取 Id-只需生成 GUID 或任何其他标识符，保留设备上将发送给您的后端推入通道 （设备标记） 一起。 在后端上只办一次调用︰ CreateOrUpdateInstallation，它完全是幂等，不妨尝试如果需要。

Amazon 的 Kindle 火如它如下所示︰

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

如果您想要对其进行更新︰ 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

对于高级方案，我们有部分更新功能，它允许修改仅特定对象属性的安装。 从根本上说部分更新是您可以运行安装对象的 JSON 修补程序操作的子集。

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

删除安装︰

    hub.deleteInstallation(installation.getInstallationId());

CreateOrUpdate、 修补程序和删除是获得与最终一致。 请求的操作只需在调用期间转到系统队列，并将在后台执行。 请注意，获取不是对于主运行时方案，但只是用于调试和故障排除，它严格限制服务。

对于安装的发送流是登记一样。 我们只是推出了一个选项，以目标为特定安装的通知-只需使用标签"InstallationId: {需要 id}"。 对于上面的情况如下所示︰

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

几个模板之一︰

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### 计划 （适用于标准层） 的通知

同作为定期发送，但一个附加的参数 scheduledTime 表示应该时发送通知。 服务接受任何点之间现在 + 5 分钟的时间，现在 + 7 天。

**安排窗口本机通知︰**

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### 导入/导出 （用于标准层）
有时它需要执行批量操作，根据登记。 通常这是为了与其他系统集成或只说大规模修复更新标记。 强烈不建议使用 Get/更新流，如果我们还谈论数千个登记。 导入/导出功能旨在涵盖方案。 基本上您提供访问某些 blob 容器存储帐户下为传入的数据和输出位置的源。

**提交导出作业︰**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


**提交导入作业︰**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

**等待，直到工作已完成︰**

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

**获取所有作业︰**

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

**使用 SAS 签名的 URI:**这是一些斑点文件或 blob 容器的 URL 和组的权限和过期时间等参数，再加上签名使用帐户的 SAS 键所做的所有这一切。 Azure 存储 Java SDK 包含丰富的功能，包括创建这种类型的 Uri。 作为简单的替代方法，您可以看 ImportExportE2E 测试类 （从 github 的位置） 有非常基本和压缩实现签名算法。

###发送通知
通知对象只是标题与正文，某些实用工具方法帮助生成本机枚举和模板通知对象。

* **Windows 应用商店和 Windows Phone 8.1 (非 Silverlight)**

        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);

* **iOS**

        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);

* **Android**

        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);

* **Windows Phone 8.0 和 8.1 Silverlight**

        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);

* **Kindle 火灾**

        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);

* **将发送到标记**

        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);

* **将发送到标记表达式**       

        hub.sendNotification(n, "foo && ! bar");

* **发送通知模板**

        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

运行 Java 代码现在应该生成显示在目标设备上的通知。

##<a name="next-steps"></a>下一步行动
本主题介绍如何创建一个简单的 Java REST 客户端通知集线器的。 从这里，您可以︰

* 下载完整[Java SDK]，其中包含完整的 SDK 代码。 
* 播放的示例︰
    - [开始使用通知集线器]
    - [发送的新闻]
    - [本地化的发送的新闻]
    - [将通知发送到身份验证的用户]
    - [将跨平台通知发送到身份验证的用户]

[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[获取启动的教程]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[开始使用通知集线器]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/
[发送的新闻]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/
[本地化的发送的新闻]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/
[将通知发送到身份验证的用户]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/
[将跨平台通知发送到身份验证的用户]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/
[Maven]: http://maven.apache.org/
 
