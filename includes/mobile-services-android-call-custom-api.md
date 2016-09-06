---
ms.openlocfilehash: ddf7a74ede2d00dcf9f23ba6e86f4f632bff6d57
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

##<a name="update-app"></a>更新应用程序以调用自定义的 API

1. 我们将添加现有按钮，标记为"完成全部"按钮和两个按钮向下移动一行。 在 Android Studio 中，快速启动项目中打开*res\layout\activity_to_do.xml*文件，查找包含名为的**按钮**元素的**LinearLayout**元素`buttonAddToDo`。 将**LinearLayout**复制并粘贴紧随原始。 从第一个**LinearLayout**中删除**按钮**元素。

2. 在第二个**LinearLayout**，删除**EditText**元素中，并添加下面的代码紧随现有的**按钮**元素︰ 

        <Button
            android:id="@+id/buttonCompleteItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="completeItem"
            android:text="@string/complete_button_text" />

    此页，请在单独的行，现有按钮旁边添加一个新按钮。

3. 第二个**LinearLayout**现在如下所示︰

         <LinearLayout
            android:layout_width="match_parent" 
            android:layout_height="wrap_content" 
            android:background="#71BCFA"
            android:padding="6dip"  >
            <Button
                android:id="@+id/buttonAddToDo"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:onClick="addItem"
                android:text="@string/add_button_text" />
            <Button
                android:id="@+id/buttonCompleteItem"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:onClick="completeItem"
                android:text="@string/complete_button_text" />
        </LinearLayout>
    

4. 打开 res\values\string.xml 文件，添加以下代码行︰

        <string name="complete_button_text">Complete All</string>



5. 在项目资源管理器中，右键单击*src*文件夹中的项目名称 (`com.example.{your projects name}`)，选择**新建**，然后**类**。 在对话框中，在类名称字段中输入**MarkAllResult** ，选择确定，并使用下面的代码替换生成的类定义︰

        import com.google.gson.annotations.SerializedName;
        
        public class MarkAllResult {
            @SerializedName("count")
            public int mCount;
            
            public int getCount() {
                return mCount;
            }
        
            public void setCount(int count) {
                    this.mCount = count;
            }
        }

    此类用于保存自定义的 API 返回的行计数值。 

6. 在**ToDoActivity.java**文件中，找到**refreshItemsFromTable**方法，并确保在代码的第一行`try`块如下所示︰

        final MobileServiceList<ToDoItem> result = mToDoTable.where().field("complete").eq(false).execute().get();

    这项进行筛选，以便查询不会返回已完成的项目。

7. 请确保**ToDoActivity.java**包含下面的导入文件的开头︰

        import com.google.common.util.concurrent.FutureCallback;
        import com.google.common.util.concurrent.Futures;
        import com.google.common.util.concurrent.ListenableFuture;

8. 在**ToDoActivity.java**文件中，添加下列方法︰

    公共 void completeItem （视图） {
        
        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll2", MarkAllResult.class ); 
            
            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }
                
                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();    
                }
            });
        }
    
    此方法处理了新建按钮的**Click**事件。 在客户端，将 POST 请求发送到新的自定义 API 调用**invokeApi**方法。 由自定义的 API 返回的结果将显示在消息对话框中，如任何错误。

## 测试应用程序

1. 从**运行**菜单上，单击**运行应用程序**以在 Android 模拟器，或连接的 Android 设备启动项目。

    这将执行您的应用程序，Android sdk，使用客户端库将发送一个查询，返回的项目，从您的移动服务生成。


2. 在应用程序中，在**插入 TodoItem**，请键入一些文本，然后单击**添加**。

3. 直到几个 todo 项添加到列表，请重复上述步骤。

4. 单击**全部完成**按钮。

    ![](./media/mobile-services-android-call-custom-api/mobile-custom-api-android-completed.png)

    指示标记为已完成的项目数，将显示一个消息对话框，筛选执行查询，以清除列表中的所有项目。