---
ms.openlocfilehash: 4e415f2a947ce8a87d27c1e3f568c7a40c6a4280
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

##<a name="add-select-images"></a>更新快速启动客户端应用程序来捕获并上载图像

在本节中将更新从拍摄的照片，并将其上载到 Azure Blob 存储[移动服务入门]教程项目。 要捕获图像，本教程使用了从[CameraCaptureTask] `Microsoft.Phone.Tasks`命名空间。 此类启动 Windows Phone 设备捕获照片上摄像头 UI，并自动将图像保存到摄像机前滚，Windows Phone 设备上。 如果不希望保存相机将鼠标移到图像，可以使用[PhotoCamera]类中的`Microsoft.Devices`命名空间相反。

1. 在解决方案资源管理器中的 Visual Studio，在该项目中，展开**属性**。 打开 WMAppManifest.xml 文件，然后在**功能**选项卡上启用相机通过单击**ID\_CAP\_ISV\_相机**。 关闭文件以保存所做的更改。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-WMAppmanifest-wp8.png)

    这将确保您的应用程序可以使用摄像机连接到计算机。 用户将请求允许摄像机访问运行该应用程序是第一次。

2. 打开 MainPage.xaml 文件，并将替换以下代码名为**ContentPanel**的**网格**元素︰

        <!--ContentPanel - place additional content here-->
        <Grid x:Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="2*" />
                <ColumnDefinition Width="2*" />
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.ColumnSpan="2" Text="Enter some text below, click Capture Image to add a captured image. Then click Save to insert a new TodoItem item into your database" TextWrapping="Wrap" Margin="12"/>
            <TextBox Grid.Row="1" Grid.ColumnSpan="2" Name="TextInput" Text="" />
            <Button Name="ButtonCaptureImage" Grid.Row="2" Click="ButtonCaptureImage_Click">Capture Image</Button>
            <Button Grid.Row ="2" Grid.Column="1" Name="ButtonSave" Click="ButtonSave_Click">Save</Button>
            <TextBlock Grid.Row="3" Grid.ColumnSpan="2" Text="Click refresh below to load the unfinished TodoItems from your database. Use the checkbox to complete and update your TodoItems" TextWrapping="Wrap" Margin="12" />
            <Button Grid.Row="4" Grid.ColumnSpan="2" Name="ButtonRefresh" Click="ButtonRefresh_Click">Refresh</Button>
            <phone:LongListSelector Grid.Row="5" Grid.ColumnSpan="2" Name="ListItems">
                <phone:LongListSelector.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Vertical">
                            <CheckBox Name="CheckBoxComplete" IsChecked="{Binding Complete, Mode=TwoWay}" Checked="CheckBoxComplete_Checked" Content="{Binding Text}" Margin="10,5" VerticalAlignment="Center"/>
                            <Image Name="ImageUpload" Source="{Binding ImageUri, Mode=OneWay}" MaxHeight="150"/>
                        </StackPanel>
                    </DataTemplate>
                </phone:LongListSelector.ItemTemplate>
            </phone:LongListSelector>
        </Grid>


    这添加一个新按钮以启动[CameraCaptureTask]和**项模板**中添加了图像并将其绑定源设置为加载的映像中的 Blob 存储服务的 URI。

3. 打开 MainPage.xaml.cs 项目文件并添加下面的**using**语句︰
    
        using Microsoft.Phone.Tasks;
        using System.IO;
        using Microsoft.WindowsAzure.Storage.Auth;
        using Microsoft.WindowsAzure.Storage.Blob;
    
4. 在 MainPage.xaml.cs 项目文件中添加以下属性来更新 TodoItem 类︰

        [JsonProperty(PropertyName = "containerName")]
        public string ContainerName { get; set; }
        
        [JsonProperty(PropertyName = "resourceName")]
        public string ResourceName { get; set; }
        
        [JsonProperty(PropertyName = "sasQueryString")]
        public string SasQueryString { get; set; }
        
        [JsonProperty(PropertyName = "imageUri")]
        public string ImageUri { get; set; } 

