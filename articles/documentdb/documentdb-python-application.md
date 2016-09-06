---
ms.openlocfilehash: 347ff44ff1a865b9c623c3b5111f32c4bf74175d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="构建 web 应用程序使用 Python 和 Flask 使用 DocumentDB |Microsoft Azure"
    description="了解如何使用 DocumentDB 来存储和访问数据从 Python 和 Flask (MVC) 的 web 应用程序驻留在 Azure 上。"
    services="documentdb"
    documentationCenter="python"
    authors="ryancrawcour"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="09/03/2015"
    ms.author="ryancraw"/>

# 构建 web 应用程序，使用 Python 和 Flask (MVC) 使用 DocumentDB

若要突出显示的客户可以高效地利用 Azure DocumentDB 来存储和查询的 JSON 文档，本文档提供了演示如何生成使用 Azure DocumentDB 投票 web 应用程序的端到端指南。

本教程展示如何使用 Azure 的 DocumentDB 服务来存储和访问数据从 Python web 应用程序驻留在 Azure 上，假定您有一些使用 Python 和 Azure 网站的前期经验。

本教程介绍︰

1. 创建和设置 DocumentDB 帐户。
2. 创建一个 Python MVC 应用程序。
3. 连接到并使用 Azure DocumentDB 从 web 应用程序。
4. Web 应用程序部署到 Azure 网站。

按照本教程中，您将生成一个简单的投票应用程序，您可以投票的民意测验。

![通过本教程中创建的 todo 列表 web 应用程序的屏幕抓图](./media/documentdb-python-application/image1.png)


## 先决条件

在这篇文章中的说明进行操作之前，应确保您已经安装了以下产品︰

- [Visual Studio 2013年](http://www.visualstudio.com/)或更高版本，或 Visual Studio 速成，是免费版。
- 从[这里][]的 Visual Studio 的 Python 工具。
- Azure 的 Visual Studio 2013，2.4 或更高版本的 SDK 可从[此处][1]。
- 从[此处][2]Python 2.7。
- 对于从[这里][3]Python 2.7 Microsoft Visual C++ 编译器。

## 步骤 1︰ 创建一个 DocumentDB 数据库帐户

让我们首先创建一个 DocumentDB 帐户。 如果您已经有一个帐户，您可以跳到[步骤 2︰ 创建新的 Python Flask web 应用程序](#Step-2:-Create-a-new-Python-Flask-Web-Application)。

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[AZURE.INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

<br/>
我们现在将指导如何创建了一个 Python Flask web 应用从零开始。

## 步骤 2︰ 创建新的 Python Flask web 应用程序

1. 打开 Visual Studio 中，单击**文件** -\> **新项目** -\> **Python** -\>， **Flask Web 项目**，然后使用名称**教程**创建一个新项目。

    对于那些新手 Flask，就可以帮助我们更快地构建在 Python 中的 web 应用程序的 web 框架。 [单击此处访问 Flask 教程][]。

    ![Python 突出显示在左边，在名称框中选择在中间和名称教程 Flask Web 项目使用 Visual Studio 中的新项目窗口的屏幕抓图](./media/documentdb-python-application/image9.png)

2. 它将询问您是否要安装外部包。 单击**安装到虚拟环境**。 请务必使用 Python 2.7 作为基本的环境，因为 PyDocumentDB 当前不支持 Python 3.x 版。  这将设置为您的项目所需的 Python 虚拟环境。

    ![本教程的 Python 工具 Visual Studio 窗口的屏幕抓图](./media/documentdb-python-application/image10.png)


## 步骤 3︰ 修改 Python Flask web 应用程序

### 向项目中添加 Flask 包

您的项目设置后，您需要添加您需要的项目，包括 pydocumentdb，DocumentDB 的 Python 包某些 Flask 程序包。

1. 打开名为**requirements.txt**的文件并替换为以下内容︰

        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0

2. 右键单击**信纸**，然后单击**从 requirements.txt 中安装**。

    ![屏幕抓图显示 env (Python 2.7) 从列表中突出显示的 requirements.txt 选择与安装](./media/documentdb-python-application/image11.png)

> [AZURE.NOTE] 在极少数情况下，您可能会看到输出窗口中的故障。 如果发生这种情况，请检查错误如果与清理。 有时，清理失败，但仍将成功 （向上滚动在输出窗口以验证此） 安装。
<a name="verify-the-virtual-environment"></a> 如果发生这种情况，则确定以继续。


### 验证虚拟环境

让我们确保一切都已正确安装。

- 启动网站这可以通过按**F5**启动 Flask 开发服务器并启动 web 浏览器。 您应该看到以下页面。

    ![在浏览器中显示的空 Flask 项目](./media/documentdb-python-application/image12.png)

### 创建数据库、 收集和文档定义

现在让我们创建应用程序投票。

- 用鼠标右键单击名为**教程**解决方案资源管理器中的文件夹中添加 Python 文件。  命名文件**forms.py**。  

        from flask.ext.wtf import Form
        from wtforms import RadioField

        class VoteForm(Form):
            deploy_preference  = RadioField('Deployment Preference', choices=[
                ('Web Site', 'Web Site'),
                ('Cloud Service', 'Cloud Service'),
                ('Virtual Machine', 'Virtual Machine')], default='Web Site')

### 向 views.py 添加所需的导入

- 在**views.py**的顶部，添加下面的导入语句。 这些导入 DocumentDB 的 PythonSDK 和 Flask 包。

        from forms import VoteForm
        import config
        import pydocumentdb.document_client as document_client


### 创建数据库、 收集和文档

- 将以下代码添加到**views.py**中。 这负责创建窗体所使用的数据库。 不要删除任何**views.py**中的现有代码。 这只是追加到结尾。

        @app.route('/create')
        def create():
            """Renders the contact page."""
            client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

            # Attempt to delete the database.  This allows this to be used to recreate as well as create
            try:
                db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
                client.DeleteDatabase(db['_self'])
            except:
                pass

            # Create database
            db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })
            # Create collection
            collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION }, { 'offerType': 'S1' })
            # Create document
            document = client.CreateDocument(collection['_self'],
                { 'id': config.DOCUMENTDB_DOCUMENT,
                'Web Site': 0,
                'Cloud Service': 0,
                'Virtual Machine': 0,
                'name': config.DOCUMENTDB_DOCUMENT })

            return render_template(
                'create.html',
                title='Create Page',
                year=datetime.now().year,
                message='You just created a new database, collection, and document.  Your old votes have been deleted')

