---
ms.openlocfilehash: c65e8bff8ce7fffb5717672a8715bf3161872c8e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
新的插入脚本注册时插入新的 Todo 项生成 SAS。

0. 如果您没有创建存储帐户，请参阅[如何创建存储帐户](../storage/storage-create-storage-account.md)。

1. 在管理门户中，单击**存储**，请单击存储帐户，然后单击**管理密钥**。 

2. 记下**存储帐户名称**和**访问键**。

    ![](./media/mobile-services-configure-blob-storage/mobile-blob-storage-account-keys.png)

3. 在您的移动服务，单击**配置**选项卡，向下滚动到**的应用程序设置**来自存储帐户，下面的每个输入的**名称**/**值**对然后单击**保存**。

    + `STORAGE_ACCOUNT_NAME`
    + `STORAGE_ACCOUNT_ACCESS_KEY`

    ![](./media/mobile-services-configure-blob-storage/mobile-blob-storage-app-settings.png)

    存储帐户访问密钥存储加密中的应用程序设置。 您可以从任何服务器脚本在运行时访问此注册表项。 有关详细信息，请参阅[应用程序设置]。

4. 在配置选项卡，请确保启用了[动态架构](http://msdn.microsoft.com/library/windowsazure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7)。 您需要启用能够将新列添加到 TodoItem 表的动态架构。 不应在任何生产服务中启用动态架构。

4. 单击**数据**选项卡，然后单击**TodoItem**表。 

5.  **Todoitem**，在中，单击**脚本**选项卡并选择**插入**、 插入函数替换为以下代码，然后单击**保存**:

        var azure = require('azure');
        var qs = require('querystring');
        var appSettings = require('mobileservice-config').appSettings;
        
        function insert(item, user, request) {
            // Get storage account settings from app settings. 
            var accountName = appSettings.STORAGE_ACCOUNT_NAME;
            var accountKey = appSettings.STORAGE_ACCOUNT_ACCESS_KEY;
            var host = accountName + '.blob.core.windows.net';
        
            if ((typeof item.containerName !== "undefined") && (
            item.containerName !== null)) {
                // Set the BLOB store container name on the item, which must be lowercase.
                item.containerName = item.containerName.toLowerCase();
        
                // If it does not already exist, create the container 
                // with public read access for blobs.        
                var blobService = azure.createBlobService(accountName, accountKey, host);
                blobService.createContainerIfNotExists(item.containerName, {
                    publicAccessLevel: 'blob'
                }, function(error) {
                    if (!error) {
        
                        // Provide write access to the container for the next 5 mins.        
                        var sharedAccessPolicy = {
                            AccessPolicy: {
                                Permissions: azure.Constants.BlobConstants.SharedAccessPermissions.WRITE,
                                Expiry: new Date(new Date().getTime() + 5 * 60 * 1000)
                            }
                        };
        
                        // Generate the upload URL with SAS for the new image.
                        var sasQueryUrl = 
                        blobService.generateSharedAccessSignature(item.containerName, 
                        item.resourceName, sharedAccessPolicy);
        
                        // Set the query string.
                        item.sasQueryString = qs.stringify(sasQueryUrl.queryString);
        
                        // Set the full path on the new new item, 
                        // which is used for data binding on the client. 
                        item.imageUri = sasQueryUrl.baseUrl + sasQueryUrl.path;
        
                    } else {
                        console.error(error);
                    }
                    request.execute();
                });
            } else {
                request.execute();
            }
        }

    这将替换插入发生 TodoItem 表中具有新的脚本时，将调用该函数。 此新的脚本生成插入、 它的有效期为 5 分钟，并到生成 SAS 的值赋新 SA`sasQueryString`返回的项的属性。 `imageUri`属性也设置新 BLOB 的资源路径，以在客户端用户界面中的绑定期间启用图像显示。

    >[AZURE.NOTE] 此代码将创建为单个 BLOB SAS。 如果需要将多个 blob 上载到使用相同的 SAS 的容器，您可以改为调用[generateSharedAccessSignature 方法](http://go.microsoft.com/fwlink/?LinkId=390455)</a>与一个空 blob 的资源名称，如下︰ 
    >                 
    >     blobService.generateSharedAccessSignature(containerName, '', sharedAccessPolicy);

接下来，您将更新快速入门应用程序通过使用 SAS 插入生成添加图像上载功能。
 
<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[应用程序设置]: http://msdn.microsoft.com/library/windowsazure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7
