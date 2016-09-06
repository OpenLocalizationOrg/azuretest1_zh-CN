---
ms.openlocfilehash: 08aa9e3022a10f97e618901f14c62cd2cbe7778f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 在应用程序项目中，打开文件`AndroidManifest.xml`。 在接下来的两个步骤中的代码，将_`**my_app_package**`_app 包装箱中的为您的项目的名称，它的价值是`package`的特性`manifest`标记。 

2. 在现有之后添加以下新权限`uses-permission`元素︰

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE" 
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" /> 
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. 添加以下代码后的`application`开始标记︰ 

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. 下载和解压缩[移动服务 Android SDK]、 打开**通知**文件夹、 将**通知 1.0.1.jar**文件复制到您的 Eclipse 项目的*库*文件夹和刷新*库*文件夹。

    > [AZURE.NOTE] 在文件名末尾的数字可能会改变后续 SDK 发行版。

5.  打开的文件*ToDoItemActivity.java*，并添加下面的导入语句︰

        import com.microsoft.windowsazure.notifications.NotificationsManager;


6. 将下面的私有变量添加到类︰ 替换_`<PROJECT_NUMBER>`_与 Google 到您在前面步骤中的应用程序分配的项目编号︰

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

7. 将*MobileServiceClient*的定义为**公共静态**，从**私有**更改，它就像这样︰

        public static MobileServiceClient mClient;



9. 接下来，我们需要添加一个新的类来处理通知。 在包资源管理器中，右键单击该程序包 (下`src`节点)，单击**新建**，单击**类**。

10. **名称**类型中`MyHandler`，在**超类**类型`com.microsoft.windowsazure.notifications.NotificationsHandler`，然后单击**完成**

    ![](./media/mobile-services-android-get-started-push/mobile-services-android-create-class.png)

    这将创建新的 MyHandler 类。

11. 添加为下面的导入语句`MyHandler`类︰

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;

    
12. 接下来，添加下面的成员`MyHandler`类︰

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        Context ctx;


13. 在`MyHandler`类中，添加以下代码以重写**onRegistered**方法，使用移动服务通知中心注册您的设备。

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);
    
            new AsyncTask<Void, Void, Void>() {
                            
                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId, null);
                        return null;
                    }
                    catch(Exception e) { 
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }



14. 在`MyHandler`类中，添加以下代码以重写**onReceive**方法，使它收到时显示通知。

        @Override
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String nhMessage = bundle.getString("message");
    
            sendNotification(nhMessage);
        }
    
        private void sendNotification(String msg) {
            mNotificationManager = (NotificationManager)
                      ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                  new Intent(ctx, ToDoActivity.class), 0);
    
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


15. 后在 TodoActivity.java 文件中，更新注册的通知处理程序类的*ToDoActivity*类的**onCreate**方法。 请确保将此代码添加*MobileServiceClient*实例化之后。


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    现在，您的应用程序将更新以支持推送通知。

<!-- URLs. -->
[移动服务 Android SDK]: http://aka.ms/Iajk6q
