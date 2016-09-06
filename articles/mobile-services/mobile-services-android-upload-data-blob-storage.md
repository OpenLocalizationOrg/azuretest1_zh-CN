---
ms.openlocfilehash: 6dc20fcd82ab75388343ce236b5ffe08ea311e92
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="用于将图像传到 blob 存储 (Android) 手机服务 |移动服务" 
    description="了解如何使用移动服务将图像传到 Azure 存储并从 Android 应用程序访问这些图像。" 
    services="mobile-services" 
    documentationCenter="android" 
    authors="RickSaling" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="09/02/2015" 
    ms.author="ricksal"/>

# 从 Android 设备到 Azure 存储上载图像

[AZURE.INCLUDE [mobile-services-selector-upload-data-blob-storage](../../includes/mobile-services-selector-upload-data-blob-storage.md)]

本主题演示如何启用您 Android Azure 移动服务的应用程序将图像传到 Azure 存储。 

移动服务使用 SQL 数据库来存储数据。 但是，它是在 Azure 存储中存储二进制大对象 (BLOB) 数据效率更高。 在本教程中，您启用移动服务快速启动应用程序以使用 Android 的拍照，并将图像传到 Azure 存储。 


## 您需要开始

在开始本教程之前，您必须先完成移动服务快速入门︰[开始使用移动服务]。 

本教程还要求如下︰

+ [Azure 存储帐户](../storage-create-storage-account.md)
+ 摄像头的 Android 设备

## 该应用程序的工作原理

上传的图片是一个多步骤的过程︰

- 首先，拍照，并将 TodoItem 行插入到包含新的元数据字段使用 Azure 存储的 SQL 数据库。
- 新的移动服务 SQL**插入**脚本询问 Azure 存储的共享访问签名 (SAS)。
- 该脚本返回给客户端 SAS 和为 blob 的 URI。
- 客户端上载照片，使用 SAS 和斑点的 URI。

那么什么是 SAS 呢？

不安全地存储数据上载到您的客户端应用程序内部的 Azure 存储服务所需的凭据。 相反，您将这些凭据存储在您的移动服务，并使用它们来生成共享访问签名 (SAS) 授予权限上载一个新映像。 SAS，5 分钟过期凭据都会返回安全地移动服务的客户端应用程序。 该应用程序使用此临时凭证要上载的图像。 有关详细信息，请参阅[共享访问签名，第 1 部分︰ 了解 SAS 模型](storage-dotnet-shared-access-signature-part-1.md)

