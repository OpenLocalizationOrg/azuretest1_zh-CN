---
ms.openlocfilehash: 5dec46b7236f4c06d7ebe42a736bb272ebbbf3e0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
我们令牌缓存应该工作在简单的情况下，令牌过期或被吊销时，会发生什么情况？ 该应用程序未运行时无法到期令牌。 这意味着令牌缓存无效。 该标记还无法终止应用程序实际运行直接使应用程序的调用或由移动服务库的调用过程时。 结果将是 HTTP 状态代码 401"未经授权"。 

我们需要能够检测的到期的令牌并刷新它。 为此我们使用[Android 的客户端库](http://dl.windowsazure.com/androiddocs/) [ServiceFilter](http://dl.windowsazure.com/androiddocs/com/microsoft/windowsazure/mobileservices/ServiceFilter.html) 。

本部分中，您将定义 ServiceFilter，将检测 HTTP 状态代码 401 响应并触发的令牌和令牌缓存刷新。 此外，该 ServiceFilter 将阻止其他出站请求在身份验证过程，以便这些请求可以使用刷新的令牌。

1. 在 Eclipse 中，打开 ToDoActivity.java 文件，并添加下面的导入语句︰
 
        import java.util.concurrent.atomic.AtomicBoolean;
        import java.util.concurrent.ExecutionException;

        import com.microsoft.windowsazure.mobileservices.MobileServiceException;
 
2. 添加到以下成员`ToDoActivity`类。 

        public boolean bAuthenticating = false;
        public final Object mAuthenticationLock = new Object();

    这些将用来帮助同步的用户的身份验证。 我们只需进行一次身份验证。 在身份验证过程中的任何调用应等待并正在使用的新身份验证令牌。

3. 在 ToDoActivity.java 文件中，将下面的方法添加到 ToDoActivity 类，用于阻止其他线程上的出站调用进行身份验证时。

        /**
         * Detects if authentication is in progress and waits for it to complete. 
         * Returns true if authentication was detected as in progress. False otherwise.
         */
        public boolean detectAndWaitForAuthentication()
        {
            boolean detected = false;
            synchronized(mAuthenticationLock)
            {
                do
                {
                    if (bAuthenticating == true)
                        detected = true;
                    try
                    {
                        mAuthenticationLock.wait(1000);
                    }
                    catch(InterruptedException e)
                    {}
                }
                while(bAuthenticating == true);
            }
            if (bAuthenticating == true)
                return true;
            
            return detected;
        }
        

4. 在 ToDoActivity.java 文件中，将下面的方法添加到 ToDoActivity 类。 此方法将实际触发等待，然后更新上出站请求令牌身份验证完成时。 

        
        /**
         * Waits for authentication to complete then adds or updates the token 
         * in the X-ZUMO-AUTH request header.
         * 
         * @param request
         *            The request that receives the updated token.
         */
        private void waitAndUpdateRequestToken(ServiceFilterRequest request)
        {
            MobileServiceUser user = null;
            if (detectAndWaitForAuthentication())
            {
                user = mClient.getCurrentUser();
                if (user != null)
                {
                    request.removeHeader("X-ZUMO-AUTH");
                    request.addHeader("X-ZUMO-AUTH", user.getAuthenticationToken());
                }
            }
        }


5. 在 ToDoActivity.java 文件中，更新`authenticate`方法的 ToDoActivity 类，以便它接受一个布尔型参数，以允许强制的令牌和令牌缓存刷新。 我们还需要使他们可领取新的令牌身份验证完成时通知任何被阻止的线程。

        /**
         * Authenticates with the desired login provider. Also caches the token. 
         * 
         * If a local token cache is detected, the token cache is used instead of an actual 
         * login unless bRefresh is set to true forcing a refresh.
         * 
         * @param bRefreshCache
         *            Indicates whether to force a token refresh. 
         */
        private void authenticate(boolean bRefreshCache) {
            
            bAuthenticating = true;
            
            if (bRefreshCache || !loadUserTokenCache(mClient))
            {
                // New login using the provider and update the token cache.
                mClient.login(MobileServiceAuthenticationProvider.MicrosoftAccount,
                        new UserAuthenticationCallback() {
                            @Override
                            public void onCompleted(MobileServiceUser user,
                                    Exception exception, ServiceFilterResponse response) {
    
                                synchronized(mAuthenticationLock)
                                {
                                    if (exception == null) {
                                        cacheUserToken(mClient.getCurrentUser());
                                        createTable();
                                    } else {
                                        createAndShowDialog(exception.getMessage(), "Login Error");
                                    }
                                    bAuthenticating = false;
                                    mAuthenticationLock.notifyAll();
                                }
                            }
                        });
            }
            else
            {
                // Other threads may be blocked waiting to be notified when 
                // authentication is complete.
                synchronized(mAuthenticationLock)
                {
                    bAuthenticating = false;
                    mAuthenticationLock.notifyAll();
                }
                createTable();
            }
        }   



6. 在 ToDoActivity.java 文件中，将此代码添加新的`RefreshTokenCacheFilter`ToDoActivity 类中的类︰

        /**
        * The RefreshTokenCacheFilter class filters responses for HTTP status code 401. 
         * When 401 is encountered, the filter calls the authenticate method on the 
         * UI thread. Out going requests and retries are blocked during authentication. 
         * Once authentication is complete, the token cache is updated and 
         * any blocked request will receive the X-ZUMO-AUTH header added or updated to 
         * that request.   
         */
        private class RefreshTokenCacheFilter implements ServiceFilter {
         
            AtomicBoolean mAtomicAuthenticatingFlag = new AtomicBoolean();                     
            
            @Override
            public ListenableFuture<ServiceFilterResponse> handleRequest(
                    final ServiceFilterRequest request, 
                    final NextServiceFilterCallback nextServiceFilterCallback
                    )
            {
                // In this example, if authentication is already in progress we block the request
                // until authentication is complete to avoid unnecessary authentications as 
                // a result of HTTP status code 401. 
                // If authentication was detected, add the token to the request.
                waitAndUpdateRequestToken(request);
     
                // Send the request down the filter chain
                // retrying up to 5 times on 401 response codes.
                ListenableFuture<ServiceFilterResponse> future = null;
                ServiceFilterResponse response = null;
                int responseCode = 401;
                for (int i = 0; (i < 5 ) && (responseCode == 401); i++)
                {
                    future = nextServiceFilterCallback.onNext(request);
                    try {
                        response = future.get();
                        responseCode = response.getStatus().getStatusCode();
                    } catch (InterruptedException e) {
                       e.printStackTrace();
                    } catch (ExecutionException e) {
                        if (e.getCause().getClass() == MobileServiceException.class)
                        {
                            MobileServiceException mEx = (MobileServiceException) e.getCause();
                            responseCode = mEx.getResponse().getStatus().getStatusCode();
                            if (responseCode == 401)
                            {
                                // Two simultaneous requests from independent threads could get HTTP status 401. 
                                // Protecting against that right here so multiple authentication requests are
                                // not setup to run on the UI thread.
                                // We only want to authenticate once. Requests should just wait and retry 
                                // with the new token.
                                if (mAtomicAuthenticatingFlag.compareAndSet(false, true))                                                                                                      
                                {
                                    // Authenticate on UI thread
                                    runOnUiThread(new Runnable() {
                                        @Override
                                        public void run() {
                                            // Force a token refresh during authentication.
                                            authenticate(true);
                // ToDoActivity.mMainActivity.authenticate(true);
    
                                        }
                                    });
                                }
    
                                // Wait for authentication to complete then update the token in the request. 
                                waitAndUpdateRequestToken(request);
                                mAtomicAuthenticatingFlag.set(false);                                                  
                            }
                        }
                    }
                }
                return future;
            }
        }


    该服务筛选器将检查每个响应的 HTTP 状态代码 401"未经授权"。 如果遇到 401，则新的登录请求，以获取新的令牌将在 UI 线程上的安装程序。 将阻止其他调用，直至完成登录，或者直到 5 尝试均告失败。 如果获得新的令牌时，触发 401 的请求将使用新标记重试，任何阻止的调用将使用新标记重试。 

7. 在 ToDoActivity.java 文件中，将此代码添加新的`ProgressFilter`ToDoActivity 类中的类︰
        
        /**
        * The ProgressFilter class renders a progress bar on the screen during the time the App is waiting for the response of a previous request.
        * the filter shows the progress bar on the beginning of the request, and hides it when the response arrived.
        */
        private class ProgressFilter implements ServiceFilter {
            @Override
            public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback nextServiceFilterCallback) {

                final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();

                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
                    }
                });

                ListenableFuture<ServiceFilterResponse> future = nextServiceFilterCallback.onNext(request);

                Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
                    @Override
                    public void onFailure(Throwable e) {
                        resultFuture.setException(e);
                    }

                    @Override
                    public void onSuccess(ServiceFilterResponse response) {
                        runOnUiThread(new Runnable() {

                            @Override
                            public void run() {
                                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.GONE);
                            }
                        });

                        resultFuture.set(response);
                    }
                });

                return resultFuture;
            }
        }
        
    此筛选器将显示进度栏上请求的开始和响应到达时将隐藏它。
    
8. 在 ToDoActivity.java 文件中，更新`onCreate`方法，如下所示︰

        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            
            setContentView(R.layout.activity_to_do);
            mProgressBar = (ProgressBar) findViewById(R.id.loadingProgressBar);
        
            // Initialize the progress bar
            mProgressBar.setVisibility(ProgressBar.GONE);
        
            try {
                // Create the Mobile Service Client instance, using the provided
                // Mobile Service URL and key
                mClient = new MobileServiceClient(
                        "https://<YOUR MOBILE SERVICE>.azure-mobile.net/",
                        "<YOUR MOBILE SERVICE KEY>", this)
                           .withFilter(new ProgressFilter())
                           .withFilter(new RefreshTokenCacheFilter());
            
                // Authenticate passing false to load the current token cache if available.
                authenticate(false);
                    
            } catch (MalformedURLException e) {
                createAndShowDialog(new Exception("Error creating the Mobile Service. " +
                    "Verify the URL"), "Error");
            }
        }


       In this code, `RefreshTokenCacheFilter` is used in addition to `ProgressFilter`. Also during `onCreate` we want to load the token cache. So `false` is passed in to the `authenticate` method.