5. 在 MainPage.xaml.cs 项目文件中更新主页类。 添加以下代码，以声明[CameraCaptureTask]并且 stream 对象将引用捕获的映像︰

        // Using the CameraCaptureTask to allow the user to capture a todo item image //
        CameraCaptureTask cameraCaptureTask;
        
        // Using a stream reference to upload the image to blob storage.
        Stream imageStream = null;

6. 在 MainPage.xaml.cs 项目文件中更新主页类。 添加以下代码以更新构造函数创建 CameraCaptureTask 并添加的事件处理程序已完成事件︰

        // Constructor
        public MainPage()
        {
            InitializeComponent();
            
            cameraCaptureTask = new CameraCaptureTask();
            cameraCaptureTask.Completed += cameraCaptureTask_Completed;
        }
        
        void cameraCaptureTask_Completed(object sender, PhotoResult e)
        {
            imageStream = e.ChosenPhoto;
        }

7. 在 MainPage.xaml.cs 项目文件中更新主页类。 添加下面的代码显示相机用户界面以允许用户单击**捕获图像**按钮时捕获的图像︰

        private void ButtonCaptureImage_Click(object sender, RoutedEventArgs e)
        {
            cameraCaptureTask.Show();
        }


8. 在 MainPage.xaml.cs 项目文件中更新主页类。 替换现有`InsertTodoItem`方法与下列代码︰
 
        private async void InsertTodoItem(TodoItem todoItem)
        {
            string errorString = string.Empty;            
            
            if (imageStream != null)
            {
                // Set blob properties of TodoItem.
                todoItem.ContainerName = "todoitemimages";
                todoItem.ResourceName = Guid.NewGuid().ToString() + ".jpg";
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
                
                // Upload the new image as a BLOB from the stream.
                CloudBlockBlob blobFromSASCredential =
                    container.GetBlockBlobReference(todoItem.ResourceName);
                await blobFromSASCredential.UploadFromStreamAsync(imageStream);
                
                // When you request an SAS at the container-level instead of the blob-level,
                // you are able to upload multiple streams using the same container credentials.

                imageStream = null;
            }              
            
            // Add the new item to the collection.
            items.Add(todoItem);
            TextInput.Text = "";
        }


    此代码发送到移动的服务，以插入新的 TodoItem，包括图像文件名称的请求。 该响应包含 SAS，然后用于插入 Blob 存储区中的图像，并且图像数据绑定的 URI。

最后一步是测试应用程序并验证上载成功。
        
##<a name="test"></a>上载您的应用程序中的图像的测试

1. 在 Visual Studio 中，您可以按 F5 键与针对一个实际的设备或仿真程序测试应用程序。

2. 在文本框中输入一些文本，然后单击**捕获图像**。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-view-wp8.png)

    这将显示摄像头捕获用户界面。 

3. 单击图像或电话要拍摄一张照片上的快照按钮。
  
    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-view-camera-wp8.png)

4. 单击**接受**来接受图像并退出相机用户界面。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-view-camera-accept-wp8.png)

5. 单击**保存**以插入新项并上载的图像。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-view-save-wp8.png)

6. 新的项目，以及上载的图像，显示在列表视图。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-view-final-wp8.png)

   >[AZURE.NOTE]将映像从 Blob 存储自动下载服务时<code>imageUri</code>的新项的属性绑定到<strong>图像</strong>控件。


[开始使用移动服务]: ../articles/mobile-services-windows-phone-get-started.md
[CameraCaptureTask]: http://msdn.microsoft.com/library/windowsphone/develop/microsoft.phone.tasks.cameracapturetask(v=vs.105).aspx
[PhotoCamera]: http://msdn.microsoft.com/library/windowsphone/develop/microsoft.devices.photocamera(v=vs.105).aspx