> [AZURE.TIP] **CreateCollection**方法采用可选的**RequestOptions**作为第三个参数。 这可以用来提供为指定类型的集合。 如果没有 offerType 提供值，则将使用默认的提供类型创建集合。 DocumentDB 提供了类型的详细信息，请参阅[DocumentDB 中的性能级别](documentdb-performance-levels.md)。
>
### 读取数据库、 回收、 文档，并将提交窗体

- 将以下代码添加到**views.py**中。 这负责设置窗体中，读取数据库、 收集和文档。 不要删除任何**views.py**中的现有代码。 这只是追加到结尾。

        @app.route('/vote', methods=['GET', 'POST'])
        def vote():
            form = VoteForm()
            replaced_document ={}
            if form.validate_on_submit(): # is user submitted vote  
                client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

                # Read databases and take the first since the id should not be duplicated.
                db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))

                # Read collections and take the first since the id should not be duplicated.
                coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))

                # Read documents and take the first since the id should not be duplicated.
                doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))

                # Take the data from the deploy_preference and increment your database
                doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
                replaced_document = client.ReplaceDocument(doc['_self'], doc)

                # Create a model to pass to results.html
                class VoteObject:
                    choices = dict()
                    total_votes = 0

                vote_object = VoteObject()
                vote_object.choices = {
                    "Web Site" : doc['Web Site'],
                    "Cloud Service" : doc['Cloud Service'],
                    "Virtual Machine" : doc['Virtual Machine']
                }
                vote_object.total_votes = sum(vote_object.choices.values())

                return render_template(
                    'results.html',
                    year=datetime.now().year,
                    vote_object = vote_object)

            else :
                return render_template(
                    'vote.html',
                    title = 'Vote',
                    year=datetime.now().year,
                    form = form)


### 创建 HTML 文件

在模板文件夹中，将添加下面的 html 文件︰ create.html、 results.html、 vote.html。

1. 将以下代码添加到**create.html**中。 它负责显示一条消息指出我们创建新的数据库、 收集和文档。

        {% extends "layout.html" %}
        {% block content %}
        <h2>{{ title }}.</h2>
        <h3>{{ message }}</h3>
        <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
        {% endblock %}

2. 将以下代码添加到**results.html**中。 它负责显示轮询结果。

        {% extends "layout.html" %}
        {% block content %}
        <h2>Results of the vote</h2>
        <br />

        {% for choice in vote_object.choices %}
        <div class="row">
            <div class="col-sm-5">{{choice}}</div>
            <div class="col-sm-5">
                <div class="progress">
                    <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0"
                     aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                        {{vote_object.choices[choice]}}
                    </div>
                </div>
            </div>
        </div>
        {% endfor %}

        <br />
        <a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
        {% endblock %}

