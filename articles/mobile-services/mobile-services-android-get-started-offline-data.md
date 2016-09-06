---
ms.openlocfilehash: 06e46bbe3b3a43d78e3539fe7600ec6b5e2330b4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="将脱机数据同步添加到您的 Android 的移动服务应用程序 |Microsoft Azure"
    description="了解如何使用 Android 应用程序中的缓存和同步脱机数据到 Azure 移动服务"
    documentationCenter="android"
    authors="RickSaling"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/30/2015"
    ms.author="ricksal"/>

# 添加于 Android 的移动服务应用程序的脱机数据同步

[AZURE.INCLUDE [mobile-services-selector-offline](../../includes/mobile-services-selector-offline.md)]

## 摘要

当移动到某个区域没有服务，或者由于网络问题，移动应用程序可以失去网络连接。 例如，使用远程建筑工地建筑行业应用程序可能需要输入计划到 Azure 稍后同步的数据。 使用 Azure 移动服务脱机同步，您可以继续工作在网络连接丢失时对许多移动应用程序至关重要。 与脱机同步，使用 Azure SQL Server 表中，本地副本，并定期重新同步两个。 

在本教程中，将更新该应用程序从[手机服务快速入门教程]以启用脱机同步，然后通过添加离线数据，同步这些项目与联机数据库，并验证在 Azure 管理门户中所做的更改进行测试的应用程序。

无论您是离线或连接，的随时对数据进行多个更改，可以产生冲突。  将探讨未来教程，说明如何处理同步冲突，您可以在其中选择要接受的更改的版本。 在本教程中，我们假定没有同步冲突和对现有数据所做的任何更改将直接应用于 Azure SQL Server。


## 您需要开始

[AZURE.INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]


## 更新应用程序支持脱机同步

脱机同步与您为从读取和写入*同步表*（使用*IMobileServiceSyncTable*接口），它是您的设备上**光 SQL**数据库的一部分。

要推送和拉入 Azure 移动服务与设备之间的更改，请使用*同步上下文*(*MobileServiceClient.SyncContext*)，它使用本地数据库用来存储本地数据进行初始化。 

1. 添加检查网络连接，通过将此代码添加到*AndroidManifest.xml*文件的权限︰

        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />


2. 将下面的**导入**语句添加到*ToDoActivity.java*中︰

        import java.util.Map;
        
        import android.widget.Toast;
        
        import com.microsoft.windowsazure.mobileservices.table.query.Query; 
        import com.microsoft.windowsazure.mobileservices.table.sync.MobileServiceSyncContext; 
        import com.microsoft.windowsazure.mobileservices.table.sync.MobileServiceSyncTable; 
        import com.microsoft.windowsazure.mobileservices.table.sync.localstore.ColumnDataType; 
        import com.microsoft.windowsazure.mobileservices.table.sync.localstore.SQLiteLocalStore; 

3. 上方的`ToDoActivity`类中，则修改此声明的`mToDoTable`变量从`MobileServiceTable<ToDoItem>`类以`MobileServiceSyncTable<ToDoItem>`类。 

         private MobileServiceSyncTable<ToDoItem> mToDoTable;

    这是您在其中定义的同步表。

4. 以处理的本地存储的初始化`onCreate`方法中，在创建之后添加以下行`MobileServiceClient`实例︰

            // Saves the query which will be used for pulling data
            mPullQuery = mClient.getTable(ToDoItem.class).where().field("complete").eq(false);

            SQLiteLocalStore localStore = new SQLiteLocalStore(mClient.getContext(), "ToDoItem", null, 1);
            SimpleSyncHandler handler = new SimpleSyncHandler();
            MobileServiceSyncContext syncContext = mClient.getSyncContext();

            Map<String, ColumnDataType> tableDefinition = new HashMap<String, ColumnDataType>();
            tableDefinition.put("id", ColumnDataType.String);
            tableDefinition.put("text", ColumnDataType.String);
            tableDefinition.put("complete", ColumnDataType.Boolean);
            tableDefinition.put("__version", ColumnDataType.String);

            localStore.defineTable("ToDoItem", tableDefinition);
            syncContext.initialize(localStore, handler).get();

            // Get the Mobile Service Table instance to use
            mToDoTable = mClient.getSyncTable(ToDoItem.class);

5. 按照上面的代码，在`try`块，额外添加一个`catch`块以下`MalformedURLException`一︰

        } catch (Exception e) {
            Throwable t = e;
            while (t.getCause() != null) {
                    t = t.getCause();
                }
            createAndShowDialog(new Exception("Unknown error: " + t.getMessage()), "Error");
        }

6. 添加此方法以验证网络连接︰

        private boolean isNetworkAvailable() {
            ConnectivityManager connectivityManager
                    = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo activeNetworkInfo = connectivityManager.getActiveNetworkInfo();
            return activeNetworkInfo != null && activeNetworkInfo.isConnected();
        }


