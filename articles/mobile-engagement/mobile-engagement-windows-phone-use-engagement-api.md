---
ms.openlocfilehash: 3bad7901dd8617a755a59cbaf7dbf0bd36fb5d2d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何在 Windows Phone Silverlight 上使用服务 API" 
    description="如何在 Windows Phone Silverlight 上使用服务 API"    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" /> 

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/07/2015" 
    ms.author="piyushjo" />

#如何在 Windows Phone Silverlight 上使用服务 API

本文档是对文档[如何集成 Windows Phone Silverlight 应用程序中移动服务](../mobile-engagement-windows-phone-integrate-engagement/)的附加补充。 它提供了有关如何使用服务 API 报告您的应用程序统计信息的深度详细信息中。

如果您只需要合作报告应用程序的会话、 活动、 系统崩溃和技术信息，那么最简单的方法是使所有您`PhoneApplicationPage`子类继承`EngagementPage`类。

如果您想要做更多，例如，如果您需要报表应用程序特定事件、 错误和作业，或如果您要以不同的方式报告应用程序的活动，不是在一个实现`EngagementPage`类，则需要使用服务 API。

服务 API 提供的`EngagementAgent`类。 通过这些方法可以访问`EngagementAgent.Instance`。

即使尚未初始化代理模块，每个 api 调用延迟，并且代理可用时将再次执行。

##交往概念

下列部件优化 Windows Phone 平台移动合作概念。

### `Session`  和  `Activity`

*活动*是通常与一个网页的应用程序，也就是说在*活动*启动时显示页，并在关闭网页时停止︰ 使用集成服务 SDK 时，这种情况`EngagementPage`类。

但*活动*还能受手动使用服务 API。 这样就可以拆分给定的页面中若干子部分，以获取有关此页 （例如已知频率和对话在此页中的使用多长时间） 的使用情况的更多详细信息。

##报告的活动

### 用户启动一个新的活动

#### 参考

            void StartActivity(string name, Dictionary<object, object> extras = null)

您需要调用`StartActivity()`每次用户活动的更改。 第一次调用此函数启动一个新的用户会话。

> [AZURE.IMPORTANT] SDK 应用程序关闭时将自动调用 EndActivity 方法。 因此，强烈建议每当活动的用户的更改，并从不调用 EndActivity 方法中，调用此方法会强制当前会话结束后调用 StartActivity 方法。

#### 示例

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### 用户结束他当前活动

#### 参考

            void EndActivity()

您需要调用`EndActivity()`至少一次当用户完成他最后一次活动。 这将通知该用户是当前处于空闲状态，并将用户会话需要关闭一次会话超时接洽 SDK (如果调用`StartActivity()`会话超时时间到期之前，只是延续会话)。

#### 示例

            EngagementAgent.Instance.EndActivity();

##报告作业

### 启动作业

#### 参考

            void StartJob(string name, Dictionary<object, object> extras = null)

您可以使用作业要在一段时间内跟踪 certains 任务。

#### 示例

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### 结束作业

#### 参考

            void EndJob(string name)

一旦任务跟踪作业已终止，应提供作业名称为此作业时，调用 EndJob 方法。

#### 示例

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##报告事件

有三种类型的事件︰

-   独立事件
-   会话事件
-   作业的事件

### 独立事件

#### 参考

            void SendEvent(string name, Dictionary<object, object> extras = null)

独立事件可以发生在一个会话上下文的外部。

#### 示例

            EngagementAgent.Instance.SendEvent("event", extra);

### 会话事件

#### 参考

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

会话事件通常用来报告他的会话期间由用户执行的操作。

#### 示例

**如果没有数据︰**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**数据︰**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### 作业的事件

#### 参考

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

作业的事件通常用来报告作业期间由用户执行的操作。

#### 示例

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##报告错误

有三种类型的错误︰

-   独立的错误
-   会话错误
-   作业错误

### 独立的错误

#### 参考

            void SendError(string name, Dictionary<object, object> extras = null)