3. 将以下代码添加到**vote.html**中。 它负责显示轮询和接受投票结果。 注册投票，该控件通过传递到 views.py 我们将识别投票强制转换，并相应地添加文档。

        {% extends "layout.html" %}
        {% block content %}
        <h2>What is your favorite way to host an application on Azure?</h2>
        <form action="" method="post" name="vote">
            {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
        </form>
        {% endblock %}

4. 用以下内容替换**index.html**的内容。 这将作为您的应用程序的登录页。

        {% extends "layout.html" %}
        {% block content %}
        <h2>Python + DocumentDB Voting Application.</h2>
        <h3>This is a sample DocumentDB voting application using PyDocumentDB</h3>
        <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear the Voting Database &raquo;</a></p>
        <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
        {% endblock %}


### 添加配置文件并更改\_\_初始化\_\_.py

1. 用鼠标右键单击项目名称教程并添加文件， **config.py**。
该配置文件被必需的 Flask 中的窗体。 您可以使用它来提供机密密钥。 此项不需要此教程不过。

2. 将以下代码添加到 config.py 中。 改变的值**DOCUMENTDB\_主机**和**DOCUMENTDB\_键**。

        CSRF_ENABLED = True
        SECRET_KEY = 'you-will-never-guess'

        DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
        DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='

        DOCUMENTDB_DATABASE = 'voting database'
        DOCUMENTDB_COLLECTION = 'voting collection'
        DOCUMENTDB_DOCUMENT = 'voting document'

3. 同样替换内容的**\_\_初始化\_\_.py**以下。

        from flask import Flask
        app = Flask(__name__)
        app.config.from_object('config')
        import tutorial.views

4. 上面提到的步骤之后，这是解决方案资源管理器中应显示方式。

    ![Visual Studio 解决方案资源管理器窗口的屏幕抓图](./media/documentdb-python-application/image15.png)


## 步骤 4︰ 在本地运行您的应用程序

1. 在屏幕上，按 f5 键或单击**运行**按钮在 Visual Studio 中，您应看到以下。

    ![Python + DocumentDB 投票应用程序显示在 web 浏览器的屏幕抓图](./media/documentdb-python-application/image16.png)

2. 单击**创建或清除投票数据库**生成数据库。

    ![创建页中的 web 应用程序的屏幕抓图](./media/documentdb-python-application/image17.png)

3. 然后，单击**投票**并选择您的选项。

    ![Web 应用程序带来的投票问题的屏幕抓图](./media/documentdb-python-application/image18.png)

4. 强制转换的每一票，为其增加相应的计数器。

    ![结果显示投票网页的屏幕截图](./media/documentdb-python-application/image19.png)


## 第 5 步︰ 将应用部署到 Azure 网站

现在，有完整的应用程序 DocumentDB 对正常工作，我们要将此部署到 Azure 网站。

1. 用鼠标右键单击解决方案资源管理器中的项目 (请确保您不仍在本地运行) 并选择**发布**。  然后，选择**Microsoft Azure 网站**。

    ![本教程使用突出显示发布选项选择解决方案资源管理器的屏幕抓图](./media/documentdb-python-application/image20.png)

2. 将 Azure 网站配置通过提供您的凭据，然后单击**发布**。

    ![发布网站窗口的屏幕抓图](./media/documentdb-python-application/image21.png)

3. 几秒钟后，Visual Studio 将完成 web 应用程序发布和启动浏览器，可以看到在 Azure 中运行方便工作 ！

## 下一步行动

祝贺您 ！ 您只是有使用 Azure DocumentDB 第一个 Python 应用程序生成并将其发布到 Azure 网站。

我们很想知道是否您发现本教程有所帮助，所以请的开头或结尾的主题使用投票按钮，让我们知道我们做的那样。 本主题是主动更新，因此我们需要您的反馈意见改进它。 如果您希望我们与您联系，随时跟进的注释中包含您的电子邮件地址。  

若要将其他功能添加到您的应用程序，查看[DocumentDB Python SDK](https://pypi.python.org/pypi/pydocumentdb)中提供的 Api。

  [单击此处访问 Flask 教程]: http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world
  [Visual Studio 速成版]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
  [此处]: http://aka.ms/ptvs
  [1]: http://go.microsoft.com/fwlink/?linkid=254281&clcid=0x409
  [2]: https://www.python.org/downloads/windows/
  [3]: http://aka.ms/vcpython27
  [Microsoft Web 平台安装程序]: http://www.microsoft.com/web/downloads/platform.aspx
  [Azure 门户]: http://portal.azure.com

测试
