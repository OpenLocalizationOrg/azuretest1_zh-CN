---
ms.openlocfilehash: a02381e0803a9e65e2ab0892f7ddd7161afc051c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

接下来，您需要更改当您注册的通知，以确保之后注册尝试对用户进行身份验证。


1. 在项目资源管理器在 Android Studio，打开 ToDoActivity.java 文件，并找到`onCreate`方法。 将下面的代码从`onCreate`方法的开始处`createTable`方法。

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

     `createTable`时，将调用方法`authenticate`方法完成。 整个`createTable`方法应类似于下面。

        private void createTable() {
        
            NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);
        
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

