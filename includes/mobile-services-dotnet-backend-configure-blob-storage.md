---
ms.openlocfilehash: bb3b71a24a640348ca6f20312591a965d0788ee4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

##安装存储客户端的移动服务项目中

能够生成 SAS，将图像传到 Blob 存储，必须先添加 NuGet 程序包安装存储客户端库的移动服务项目中。 

1. 在**解决方案资源管理器**在 Visual Studio 中的移动服务项目中，用鼠标右键单击，然后选择**管理 NuGet 程序包**。

2. 在左窗格中，选择**联机**类别，选择**仅 Stabile**、 **WindowsAzure.Storage**搜索、 单击**安装**在**Azure 存储**软件包，然后接受许可协议。 

    ![](./media/mobile-services-configure-blob-storage/mobile-add-storage-nuget-package-dotnet.png)

    这将 Azure 存储服务的客户端库添加到移动服务项目。

##更新数据模型中的 TodoItem 定义

TodoItem 类定义数据对象，并且您需要在客户端上向此类添加相同的属性。

1. 在 Visual Studio 2013，打开您的移动服务项目，展开 DataObjects 文件夹中，然后打开 TodoItem.cs 项目文件。
    
2. **TodoItem**类中加入以下新的属性︰

        public string containerName { get; set; }
        public string resourceName { get; set; }
        public string sasQueryString { get; set; }
        public string imageUri { get; set; } 

    这些属性用来生成 SAS 还可以存储图像的信息。 请注意这些属性的大小写匹配的 JavaScript 后端版本。 

    >[AZURE.NOTE] 当使用默认数据库初始值设定项时，实体框架将除去并重新创建数据库，在第一个代码定义中检测到数据模型更改时。 若要使此数据模型更改和维护数据库中的现有数据，则必须使用代码第一个迁移。 默认初始值设定项不能用于 Azure 中的 SQL 数据库。 有关详细信息，请参阅[如何使用代码第一个迁移来更新数据模型](../articles/mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)。

##TodoItem 控制器生成共享的访问签名更新 

现有**TodoItemController**将更新，以便插入新的 TodoItem 时， **PostTodoItem**方法将生成 SAS。 您还 

0. 如果您没有创建存储帐户，请参阅[如何创建存储帐户]。

1. 在管理门户中，单击**存储**，请单击存储帐户，然后单击**管理密钥**。 

2. 记下**存储帐户名称**和**访问键**。
 
3. 在您的移动服务，单击**配置**选项卡，向下滚动到**的应用程序设置**来自存储帐户，下面的每个输入的**名称**/**值**对然后单击**保存**。

    + `STORAGE_ACCOUNT_NAME`
    + `STORAGE_ACCOUNT_ACCESS_KEY`

    ![](./media/mobile-services-configure-blob-storage/mobile-blob-storage-app-settings.png)

    存储帐户访问密钥存储加密中的应用程序设置。 您可以从任何服务器脚本在运行时访问此注册表项。 有关详细信息，请参阅[应用程序设置]。

4. 在解决方案资源管理器在 Visual Studio 中，打开手机信息服务项目的 Web.config 文件，添加以下新的应用程序的设置，将占位符替换为您只需设置在门户中的存储帐户名称和访问键︰

        <add key="STORAGE_ACCOUNT_NAME" value="**your_account_name**" />
        <add key="STORAGE_ACCOUNT_ACCESS_KEY" value="**your_access_token_secret**" />

    它可让您测试代码，您在发布之前的本地计算机上运行时，移动服务使用这些存储的设置。 Azure 中运行时，移动服务而应使用应用程序设置值设置在门户中，将忽略这些项目的设置。 

7.  控制器文件夹中打开 TodoItemController.cs 文件，并添加下面的**using**指令︰

        using System;
        using Microsoft.WindowsAzure.Storage.Auth;
        using Microsoft.WindowsAzure.Storage.Blob;
  
8.  用以下代码替换现有的**PostTodoItem**方法︰

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            string storageAccountName;
            string storageAccountKey;

            // Try to get the Azure storage account token from app settings.  
            if (!(Services.Settings.TryGetValue("STORAGE_ACCOUNT_NAME", out storageAccountName) |
            Services.Settings.TryGetValue("STORAGE_ACCOUNT_ACCESS_KEY", out storageAccountKey)))
            {
                Services.Log.Error("Could not retrieve storage account settings.");
            }

            // Set the URI for the Blob Storage service.
            Uri blobEndpoint = new Uri(string.Format("https://{0}.blob.core.windows.net", storageAccountName));

            // Create the BLOB service client.
            CloudBlobClient blobClient = new CloudBlobClient(blobEndpoint, 
                new StorageCredentials(storageAccountName, storageAccountKey));

            if (item.containerName != null)
            {
                // Set the BLOB store container name on the item, which must be lowercase.
                item.containerName = item.containerName.ToLower();

                // Create a container, if it doesn't already exist.
                CloudBlobContainer container = blobClient.GetContainerReference(item.containerName);
                await container.CreateIfNotExistsAsync();

                // Create a shared access permission policy. 
                BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

                // Enable anonymous read access to BLOBs.
                containerPermissions.PublicAccess = BlobContainerPublicAccessType.Blob;
                container.SetPermissions(containerPermissions);

                // Define a policy that gives write access to the container for 5 minutes.                                   
                SharedAccessBlobPolicy sasPolicy = new SharedAccessBlobPolicy()
                {
                    SharedAccessStartTime = DateTime.UtcNow,
                    SharedAccessExpiryTime = DateTime.UtcNow.AddMinutes(5),
                    Permissions = SharedAccessBlobPermissions.Write
                };

                // Get the SAS as a string.
                item.sasQueryString = container.GetSharedAccessSignature(sasPolicy); 

                // Set the URL used to store the image.
                item.imageUri = string.Format("{0}{1}/{2}", blobEndpoint.ToString(), 
                    item.containerName, item.resourceName);
            }

            // Complete the insert operation.
            TodoItem current = await InsertAsync(item);
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

    此 POST 方法现在生成新插入的项，它的有效期为 5 分钟，并赋以值对生成 SAS 的 SA`sasQueryString`返回的项的属性。 `imageUri`属性也设置新 BLOB 的资源路径，以在客户端用户界面中的绑定期间启用图像显示。

    >[AZURE.NOTE] 此代码将创建为单个 BLOB SAS。 如果需要将多个 blob 上载到使用相同的 SAS 的容器，可以改为调用<a href="http://go.microsoft.com/fwlink/?LinkId=390455" target="_blank">generateSharedAccessSignature 方法</a>与一个空 blob 的资源名称，如下︰
    <pre><code>blobService.generateSharedAccessSignature(containerName, '', sharedAccessPolicy);</code></pre>

接下来，您将更新快速入门应用程序通过使用 SAS 插入生成添加图像上载功能。
 
<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[如何创建存储帐户。]: ../articles/storage/storage-create-storage-account.md
[应用程序设置]: http://msdn.microsoft.com/library/windowsazure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7
