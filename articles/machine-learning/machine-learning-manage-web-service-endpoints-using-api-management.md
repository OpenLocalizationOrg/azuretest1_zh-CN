---
ms.openlocfilehash: c63162aa837c05ff4bbc30258e1d4ae130133297
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="了解如何管理 AzureML web 服务使用 API 管理 |Microsoft Azure"
    description="演示如何管理 AzureML web 服务使用 API 管理指南。"
    keywords="机器学习，api 管理"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="paulettm"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="roalexan" />


# 了解如何管理使用 API 管理 AzureML web 服务

##概述

本指南介绍了如何快速开始使用 API 管理来管理您的 AzureML web 服务。

##什么是 Azure API 管理？

Azure 的 API 管理是 Azure 服务，允许您通过定义用户访问权限、 使用带宽限制功能，和仪表板监视管理 REST API 端点。 单击[此处](http://azure.microsoft.com/services/api-management/)在 Azure API 管理的详细信息。 单击[此处](api-management/api-management-get-started.md)有关如何开始使用 Azure API 管理的指南。 该其他指南，这本指南根据，涵盖多个主题，其中包括通知配置层定价、 响应处理、 用户身份验证创建产品、 开发人员预订和使用 dashboarding。

##AzureML 是什么？

AzureML 是 Azure 服务使您能够轻松地构建、 部署和共享高级分析功能解决方案的机器学习的。 单击[此处](http://azure.microsoft.com/services/machine-learning/)有关 AzureML 的详细信息。

##先决条件

若要完成本指南，您需要︰

* Azure 帐户。 如果您没有 Azure 帐户，请单击[此处](http://azure.microsoft.com/pricing/free-trial/)有关如何创建免费的试用帐户的详细信息。
* AzureML 帐户。 如果没有 AzureML 帐户，请单击[此处](https://studio.azureml.net/)有关如何创建免费的试用帐户的详细信息。
* 工作区、 服务和 AzureML 实验作为 web 服务发布的 api_key。 单击[此处](machine-learning/machine-learning-create-experiment.md)为详细介绍了如何创建一个 AzureML 实验。 单击[此处](machine-learning/machine-learning-publish-a-machine-learning-web-service.md)有关如何发布 AzureML 实验作为 web 服务的详细信息。 此外，附录 A 已说明如何创建和测试简单的 AzureML 实验，将其作为 web 服务发布。

##创建 API 管理实例

下面是使用 API 管理来管理您的 AzureML web 服务的步骤。 首先创建一个服务实例。 登录到[管理门户](https://manage.windowsazure.com/)并单击**新建** > **应用程序服务** > **API 管理** > **创建**。

![创建实例](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

指定一个唯一的**URL**。 本指南使用**demoazureml** ，您将需要选择不同的地方。 服务实例中选择所需的**订阅**和**地区**。 您的选择后，单击下一步按钮。

![创建服务 1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

指定的**组织名称**的值。 本指南使用**demoazureml** ，您将需要选择不同的地方。 **管理员电子邮件**字段中输入您的电子邮件地址。 此电子邮件地址用于从 API 管理系统的通知。

![创建服务 2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

单击此复选框可创建服务实例。 *需要三十分钟的时间新服务创建的*。

##创建 API

服务实例创建之后下, 一步是创建 API。 API 包含一组可以从客户端应用程序调用的操作。 API 操作是对现有 web 服务代理。 本指南创建 Api 对现有的 AzureML RR 和 BES web 服务的代理。

创建和配置从 API 发布门户网站，通过 Azure 管理门户访问 Api。 若要达到发布者门户，选择服务实例。

![选择服务实例](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

单击**管理**API 管理服务 Azure 门户中。

![管理服务](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

单击在左侧， **API 管理**菜单中的**Api** ，然后单击**添加 API**。

![api 管理菜单](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

作为**Web API 名称**键入**AzureML 演示 API** 。 键入**https://ussouthcentral.services.azureml.net**作为**Web 服务 URL**。 键入**Web API URL 后缀**为**azureml 演示**。 检查**HTTPS**作为**Web API URL**方案。 选择**初学者**作为**产品**。 完成后，单击**保存**以创建 API。

![添加新的 api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##添加操作

单击**添加操作**将操作添加到此 API。

![添加操作](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

将显示**新建操作**窗口中，将默认选中**签名**选项卡。

##添加的 RR 操作

首先创建 AzureML RR 服务操作。 作为**HTTP 谓词**选择**开机自检**。 类型**/workspaces/ {区} {} 系统 /services/ / execute?api 版本 = {apiversion} & 详细信息 = {详细信息}**为**URL 模板**。 以**显示名称**中键入**RR 执行**。

![添加的 rr 操作签名](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

单击**响应** > 左并选择**添加** **200 确定**。 单击**保存**以保存此操作。

![添加的 rr 操作响应](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##添加 BES 操作

因为它们是非常类似于添加的 RR 操作，将不包括为 BE 操作屏幕抓图。

###提交 （但是不启动） 执行批处理作业

单击**添加操作**api 添加 AzureML BE 操作。 对于**HTTP 谓词**选择**开机自检**。 类型**/workspaces/ {区} {} 系统 /services/ / jobs?api 版本 = {apiversion}** **URL 模板**。 **显示名称**，请键入**BES 提交**。 单击**响应** > 左并选择**添加** **200 确定**。 单击**保存**以保存此操作。

###开始执行批处理作业

单击**添加操作**api 添加 AzureML BE 操作。 对于**HTTP 谓词**选择**开机自检**。 类型**/workspaces/ {区} {} 系统 /services/ /jobs/ {作业 id} / start?api 版本 = {apiversion}** **URL 模板**。 **显示名称**，请键入**BE 开始**。 单击**响应** > 左并选择**添加** **200 确定**。 单击**保存**以保存此操作。

###获取状态或在执行批处理作业的结果

单击**添加操作**api 添加 AzureML BE 操作。 选择**获取**的**HTTP 谓词**。 类型**/workspaces/ {区} {} 系统 /services/ /jobs/ {作业 id} ?api 版本 = {apiversion}** **URL 模板**。 **BES 状态**输入**显示名称**。 单击**响应** > 左并选择**添加** **200 确定**。 单击**保存**以保存此操作。

###删除执行批处理作业

单击**添加操作**api 添加 AzureML BE 操作。 对于**HTTP 谓词**中选择**删除**。 类型**/workspaces/ {区} {} 系统 /services/ /jobs/ {作业 id} ?api 版本 = {apiversion}** **URL 模板**。 **显示名称**，请键入**BE 删除**。 单击**响应** > 左并选择**添加** **200 确定**。 单击**保存**以保存此操作。

##从开发人员门户中调用操作

可以直接从开发人员门户中，可以很方便地查看和测试操作的 API 调用操作。 在本指南的步骤中，您将调用已添加到**AzureML 演示 API**的**RR 执行**方法。 单击顶部菜单中的**开发人员门户**的管理门户。

![开发人员门户](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

从顶部的菜单中，单击**Api** ，然后单击**AzureML 演示 API**以查看可用的操作。

![demoazureml api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

选择**RR 执行**该操作。 单击**试试看**。

![try it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

请求的参数，请键入您**工作区**、**服务**、 **2.0**的**apiversion**，和**真正**的**详细信息**。 AzureML web 服务面板中，可以找到您的**工作区**和**服务**（请参阅附录 A 中**测试 web 服务**）。

请求标头，请单击**添加标题**，然后键入**内容类型**和**应用程序/json**，然后单击**添加标题**并键入**授权**和**载体<YOUR AZUREML SERVICE API-KEY>**。 AzureML web 服务面板中，可以找到您的**api 密钥**（请参见附录 A 中**测试 web 服务**）。

类型**{"输入": {"输入 1": {"ColumnNames":"Col2""值": [["这是很好的一天"]]}}，"GlobalParameters": {}}**请求正文。

![azureml 演示 api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

单击**发送**。

![send](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

调用的操作后，开发人员门户将显示从后端服务、**响应状态**、**响应标头**和任何**响应内容**的**请求的 URL** 。

![响应状态](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##附录 A-创建和测试简单的 AzureML 的 web 服务

###创建实验

以下是用于创建简单的 AzureML 实验和将其发布为 web 服务的步骤。 Web 服务将用作输入列的任意文本，并返回一组表示为整数的功能。 例如︰

文字 | 哈希的文本
--- | ---
这是很好的一天 | 1 1 2 2 0 2 0 1

首先，使用您选择的浏览器导航到︰ [https://studio.azureml.net/](https://studio.azureml.net/) ，并输入您的凭据登录。 接下来，创建新的空白实验。

![实验的搜索模板](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

将其重命名为**SimpleFeatureHashingExperiment**。 展开**保存数据集**并拖动实验**从 Amazon 的书评**。

![简单-功能-哈希-实验](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

展开**数据转换**和**操作**，实验中拖动**项目列**。 连接到**项目列****从 Amazon 的书评**。

![项目列](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

单击**项目列**然后单击**启动列选定器**并选择**第 2 列**。 单击选中要应用这些更改。

![选择列](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

展开**文本分析**，拖动到**哈希功能**实验。 **哈希功能**连接**项目的列**。

![连接项目列](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

**Hashing bitsize**，请键入**3** 。 这将创建 8 (23) 的列。

![哈希 bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

此时，您可以单击**运行**以测试实验。

![运行](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###创建 web 服务

现在创建的 web 服务。 展开**Web 服务**，然后**输入**拖动实验。 **哈希功能**连接**输入**。 此外将**输出**拖到实验。 **哈希功能**连接**输出**。

![输出到功能希](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

单击**发布 web 服务**。

![发布 web 服务](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

单击**是**以发布实验。

![发布是](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###测试 web 服务

AzureML web 服务包括 RSS （请求/响应服务） 和 BES （批处理执行服务） 终结点。 RSS 是为同步执行。 BE 是异步作业执行。 若要测试您的 web 服务与下面的示例 Python 源，您可能需要下载并安装的 Python 的 Azure SDK (请参见︰[如何安装 Python](python-how-to-install.md))。

下面的示例源将还需要**工作区**、**服务**和**api_key**的实验。 通过为实验在 web 服务面板中单击**请求/响应**或**执行批处理**，您可以找到工作区和服务。

![查找工作区和服务](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

您可以通过单击 web 服务面板中的实验发现**api_key** 。

![查找 api 密钥](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####测试 RR 终结点

#####测试按钮

测试的 RR 终结点的简单方法是在 web 服务面板中单击**测试**。

![测试](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

**第 2 列**，请键入**这是很好的一天**。 单击复选标记。

![输入数据](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

您将看到类似

![示例输出](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####示例代码

另一种方法来测试您的 RR 是从客户端代码。 如果单击**请求/响应**的仪表板和滚动到底部，您将看到示例代码 C#、 Python 和。您还会看到该 RR 请求，包括请求的 URI 的语法标头和正文。

本指南介绍了 Python 的示例工作。 您将需要使用**工作区**、**服务**、 和**api_key**的实验对其进行修改。

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####测试 BES 终结点
单击**执行批处理**的仪表板和滚动到底部。 您将看到示例代码为 C#，Python 和。您还将看到的语法的 BE 的请求提交作业、 开始工作、 获得的状态或结果的一项工作，和删除作业。

本指南介绍了 Python 的示例工作。 您需要使用**工作区**、**服务**、 和**api_key**的实验对其进行修改。 此外，您需要修改**存储帐户名称**、**帐户密钥存储**和**存储容器名称**。 最后，您将需要修改**输入的文件**的位置和**输出文件**的位置。

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()

测试