会话错误与独立错误可能发生在一个会话上下文的外部。

#### 示例

            EngagementAgent.Instance.SendError("errorName", extras);

### 会话错误

#### 参考

            void SendSessionError(string name, Dictionary<object, object> extras = null)

会话错误通常用来报告他的会话过程中影响用户的错误。

#### 示例

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### 作业错误

#### 参考

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

可以与正在运行的作业，而不是正相关的当前用户会话相关的错误。

#### 示例

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##崩溃报告

该代理提供两个方法来处理故障。

### 发送异常

#### 参考

            void SendCrash(Exception e, bool terminateSession = false)

#### 示例

您可以通过调用发送任何时候异常︰

            EngagementAgent.Instance.SendCrash(aCatchedException);

此外可以使用可选参数以终止合作会话在同一时间比发送崩溃。 若要执行此操作，调用︰

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

如果您这样做时，会话和作业将关闭发送崩溃之后。

### 发送未经处理的异常

#### 参考

            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

服务还提供了发送未经处理的异常的方法。 这是在 silverlight UnhandledException 事件处理程序中使用时特别有用。

此方法将**总是**被调用后终止服务会话和作业。

#### 示例

可用于实现您自己的 UnhandledException 处理程序 （尤其是如果您禁用了自动崩溃报告服务的功能）。 例如，在`Application_UnhandledException`的方法`App.xaml.cs`文件︰

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code
            
              EngagementAgent.Instance.SendCrash(e);
            }

##OnActivated

### 参考

            void OnActivated(ActivatedEventArgs e)

当用户离开向前，一个应用程序之后引发已禁用的事件时，操作系统将尝试将应用程序放入一个不活跃的状态。 然后，应用程序进行逻辑删除。 在此过程中终止应用程序，但保留有关状态的应用程序和应用程序中的各个页面的一些数据。

您必须插入`EngagementAgent.Instance.OnActivated(e)`在`Application_Activated`App.xaml.cs 文件时应用程序已被已逻辑删除重置服务代理中的方法。

### 示例

            // Inside your App.xaml.cs file
            
            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

##设备 Id

            String GetDeviceId()

可以通过调用此方法来获取接洽的设备 id。

##其他参数

可以将任意数据附加到事件、 错误、 活动或作业。 这些数据可以使用字典结构化。 可以是任何类型的键和值。

因此，如果您想要在其他方案中插入您自己的类型需要添加此类型的数据协定，其他方案数据进行序列化。

### 示例

我们创建一个新类"人员"。

            using System.Runtime.Serialization;
            
            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

然后，我们将添加`Person`为一个额外的实例。

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] 如果说其他类型的对象时，请确保实现其 tostring （） 方法返回可读字符串。

### 限制

#### 密钥

在对象中的每个键必须与下面的正则表达式匹配︰

`^[a-zA-Z][a-zA-Z_0-9]*$`

这意味着至少一个字母后, 跟字母、 数字或下划线，必须从开始键 (\_)。

#### 大小

显示额外内容被限制为**1024年**个字符，每个调用。

##报告应用程序信息

### 参考

            void SendAppInfo(Dictionary<object, object> appInfos)

您可以手动报告跟踪信息 （或任何其他应用程序特定信息），它使用 SendAppInfo() 函数。

请注意，可以将这些信息发送增量︰ 只给定键的最新值将保存对某个给定设备。 事件的其他方案，如使用词典\<对象时，对象\>附加信息。

### 示例

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### 限制

#### 密钥

在对象中的每个键必须与下面的正则表达式匹配︰

`^[a-zA-Z][a-zA-Z_0-9]*$`

这意味着至少一个字母后, 跟字母、 数字或下划线，必须从开始键 (\_)。

#### 大小

应用程序信息仅限于每个调用的**1024年**个字符。

在前面的示例中，发送到服务器的 JSON 是 44 个字符︰

            {"subscription":"2013-12-07","premium":"true"}
 

测试
