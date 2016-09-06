---
ms.openlocfilehash: 41ca5eadff8ec1b847307c5272355ae8cbfa06c2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="配置单元和猪的 HDInsight 中使用 Python |Microsoft Azure"
    description="了解如何在 HDInsight，在 Azure 上 Hadoop 技术堆栈中使用 Python 用户定义函数 (UDF) 从配置单元和小猪。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="07/06/2015" 
    ms.author="larryfr"/>

#使用 Python 配置单元和 HDInsight 中的小猪

配置单元和小猪非常适合处理的数据在 HDInsight，但有时您需要较一般用途语言。 配置单元和小猪，可以创建用户定义函数 (UDF) 使用不同的编程语言。 在本文中，您将学习如何使用 Python UDF 从配置单元和小猪。

> [AZURE.NOTE] 本文中的步骤适用于 HDInsight 群集版本 2.1、 3.0、 3.1 和 3.2。


##<a name="python"></a>Python 在 HDInsight

默认情况下，HDInsight 3.0 和更高版本的群集上安装 Python2.7。 配置单元可以用于与此版本的 Python （配置单元和 Python 使用标准输出/STDIN 之间传递数据） 流处理。

HDInsight 还包括 Jython，是用 Java 编写的 Python 实现。 小猪能理解如何去交谈 Jython 而不必求助于流中，以便更好地使用小猪时。

###<a name="hivepython"></a>配置单元和 Python

Python 可以用作 UDF 从配置单元通过**变换**HiveQL 语句。 例如，下面的 HiveQL 调用存储在**streaming.py**文件中的 Python 脚本。

**基于 linux * 的 HDInsight**

    add file wasb:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

**基于 Windows 的 HDInsight**

    add file wasb:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'D:\Python27\python.exe streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

> [AZURE.NOTE] 在基于 Windows 的 HDInsight 群集上**USING**子句必须指定 python.exe 的完整路径。 这始终是`D:\Python27\python.exe`。

下面是此示例的用途︰

1. **将文件添加**语句开头的文件向**streaming.py**文件分布式缓存，因此它是群集中所有节点可以访问。

2. **选择转换...使用**语句从**hivesampletable**中，选择数据，然后将客户机 id、 devicemake 和 devicemodel 传递给**streaming.py**脚本。

3. **AS**子句说明了从**streaming.py**返回的字段

<a name="streamingpy"></a>
这里是 HiveQL 示例所使用的**streaming.py**文件。

    #!/usr/bin/env python

    import sys
    import string
    import hashlib

    while True:
      line = sys.stdin.readline()
      if not line:
        break

      line = string.strip(line, "\n ")
      clientid, devicemake, devicemodel = string.split(line, "\t")
      phone_label = devicemake + ' ' + devicemodel
      print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])

由于我们使用流，此脚本必须执行下列操作︰

1. 从标准输入读取数据。 这通过使用`sys.stdin.readline()`在此示例中。

2. 尾部换行符被删除使用`string.strip(line, "\n ")`，因为我们只是需要的文本数据和未行指示符结尾。

2. 在进行流处理时，单个行包含用制表符字符之间的每个值的所有值。 因此`string.split(line, "\t")`可用于拆分处返回的字段只是每个选项卡上输入。

3. 处理完成后，必须为一条线，并用一个选项卡之间的每个字段到 STDOUT 写入输出。 这通过使用`print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`。

4. 所有发生在`while`循环中，程序会重复，直到无`line`被读取，此时`break`退出循环和脚本终止。

此外，该脚本只是串联的输入的值`devicemake`， `devicemodel`，并计算串联值的哈希值。 很简单的但它描述了从配置单元调用任何 Python 脚本的运行方式的基础知识︰ 循环播放，读取输入直到没有更多，分离的每行输入在选项卡，进程，编写单个制表符分隔输出行。

