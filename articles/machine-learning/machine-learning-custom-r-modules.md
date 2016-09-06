---
ms.openlocfilehash: 39d1c820df716dfa8fbec1d5a8ddf47d8070cff3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="作者在 Azure 的机器学习的自定义 R 模块 |Microsoft Azure" 
    description="创作自定义 Azure 机器学习中的 R 模块的快速入门。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"  
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="tbd" 
    ms.date="07/29/2015" 
    ms.author="bradsev;ankarloff" />


# 作者在 Azure 机器学习中的自定义 R 模块

本主题介绍如何创作并部署自定义 R 在 Azure 机器学习模块。 它说明了什么是 R 的自定义模块以及哪些文件用于定义它们。 它演示了如何构造定义模块的文件以及如何注册在机器学习区部署的模块。 然后详细介绍的元素和属性定义的自定义模块中使用。 此外讨论了如何使用辅助功能、 文件及多个输出。 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 什么是自定义 R 模块？
**自定义模块**是一个用户定义的模块，可以上载到您的工作区以及执行了 Azure 机器学习实验的一部分。 **R 的自定义模块**是执行用户定义 R 函数的自定义模块。 R 是一种编程语言的统计计算和图形，被广泛用于统计学家和数据科学家实现算法。 目前，R 是在自定义模块，所支持的唯一语言，但会在将来添加其他语言支持释放。

自定义模块在意义上，它们可以像任何其他模块使用 Azure 机器学习中具有**一类状态**。 它们可以执行与其他模块，包括在已发布的试验或可视化效果。 用户可以控制模块、 输入和输出端口使用、 建模参数和其他各种运行时行为由实现的算法。 它创建，不能发布到社区的实验区，自定义模块才可用。

## 自定义 R 模块中的文件
由一个至少包含两个文件的.zip 文件定义自定义 R 模块︰

* 实现由模块公开的 R 函数的**源文件**
* 描述的**XML 定义文件**的自定义模块接口

其他的辅助文件也可以包含在.zip 文件中，提供可以通过自定义模块的功能。 此选项是如下所述。

## 快速入门示例︰ 定义，包装，和注册自定义 R 模块
本示例演示如何构造自定义 R 模块所需的文件，它们打包为 zip 文件，并在机器学习区注册该模块。 可以从[此处](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409)下载示例 zip 包和示例文件。

例如，从两个数据集 （数据帧） 修改添加行模块用于连接行 （观察） 的标准实现的**自定义添加行**模块。 标准的添加行模块使用 rbind 算法与第一个输入数据集的结尾追加第二个输入数据集的行。 自定义`CustomAddRows`函数同样接受两个数据集，但也接受附加布尔交换参数作为输入。 如果交换参数为**FALSE**，则将返回相同的数据集，作为标准的实施。 但如果交换参数为**TRUE**，它的第一个输入数据集的行的结尾追加第二个数据集的相反。 文件实现 R`CustomAddRows`由**自定义添加行**模块公开的函数包含下面的 R 代码。

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

公开此`CustomAddRows`Azure 机器学习模块，XML 定义文件的功能必须创建指定**自定义添加行**模块应外观和行为。 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another. Dataset 2 is concatenated to Dataset 1 when Swap is false, and vice versa when Swap is true.</Description>
    
    <!-- Specify the base language, script file and R function to use for this module. -->      
        <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />  
        
    <!-- Define module input and output ports -->
    <!-- Note: The values of the id attributes in the Input and Arg elements must match the parameter names in the R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>Combined dataset</Description>
            </Output>
        </Ports>
        
    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>

 
注意，XML 文件中的**输入**和**Arg**元素的**id**特性的值必须完全匹配的 R 代码的函数参数名称 （*dataset1*、 *dataset2*和*交换*中的示例）。 **语言**元素的**入口点**属性的值与此类似，必须完全匹配在 R 脚本函数的名称 (在此示例中的*CustomAddRows* )。 相反，**输出**元素的**id**属性不对应的 R 脚本中的任何变量。 需要多个输出时，只需返回一个列表从 R 函数放在相同的顺序输出到 XML 文件中声明的结果。

