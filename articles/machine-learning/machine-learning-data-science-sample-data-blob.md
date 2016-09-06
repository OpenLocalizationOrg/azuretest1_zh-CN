---
ms.openlocfilehash: 5fa46fa49caeb355e988462f44e34660da2467ee
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure Blob 存储中的数据采样 |Microsoft Azure" 
    description="在 Azure Blob 存储示例数据" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="msolhab" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="sunliangms;fashah;msolhab;garye;bradsev" /> 

#<a name="heading"></a>在 Azure Blob 存储示例数据

本文介绍了通过以编程方式下载它，然后它采样与 Python 代码示例存储在 Azure Blob 存储的采样数据。 若要执行此操作的步骤如下所示︰

## 下载和下示例数据
1. 从 Azure blob 存储使用 blob 服务从下面的 Python 代码示例下载数据︰ 

        from azure.storage import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))

2. Pandas 数据的帧从上面下载的文件中读入数据。

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. 向下双样本数据使用`numpy`的`random.choice`，如下所示︰

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

    现在，您可以使用与 1%样本的进一步研究和功能生成上面的数据帧。

##<a name="heading"></a>上载数据并读入 Azure 机器学习

下面的代码示例可用于下双样本数据，并直接在 Azure ML 中使用它︰

1. 数据帧写入本地文件

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. 将本地文件上载到 Azure 的斑点，使用下面的代码示例︰

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. 从 Azure blob 中下面的图像所示使用 Azure ML[读取器](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/)读取的数据︰
 
![读取器 blob][1]

[1]: ./media/machine-learning-data-science-sample-data-blob/reader_blob.png


<!-- Module References -->
[读取器]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
测试
