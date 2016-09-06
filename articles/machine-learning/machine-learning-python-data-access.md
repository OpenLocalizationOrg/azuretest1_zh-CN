---
ms.openlocfilehash: da6b685bc68097aa728a0290565a2e84a9e9c24d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="访问与机器学习 Python 客户端库的数据集 |Microsoft Azure" 
    description="安装并使用 Python 的客户端库来访问和本地的 Python 环境中安全地管理 Azure 机器学习数据。" 
    services="machine-learning" 
    documentationCenter="python" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/14/2015" 
    ms.author="huvalo;bradsev" />


#访问与使用 Azure 机器学习 Python 客户端库的 Python 的数据集 

Microsoft Azure 机器学习 Python 客户端库预览可以从本地的 Python 环境到 Azure 机器学习数据集能够安全地访问，并可以创建和管理工作区中的数据集。

本主题提供如何指导︰

* 安装客户端计算机学习 Python 库 
* 访问并上载数据集，包括说明如何获得授权从您本地的 Python 环境访问 Azure 机器学习数据集
*  从实验中访问中间数据集
*  使用 Python 的客户端库可以枚举数据集、 访问元数据、 读取数据集的内容、 创建新的数据集和更新现有数据集

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>先决条件

已在下列环境下测试 Python 客户端库︰

 - Windows、 Mac 和 Linux
 - Python 2.7，3.3 和 3.4

它具有以下软件包的依赖项︰

 - 请求
 - python dateutil
 - pandas