请参阅有关如何在 HDInsight 群集上运行此示例的[运行示例](#running)。

###<a name="pigpython"></a>小猪和 Python

可通过**生成**语句作为 UDF 从猪的 Python 脚本。 例如，下面的示例使用存储在**jython.py**文件中的 Python 脚本。

    Register 'wasb:///jython.py' using jython as myfuncs;
    LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
    LOG = FILTER LOGS by LINE is not null;
    DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
    DUMP DETAILS;

下面是此示例如何工作︰

1. 它注册文件包含使用**Jython**，Python 脚本 (**jython.py**)，并将其公开给猪为**myfuncs**。 Jython 中 Java，Python 实现，并且在为猪的同一 Java 虚拟机中运行。 这样，我们就将与传统函数类似的 Python 脚本使用的配置单元的流方法与调用。

2. 下一行将加载示例数据文件中， **sample.log**到**日志**中。 由于此日志文件没有一致的架构，它还定义了每个记录 （在此例中，**行**） 为**chararray**。 实质上，Chararray 是一个字符串。

3. 第三行筛选出 null 值的字段，存储到**日志**操作的结果。

4. 接下来，它循环访问**日志**中的记录并使用**生成**调用加载为**myfuncs** **jython.py**脚本中包含的**create_structure**方法。  **行**用于将当前记录传递给该函数。

5. 最后，输出转储到标准输出使用**转储**命令。 这只是在操作完成; 之后会立即显示结果在实际脚本中像通常那样**存储**数据到一个新文件。

<a name="jythonpy"></a>
这里是猪的示例使用**jython.py**文件︰

    @outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
    def create_structure(input):
      if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
      date, time, classname, level, detail = input.split(' ', 4)
      return date, time, classname, level, detail

还记得，我们以前只是定义输入不一致的架构，因此输入 chararray 作为**行**吗？ **Jython.py**的作用是将数据转换为输出一致的架构。 其工作原理如下︰

1. **@OutputSchema**语句定义将返回到小猪的数据的格式。 在这种情况下，它是**数据袋**，这是猪的数据类型。 包包含了以下字段，它们都是 chararray （字符串）︰

    * 日期-创建该日志条目的日期
    * 时间-日志条目的创建的时间
    * 类名-为创建项的类名
    * 级别的日志级别
    * 详细信息 — 详细日志条目详细信息

2. 接下来，**定义 create_structure(input)**定义猪将传递到行项的函数。

3. 示例数据， **sample.log**，主要是符合日期、 时间、 类名、 级别和我们想要返回的主体架构。 但它也包含字符串*java.lang.Exception*，需要进行修改以匹配的模式开头的几行。 **If**语句检查，然后 massages 移动到结束时，将数据中的行与我们预期的输出架构的*java.lang.Exception*字符串的输入的数据。

4. 接下来，**拆分**命令用于拆分处的前四个空格字符的数据。 这将导致分配**日期**、**时间**、**类名**、**级别**，和**详细**的五个值。

5. 最后，对猪的返回值。

当数据返回给小猪时，它将在**@outputSchema**语句中定义具有一致的架构。

##<a name="running"></a>运行示例

如果您正在使用基于 Linux 的 HDInsight 群集，使用**SSH**执行以下步骤。 如果您使用的基于 Windows HDInsight 群集和 Windows 客户端，使用**PowerShell**步骤。

###SSH

使用 SSH 的详细信息，请参阅<a href="../hdinsight-hadoop-linux-use-ssh-unix/" target="_blank">使用 SSH 使用 HDInsight 从 Linux、 Unix 或 OS X 上的基于 Linux 的 Hadoop</a>或<a href="../hdinsight-hadoop-linux-use-ssh-windows/" target="_blank">使用 SSH 与从 Windows HDInsight 上的基于 Linux 的 Hadoop</a>。

1. 使用 Python 示例[streaming.py](#streamingpy)和[jython.py](#jythonpy)，创建您的开发计算机上的文件的本地副本。

2. 使用`scp`文件复制到 HDInsight 群集。 例如，以下会将文件复制到一个名为**mycluster**的群集。

        scp streaming.py jython.py myuser@mycluster-ssh.azurehdinsight.net:

3. 使用 SSH 连接到群集。 例如，以下将连接到群集命名为用户**myuser** **mycluster** 。

        ssh myuser@mycluster-ssh.azurehdinsight.net

4. 从 SSH 会话中，添加到群集的 WASB 存储以前上载的 python 文件。

        hadoop fs -copyFromLocal streaming.py /streaming.py
        hadoop fs -copyFromLocal jython.py /jython.py

后上传文件，使用以下步骤运行配置单元和猪的作业。

####配置单元

1. 使用`hive`命令以启动配置单元外壳。 您应该会看到`hive>`提示后外壳程序已加载。

2. 在输入以下`hive>`提示。

        add file wasb:///streaming.py;
        SELECT TRANSFORM (clientid, devicemake, devicemodel)
          USING 'streaming.py' AS
          (clientid string, phoneLabel string, phoneHash string)
        FROM hivesampletable
        ORDER BY clientid LIMIT 50;

3. 在输入后的最后一行，应开始作业。 最终它将返回输出类似于以下。

        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####小猪

1. 使用`pig`命令来启动外壳程序。 您应该会看到`grunt>`提示后外壳程序已加载。

2. 输入以下语句在`grunt>`提示。

        Register wasb:///jython.py using jython as myfuncs;
        LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;

3. 输入以下行后, 应开始作业。 最终它将返回输出类似于以下。

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

###PowerShell

这些步骤将使用 Azure PowerShell。 如果这已经不是安装和配置开发计算机上，请使用下列步骤之前看到[如何安装和配置 Azure PowerShell](../install-configure-powershell.md) 。

1. 使用 Python 示例[streaming.py](#streamingpy)和[jython.py](#jythonpy)，创建您的开发计算机上的文件的本地副本。

2. 使用下面的 PowerShell 脚本的**streaming.py**和**jython.py**文件上载到服务器。 替换 Azure HDInsight 群集和脚本的前三行上的**streaming.py**和**jython.py**文件的路径的名称。

        $clusterName = YourHDIClusterName
        $pathToStreamingFile = "C:\path\to\streaming.py"
        $pathToJythonFile = "C:\path\to\jython.py"

        $hdiStore = get-azurehdinsightcluster -name $clusterName
        $storageAccountName = $hdiStore.DefaultStorageAccount.StorageAccountName.Split(".",2)[0]
        $storageAccountKey = $hdiStore.defaultstorageaccount.storageaccountkey
        $defaultContainer = $hdiStore.DefaultStorageAccount.StorageContainerName

        $destContext = new-azurestoragecontext -storageaccountname $storageAccountName -storageaccountkey $storageAccountKey
        set-azurestorageblobcontent -file $pathToStreamingFile -Container $defaultContainer -Blob "streaming.py" -context $destContext
        set-azurestorageblobcontent -file $pathToJythonFile -Container $defaultContainer -Blob "jython.py" -context $destContext

    此脚本检索信息 HDInsight 群集，然后提取帐户和默认的存储帐户，键，会将文件上载到根容器。

    > [AZURE.NOTE] [将 HDInsight 中的 Hadoop 作业的数据上载](hdinsight-upload-data.md)文档中找不到上载脚本的其他方法。

上传文件之后, 使用以下 PowerShell 脚本开始作业。 作业完成时输出应写入 PowerShell 控制台。

####配置单元

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName

    $HiveQuery = "add file wasb:///streaming.py;" +
                 "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                   "USING 'D:\Python27\python.exe streaming.py' AS " +
                   "(clientid string, phoneLabel string, phoneHash string) " +
                 "FROM hivesampletable " +
                 "ORDER BY clientid LIMIT 50;"

    $jobDefinition = New-AzureHDInsightHiveJobDefinition -Query $HiveQuery -StatusFolder '/hivepython'

    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for the Hive job to complete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    # Uncomment the following to see stderr output
    # Get-AzureHDInsightJobOutput -StandardError -JobId $job.JobId -Cluster $clusterName
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput

**配置单元**作业的输出应类似于以下内容︰

    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####小猪

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName

    $PigQuery = "Register wasb:///jython.py using jython as myfuncs;" +
                "LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);" +
                "LOG = FILTER LOGS by LINE is not null;" +
                "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
                "DUMP DETAILS;"

    $jobDefinition = New-AzureHDInsightPigJobDefinition -Query $PigQuery -StatusFolder '/pigpython'

    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    # Uncomment the following to see stderr output
    # Get-AzureHDInsightJobOutput -StandardError -JobId $job.JobId -Cluster $clusterName
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput

**小猪**作业的输出应类似于如下︰

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

##<a name="troubleshooting"></a>疑难解答

这两个用于运行代码示例的示例 PowerShell 脚本包含的注释的行，会显示该作业的错误输出。 如果您未看到预期的输出作业，请取消注释以下行，如果错误信息表明有问题，请参阅。

    # Get-AzureHDInsightJobOutput -StandardError -JobId $job.JobId -Cluster $clusterName

错误信息 (STDERR，) 并将该结果的作业 (STDOUT，) 还记录到您在以下位置的群集的默认 blob 容器中。

为此作业.|看一看这些文件 blob 容器中
---|---
配置单元|/ HivePython/stderr<p>/ HivePython/标准输出
小猪|/ PigPython/stderr<p>/ PigPython/标准输出

##<a name="next"></a>下一步行动

如果您需要加载默认情况下未提供的 Python 模块，请参阅有关如何执行此操作的示例[部署到 Azure HDInsight 模块的方式](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx)。

有关使用其他方法小猪、 配置单元，以及有关使用 MapReduce，请参阅以下。

* [使用 HDInsight 配置单元](hdinsight-use-hive.md)

* [使用 HDInsight 的小猪](hdinsight-use-pig.md)

* [HDInsight 使用 MapReduce](hdinsight-use-mapreduce.md)

测试
