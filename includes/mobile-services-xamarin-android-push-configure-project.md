---
ms.openlocfilehash: ccd88e60e44f21e4ad5cb1c8ad749627f959ded3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 在解决方案视图中，展开**组件**文件夹中的 Xamarin.Android 应用程序，请确保已安装该 Azure 移动服务包。 

2. 用鼠标右键单击**组件**文件夹单击**获取更多组件...**、 搜索**Google 云消息传递客户端**组件，将其添加到项目中。 

1. 打开 ToDoActivity.cs 项目文件，然后添加以下使用类的语句︰

        using Gcm.Client;

2. 在**ToDoActivity**类中，添加以下新的代码︰ 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    这使您可以访问服务过程中的移动服务客户端实例。

3. 更改现有的移动服务客户端声明为公共的如下所示︰

        public MobileServiceClient client { get; private set; }

4.  **MobileServiceClient**创建后， **OnCreate**方法中，添加以下代码︰

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

**ToDoActivity**您现在准备添加推式通知。