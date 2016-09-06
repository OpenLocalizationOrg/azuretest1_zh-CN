---
ms.openlocfilehash: ad4070986eb6daf7fecb1226ee48d1a852cc8d64
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何拨打电话 Twilio (.NET) |Microsoft Azure" 
    description="了解如何拨打电话和发送短消息与 Twilio API 服务在 Azure 上。 用.NET 编写的代码样本。" 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/02/2015" 
    ms.author="microsofthelp@twilio.com"/>




# 如何使在 Azure 上 web 角色中使用 Twilio 电话呼叫

本指南说明如何使用 Twilio 来打电话从 Azure 中承载的网页。 生成的应用程序提示用户输入电话值，如下面的屏幕快照中所示。

![使用 Twilio 和 ASP.NET 的 azure 调用窗体][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>先决条件

您将需要执行下列操作以使用本主题中的代码︰

1. 获得 Twilio 帐户和身份验证令牌。 若要开始使用 Twilio，注册在[https://www.twilio.com/try-twilio][try_twilio]。 您可以评估定价在[http://www.twilio.com/pricing][twilio_pricing]。 有关 API 由 Twilio 提供的信息，请参阅[http://www.twilio.com/voice/api][twilio_api]。
2. 将 Twilio.NET 库添加到您的 web 角色。 请参阅"添加 Twilio 库对 web 角色项目，"在本主题的后面部分。

您应该熟悉在 Azure 上创建一个基本的 web 角色。

## <a name="howtocreateform"></a>如何︰ 创建 web 窗体发出呼叫

<a id="use_nuget"></a>要添加到您的 web 角色项目 Twilio 库︰

1.  在 Visual Studio 中打开您的解决方案。
2.  用鼠标右键单击**引用**。
3.  单击**管理 NuGet 程序包**。
4.  单击**联机**。
5.  在搜索在线框中，键入*twilio*。
6.  Twilio 软件包上，单击**安装**。

下面的代码演示如何创建 web 窗体检索呼叫用户数据。 在此示例中，创建一个名为**TwilioCloud**的 ASP.NET web 角色。

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>如何︰ 创建的代码进行调用
下面的代码，当用户完成窗体调用时，创建调用消息并生成调用。 在此示例中，按钮的 onclick 事件处理程序在窗体上运行代码。 （使用您的 Twilio 帐户和身份验证令牌，而不是分配给**帐户**和**authToken**下面代码中的占位符值）。

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

进行调用时，并会显示 Twilio 端点、 API 版本和通话状态。 下面的屏幕快照显示了运行示例输出。

![使用 Twilio 和 ASP.NET 的 azure 调用响应][twilio_dotnet_basic_form_output]

在[http://www.twilio.com/docs/api/twiml][twiml]找不到有关 TwiML 的详细信息。 有关详细信息&lt;说&gt;和[http://www.twilio.com/docs/api/twiml/say][twilio_say]可以找到其他 Twilio 动词。

## <a id="nextsteps"></a>下一步行动
提供此代码来显示您在 ASP.NET web 角色在 Azure 上使用 Twilio 的基本功能。 在生产环境中部署到 Azure 之前, 可能需要添加更多错误处理或其他功能。 例如︰

* 而不是使用 web 窗体，您可以使用 Azure Blob 存储或 SQL Azure 数据库实例存储电话号码，并调用文本。 有关使用 Azure 中的 blob 信息，请参阅[如何使用.NET 中的 Azure Blob 存储服务][howto_blob_storage_dotnet]。 有关使用 SQL 数据库的信息，请参阅[如何使用 Azure.NET 应用程序中的 SQL 数据库][howto_sql_azure_dotnet]。
* 从您的部署配置设置，而不是硬编码的值在窗体中，您可以检索 Twilio 帐户 ID 和身份验证令牌使用 RoleEnvironment.getConfigurationSettings。 关于 RoleEnvironment 类的信息，请参阅[Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet]。
* 阅读在[https://www.twilio.com/docs/security][twilio_docs_security]的 Twilio 安全准则。
* 了解有关 Twilio 在[https://www.twilio.com/docs][twilio_docs]。

##<a name="seealso"></a>请参见
* [如何使用 Twilio 进行语音和 Azure 的 SMS 功能](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx

测试
