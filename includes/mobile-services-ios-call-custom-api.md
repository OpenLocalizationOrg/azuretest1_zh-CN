---
ms.openlocfilehash: f9e959130f04c4dd1b63eb3ca59073521ebbe249
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

## <a name="update-app"></a>从 iOS 应用程序中调用自定义的 API

若要从 iOS 客户端调用此自定义的 API，使用`MSClient invokeAPI`方法。 有两种版本的此方法、 JSON 格式的请求，和任何数据类型:

    /// Invokes a user-defined API of the Mobile Service.  The HTTP request and
    /// response content will be treated as JSON.
    -(void)invokeAPI:(NSString *)APIName
                body:(id)body
          HTTPMethod:(NSString *)method
          parameters:(NSDictionary *)parameters
             headers:(NSDictionary *)headers
          completion:(MSAPIBlock)completion;

    /// Invokes a user-defined API of the Mobile Service.  The HTTP request and
    /// response content can be of any media type.
    -(void)invokeAPI:(NSString *)APIName
                data:(NSData *)data
          HTTPMethod:(NSString *)method
          parameters:(NSDictionary *)parameters
             headers:(NSDictionary *)headers
          completion:(MSAPIDataBlock)completion;


例如，若要将一个 JSON 请求发送到一个自定义的 API，名为"sendEmail"，传递`NSDictionary`请求参数︰

    NSDictionary *emailHeader = @{ @"to": @"email.com", @"subject" : @"value" };

    [self.client invokeAPI:@"sendEmail"
         body:emailBody
         HTTPMethod:@"POST"
         parameters:emailHeader
         headers:nil
         completion:completion ];
        