我们建议使用 Python 分发如上面列出[Anaconda](http://continuum.io/downloads#all)或[繁星组成](https://store.enthought.com/downloads/)，随 Python，IPython 和三个包的安装。 尽管 IPython 并不严格要求，它是用于操作和交互式可视化数据良好的环境。


###<a name="installation"></a>如何安装 Azure 机器学习 Python 客户端库

Azure 机器学习 Python 客户端库还必须安装才能完成本主题中列出的任务。 它是从[Python 包索引](https://pypi.python.org/pypi/azureml)可用。 以 Python 的环境中安装，请从您本地的 Python 环境中运行以下命令︰

    pip install azureml

或者，您可以下载并安装从[github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python)上的源。

    python setup.py install

如果您有在您的计算机上安装 git，您可以使用 pip 直接安装 git 存储库︰

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Studio 代码段用于访问数据集

Python 的客户端库可以以编程方式访问现有数据集从已运行的实验。

在 Studio web 界面上，您可以生成包含所有必要信息，下载和反数据集作为位置的计算机上的 Pandas DataFrame 对象序列化的代码段。

### <a name="security"></a>安全的数据访问

使用 Python 的客户端库包括您的工作区 id 和授权提供 Studio 代码段标记。 这些提供给您的工作区的完全访问权限，并且必须保护，类似于密码。

出于安全考虑，才可以将**所有者**设置为该工作区的角色的用户可以使用代码段的功能。 您的角色**设置**下**用户**页上显示在 Azure 机器学习 Studio。

![安全性][security]

如果不将您的角色设置为**所有者**，您可以请求重新被邀请作为所有者，或要求您提供的代码段的工作区的所有者。

若要获得授权标记，可以执行以下任一操作︰

1. 询问一个令牌从所有者。 所有者可以访问其授权令牌 Studio 在其工作区中的设置页面。 从左窗格中选择**设置**，然后单击**授权标记**以查看主、 辅标记上。  尽管主或辅助授权令牌可以使用代码段中，建议所有者只共享辅助授权令牌。

    ![](http://i.imgur.com/h33GoZX.jpg)

2. 被提升为所有者的角色要求。  要做到这一点，需要首先将您从工作区中删除，然后重新邀请您参加它的所有者当前工作区的所有者。

一旦开发人员获得工作区 id 和授权令牌，它们将能够访问工作区中使用代码段而不考虑其角色。

在**设置**下**授权标记**页上管理授权令牌。 您可以重新生成它们，但此过程会撤消前一个标记访问。

### <a name="accessingDatasets"></a>从本地的 Python 应用程序访问数据集

1. 在机器学习 Studio 中，在**数据集**上单击左侧导航栏中。

2. 选择您想要访问的数据集。 从**我的数据集**列表或**示例**列表，您可以选择任何数据集。

3. 从底部工具栏中，单击在**生成数据访问代码**。 注意是否数据与 Python 客户端库不兼容的格式，将禁用此按钮。

    ![数据集][datasets]

4. 从窗口显示，并将其复制到剪贴板中选择代码段。

    ![访问代码][dataset-access-code]

5. 将代码粘贴到您本地的 Python 应用的笔记本。

    ![笔记本][ipython-dataset]

### <a name="accessingIntermediateDatasets"></a>访问中间数据集从机器学习的实验

实验运行机器学习 Studio 中后，就可以从模块的输出节点访问的中间数据集。 中间数据集是数据已被创建并已运行模型工具时用的中间步骤。

只要是 Python 客户端库与兼容的数据格式，则可以访问中间数据集。

支持以下格式 (这些常数是在`azureml.DataTypeIds`类):

 - 纯文本
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

您可以通过将鼠标指向某个模块输出节点确定格式。 它和节点名称，在工具提示中显示。

一些模块，如[拆分][拆分]模块，输出为格式名为`Dataset`，这 Python 客户端库不支持。

![数据集格式][dataset-format]

您将需要使用转换模块，如[转换为 CSV][转换为 csv]，进入受支持的格式输出。

![GenericCSV 格式][csv-format]

下面的步骤演示了一个示例，创建一个实验、 运行和访问中间数据集。

1. 创建新的实验。

2. 插入一个**成人人口普查收入二元分类数据集**模块。

3. 插入一个[拆分][拆分]模块，并将其输入到数据集模块输出的连接。

4. 将[转换为 CSV][转换为 csv]模块插入并连接到一个[拆分][拆分]模块输出其输入。

5. 保存实验，运行它，并等待其完成运行。

6. 单击输出节点[转换为 CSV][转换为 csv]模块上。

7. 将显示一个上下文菜单，选择**生成数据访问代码**。

    ![上下文菜单][experiment]

8. 将出现一个窗口。 选择代码段并将其复制到剪贴板。

    ![访问代码][intermediate-dataset-access-code]

9. 将代码粘贴到您的笔记本中。

    ![笔记本][ipython-intermediate-dataset]

10. 您可以可视化数据使用 matplotlib。 这将显示在年龄列的直方图中︰

    ![直方图][ipython-histogram]


##<a name="clientApis"></a>使用机器学习 Python 客户端库可以访问、 读取、 创建和管理数据集

### Workspace

工作区是 Python 客户端库的入口点。 提供`Workspace`与您的工作区 id 和授权的类标记来创建实例︰

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### 枚举数据集

若要枚举给定工作区中的所有数据集︰

    for ds in ws.datasets:
        print(ds.name)

若要枚举只是用户创建的数据集︰

    for ds in ws.user_datasets:
        print(ds.name)

若要枚举只是示例数据集︰

    for ds in ws.example_datasets:
        print(ds.name)

您可以访问数据集的名称 （区分大小写）︰

    ds = ws.datasets['my dataset name']

或者，您可以按索引访问它︰

    ds = ws.datasets[0]


### 元数据

数据集有元数据，以及内容。 （中间数据集都将此规则的例外，不具有任何元数据）

在创建时，由用户分配某些元数据值︰

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

有些则是通过 Azure ML 赋的值︰

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

请参阅`SourceDataset`上可用元数据的详细信息的类。


### 阅读内容

代码段自动通过机器学习 Studio 提供下载，并反序列化到一个 Pandas DataFrame 对象的数据集。 这是与`to_dataframe`方法︰

    frame = ds.to_dataframe()

如果您希望下载的原始数据，并自己执行反序列化，这是一个选项。 此刻，这是唯一的选项，如 ARFF'，Python 的客户端库不能反序列化格式。

以文本形式读取内容︰

    text_data = ds.read_as_text()

作为二进制文件中读取内容︰

    binary_data = ds.read_as_binary()

还可以打开内容流︰

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### 创建新的数据集

Python 的客户端库允许您将数据集从 Python 程序上载。 这些数据集可用于您的工作区。

如果您有在 Pandas DataFrame 的数据，请使用以下代码︰

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

如果您的数据已序列化，您可以使用︰

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Python 的客户端库都无法序列化以下格式为 Pandas DataFrame (对于这些常数是在`azureml.DataTypeIds`类):

 - 纯文本
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### 更新现有数据集

如果您试图上载新的数据集以匹配现有的数据集名称，则将获得冲突错误。

若要更新现有数据集，首先需要获取对现有数据集的引用︰

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

然后使用`update_from_dataframe`进行序列化和替换在 Azure 上的数据集的内容︰

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

如果您想要序列化的数据为不同的格式，指定的值的可选`data_type_id`参数。

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

您可以选择通过指定的值设置新的描述`description`参数。

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

您可以选择设置一个新名称，由指定的值`name`参数。 从现在起，就可以检索数据集使用的新名称。 以下代码将更新的数据、 名称和说明。

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

`data_type_id`， `name` ，`description`参数都是可选，并默认为其以前的值。 `dataframe`参数始终是必需的。

如果您的数据已序列化，则使用`update_from_raw_data`而不是`update_from_dataframe`。 它的工作类似，只是通过在`raw_data`而不是`dataframe`。



<!-- Images -->
[安全性]:./media/machine-learning-python-data-access/security.png
[数据集格式]:./media/machine-learning-python-data-access/dataset-format.png
[csv 格式]:./media/machine-learning-python-data-access/csv-format.png
[数据集]:./media/machine-learning-python-data-access/datasets.png
[数据集访问代码]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython 数据集]:./media/machine-learning-python-data-access/ipython-dataset.png
[实验]:./media/machine-learning-python-data-access/experiment.png
[中间数据集访问代码]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython 中间数据集]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython 直方图]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[转换为 csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[拆分]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
测试
