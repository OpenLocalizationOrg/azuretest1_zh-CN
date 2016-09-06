---
ms.openlocfilehash: 880c840cc56c2daf08b26a391f908d24f9e1c223
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="内部使用 blob 存储 (Java) 的应用程序 |Microsoft Azure"
    description="了解如何创建控制台应用程序将图像上载到 Azure，然后在浏览器中显示图像。 在 Java 代码示例。"
    services="storage"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/31/2015" 
    ms.author="robmcm"/>

# 内部部署的应用程序与 blob 存储

## 概述

下面的示例演示如何使用 Azure 存储在 Azure 存储图像。 这篇文章中的代码是一个控制台应用程序，将图像上载到 Azure，然后创建在浏览器中显示的图像的 HTML 文件。

## 先决条件

- Java 开发人员工具箱 (JDK)，1.6 或更高版本已安装。
- Azure SDK 安装。
- 对于 Java，Azure 库 JAR 和任何适用的依赖性 Jar，安装，在 Java 编译器所使用的生成路径。 有关安装 Java Azure 库的信息，请参阅[下载 Java Azure SDK][]。
- 尚未设置 Azure 存储帐户。 这篇文章中的代码将使用的帐户名称和存储帐户的帐户密码。 有关检索帐户密钥有关的信息创建存储帐户，以及[如何管理存储帐户][]的信息，请参阅[如何创建存储帐户]。
- 您已创建一个名为的本地图像文件存储在路径 c:\\myimages\\image1.jpg。 另外，修改  **FileInputStream**的构造函数中使用不同的图像的路径和文件名称的示例。

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## 使用 Azure blob 存储上载的文件

这里介绍的分步过程。 如果您想要跳，整个代码会出现本文中稍后介绍。

首先，包括 Azure 核心存储类、 Azure blob 客户端类、 Java IO 类和**URISyntaxException**类的导入的代码。

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