保存为*CustomAddRows.R*和*CustomAddRows.xml*这两个文件，然后压缩这些文件到一个*CustomAddRows.zip*文件。

在机器学习区中注册它们，转到您的工作区中机器学习工作室、 单击底部的**+ 新建**按钮并选择**模块-> 从压缩包**将上载到新的自定义添加行模块。

![](http://i.imgur.com/RFJhCls.png)

**自定义添加行**模块现已准备好通过机器学习实验过程进行访问。

## XML 定义文件中的元素

### 模块元素
该**模块**元素用于在 XML 文件中定义的自定义模块。 可以使用多个**模块**元素的一个 XML 文件中定义多个模块。 工作区中的每个模块必须具有唯一的名称。 注册自定义模块与现有的自定义模块的名称相同，则将用新替换现有的模块。 但是，可以自定义模块与模块组件面板的自定义类别中显示的现有 Azure 机器学习模块，并将具有相同的名称注册。

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another...</Description>/> 


在**Module**元素中，您可以指定一个可选的**所有者**元素嵌入到模块以及**描述**元素，它是快速帮助模块并将鼠标悬停在机器学习的用户界面中的模块中显示的文本。

**规则中的模块元素的字符限制**︰

* 在**Module**元素中的**名称**属性的值不能超过 64 个字符的长度。 
* **Description**元素的内容不能超过 128 个字符的长度。
* 元素内容的**所有者**不能超过 32 个字符的长度。

* * 指示模块的结果是否确定的或不确定

默认情况下，所有模块都都是确定性。 也就是说，给定参数的不变集，模块应该返回相同的结果每一次运行它时。 给出这种现象，Azure 机器学习 Studio 不会重新运行标记为确定性，除非已更改参数或输入的数据的模块。 缓存结果导致试验执行得更快。

但是，如果您的模块使用了一个函数，每次运行它时 — 例如返回不同的结果，RAND 或函数返回当前日期和时间 – 您可以视为非确定性通过可选的**isDeterministic**属性设置为**false**指定的模块。 该模块将重新运行每次运行试验时，即使没有改变输入模块和参数。 

### 语言定义
您的 XML 定义文件中的**语言**元素用于指定的自定义模块语言。 目前，R 是唯一受支持的语言。 **SourceFile**属性的值必须是包含要运行的模块时调用的函数 R 文件的名称。 此文件必须是 zip 包的一部分。 **入口点**属性的值是所调用的函数的名称，并且必须匹配有效函数在源文件中定义的。

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### 端口
子元素的 XML 定义文件的**端口**部分中指定的自定义模块的输入和输出端口。 这些元素中的顺序确定版面有经验 (UX) 的用户。 第一个子**输入**或**输出****端口**元素的 XML 文件中列出将机器学习体验中最左侧的输入的端口
每个输入和输出端口可能有可选**描述**子元素，它指定当用户将鼠标光标悬停在上机器学习的用户界面中的端口时显示的文本。

**端口规则**︰

* 最大**输入和输出端口**数是每个 8。

### 输入的元素
输入的端口允许用户将数据传递到您的工作区和 R 的函数。 有关受支持的**数据类型**输入的端口如下所示︰ 

**数据表︰**这种类型作为 data.frame 传递到您 R 的函数。 事实上，任何类型 （例如，CSV 文件或 ARFF 文件） 所支持的机器学习和**数据表**与兼容自动转换为 data.frame。 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
        </Input>

每个**数据表中**输入端口相关联的**id**属性必须具有一个唯一的值，此值必须与在 R 函数及其相应命名的参数相匹配。
根据实验中的输入的值都不传递的可选**DataTable**端口传递**NULL** R 函数并将忽略可选 zip 端口，如果未连接的输入。 **IsOptional**特性对于**数据表**和**压缩**类型是可选的并为*false* ，默认情况。
       
**邮政编码︰**自定义模块可以作为输入接受一个 zip 文件。 此输入将解压缩到您的函数 R 工作目录

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files will be extracted to the R working directory.</Description>
        </Input>

R 的自定义模块，不需要匹配 R 函数的任何参数，因为 zip 文件自动提取到 R 工作目录 Zip 端口的 id。

**输入的规则︰**

* **Input**元素的**id**特性的值必须是有效的 R 变量名。
* **Input**元素的**id**特性的值不能超过 64 个字符。
* **输入**元素的**名称**属性的值不能超过 64 个字符。
* **Description**元素的内容不能超过 128 个字符
* **Input**元素的**类型**特性的值必须为*Zip*或*数据表*。
* **Input**元素的**isOptional**属性的值不是必需的 （和为*false*时未指定默认情况下）;但是，如果指定了它，则它必须是*真*或*假*。

### 输出元素

**标准的输出端口︰**输出端口从 R 函数，再由后续模块映射到的返回值。 *数据表*是当前支持的唯一的标准输出端口类型。 （支持*学员*和*转换*是即将推出）。*数据表中*输出定义为︰

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

在 R 的自定义模块的输出， **id**属性的值不需要与任何在 R 脚本中，但它必须是唯一的。 单个模块输出时，R 的函数的返回值必须是*data.frame*。 要输出的数据类型支持的多个对象，相应的输出端口需要在 XML 定义文件中指定和对象需要以列表的形式返回。 将分配的输出对象输出端口从左到右，以反映该对象被置于返回列表的顺序。
 
例如，如果您想要输出数据集、 Dataset1 和 Dataset2 分别输出端口数据集、 dataset1 和 dataset2 从左到右，然后输出端口在 CustomAddRows.xml 文件如下定义︰

例如，如果您想要修改**自定义添加行**模块输出原始的两个数据集、 *dataset1*和*dataset2*，此外为新加入数据集*数据集*(顺序，从左到右，为︰*数据集*、 *dataset1*、 *dataset2*)，然后定义输出端口的 CustomAddRows.xml 文件中，如下所示︰

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


然后，在 CustomAddRows.R 中的正确顺序的列表中返回的对象的列表︰

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 
    
**的可视化输出︰**您还可以指定输出端口的类型*的可视化*显示的输出从 R 图形设备和控制台输出。 此端口不是 R 函数输出的一部分并不会干扰的顺序的其他输出端口的类型。 若要将可视化端口添加到自定义模块，添加其**类型**属性的值为*可视化***输出**元素︰

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View the R console graphics device output.</Description>
    </Output>
    
**输出的规则︰**

* **Output**元素的**id**特性的值必须是有效的 R 变量名。
* **Output**元素的**id**特性的值不能超过 32 个字符。
* **Output**元素的**名称**属性的值不能超过 64 个字符。
* **输出**元素的**type**属性值必须是*可视化效果*。

### 参数
其他数据可以传递给 R 函数中，通过**参数**元素中定义的模块参数中。 这些参数显示在右大多数机器学习的用户界面属性窗格中选定模块时。 参数可以是任何受支持的类型，或者您可以创建自定义的枚举时需要。 与**端口**元素相似，**参数**元素可以具有可选**描述**元素，它指定当鼠标悬停在参数名称时显示的文本。
模块，如默认值、 如何和如何为可选属性可以添加到任何参数中，作为**属性**元素的属性。 无效属性的**属性**元素取决于参数类型，并且与以下受支持的参数类型的描述。
与输入和输出，这一点至关重要，每个参数具有与其关联的唯一 id 值。 此外，该 id 值必须对应于您 R 的函数中的命名参数。 在我们的快速入门示例关联的 id 参数是*交换*。

### Arg 元素
使用**参数**部分的 XML 定义文件的**Arg**子元素定义模块参数。 作为子元素在**端口**部分中，在**参数**部分中的参数的顺序定义的布局在体验中遇到 参数将出现从上向下在用户界面中的 XML 文件中的定义的顺序相同。 下面列出了机器学习支持的参数的类型。 

**int** – 一个整数 （32 位） 的类型参数。

        <Arg id="intValue1" name="Int Param" type="int">
            <Properties min="0" max="100" default="0" />
            <Description>Integer Parameter</Description>
       </Arg>



* *可选的属性*︰**最小值**、**最大值**和**默认值**

**双**-双精度类型参数。

       <Arg id="doubleValue1" name="Double Param" type="double">
           <Properties min="0.000" max="0.999" default="0.3" />
           <Description>Double Parameter</Description>
       </Arg>


* *可选的属性*︰**最小值**、**最大值**和**默认值**

**布尔值**--一个由体验中的某个复选框的布尔型参数

        <Arg id="boolValue1" name="Boolean Param" type="bool">
            <Properties default="true" />
            <Description>Boolean Parameter</Description>
        </Arg>



* *可选属性*︰**默认**-假如果未设置

**字符串**︰ 标准字符串

        <Arg id="stringValue1" name="My string Param" type="string">
           <Properties default="Default string value." isOptional="true" />
           <Description>String Parameter 1</Description>
        </Arg>


* *可选属性*︰**默认**和**isOptional** -不包含默认值的可选字符串将传递 null 作为给 R 函数如果用户没有否则提供一个值。

**ColumnPicker**︰ 一个列选择参数。 此类型将在用户体验中呈现为列选择器。 此处使用**属性**元素指定 id 的列将需要从中选择，其中目标端口类型必须是*数据表中*的端口。 列选择的结果将作为列表包含所选的列的名称的字符串传递给 R 的函数。 

        <Arg id="colset" name="Column set" type="ColumnPicker">   
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *所需属性*︰ **portId** -匹配与*数据表*类型的 Input 元素的 id。
* *可选属性*︰
    * **allowedTypes** -筛选的列类型，用户可以选择从。 有效值包括︰ 
        *   Numeric
        *   Boolean
        *   分类
        *   String
        *   对象中，从所有 TermSet 对象中返回不属于
        *   功能
        *   分数
        *   所有

    * **默认**的列选择器的有效的默认选项包括︰ 
        * 无
        * NumericFeature
        * NumericLabel
        * NumericScore
        * NumericAll
        * BooleanFeature
        * BooleanLabel
        * BooleanScore
        * BooleanAll
        * CategoricalFeature
        * CategoricalLabel
        * CategoricalScore
        * CategoricalAll
        * StringFeature
        * StringLabel
        * StringScore
        * StringAll
        * AllLabel
        * AllFeature
        * AllScore
        * 所有

                                                        
**下拉列表**︰ 用户指定枚举 （下拉列表） 的列表。 使用**Item**元素的**属性**元素中指定的下拉列表项。 对于每个**项**的**id**必须是唯一的和有效的 R 变量和项的名称是向用户显示的文本和值传递给 R 函数。

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>  

* *可选属性*︰
    * **默认**-默认属性的值必须与从一个**Item**元素的 id 值。


### 辅助文件

放置在您的自定义模块的 ZIP 文件中的所有文件都将在执行期间可供使用。 如果存在一个目录结构，它将被保留。 这意味着采购文件将处理相同本地及在 Azure 机器学习执行。 请注意，所有文件将被都解压缩到 src 目录以便所有路径都应有 src / 前缀。

例如，假设您要从数据集，删除任何行使用 NAs 和之前将其输出到 CustomAddRows，删除任何重复的行，并且您已经编写了在 RemoveDupNARows.R 文件中的作用，R 函数︰

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
可以源辅助文件 RemoveDupNARows.R 中的 CustomAddRows 函数︰

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
            } else { 
                dataset <- rbind(dataset1, dataset2)) 
            } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

接下来，上载包含 CustomAddRows.R、 CustomAddRows.xml 和 RemoveDupNARows.R 作为自定义 R 模块的 zip 文件。

## 执行环境 ##
R 脚本的执行环境使用相同版本的 R 作为**执行 R 脚本**模块中，并且可以使用相同的默认包。 可以通过自定义模块将 zip 包中包括这些和 R 环境中那样在 R 脚本加载到您的自定义模块添加其他 R 程序包。 

**执行环境的限制**包括︰

* 非持久文件系统︰ 写文件运行该自定义模块时将不会保留跨多个相同模块的运行。
* 无法访问网络



 

测试
