.. include:: ../LINKS.rst

.. _chapter2OAuth:


OAuth
===================


.. sidebar:: 推荐参考
    :subtitle: `OAuth`_ 标准文档
    
    :rfc:`5849`


`OAuth`_ 是一种开放的协议,旨在标准化桌面与网上应用程序访问用户个人数据的方式. 
OAuth提供了一种机制,让用户授权访问私有数据,而不用共享它们的私有凭据(用户名/密码). 
由于它的安全性以及存在一组标准库,许多网站已经启用使用OAuth的API. 

本教程将引导我们了解如何创建可以通过OAuth访问某个API的 扩展程序,
并提供了可以在其它扩展程序中重用的库. 

本教程使用 `Google文档列表数据API`_ 作为启用OAuth的API端点的例子. 





学习要求
------------------------------------------------------------------------------

教程要求具有编写 `Google`_ `Chrome`_ 浏览器扩展程序的一些经验，
并且要求对 `三方OAuth` 流程有一定的了解。

尽管不需要有 `Google文档列表数据API`_（或者其它Google数据API）的开发背景，但是理解这一协议可能有所帮助。

入门 Getting started
------------------------------------------------------------------------------

首先, 从Chromium源代码树中的 `.../examples/extensions/oauth_contacts/ <http://src.chromium.org/viewvc/chrome/trunk/src/chrome/common/extensions/docs/examples/extensions/oauth_contacts/>`_ .

目录将如下三个文件复制到计算机中：

- :download:`chrome_ex_oauth.html <../_static/examples/extensions/oauth_contacts/chrome_ex_oauth.html>` ——用于重定向至 `oauth_callback` URL的中间页面
- :download:`chrome_ex_oauth.js <../_static/examples/extensions/oauth_contacts/chrome_ex_oauth.js>` ——核心 `OAuth`_ 库
- :download:`chrome_ex_oauthsimple.js <../_static/examples/extensions/oauth_contacts/chrome_ex_oauthsimple.js>` ——对 `chrome_ex_oauth.js` 有用的包装

将这三个文件放在我们扩展程序的根目录中
（或者您的 `JavaScript`_ 代码存放的位置，然后在您的后台页面中按照以下顺序包含那两个.js文件：

.. code-block:: html

    <script type="text/javascript" src="chrome_ex_oauthsimple.js"></script>
    <script type="text/javascript" src="chrome_ex_oauth.js"></script>

这样后台页面将能管理 `OAuth`_ 流程。



扩展程序中的OAuth认证
------------------------------------------------------------------------------

若对 `OAuth`_ 协议比较熟悉的话,就知道 `OAuth`_ 认证包括三个步骤：

#. 获得初始的请求令牌
#. 让用户为请求令牌授权
#. 获得访问令牌

在扩展程序的受限环境中，实现这一流程有一定的技巧性。
主要困难在:
- 服务提供商和应用程序之间无法建立使用者密钥/机密
- 在授权过程之后没有网上应用程序的URL让用户重定向。

幸运的是， `Google`_ 和另外几家公司已经在解决 `用以可安装应用程序的OAuth <http://code.google.com/apis/accounts/docs/OAuthForInstalledApps.html>`_  ，
这样我们就可以从扩展程序的环境中使用。

在扩展程序的 `OAuth`_ 认证过程中，使用者 `key/secret` 为 `'anonymous'/'anonymous'` ，
我们也可以提供一个 `应用程序名称` （而不是应用程序URL）给用户来授予访问权限。

最终结果是一样的：

#. 后台页面请求初始令牌
#. 打开新标签页进入授权页面
#. 最后发出异步调用请求访问令牌。


    Setup code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
要初始化库，在后台页面中创建一个 `ChromeExOAuth` 对象：

::

    var oauth = ChromeExOAuth.initBackgroundPage({
      'request_url': <OAuth 请求 URL>,
      'authorize_url': <OAuth 认证 URL>,
      'access_url': <OAuth 访问令牌 URL>,
      'consumer_key': <OAuth 消费者密钥>,
      'consumer_secret': <OAuth 消费者机密>,
      'scope': <要访问的数据范围。并不是所有 OAuth 提供商都需要这一属性>,
      'app_name': <应用程序名称。并不是所有 OAuth 提供商都需要这一属性>
    });


在 `Google文档列表数据API`_ 和 `Google`_ 的 `OAuth`_ 端点之间，初始化代码可以像下面这样：