>[AZURE.NOTE] [下面](https://github.com/Azure/mobile-services-samples/tree/master/UploadImages)是此应用程序的已完成客户端源的代码部分。

## 更新管理门户中的注册的插入脚本

[AZURE.INCLUDE [mobile-services-configure-blob-storage](../../includes/mobile-services-configure-blob-storage.md)]


## 更新快速入门应用程序捕获和上传图像。

### 引用的 Azure 存储 Android 的客户端库

1. 将引用添加到库中时，**应用程序**在 > **build.gradle**文件中添加对这行`dependencies`部分中︰

        compile 'com.microsoft.azure.android:azure-storage-android:0.6.0@aar'


2. 更改`minSdkVersion`15 （摄像头 API 所需） 的值。

3. 按**同步项目 Gradle 文件**图标。

### 更新清单文件的相机和存储

1. 指定您的应用程序需要通过将此行添加到**AndroidManifest.xml**中有可用的相机︰

        <uses-feature android:name="android.hardware.camera"
            android:required="true" />

2. 指定您的应用程序需要通过将此行添加到**AndroidManifest.xml**写到外部存储权限︰

        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

    注意，Android 的外部存储并不一定是 SD 卡︰ 有关详细信息，请参阅[保存文件](http://developer.android.com/training/basics/data-storage/files.html)。

### 新的用户界面更新资源文件

1. 添加到*值*目录中的**strings.xml**文件中添加以下新的按钮的标题︰

        <string name="preview_button_text">Take Photo</string>
        <string name="upload_button_text">Upload</string>

2. 在**activity_to_do.xml**文件中**res = > 版式**文件夹，添加此段代码之前**添加**按钮的现有代码。

         <Button
             android:id="@+id/buttonPreview"
             android:layout_width="120dip"
             android:layout_height="wrap_content"
             android:onClick="takePicture"
             android:text="@string/preview_button_text" />

3. **添加**按钮元素替换为以下代码︰

         <Button
             android:id="@+id/buttonUpload"
             android:layout_width="100dip"
             android:layout_height="wrap_content"
             android:onClick="uploadPhoto"
             android:text="@string/upload_button_text" />


### 添加代码以捕获照片

1. 在**ToDoActivity.java**中添加此代码以创建**文件**对象的唯一名称。

        // Create a File object for storing the photo
        private File createImageFile() throws IOException {
            // Create an image file name
            String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
            String imageFileName = "JPEG_" + timeStamp + "_";
            File storageDir = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_PICTURES);
            File image = File.createTempFile(
                    imageFileName,  /* prefix */
                    ".jpg",         /* suffix */
                    storageDir      /* directory */
            );
            return image;
        }

5. 添加此代码以启动摄像头的 Android 应用程序。 然后可以拍照，和一个看起来没有问题，请按**保存**，它会将其存储在您刚刚创建的文件。

        // Run an Intent to start up the Android camera
        static final int REQUEST_TAKE_PHOTO = 1;
        public Uri mPhotoFileUri = null;
        public File mPhotoFile = null;
        
        public void takePicture(View view) {
            Intent takePictureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
            // Ensure that there's a camera activity to handle the intent
            if (takePictureIntent.resolveActivity(getPackageManager()) != null) {
                // Create the File where the photo should go
                try {
                    mPhotoFile = createImageFile();
                } catch (IOException ex) {
                    // Error occurred while creating the File
                    //
                }
                // Continue only if the File was successfully created
                if (mPhotoFile != null) {
                    mPhotoFileUri = Uri.fromFile(mPhotoFile);
                    takePictureIntent.putExtra(MediaStore.EXTRA_OUTPUT, mPhotoFileUri);
                    startActivityForResult(takePictureIntent, REQUEST_TAKE_PHOTO);
                }
            }
        }

### 添加代码以将照片文件上载到 blob 存储


1. 首先，我们将添加一些新的元数据字段`ToDoItem`通过将此代码添加到**ToDoItem.java**的对象。

        /**
         *  imageUri - points to location in storage where photo will go
         */
        @com.google.gson.annotations.SerializedName("imageUri")
        private String mImageUri;
    
        /**
         * Returns the item ImageUri
         */
        public String getImageUri() {
            return mImageUri;
        }
    
        /**
         * Sets the item ImageUri
         *
         * @param ImageUri
         *            Uri to set
         */
        public final void setImageUri(String ImageUri) {
            mImageUri = ImageUri;
        }
    
        /**
         * ContainerName - like a directory, holds blobs
         */
        @com.google.gson.annotations.SerializedName("containerName")
        private String mContainerName;
    
        /**
         * Returns the item ContainerName
         */
        public String getContainerName() {
            return mContainerName;
        }
    
        /**
         * Sets the item ContainerName
         *
         * @param ContainerName
         *            Uri to set
         */
        public final void setContainerName(String ContainerName) {
            mContainerName = ContainerName;
        }
    
        /**
         *  ResourceName
         */
        @com.google.gson.annotations.SerializedName("resourceName")
        private String mResourceName;
    
        /**
         * Returns the item ResourceName
         */
        public String getResourceName() {
            return mResourceName;
        }
    
        /**
         * Sets the item ResourceName
         *
         * @param ResourceName
         *            Uri to set
         */
        public final void setResourceName(String ResourceName) {
            mResourceName = ResourceName;
        }
    
        /**
         *  SasQueryString - permission to write to storage
         */
        @com.google.gson.annotations.SerializedName("sasQueryString")
        private String mSasQueryString;
    
        /**
         * Returns the item SasQueryString
         */
        public String getSasQueryString() {
            return mSasQueryString;
        }
    
        /**
         * Sets the item SasQueryString
         *
         * @param SasQueryString
         *            Uri to set
         */
        public final void setSasQueryString(String SasQueryString) {
            mSasQueryString = SasQueryString;
        }

2. 没有参数的构造函数替换此代码︰

        /**
         * ToDoItem constructor
         */
        public ToDoItem() {
            mContainerName = "";
            mResourceName = "";
            mImageUri = "";
            mSasQueryString = "";
        }

3. 使用此代码替换的多参数构造函数︰

        /**
         * Initializes a new ToDoItem
         *
         * @param text
         *            The item text
         * @param id
         *            The item id
         */
        public ToDoItem(String text,
                        String id,
                        String containerName,
                        String resourceName,
                        String imageUri,
                        String sasQueryString) {
            this.setText(text);
            this.setId(id);
            this.setContainerName(containerName);
            this.setResourceName(resourceName);
            this.setImageUri(imageUri);
            this.setSasQueryString(sasQueryString);
        }


4. 在**ToDoActivity.java**文件中，使用以下代码将上载图像替换**addItem**方法在**ToDoActivity.java**中。

        /**
         * Add a new item
         *
         * @param view
         *            The view that originated the call
         */
        public void uploadPhoto(View view) {
            if (mClient == null) {
                return;
            }
    
            // Create a new item
            final ToDoItem item = new ToDoItem();
    
            item.setText(mTextNewToDo.getText().toString());
            item.setComplete(false);
            item.setContainerName("todoitemimages");
    
            // Use a unigue GUID to avoid collisions.
            UUID uuid = UUID.randomUUID();
            String uuidInString = uuid.toString();
            item.setResourceName(uuidInString);
    
            // Send the item to be inserted. When blob properties are set this
            // generates an SAS in the response.
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                            final ToDoItem entity = addItemInTable(item);
        
                            // If we have a returned SAS, then upload the blob.
                            if (entity.getSasQueryString() != null) {
        
                           // Get the URI generated that contains the SAS
                            // and extract the storage credentials.
                            StorageCredentials cred = 
                                new StorageCredentialsSharedAccessSignature(entity.getSasQueryString());
                            URI imageUri = new URI(entity.getImageUri());
    
                            // Upload the new image as a BLOB from a stream.
                            CloudBlockBlob blobFromSASCredential =
                                    new CloudBlockBlob(imageUri, cred);
    
                            blobFromSASCredential.uploadFromFile(mPhotoFileUri.getPath());
                        }
    
                        runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                if(!entity.isComplete()){
                                    mAdapter.add(entity);
                                }
                            }
                        });
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
    
            runAsyncTask(task);
    
            mTextNewToDo.setText("");
        }
    