声明一个名为**StorageSample**，类和包含左括号， **{**。

    public class StorageSample {

在**StorageSample**类中，将声明默认终结点协议、 存储帐户名和您存储的访问密钥，所指定的 Azure 存储帐户将包含一个字符串变量。 替换占位符值**您\_帐户\_名称**和**您\_帐户\_键**使用您的帐户名称和帐户密钥，分别。

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

添加在声明中对**主**，包括**在 try**块中，并且包括有必要打开括号**{**。

    public static void main(String[] args)
    {
        try
        {

变量声明为以下类型 （本例中的使用进行说明）︰

-   **CloudStorageAccount**︰ 用于初始化使用 Azure 存储帐户名和键，该帐户对象以及创建 blob 客户端对象。
-   **CloudBlobClient**︰ 用于访问 blob 服务。
-   **CloudBlobContainer**︰ 用于创建 blob 容器，列出 blob 的容器中，并删除该容器。
-   **CloudBlockBlob**︰ 用于本地图像文件上载到该容器。

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

将一个值分配给**帐户**变量。

    account = CloudStorageAccount.parse(storageConnectionString);

将一个值分配给**serviceClient**变量。

    serviceClient = account.createCloudBlobClient();

为**容器**变量分配值。 我们将对名为**gettingstarted**的容器的引用。

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

创建容器。 如果它不存在 （并返回**true**），则此方法将创建容器。 如果容器不存在，则它将返回**false**。 **CreateIfNotExists**的替代方法是**创建**的方法 （它将返回一个错误，如果该容器已经存在）。

    container.createIfNotExists();

为容器设置匿名访问。

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

获取引用块斑点，这将代表在 Azure 存储中的 blob。

    blob = container.getBlockBlobReference("image1.jpg");

使用**文件**构造函数来获取对将要上传本地创建文件的引用。 请确保在运行代码之前创建了此文件。

    File fileReference = new File ("c:\\myimages\\image1.jpg");

通过调用**CloudBlockBlob.upload**方法的本地文件上载。 **CloudBlockBlob.upload**方法的第一个参数是一个**FileInputStream**对象，该对象表示将上载到 Azure 存储本地文件。 第二个参数是大小，以字节为单位的文件。

    blob.upload(new FileInputStream(fileReference), fileReference.length());

调用帮助器函数**MakeHTMLPage**，以一个基本的 HTML 页面，其中包含名为**&lt;图像&gt;**与源元素设置为现 Azure 存储帐户中的 blob。 **MakeHTMLPage**的代码将在本文后面讨论。

    MakeHTMLPage(container);

打印出一个状态消息和信息创建的 HTML 页面。

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

通过插入右括号关闭**try**块︰ **}**

处理以下异常︰

-   **FileNotFoundException**: **FileInputStream**或**FileOutputStream**的构造函数可能会引发。
-   **StorageException**︰ 可能引发 Azure 的客户端存储库。
-   **URISyntaxException**: **ListBlobItem.getUri**方法可能会引发。
-   **异常**︰ 一般异常处理。

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

通过插入右括号关闭**主**︰ **}**

创建名为**MakeHTMLPage**来创建基本的 HTML 页面的方法。 此方法具有类型**CloudBlobContainer**，它可用于循环访问列表上载 blob 中的参数。 此方法将引发类型**FileNotFoundException**，可引发的异常的构造函数**FileOutputStream** ，和**URISyntaxException**，这可能会引发**ListBlobItem.getUri**方法。 包括左括号，则**{**。

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

创建一个名为**index.html**的本地文件。

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

写入本地文件，添加在**&lt;html&gt;**，**&lt;头&gt;**，和**&lt;体&gt;**元素。

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

循环访问上载 blob 的列表。 对于每个 blob，在 HTML 页面中创建**&lt;m g&gt;**已发送到 blob 的 URI，因为它在 Azure 存储帐户中存在其**src**属性的元素。
虽然如果您添加了更多，在此示例中，添加一个图像，此代码将循环访问所有这些。

为简单起见，此示例假定每个上载的 blob 是图像。 如果您已经更新 blob 以外的图像或页面 blob 处理而不是块 blob，根据需要调整该代码。

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

关闭**&lt;体&gt;**元素和**&lt;html&gt;**元素。

    stream.println("</body>");
    stream.println("</html>");

关闭本地文件。

    stream.close();

通过插入右括号关闭**MakeHTMLPage** : **}**

通过插入右括号关闭**StorageSample** : **}**

下面是此示例的完整代码。 请记住要修改占位符值**您\_帐户\_名称**和**您\_帐户\_键**分别使用您的帐户名称和帐户密钥。

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

除了将您本地映像的文件上载到 Azure 存储中，该代码示例创建本地文件 namedindex.html，可以在浏览器中查看您已上载的图像中打开它。

因为代码中包含您的用户名和帐户密码，确保源代码是安全的。

## 删除容器

负责存储，因为您可以在完成之后删除**gettingstarted**容器试验本示例。 若要删除容器，使用**CloudBlobContainer.delete**方法。

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

若要调用**CloudBlobContainer.delete**方法时，初始化**CloudStorageAccount**、 **ClodBlobClient**和**CloudBlobContainer**对象的过程是相同的**createIfNotExist**方法所示。 下面是一个完整的示例删除名为**gettingstarted**的容器。

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

其他 blob 存储类和方法的概述，请参阅[如何使用 Java 中的 blob 存储服务]。

## 下一步行动

按照这些链接以了解更多关于更复杂的存储任务。

- [Azure 存储 Java SDK][]
- [Azure 存储客户端 SDK 参考][]
- [REST API，azure 存储][]
- [Azure 存储团队博客][]

  [Azure 的 Java SDK 下载]: http://go.microsoft.com/fwlink/?LinkID=525671
  [如何创建存储帐户。]: storage-create-storage-account.md#create-a-storage-account
  [如何管理存储帐户]: storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys
  [如何使用 Java 中的 Blob 存储服务]: storage-java-how-to-use-blob-storage.md
  [Azure 存储 Java SDK]: https://github.com/azure/azure-storage-java
  [Azure 存储客户端 SDK 参考]: http://dl.windowsazure.com/storage/javadoc/
  [REST API，azure 存储]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Azure 存储团队博客]: http://blogs.msdn.com/b/windowsazurestorage/