::

    var oauth = ChromeExOAuth.initBackgroundPage({
      'request_url': 'https://www.google.com/accounts/OAuthGetRequestToken',
      'authorize_url': 'https://www.google.com/accounts/OAuthAuthorizeToken',
      'access_url': 'https://www.google.com/accounts/OAuthGetAccessToken',
      'consumer_key': 'anonymous',
      'consumer_secret': 'anonymous',
      'scope': 'https://docs.google.com/feeds/',
      'app_name': '我的 Google 文档扩展程序'
    });



获得以及认证请求令牌    Fetching and authorizing a request token
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

一旦设置好了后台页面，调用 `authorize()`函式，开始OAuth认证，使用户重定向至 `OAuth`_ 提供商。
客户端库抽象了这一过程的大部分，所以我们需要做的只是向 `authorize()`函式传递一个回调函式，
然后会打开一个新标签页，并重定向。

::

    oauth.authorize(function() {
      // …准备好获取个人数据…
    });


不需要提供任何额外的逻辑来存储令牌和机密，因为该库已经在浏览器的 `本地存储`(localStorage) 中储存了这些值。
如果库已经为当前作用域(current scope) 储存了访问令牌，则不用再打开新标签页。

注意,不论什么情况,回调函式都会被调用的。



发送已签名的API请求    Sending signed API requests
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

一旦我们指定的回调函式开始执行，
就可以调用 `sendSignedRequest()`向我们的API发送已签名的请求。
`sendSignedRequest()`有三个参数：
URI、回调函数和可选的参数对象。

回调函数有两个参数：响应文本以及用来发出请求的 `XMLHttpRequest` 对象。

发送HTTP GET请求的例子：

.. code-block:: js

    function callback(resp, xhr) {
      // …处理文本响应…
    };

    function onAuthorized() {
      var url = 'https://docs.google.com/feeds/default/private/full';
      var request = {
        'method': 'GET',
        'parameters': {'alt': 'json'}
      };

      // 发送：GET https://docs.google.com/feeds/default/private/full?alt=json
      oauth.sendSignedRequest(url, callback, request);
    };

    oauth.authorize(onAuthorized);


更复杂的使用HTTP POST请求的例子:

.. code-block:: js

    function onAuthorized() {
      var url = 'https://docs.google.com/feeds/default/private/full';
      var request = {
        'method': 'POST',
        'headers': {
          'GData-Version': '3.0',
          'Content-Type': 'application/atom+xml'
        },
        'parameters': {
          'alt': 'json'
        },
        'body': '要发送的数据'
      };

      // 发送：POST https://docs.google.com/feeds/default/private/full?alt=json
      oauth.sendSignedRequest(url, callback, request);
    };


默认情况下， `sendSignedRequest()` 函式在URL中
（通过调用 `oauth.signURL()` ）发送 `oauth_*` 参数。
如果我们希望在 `Authorization` 头信息中发送 `oauth_*` 参数（或者需要直接访问已生成的头信息），
使用 `getAuthorizationHeader()`。

它的参数包括URL、HTTP方法以及可选的URL查询参数对象，表示为键/值对。

如下例子,正如前述使用 `getAuthorizationHeader()` 和 `XMLHttpRequest` 对象：

.. code-block:: js

    function stringify(parameters) {
      var params = [];
      for(var p in parameters) {
        params.push(encodeURIComponent(p) + '=' +
                    encodeURIComponent(parameters[p]));
      }
      return params.join('&');
    };

    function onAuthorized() {
      var method = 'POST';
      var url = 'https://docs.google.com/feeds/default/private/full';
      var params = {'alt': 'json'};

      var xhr = new XMLHttpRequest();
      xhr.onreadystatechange = function(data) {
        callback(xhr, data);
      };
      xhr.setRequestHeader('GData-Version', '3.0');
      xhr.setRequestHeader('Content-Type', 'application/atom+xml');
      xhr.setRequestHeader('Authorization', oauth.getAuthorizationHeader(url, method, params));
      xhr.open(method, url + '?' + stringify(params), true);

      xhr.send('Data to send');
    };




示例代码 Sample code
------------------------------------------------------------------------------

使用这些技术的示例扩展程序在Chromium源代码树的如下位置可用：

- `.../examples/extensions/gdocs/ <http://src.chromium.org/viewvc/chrome/trunk/src/chrome/common/extensions/docs/examples/extensions/gdocs/>`_
- `.../examples/extensions/oauth_contacts/ <http://src.chromium.org/viewvc/chrome/trunk/src/chrome/common/extensions/docs/examples/extensions/oauth_contacts/>`_



