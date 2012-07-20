.. include:: ../../LINKS.rst

.. _chapter5webstore2InlineInstall:


启用"内联式"安装" Using Inline Installation
==============================================================================



一旦:ref:`发布 <chapter5webstore2Publishing>` 了应用或扩展，
可能会疑惑，用户将如何找到并安装我们的应用程序?
对于用户而言,虽然到 `Chrome`_ Web Store浏览，并找到我们的项目，
算得的上是个非常简单的一键安装过程。
然而，如果用户已经在我们的网站上浏览中，
这时可就是繁琐的了，
因为为他们想完成安装就得 -- 需要从我们的网站导航到 `Chrome网上应用店`_ ，完成安装，然后再返回继续浏览。

.. figure:: ../../_static/web-store/images/imageAssets/inline_install_dialog.png
   :align: right


`Google`_ `Chrome`_ 15之后, 我们可以启动应用和扩展从自己的网站以“内联”方式进行安装。
虽说, 应用和扩展仍然托管在 `Chrome网上应用店`_ ，但用户不再需要离开当前网站就能完成安装。


当用户安装应用或是扩展时, 将看到类似右图的"内联式"安装对话框。
就像直接从 `Chrome网上应用店`_ 安装时显示的对话框一样，
能列出应用或扩展安装所需要的所有权限要求。
此外，还包括 `Chrome网上应用店`_ 中应用或扩展的 平均等级和用户数量，
从而赋予用户安装时额外的信心。


概述
------------------------------------------------------------------------------



内嵌式安装由两部分组成:

-  `<link>` 声明标记
-  `chrome.webstore.install()` `JavaScript`_ 函数。

还必须先验证触发内嵌安装的链接,
即,指向 `Chrome网上应用店`_ 等效页面的链接。


增加 `Chrome网上应用店`_ 链接    Adding a Chrome Web Store link
------------------------------------------------------------------------------



我们的网站必须在HTML源代码 `<head>` 标签部分包含必要的
`<link rel="chrome-webstore-item" href="https://chrome.google.com/webstore/detail/itemID">`
其中,使用的URL是应用或扩展对应在 `Chrome网上应用店`_ 的 `URL`_。
可以在 `Chrome开发者信息中心`_ 点击应用程序或扩展名查到。


.. note:: 注意

    - 该 `URL`_ 必须使用 `HTTPS`协议
    - 不得包含任何其他的查询参数。

例如，
`Google Docs <https://chrome.google.com/webstore/detail/apdfllckaahabafndbhieahigkjlhalf>`_
应用程序 `<link>` 标记看起来像这样：


.. code-block:: html
  :emphasize-lines: 2

    <link rel="chrome-webstore-item"
        href="https://chrome.google.com/webstore/detail/apdfllckaahabafndbhieahigkjlhalf">





触发"内朕"安装 Triggering inline installation
------------------------------------------------------------------------------


要真正激发"内联式"安装必须调用函式:
`chrome.webstore.install(url, successCallback, failureCallback)`
而且,只能通过响应用户行为而触发,
例如在 `Click事件` 处理时;
否则会抛出一个异常。
函数可以有以下参数：

`url` ('optional' string):
    如果页面头部有多个声明 `Chrome网上应用店`_ `关联项`的 `<link>`标签，可以选择使用哪一个进行 "内联"安装。如果被省略，那么第一（或唯一）的链接将被使用。如果传递的 `URL`_ 不存在于页面声明中,将抛出异常。
`successCallback` ('optional' function):
    当内嵌安装成功完成时,此函式将被调用
    (在安装对话框弹出,用户同意将扩展安装进来后)
    不妨以为标记,来隐藏相关界面元素，以便提示用户应用或扩展已安装。
failureCallback (optional function):
    当"内联式"安装没有顺利完成时,此函数将被调用。
    可能的原因有:用户取消了对话框, 关联项没在 `Chrome网上应用店`_ 商店中找到，或正在从一个非验证的网站发起安装。
    回调中给出了代表具体失败细节的字符串作为参数。
    不妨检查或记录该字符串用于调试目的，但最好表依赖特定的回递字符串( :sub:`俺猜可能Google 随时会变更此回传字串`)。


为"内联"安装准备用户界面元素 User interface elements for inline installation
------------------------------------------------------------------------------

"内联"安装背后的关键概念之一就是允许作为开发者的我们
可以选择当用户安装应用或扩展时,何时向用户进行确认.
- 如果该扩展,对我们的网站是至关重要的，可能希望使用弹出式对话框或其它引人注目的用户界面元素,来提示用户。
- 如果该扩展,仅提供辅助功能，可能就期望隐藏在网站的二级页面里


由于"内联式"安装必须通过用户动作触发（例如，鼠标点击），
因此，建议配合可点击的用户界面元素完成触发，如按钮。
进一步的,建议使用和 `Chrome网上应用店`_ 里相同的按钮文字（英语版本的是 “Add to Chrome”）。


检查是否已经安装 Checking if an item is already installed
------------------------------------------------------------------------------

我们当然会期望,仅显示用户没有安装的扩展,以及对应的页面效果元素.

要检查是否已经安装了一个应用程序，
可以使用 
:ref:`chrome.app.isInstalled <chapter5webstoreFAQ>`属性。
根据应用页面中可包含的`URL`查询，
可以根据其属性对进行适当的显示或隐藏。

例如：

.. code-block:: html
  :emphasize-lines: 1,3-4

    <button onclick="chrome.webstore.install()" id="install-button">Add to Chrome</button>
    <script>
    if (chrome.app.isInstalled) {
      document.getElementById('install-button').display = 'none';
    }
    </script>



通过嵌入网页的脚本，
可以与扩展沟通，以便知道是否已经安装。
例如，可以针对安装页面构造内容脚本：


.. code-block:: js

    var isInstalledNode = document.createElement('div');
    isInstalledNode.id = 'extension-is-installed';
    document.body.appendChild(isInstalledNode);


这样就可以检查安装页面中相关 `DOM`_ 节点是否存在，这标志着扩展是否已安装：

.. code-block:: html
  :emphasize-lines: 1,3-4

    <button onclick="chrome.webstore.install()" id="install-button">Add to Chrome</button>
    <script>
    if (document.getElementById('extension-is-installed')) {
      document.getElementById('install-button').display = 'none';
    }
    </script>





验证网站要求 Verified site requirement
------------------------------------------------------------------------------



出于安全原因，"内嵌式"安装可以只被启动于在 `Chrome网上应用店`_ `经过验证 <http://www.google.com/support/webmasters/bin/answer.py?hl=en&answer=34592>`_ 的项目网站页面中
(通过网站 `管理员工具 <http://www.google.com/webmasters/>`_ )


.. note:: 注意

    - 如果我们拥有验证域名的所有权（例如，http://example.com）
    - 那么,可以启动任何子域或页面进行内联安装
    - （例如，http://app.example.com 或 http://example.com/page.html ）


.. seealso:: (^.^)
    
    原文: `Chrome Web Store:Using Inline Installation <https://developers.google.com/chrome/web-store/docs/inline_installation>`_

    
