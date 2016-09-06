---
ms.openlocfilehash: 3ef00c18424a25f184025bd4b6095d1dcef653d4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
##<a name="add-select-images"></a>更新快速启动客户端应用程序来捕获并上载图像

1. 在 Visual Studio，打开 Package.appxmanifest 文件，并在**功能**选项卡中启用的**网络摄像头**和**麦克风**功能。

    ![](./media/mobile-services-windows-store-dotnet-upload-to-blob-storage/mobile-app-manifest-camera.png)
 
    这将确保您的应用程序可以使用摄像机连接到计算机。 用户将请求允许摄像机访问运行该应用程序是第一次。

1. 打开 MainPage.xaml 文件，然后**StackPanel**元素直接后的第一个**任务**元素替换为以下代码︰

        <StackPanel Orientation="Horizontal" Margin="72,0,0,0">
            <TextBox Name="TextInput" Margin="5" MaxHeight="40" MinWidth="300"></TextBox>
            <Button Name="btnTakePhoto" Style="{StaticResource PhotoAppBarButtonStyle}"
                    Click="OnTakePhotoClick" />
            <Button Name="ButtonSave" Style="{StaticResource UploadAppBarButtonStyle}" 
                    Click="ButtonSave_Click"/>
        </StackPanel>

2. **数据模板**中的**StackPanel**元素替换为以下代码︰

        <StackPanel Orientation="Vertical">
            <CheckBox Name="CheckBoxComplete" IsChecked="{Binding Complete, Mode=TwoWay}" 
                        Checked="CheckBoxComplete_Checked" Content="{Binding Text}" 
                        Margin="10,5" VerticalAlignment="Center"/>
            <Image Name="ImageUpload" Source="{Binding ImageUri, Mode=OneWay}"
                    MaxHeight="250"/>
        </StackPanel> 

    这**项模板**中添加了图像，并将其绑定源设置为加载的映像中的 Blob 存储服务的 URI。

3. 打开 MainPage.xaml.cs 项目文件并添加下面的**using**语句︰
    
        using Windows.Media.Capture;
        using Windows.Storage;
        using Windows.UI.Popups;
        using Microsoft.WindowsAzure.Storage.Auth;
        using Microsoft.WindowsAzure.Storage.Blob;
    
4. 在 TodoItem 类中，添加以下属性︰

        [JsonProperty(PropertyName = "containerName")]
        public string ContainerName { get; set; }
        
        [JsonProperty(PropertyName = "resourceName")]
        public string ResourceName { get; set; }
        
        [JsonProperty(PropertyName = "sasQueryString")]
        public string SasQueryString { get; set; }
        
        [JsonProperty(PropertyName = "imageUri")]
        public string ImageUri { get; set; } 

    >[AZURE.NOTE]JavaScript 后端移动服务中的 TodoItem 对象添加新属性，您必须在您的移动服务中启用动态架构。 启用动态架构后，新的列将自动添加到 TodoItem 表映射到这些新属性。 .NET 后端移动服务，请参阅主题[如何到.NET 后端移动服务数据模型更改](../articles/mobile-services/mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)。

5. 在主页类中，添加以下代码︰

        // Use a StorageFile to hold the captured image for upload.
        StorageFile media = null;
        
        private async void OnTakePhotoClick(object sender, RoutedEventArgs e)
        {
            // Capture a new photo or video from the device.
            CameraCaptureUI cameraCapture = new CameraCaptureUI();
            media = await cameraCapture
                .CaptureFileAsync(CameraCaptureUIMode.PhotoOrVideo);
        }

    此代码显示摄像头 UI 来捕获映像，并将图像保存到存储文件。

6. 替换现有`InsertTodoItem`方法与下列代码︰
 
        private async void InsertTodoItem(TodoItem todoItem)
        {
            string errorString = string.Empty;
            
            if (media != null)
            {
                // Set blob properties of TodoItem.
                todoItem.ContainerName = "todoitemimages";
                todoItem.ResourceName = media.Name;
            }
            
            // Send the item to be inserted. When blob properties are set this
            // generates an SAS in the response.
            await todoTable.InsertAsync(todoItem);
            
            // If we have a returned SAS, then upload the blob.
            if (!string.IsNullOrEmpty(todoItem.SasQueryString))
            {
                // Get the URI generated that contains the SAS 
                // and extract the storage credentials.
                StorageCredentials cred = new StorageCredentials(todoItem.SasQueryString);
                var imageUri = new Uri(todoItem.ImageUri);
                
                // Instantiate a Blob store container based on the info in the returned item.
                CloudBlobContainer container = new CloudBlobContainer(
                    new Uri(string.Format("https://{0}/{1}",
                        imageUri.Host, todoItem.ContainerName)), cred);

                // Get the new image as a stream.
                using (var fileStream = await media.OpenStreamForReadAsync())
                {                                       
                    // Upload the new image as a BLOB from the stream.
                    CloudBlockBlob blobFromSASCredential =
                        container.GetBlockBlobReference(todoItem.ResourceName);
                    await blobFromSASCredential.UploadFromStreamAsync(fileStream.AsInputStream());
                }
                
                // When you request an SAS at the container-level instead of the blob-level,
                // you are able to upload multiple streams using the same container credentials.
            }
            
            // Add the new item to the collection.
            items.Add(todoItem);
        }

    此代码发送到移动的服务，以插入新的 TodoItem，包括图像文件名称的请求。 该响应包含 SAS，然后用于插入 Blob 存储区中的图像，并且图像数据绑定的 URI。

最后一步是测试应用程序并验证上载成功。
        
##<a name="test"></a>上载您的应用程序中的图像的测试

1. 在 Visual Studio 中，请按 F5 键以运行该应用程序。

2. 在**插入 TodoItem**文本框中输入文本，然后单击**照片**。

    ![](./media/mobile-services-windows-store-dotnet-upload-to-blob-storage/mobile-quickstart-blob-appbar.png)

    这将显示摄像头捕获用户界面。 

3. 单击该图像可以拍摄一张照片，然后单击**确定**。
  
    ![](./media/mobile-services-windows-store-dotnet-upload-to-blob-storage/mobile-quickstart-blob-camera.png)

4. 请单击**上载**插入新项和上传图像。

    ![](./media/mobile-services-windows-store-dotnet-upload-to-blob-storage/mobile-quickstart-blob-appbar2.png)

5. 新的项目，以及上载的图像，显示在列表视图。

    ![](./media/mobile-services-windows-store-dotnet-upload-to-blob-storage/mobile-quickstart-blob-ie.png)

    >[AZURE.NOTE]将映像从 Blob 存储自动下载服务时<code>imageUri</code>的新项的属性绑定到<strong>图像</strong>控件。

