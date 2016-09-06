---
ms.openlocfilehash: 98156330de1ab6b603c0cab45c53e5f4f11b90de
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 在 Eclipse 中包资源管理器中打开 ToDoActivity.java 文件，并添加下面的导入语句。

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. 将下面的方法添加到**ToDoActivity**类中︰ 
    
    专用的 void authenticate() {/ / 使用 Google 提供的登录。
        
        ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);

        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
            @Override
            public void onFailure(Throwable exc) {
                createAndShowDialog((Exception) exc, "Error");
            }           
            @Override
            public void onSuccess(MobileServiceUser user) {
                createAndShowDialog(String.format(
                        "You are now logged in - %1$2s",
                        user.getUserId()), "Success");
                createTable();  
            }
        });     
    }


    这将创建一个新的方法来处理身份验证过程。 通过使用 Google 登录，用户进行身份验证。 将显示一个对话框，其中显示已经过身份验证的用户的 ID。 您不能继续没有积极的身份验证。

    > [AZURE.NOTE] 如果您使用的 Google 不标识提供程序，更改传递给**login**方法上面到以下任一值︰ _MicrosoftAccount_、 _Facebook_、_使用 Twitter_，或_windowsazureactivedirectory_。

3. 在**onCreate**方法中，实例化的代码之后添加以下代码行`MobileServiceClient`对象。

        authenticate();

    此调用将启动身份验证过程。

4. 移动后的剩余代码`authenticate();`在**onCreate**方法为新的**createTable**方法中，其中如下所示︰

        private void createTable() {
    
            // Get the Mobile Service Table instance to use
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from the Mobile Service
            refreshItemsFromTable();
        }

9. 从**运行**菜单，然后单击**运行**以启动该应用程序并使用您选定的标识提供商登录。 

    成功登录后，应用程序应该运行错误，并应能为移动服务的查询，并对数据进行更新。