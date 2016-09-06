---
ms.openlocfilehash: 92c67669736440f6a86cf5fa1e3cb2339e26f238
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="执行 Python 机器学习脚本 |Microsoft Azure" 
    description="大纲设计原则在 Azure 机器学习和基本用法的方案、 能力和限制的 Python 脚本支持。" 
    keywords="python 机器学习，pandas，python pandas，python 脚本执行 python 脚本"
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2015" 
    ms.author="bradsev" />


# 在 Azure 机器学习 Studio 执行 Python 机器学习脚本

本主题介绍基础的当前支持的 Azure 机器学习中的 Python 脚本的设计原则。 此外概述主要功能，包括导入现有的代码，导出可视化项支持，并且最后的一些限制和正在进行的工作进行讨论。

[Python](https://www.python.org/)是许多数据科学家的工具胸部中不可缺少的工具。 它具有优雅和简洁的语法，跨平台支持，大量的强大库和成熟的开发工具。 Python 中正在使用的工作流建模、 机器学习中最常用的所有阶段从数据接收和处理，到培训、 验证和部署模型的功能结构。 

Azure 机器学习 Studio 到各个部分的机器学习实验，也无缝地将其发布为 Microsoft Azure 上的可扩展的、 实施了 web 服务支持嵌入的 Python 脚本。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## 在机器学习中的 Python 脚本的设计原则
在 Azure 机器学习 Studio Python 的主要接口是通过[执行 Python 脚本][执行 python 脚本]模块图 1 中所示。

![image1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

图 1。 **执行 Python 脚本**模块中。

[执行 Python 脚本][执行 python 脚本]模块接受多达三个输入，并生成最多两个输出 （在下面讨论），就像其 R 相似之处，[执行 R 脚本][执行 r 脚本]模块。 作为特殊命名入口点在参数框中输入执行 Python 代码调用函数`azureml_main`。 下面是用来实施本模块的主要设计原则︰

1.  *必须为 Python 用户惯用语。* 因此顶级模块中放置大量的可执行语句是相对较少，大多数 Python 用户模块中的函数作为挑选他们的代码。 因此，脚本中还采用特殊命名的 Python 函数而不是只是一个语句序列。 在该函数中公开的对象是标准的 Python 库类型，例如， [Pandas](http://pandas.pydata.org/)数据帧和[NumPy](http://www.numpy.org/)数组。
2.  *必须具有本地之间的高保真和云的执行。* 用于执行 Python 代码的后端基于[Anaconda](https://store.continuum.io/cshop/anaconda/) 2.1，广泛用于跨平台科学 Python 分发。 它配备有接近 200 的最常见的 Python 包。 因此，数据科学家可以调试并限定他们在他或她本地 Azure 机器学习兼容 Anaconda 环境使用现有的开发环境，如[IPython](http://ipython.org/)笔记本或[Visual Studio 的 Python 工具]的代码并运行该 Azure 机器学习实验的一部分的高信任度。 此外，`azureml_main`入口点是香草的 Python 函数，并且可以编写不使用 Azure 机器学习特定代码或安装 SDK。
3.  *必须无缝地与其他 Azure 机器学习模块可组合。* [执行 Python 脚本][执行 python 脚本]模块作为输入和输出，接受标准的 Azure 机器学习数据集。 基础框架透明、 有效地弥补了 Azure 机器学习和 Python 运行时 （支持功能，例如缺少的值）。 因此可在现有的 Azure 机器学习工作流，包括已调入 R 和 SQLite 结合 Python。 一个可以因此设想工作流程序︰
  * 对数据的预处理和清洗，使用 Python 和 Pandas 
  * 将数据传送到 SQL 转换，将多个数据集连接到表单功能 
  * 在 Azure 机器学习，使用广泛的算法模型定型和 
  * 计算和后处理结果使用键。


## 机器学习中的 Python 脚本的基本用法方案
在本节中，我们调查的一些[执行 Python 脚本][执行 python 脚本]模块的基本用法。
如前所述，Pandas 数据帧作为公开任何输入到 Python 模块。 在*数据分析的 Python*中找不到 Python Pandas 以及如何使用它来高效地处理数据的详细信息 (Sebastopol，CA。: O'Reilly，2012年) 由西部 McKinney。 该函数必须返回单个 Pandas 数据帧打包在 Python[规则](https://docs.python.org/2/c-api/sequence.html)如元组，列表或 NumPy 数组。 在第一个模块的输出端口然后返回该序列的第一个元素。 图 2 显示了此方案。

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

图 2。 映射的输入端口的参数和返回值到输出端口。

更详细的方式输入的端口映射到参数的语义`azureml_main`函数表 1 所示︰

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

表 1。 函数参数的输入端口的映射。

请注意，函数参数的输入的端口之间的映射是定位，即连接的第一个输入的端口映射到第一个参数的函数，第二个输入 （如果已连接） 被映射到该函数的第二个参数。

## 输入和输出类型的转换
如上文所述，Azure 机器学习中的输入数据集转换为 Pandas 中的帧和输出数据帧转换回 Azure 机器学习数据集的数据。 执行下列转换︰

1.  字符串和数值的列转换为的是缺少数据集中的值将转换为 Pandas 中的 NA 值。 在返回途中发生相同的转换 （NA Pandas 中的值转换为在 Azure 机器学习中缺少的值）。
2.  Pandas 中的索引向量在 Azure 机器学习中不受支持并且 Python 函数中的所有输入的数据帧始终具有 64 位数字索引从 0 开始通过减 1 的行的数目。 
3.  Azure 机器学习数据集不能有重复的列名称和列不是字符串的名称。 如果输出数据帧包含非数字列，框架调用`str`上的列名称。 同样，任何重复的列名称是自动出错，确保名称是唯一的。 后缀 (2) 被添加到第一个重复项，(3) 为第二个重复，等等。

## 留给 Python 脚本
发布为 web 服务时，会调用计分实验中的任何[执行 Python 脚本][执行 python 脚本]模块。 例如，图 3 显示了包含要计算一个 Python 表达式的代码在计分实验。 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

图 3。 评估一个 Python 表达式的 web 服务。

本实验从创建的 web 服务将作为输入 Python 表达式 （作为字符串），将其发送到 Python 解释器，并返回包含的表达式和计算的结果的表。

## 导入现有的 Python 脚本模块
许多数据科学家的常见用例是将现有的 Python 脚本合并到 Azure 机器学习的实验。 而不是串联，并将所有代码都粘贴到一个脚本中，[执行 Python 脚本][执行 python 脚本]模块接受一个 zip 文件，包含 Python 模块可以连接到第三个输入的端口。 该文件然后解压缩通过在运行时执行框架和内容被添加到 Python 解释器的库路径。 `azureml_main`函数可以然后直接导入这些模块的入口点。

举一个例子，考虑该文件包含一个简单的"Hello，世界"函数的 Hello.py。

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

图 4。 用户定义的函数。

接下来，我们可以创建一个文件 Hello.zip 包含 Hello.py:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

图 5。 Zip 文件包含用户定义的 Python 代码。

然后，此形式上载数据集到 Azure 机器学习 Studio。
如果我们再创建和运行一个简单的试验使用该模块︰

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

图 6。 用户定义的 Python 代码示例体验作为一个 zip 文件上载。

模块输出显示已解包的 zip 文件，而使用函数`print_hello`确实运行。
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)
 
图 7。 在[执行 Python 脚本][执行 python 脚本]模块内使用用户定义的函数。

## 使用可视化效果
创建在浏览器上使用 MatplotLib，可以可视化的图形可以由[执行 Python 脚本][执行 python 脚本]返回。 但图不自动重定向到图像时使用键。因此用户必须显式保存任何图到 PNG 文件如果要返回到 Azure 机器学习。 

为了从 MatplotLib 生成图像，必须竞争的以下过程︰

* 切换到"聚集"的后端从默认的基于 Qt 的呈现程序 
* 创建新的图对象 
* 轴和它中生成所有图 
* 将图形保存到 PNG 文件 

下面的图 8 创建一个散点图矩阵在 Pandas 中使用 scatter_matrix 函数，说明了此过程。
 
![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

图 8。 将 MatplotLib 数字保存到图像中。



图 9 显示了使用通过第二个输出端口返回图上面所示的脚本的一个实验。

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 
     
![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

图 9。 从 Python 代码生成可视化的图形。

请注意，可以通过将其保存到不同的图像返回多个图形，Azure 机器学习运行时拾取所有图像并将它们连接起来的可视化效果。


## 高级的示例
Azure 机器学习中安装 Anaconda 环境包含公共包，如 NumPy、 SciPy 和 Scikits 学习以及这些可有效地用于典型机器学习管道中的各种数据处理任务。 举一个例子，下面的实验和脚本演示如何使用 ensemble 学员中 Scikits-学习如何计算功能重要性分数为数据集。 分数然后可以用于执行前喂养到另一台机器学习模型的监察的功能选择。

Python 函数来计算基于它的功能的重要性分数和顺序如下所示︰

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

图 10。 排名功能正常的成绩。
 
下面的实验然后计算，并返回中"Pima 印度洋糖尿病"数据集在 Azure 机器学习中的重要性分数的功能︰

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    
    
图 11。 Pima 印度洋糖尿病数据集内的排名功能试验。

## 限制 
目前，[执行 Python 脚本][执行 python 脚本]具有以下限制︰

1.  *沙盒执行。* Python 运行时当前沙盒，因此，不允许访问网络或本地文件系统中一种持久的方式。 在本地保存的所有文件是独立且删除模块完成后。 Python 代码不能访问大多数例外的当前目录和其子目录，在运行的计算机上的目录。
2.  *缺乏完善的开发和调试支持。* 目前，Python 模块不支持智能感知和调试等 IDE 功能。 此外，如果该模块在运行时失败，完整的 Python 堆栈跟踪可用，但必须在模块的输出日志中查看。 我们目前建议用户开发和调试他们的 Python 脚本，例如 IPython 的环境中，然后将代码导入模块。
3.  *一个数据帧输出。* 只允许 Python 入口点返回单个数据帧作为输出。 不是当前可以返回到 Azure 机器学习运行时返回任意的 Python 对象，如直接培训模型。 如[执行 R 脚本][执行 r 脚本]，它具有同样的局限性，它但是在很多情况下要 pickle 对象转换为字节数组，然后返回该数据帧内。
4.  *无法 Python 安装进行自定义*。 目前，添加自定义的 Python 模块的唯一办法是通过上文所述的 zip 文件机制。 虽然这是适用于小型的模块，是很大 （尤其是那些具有本机 Dll） 的模块或模块的大量麻烦。 


##结论
[执行 Python 脚本][执行 python 脚本]模块允许数据科学家将现有的 Python 代码合并到云托管的机器学习 Azure 机器学习中的工作流，并无缝地实施它们作为 web 服务的一部分。 Python 脚本模块与 Azure 机器学习中的其他模块的自然互操作，可以用于各种预处理与特征提取、 计算和结果的后期处理从数据探索任务。 后端运行时用于执行基于 Anaconda，经过和广泛使用的 Python 分发。 这样简单的用户对板载现有的代码资产到云。

随后，我们希望提供额外的功能，如能够训练和实施在 Python 中的模型并进行开发和调试代码在 Azure 机器学习 Studio 提供更佳支持[执行 Python 脚本][执行 python 脚本]模块。


<!-- Module References -->
[执行 python 脚本]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[执行 r 脚本]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[Visual Studio 的 Python 工具]: http://aka.ms/ptvs
测试t
