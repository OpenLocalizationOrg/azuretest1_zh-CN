---
ms.openlocfilehash: 8ece4354735a9737a5b6e35cfe28d50237ff1812
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
现在，您的移动服务已准备就绪，可以更新应用程序而不是本地集合中移动服务中存储的项目。 

1. 如果您尚没有[移动服务 Android SDK]，立即下载并展开压缩的文件。

2. 复制`.jar`文件从`mobileservices`到 SDK 文件夹`libs`GetStartedWithData 项目的文件夹。

3. 在 Eclipse 中包资源管理器，右键单击`libs`文件夹中，单击**刷新**，和复制的 jar 文件才会出现

    这将添加到工作区中移动服务 SDK 参考。

4. 打开 AndroidManifest.xml 文件，并添加下面的行，从而使应用程序能够访问在 Azure 中的移动服务。

        <uses-permission android:name="android.permission.INTERNET" />

5. 从包资源管理器，打开 TodoActivity.java 文件位于 com.example.getstartedwithdata 包中，并请取消注释下面的代码行︰ 

        import java.net.MalformedURLException;
        import android.os.AsyncTask;
        import com.google.common.util.concurrent.FutureCallback;
        import com.google.common.util.concurrent.Futures;
        import com.google.common.util.concurrent.ListenableFuture;
        import com.microsoft.windowsazure.mobileservices.MobileServiceClient;
        import com.microsoft.windowsazure.mobileservices.MobileServiceList;
        import com.microsoft.windowsazure.mobileservices.http.NextServiceFilterCallback;
        import com.microsoft.windowsazure.mobileservices.http.ServiceFilter;
        import com.microsoft.windowsazure.mobileservices.http.ServiceFilterRequest;
        import com.microsoft.windowsazure.mobileservices.http.ServiceFilterResponse;
        import com.microsoft.windowsazure.mobileservices.table.MobileServiceTable;

 
6. 注释掉下面的行︰

        import java.util.ArrayList;
        import java.util.List;

7. 我们将删除当前由该应用程序，因此我们可以将其替换为移动服务的内存中列表。 在**ToDoActivity**类中，注释下面的定义现有**toDoItemList**列表的代码行。

        public List<ToDoItem> toDoItemList = new ArrayList<ToDoItem>();

8. 保存该文件，而且该项目将显示生成错误。 其余的三个位置的搜索位置`toDoItemList`变量在使用时和注释所指示的部分。 这完全删除的内存列表。 

9. 现在，我们添加我们的移动服务。 取消注释下面的代码行︰

        private MobileServiceClient mClient;
        private private MobileServiceTable<ToDoItem> mToDoTable;

10. 找到在文件底部的*ProgressFilter*类并取消它。 *MobileServiceClient*运行网络操作时，此类显示一个加载的指标。


11. 在管理门户中，单击**移动服务**，然后单击您刚刚创建的移动服务。

12. 单击**仪表板**选项卡和记下**网站 URL**，然后单击**管理密钥**并记下**应用程序注册表项**。

    ![](./media/download-android-sample-code/mobile-dashboard-tab.png)

    从应用程序代码中访问移动服务时，您将需要这些值。

13. 在**onCreate**方法中，取消注释下面的代码行定义**MobileServiceClient**变量︰

        try {
        // Create the Mobile Service Client instance, using the provided
        // Mobile Service URL and key
            mClient = new MobileServiceClient(
                    "MobileServiceUrl",
                    "AppKey", 
                    this).withFilter(new ProgressFilter());

            // Get the Mobile Service Table instance to use
            mToDoTable = mClient.getTable(ToDoItem.class);
        } catch (MalformedURLException e) {
            createAndShowDialog(new Exception("There was an error creating the Mobile Service. Verify the URL"), "Error");
        }

    这将创建*MobileServiceClient* ，用来访问您的移动服务的一个新实例。 它还创建代理的移动服务中的数据存储到使用*MobileServiceTable*实例。

14. 在上面的代码中，将`MobileServiceUrl`和`AppKey`使用 URL 和应用程序从您的移动服务，按这种顺序键。



15. Uncommment **checkItem**方法的以下行︰

        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    mToDoTable.update(item).get();
                    runOnUiThread(new Runnable() {
                        public void run() {
                            if (item.isComplete()) {
                                mAdapter.remove(item);
                            }
                            refreshItemsFromTable();
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();

    这将项更新发送到移动服务，并从适配器中删除选定的项。
    
16. Uncommment **addItem**方法的以下行︰
    
        // Insert the new item
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    mToDoTable.insert(item).get();
                    if (!item.isComplete()) {
                        runOnUiThread(new Runnable() {
                            public void run() {
                                mAdapter.add(item);
                            }
                        });
                    }
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
        

    此代码创建一个新项并将其插入到表中，移动远程服务中。

18. Uncommment **refreshItemsFromTable**方法的以下行︰

        // Get the items that weren't marked as completed and add them in the adapter
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final MobileServiceList<ToDoItem> result = mToDoTable.where().field("complete").eq(false).execute().get();
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            mAdapter.clear();

                            for (ToDoItem item : result) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();

    此查询移动服务，并返回未标记为已完成的所有项目。 项目将添加到绑定适配器。
        

<!-- URLs. -->
[移动服务 Android SDK]: http://aka.ms/Iajk6q