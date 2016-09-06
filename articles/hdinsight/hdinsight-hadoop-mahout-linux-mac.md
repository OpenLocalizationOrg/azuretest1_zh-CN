---
ms.openlocfilehash: 569a1a80a1d2f3774825e2fd0c6b3816473cf2ec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="生成建议使用 Mahout 和 Hadoop |Microsoft Azure"
    description="了解如何使用 Apache Mahout 机器学习库生成影片与基于 Linux 的 HDInsight (Hadoop) 的建议。"
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
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2015"
    ms.author="larryfr"/>

#通过在 HDInsight （预览） 的基于 Linux 的 Hadoop 使用 Apache Mahout 生成影片建议

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

了解如何使用[Apache Mahout](http://mahout.apache.org)机器学习与 Azure HDInsight 库生成影片的建议。

Mahout 是 Apache Hadoop[机器学习][ml]库。 Mahout 包含用于处理数据，例如，筛选、 分类和聚类的算法。 在本文中，将使用一个推荐引擎来生成影片建议根据您的朋友已经了解的电影。

> [AZURE.NOTE] 此文档中的步骤需要在 HDInsight 群集 （预览） 基于 Linux 的 Hadoop。 在 Mahout 中使用基于 Windows 群集的信息，请参阅[通过使用 HDInsight 中的基于 Windows 的 Hadoop 使用 Apache Mahout 生成影片建议](hdinsight-mahout.md)

##先决条件

* 基于 linux * 的 Hadoop，HDInsight 群集上。 有关创建一条信息，请参阅[开始使用 HDInsight 中的基于 Linux 的 Hadoop][getstarted]

##<a name="recommendations"></a>理解的建议

Mahout 所提供的功能之一是建议引擎。 此引擎接受数据的格式为`userID`， `itemId`，和`prefValue`（该项目的用户首选项）。 Mahout 然后可以执行共同出现的分析，以确定︰_拥有某一项的首选项的用户也具有这些其他项目的首选项_。 Mahout 然后确定用户与类似项目的首选项，可用来提出建议。

下面是使用电影非常简单的示例︰

* __共同的出现__︰ 李先生，Alice 和 Bob 所有喜欢_星球大战_，_回到帝国罢工_，并_返回 Jedi_。 Mahout 确定用户喜欢这些电影之一也像其他两个。

* __共同的出现__︰ 小明和小红还喜欢_幻像威胁_、_克隆攻击_，以及_Sith 的报复_。 Mahout 确定还喜欢以前的三个电影的用户喜欢这三种。

* __相似性建议__︰ 因为李先生喜欢的前三个电影，Mahout 看电影，其他具有相似参数很喜欢，但李先生不监视 （喜欢/等级）。 在这种情况下，Mahout 建议_幻像威胁_、_克隆攻击_，以及_Sith 的报复_。

##加载数据

为方便起见， [GroupLens 研究][movielens]提供评级数据与 Mahout 兼容的格式的电影。 使用以下步骤来下载数据，然后将其加载到群集的默认存储区︰

1. 使用 SSH 连接到基于 Linux 的 HDInsight 群集。 连接时要使用的地址是`CLUSTERNAME-ssh.azurehdinsight.net`，端口为`22`。

    使用 SSH 连接到 HDInsight 的详细信息，请参阅以下文档︰

    * **Linux、 Unix 或 OS X 客户端**︰[连接到基于 Linux 的 HDInsight 从 Linux、 OS X 或 Unix 群集](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)，请参阅

    * **Windows 客户端**︰[连接到基于 Linux 的 HDInsight 从 Windows 群集](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)，请参阅

2. 下载 MovieLens 100 k 归档，其中包含 100000 从 1700年电影 1000 个用户的分级。

        curl -O http://files.grouplens.org/datasets/movielens/ml-100k.zip

3. 解压缩存档文件使用以下命令︰

        unzip ml-100k.zip

    它将提取到新文件夹名为**ml-100k**的内容。

4. 使用以下命令将数据复制到 HDInsight 存储︰

        cd ml-100k
        hadoop fs -copyFromLocal ml-100k/u.data /example/data/u.data

    在此文件中包含的数据具有结构的`userID`， `movieID`， `userRating`，和`timestamp`，告诉我们如何高，每个用户评价影片。 下面是数据的一个示例︰


        196 242 3   881250949
        186 302 3   891717742
        22  377 1   878887116
        244 51  2   880606923
        166 346 1   886397596

##运行作业

使用下面的命令运行建议作业︰

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE --input /example/data/u.data --output /example/data/mahoutout  --tempDir /temp/mahouttemp

> [AZURE.NOTE] 作业可能需要几分钟才能完成，并且可以运行多个 MapReduce 作业。

##查看输出

1. 完成作业后，可以使用下面的命令以查看生成的输出︰

        hadoop fs -text /example/data/mahoutout/part-r-00000

    输出将显示如下所示︰

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    第一列是`userID`。 中包含的值 [和] 是`movieId`:`recommendationScore`。

2. **Ml-100k**目录中包含的其他数据的一些可以用来使数据更多的用户友好。 首先，下载数据使用下面的命令︰

        hadoop fs -copyToLocal /example/data/mahoutout/part-r-00000 recommendations.txt

    这会将输出数据复制到名为**recommendations.txt**的当前目录中的文件。

3. 使用下面的命令创建一个新的 Python 脚本，将查找电影名称建议输出中的数据︰

        nano show_recommendations.py

    当编辑器打开时，使用以下内容作为该文件的内容︰

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    按下**Ctrl-X**、 **Y**和最后**输入**要保存的数据。

3. 使用下面的命令使可执行文件︰

        chmod +x show_recommendations.py

4. 运行 Python 脚本︰

        ./show_recommendations.py 4 ml-100k/u.data ml-100k/u.item recommendations.txt

    这要看用户 ID 为 4 的生成的建议。

    * **U.data**文件用来检索用户已分级的影片
    * **U.item**文件用于检索的电影名称
    * **Recommendations.txt**用于检索此用户的影片建议

    此命令的输出将类似如下︰

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##删除临时数据

Mahout 作业不要删除处理作业时创建的临时数据。 `--tempDir`参数指定示例作业隔离到特定路径中很容易删除的临时文件中。 若要删除临时文件，请使用下面的命令︰

    hadoop fs -rm -f -r wasb:///temp/mahouttemp

> [AZURE.WARNING] 如果不删除临时文件或输出文件，您将收到无法覆盖文件错误消息如果您重新运行作业具有相同`--tempDir`路径。

##下一步行动

现在，您已经学习了如何使用 Mahout，发现 HDInsight 上使用数据的其他方法︰

* [使用 HDInsight 配置单元](../hadoop-use-hive.md)
* [与 HDInsight 的小猪](../hadoop-use-pig.md)
* [MapReduce 与 HDInsight](../hadoop-use-mapreduce.md)

[生成]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[10 万]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[将上载]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[林]: http://en.wikipedia.org/wiki/Random_forest
[管理]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[工具]: https://github.com/Blackmist/hdinsight-tools
 
测试
