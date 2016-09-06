---
ms.openlocfilehash: 2d628711992fa7937c439b87f30b3897bc49d810
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 打开项目文件 mainpage.xaml.cs，然后将下面的代码段添加到主页类︰
    
        private MobileServiceUser user;
        private async System.Threading.Tasks.Task Authenticate()
        {
            while (user == null)
            {
                string message;
                try
                {
                    user = await App.MobileService
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    message =
                        string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }

                MessageBox.Show(message);
            }
        }

    这将创建一个成员变量来存储当前用户和一个方法来处理身份验证过程。 通过使用 Facebook 登录，用户进行身份验证。

    >[AZURE.NOTE]如果您使用的 Facebook 不标识提供程序，则更改的值<strong>MobileServiceAuthenticationProvider</strong>以上为您的提供程序的值。</p>
    </div>

2. 删除或注释出现有的**OnNavigatedTo**方法重写并将其替换为以下方法处理的页**加载**事件。 

        async void MainPage_Loaded(object sender, RoutedEventArgs e)
        {
            await Authenticate();
            RefreshTodoItems();
        }

    此方法调用新的**身份验证**方法。 

3. 用以下代码替换主页构造函数︰

        // Constructor
        public MainPage()
        {
            InitializeComponent();
            this.Loaded += MainPage_Loaded;
        }

    此构造函数还将注册加载事件的处理程序。
        
4. 按 F5 键运行应用程序和登录到您选定的标识提供程序使用该应用程序。 

    成功登录后，应用程序应该运行错误，并应能为移动服务的查询，并对数据进行更新。