7. 添加要同步的本地*SQL 光*存储和 Azure SQL Server 之间此方法︰

        public void syncAsync(){
            if (isNetworkAvailable()) {
                new AsyncTask<Void, Void, Void>() {
    
                    @Override
                    protected Void doInBackground(Void... params) {
                        try {
                            mClient.getSyncContext().push().get();
                            mToDoTable.pull(mPullQuery).get();

                        } catch (Exception exception) {
                            createAndShowDialog(exception, "Error");
                        }
                        return null;
                    }
                }.execute();
            } else {
                Toast.makeText(this, "You are not online, re-sync later!" +
                        "", Toast.LENGTH_LONG).show();
            }
        }


8. 在`onCreate`方法，将此代码添加作为下一步到最后一行，右前对`refreshItemsFromTable`:

            syncAsync();

    这将导致设备上启动同步与 Azure 表。 否则会显示上次脱机内容的本地存储区。


 
9. 更新的代码在`refreshItemsFromTable`方法来使用此查询 (第一行中的代码`try`块):

        final MobileServiceList<ToDoItem> result = mToDoTable.read(mPullQuery).get();

10. 在`onOptionsItemSelected`方法替换内容的`if`，此代码块︰

            syncAsync();
            refreshItemsFromTable();

    在右上角中按**刷新**按钮时，将运行该代码。 这是您同步您的本地存储到 Azure，除了在启动同步的主要方式。 这鼓励批处理同步变化，并考虑到从 Azure 拉是一个成本相对较高的操作效率会高。 此外可以设计这个应用程序在每次更改同步通过添加调用`syncAsync`到`addItem`，`checkItem`方法，如果您的应用程序需要此。


## 测试应用程序

在本节中，将测试与 WiFi 的行为，然后关闭 WiFi 来创建脱机的情况。 

添加数据项时，它们都保存在本地 SQL 光存储，但不是同步到移动服务，直到您按下的**刷新**按钮。 其他应用程序可能会有不同的要求，有关数据需要同步，但出于演示目的，本教程中有用户显式请求它。 

当您按该按钮时，启动一个新的后台任务，和第一个推进本地所做的所有更改存储通过使用同步上下文中，并从 Azure 的所有更改过的数据再拉到本地表。


### 在线测试

允许测试以下方案。

1. 添加一些新的项目在您的设备; 
2. 检查的项目不显示在门户中; 
3. 下一步按**刷新**并验证，然后显示它们。
4. 更改或添加一项在门户中，然后按**刷新**并验证，所做的更改显示在您的设备上。

### 离线测试

<!-- Now if you run the app and tap the refresh button, you should see all the items from the server. At that point you should be able to turn off the networking from the device by placing it in *Airplane Mode*, and continue making changes – the app will work just fine. When it’s time to sync the changes to the server, turn the network back on, and tap the **Refresh** button again.

One thing which is important to point out: if there are pending changes in the local store, a pull operation will first push those changes to the server (so that if there are changes in the same row, the push operation will fail and the application has an opportunity to handle the conflicts appropriately). That means that the push call in the code above isn’t necessarily required, but I think it’s always a good practice to be explicit about what the code is doing.
-->

1. 在*飞机模式下*放置设备或模拟器。 这将创建脱机的情况。

2. 添加一些*ToDo*项，或标记为已完成的一些项目。 请退出设备或模拟器 （或强制关闭应用程序），然后重新启动。 验证所做的更改已被保存在设备上因为持有本地 SQL 光存储。

3. 查看 Azure *TodoItem*表的内容。 验证新项_尚未与服务器同步_︰

   - JavaScript 后端，请转到管理门户，并单击数据选项卡可查看的内容`TodoItem`表。
   - 查看.NET 后端，使用*SQL Server 管理 Studio*，例如一个 SQL 工具或其他客户端如*Fiddler*或*把邮递员弄*的表内容。

4. 打开 WiFi 设备或模拟器中。 接下来，请按**刷新**按钮。

5. 在 Azure 门户再次查看 TodoItem 的数据。 现在应该显示新的和更改 TodoItems。


## 下一步行动

* [使用软中移动服务中删除][软删除]

## 其他资源

* [在移动服务 Azure 云盖︰ 脱机同步]

* [Azure 星期五︰ Azure 的移动服务在脱机启用应用程序]\(注︰ 演示是 windows，但功能讨论适用于所有平台\)


<!-- URLs. -->

[获取示例应用程序]: #get-app
[检查的核心数据模型]: #review-core-data
[检查移动服务的同步代码]: #review-sync
[更改应用程序的同步行为]: #setup-sync
[测试应用程序]: #test-app


[在 GitHub 上的移动服务的示例存储库]: https://github.com/Azure/mobile-services-samples


[开始使用移动服务]: mobile-services-android-get-started.md
[有关数据入门]: mobile-services-android-get-started-data.md
[处理脱机支持移动服务与冲突]:  mobile-services-android-handling-conflicts-offline-data.md
[软删除]: mobile-services-using-soft-delete.md

[在移动服务 Azure 云盖︰ 脱机同步]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[在移动服务 Azure azure 星期五︰ 允许脱机使用的应用程序]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

[移动服务快速入门教程]: mobile-services-android-get-started.md

测试
