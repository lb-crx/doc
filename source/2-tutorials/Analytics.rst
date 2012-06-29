.. include:: ../LINKS.rst

.. _chapter2Analytics:


Google Analytics（分析）
==============================================================================

本教程演示如何使用Google Analytics（分析）来追踪您的扩展程序的使用情况

要求
------------------------------------------------------------------------------

本教程要求对编写 `Google`_ `Chrome`_ 浏览器的扩展程序有一定的了解。
如果需要有关如何编写扩展程序的信息，
请阅读 :ref:`入门教程 <chapter0hallo>` 。

还需要设置一个
`Google Analytics （分析）帐户 <http://www.google.com/analytics>`_
来追踪您的扩展程序。


.. note:: 注意

    - 当设置账户时，在网站URL域中可以使用任何值
    - 因为扩展程序没有自己的URL


.. figure:: ../_static/images/tut_analytics/screenshot01.png


.. warning:: (#_#)

    - 另外注意Google Analytics（分析）至少需要版本 `4.0.302.2` 以上的Google Chrome浏览器才能正常工作，
    - 使用更早版本Google Chrome浏览器的用户不会在您的Google Analytics（分析）报告中出现。
    - 在 :ref:`常见问题 <chapterFAQ>` 中进一步查阅,了解如何确定Google Chrome浏览器的每一个版本都部署到哪一个平台上了。



安装追踪代码 Installing the tracking code
------------------------------------------------------------------------------


标准的分析追踪代码段是 `https://` 协议网址下载由 `SSL`_ 保护的 `ga.js` ,
`Chrome`_ 扩展应用 `只能` 使用 `SSL`_ 保护 `ga.js` 版本,
通过不安全的 `HTTP`_ 加载 `ga.js` 是为默认
:ref:`内容安全策略（CSP） <chapter3-CSP>`
不允许的!

另外 `Chrome`_ 扩展协议 `chrome-extension://` 中,
需要略作修订,直接从 `https://ssl.google-analytics.com/ga.js` 加载,
而不能使用默认的文件位置.


如下是一个修改过的代码片段，用于 `异步跟踪API <http://code.google.com/apis/analytics/docs/tracking/asyncTracking.html>`_ :


.. code-block:: js

    (function() {
      var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
      ga.src = 'https://ssl.google-analytics.com/ga.js'; // <-- 就这行!
      var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
    })();


同时,还要配合放松默认的安全策略,以便扩展可以加载对应资源.
在 :ref:`manifest.json <chapter3-Manifest>` 中配置成类似如下:


.. code-block:: js

    {
      ...,
      "content_security_policy": "script-src 'self' https://ssl.google-analytics.com; object-src 'self'",
      ...
    }


Here is a popup page (popup.html) which loads the asynchronous tracking code via an external JavaScript file (popup.js) and tracks a single page view: 
以下便是弹出窗口 (`popup.html`) 配合外部 `JavaScript`_ 代码(`popup.js`) 进行异步追踪的示例:

::

    popup.js:
    =========

    var _gaq = _gaq || [];
    _gaq.push(['_setAccount', 'UA-XXXXXXXX-X']);
    _gaq.push(['_trackPageview']);

    (function() {
      var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
      ga.src = 'https://ssl.google-analytics.com/ga.js';
      var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
    })();

    popup.html:
    ===========
    <!DOCTYPE html>
    <html>
     <head>
       ...
      <script src="popup.js"></script>
     </head>
     <body>
       ...
     </body>
    </html>


Keep in mind that the string 

.. note:: 注意!

    - 类似 `UA-XXXXXXXX-X` 的字串,是我们的 `Google 分析帐号 <http://www.google.com/analytics>`_ 编码



查阅页面追踪  Tracking page views
------------------------------------------------------------------------------

`_gaq.push(['_trackPageview']);`
代码将跟踪单个页面访问，这一代码可以用在扩展程序中的任何一个页面中。

- 放在后台页面中，则每一次浏览器会话记录一次访问。
- 放在弹出内容中，则每一次打开弹出内容记录一次访问。


通过查看我们扩展程序中的每一个页面的访问数据，
就可以了解用户每一次浏览器会话与扩展程序有多少次交互。

.. figure:: ../_static/images/tut_analytics/screenshot02.png



监视分析请求 Monitoring analytics requests
------------------------------------------------------------------------------

要确保来自我们扩展程序的跟踪数据发送到了Google Analytics（分析），
可以在开发人员工具窗口中审查我们扩展程序中的页面（有关更多信息请参见 :ref:`调试 <chapter2debugging>` 教程）。

如下图所示，如果一切设置妥当，应该看到文件名为 `__utm.gif` 的请求。

.. figure:: ../_static/images/tut_analytics/screenshot04.png


事件追踪 Tracking events
------------------------------------------------------------------------------

通过配置事件跟踪，我们可以追踪用户与扩展程序的哪一部分交互最多。

例如，如果扩展里有三个按钮供用户单击：

.. code-block:: html

    <button>Button 1</button>
    <button>Button 2</button>
    <button>Button 3</button>


编写函式，向Google Analytics（分析）发送单击事件：

.. code-block:: js

    function trackButton(e) {
        _gaq.push(['_trackEvent', e.target.id, 'clicked']);
      };


对所有按钮绑定点击的事件句柄:

.. code-block:: js

    var buttons = document.querySelectorAll('button');
      for (var i = 0; i < buttons.length; i++) {
        buttons[i].addEventListener('click', trackButtonClick);
      }


Google Analytics（分析）事件跟踪概览页面将提供每一个按钮单击次数的度量：

.. figure:: ../_static/images/tut_analytics/screenshot03.png


使用这种方法，我们可以看到扩展程序的哪些部分利用太多或太少，
这一信息可以帮助我们, 决策有关重新设计用户界面或者实现额外功能。

更多有关事件跟踪API的使用信息，参见Google Analytics（分析）的
`开发者文档 <http://code.google.com/intl/zh-CN/apis/analytics/docs/tracking/eventTrackerOverview.html>`_ 。


示例代码 Sample code
------------------------------------------------------------------------------

使用这些技术的示例扩展程序在Chromium源代码树中的以下位置

- `.../examples/tuturoials/analytics/ <../_static/examples/tutorials/analytics/>`_


.. seealso:: (^.^)
    
    原文: `Tutorial: Google Analytics <http://code.google.com/chrome/extensions/tut_analytics.html>`_