此代码为要插入新的 TodoItem 的移动服务发送一个请求。 该响应包含的 SA，则用于上载到 Azure 存储中的 blob 从本地存储的图像。 


## 上载图像的测试 

1. Android 的 Studio 中按**运行**。 在对话框中，选择要使用的设备。

2. 当应用程序用户界面出现，标记**添加一个 ToDo 项**文本框中输入文本。

3. 按下**拍照**。 相机应用程序启动时，需要一张照片。 按复选标记以接受照片。

4. 按**传**。 请注意如何 ToDoItem 已添加到列表中，像往常一样。

5. 在 Microsoft Azure 门户中，转到您的存储帐户，**容器**选项卡，然后在列表中按您的容器的名称。

6. 将显示您已上载的 blob 的文件的列表。 选择一个，按下**下载**。

7. 现在上载的图像将显示在浏览器窗口。


## <a name="next-steps"> </a>下一步行动

现在，您已经能够安全地将图像上载将 Blob 服务与集成在您的移动服务，签出的后端服务和集成的一些主题︰

+ [从 SendGrid 的移动服务发送电子邮件]
 
  了解如何添加到您的移动服务使用 SendGrid 的电子邮件服务的电子邮件功能。 本主题演示如何添加服务器端脚本发送邮件使用 SendGrid。

+ [计划中移动服务的后端作业]

  了解如何使用移动服务作业计划程序功能来定义服务器按照您定义的计划执行的脚本代码。

+ [移动服务服务器脚本引用]

  使用服务器脚本来执行服务器端任务和与其他 Azure 组件和外部资源集成的参考主题。
 
+ [移动服务.NET 帮助概念参考]

  了解更多关于如何使用.NET 的移动服务
  
 
<!-- Anchors. -->
[安装客户端存储库]: #install-storage-client
[更新的客户端应用程序来捕获图像]: #add-select-images
[更新生成 SAS 的插入脚本]: #update-scripts
[上载图像来测试应用程序]: #test
[下一步行动]:#next-steps

<!-- Images. -->

[2]: ./media/mobile-services-windows-store-dotnet-upload-data-blob-storage/mobile-add-storage-nuget-package-dotnet.png


<!-- URLs. -->
[从 SendGrid 的移动服务发送电子邮件]: store-sendgrid-mobile-services-send-email-scripts.md
[计划中移动服务的后端作业]: mobile-services-schedule-recurring-tasks.md
[向 Windows 应用商店应用程序从一个.NET 后端使用服务总线发送推式通知]: http://go.microsoft.com/fwlink/?LinkId=277073&clcid=0x409
[移动服务服务器脚本引用]: mobile-services-how-to-use-server-scripts.md
[开始使用移动服务]: mobile-services-javascript-backend-windows-store-dotnet-get-started.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[如何创建存储帐户。]: ../storage-create-storage-account.md
[Azure 存储客户端库，用于存储应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=276866 
[移动服务.NET 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[应用程序设置]: http://msdn.microsoft.com/library/windowsazure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7
 

测试
