.. include:: ../LINKS.rst

.. _chapter3-CSP:



内容安全策略 Content Security Policy (CSP)
==================================================================



为了缓解很大一部分潜在的跨站脚本问题，Chrome浏览器的扩展程序系统引入了 `内容安全策略（CSP） <http://dvcs.w3.org/hg/content-security-policy/raw-file/tip/csp-specification.dev.html>`_ 的一般概念。这将引入一些相当严格的策略，会使得扩展程序在默认情况下更加安全，并向您提供创建并强制应用一些规则，管理您的扩展程序和应用程序允许载入的内容类型。

大体上，CSP以白名单/黑名单的机制对您的扩展程序载入或执行的资源起作用。为您的扩展程序定义一项合理的策略使您仔细考虑您的扩展程序需要的资源，并且使浏览器确保您的扩展程序只能访问指定的那些资源。这些策略提供了比您的扩展程序请求的
:ref:`主机权限 <chapter3-Manifest-permissions>` 更高的安全性，它们是额外的保护层，而不是主机权限的替代品。

在网页中，这样的策略通过HTTP头信息或者meta元素定义。在Chrome浏览器的扩展程序系统中，它们都不是合适的方式。扩展程序的策略通过扩展程序的
:ref:`manifest.json <chapter3-Manifest>` 文件定义，如下所示：
::

    {
      ...,
      "content_security_policy": "[策略字符串写在这里]"
      ...
    }


默认策略限制 Default Policy Restrictions
------------------------------------------------------------------------------


没有定义
:ref:`manifest.json <chapter3-Manifest>` （清单文件版本） 的扩展程序包没有默认的内容安全策略，而选择
:ref:`manifest.json <chapter3-Manifest>` 2的扩展程序具有如下默认的内容安全策略：
::

    script-src 'self'; object-src 'self'

这一策略通过两种方式限制扩展程序和应用程序，来增强安全性：




内嵌JavaScript代码将不会执行    Inline JavaScript will not be executed
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

内嵌 `JavaScript`_ 代码以及危险的字符串到 `JavaScript`_ 方法（例如 `eval` ）将不会执行。
这一限制既禁用了内嵌的 `<script>` 块， **同时也包括** 内嵌的事件处理函数
（例如 `<button onclick="...">` ）。

第一个限制使我们不可能意外地执行任何恶意的第三方提供的脚本，彻底消除了一大部分的跨站脚本攻击。
然而，这也确实要求编写代码时清晰地将内容与行为分开（这也是我们当然应该做的吧？）。

举一个例子可能会更加清楚，
可能想要编写一个包含如下内容的popup.html，作为 :ref:`浏览器按钮的弹出内容 <chapter1-BrowserActions>` ：

.. code-block:: js
  :emphasize-lines: 5,20

    <!doctype html>
    <html>
      <head>
        <title>我做的很棒的弹出内容！</title>
        <script>
          function awesome() {
            // 做些很棒的事情！
          }

          function totallyAwesome() {
            // 做些棒极了的事情！
          }

          function clickHandler(element) {
            setTimeout("awesome(); totallyAwesome()", 1000);
          }
        </script>
      </head>
      <body>
        <button onclick="clickHandler(this)">
          单击看看会发生什么！
        </button>
      </body>
    </html>



三个地方需要修改，才能使以上代码按照您预期的方式工作：

#. `clickHandler` 定义需要移至外部的 `JavaScript`_ 文件中（例如 `popup.js` 就不错）。
#. 内嵌的事件处理定义必须通过 `addEventListener` 重写并放在 `popup.js` 中。
#. `setTimeout` 调用需要重写，避免将字符串 `"awesome(); totallyAwesome()"` 转换为 `JavaScript`_ 来执行。

做出这些更改后代码如下所示：

.. code-block:: js
  :emphasize-lines: 11-15,18,21

    //popup.js:
    //=========

    function awesome() {
      // 做些很棒的事情！
    }

    function totallyAwesome() {
      // 做些棒极了的事情！
    }

    function awesomeTask() {
      awesome();
      totallyAwesome();
    }

    function clickHandler(e) {
      setTimeout(awesomeTask, 1000);
    }

    // 通过监听文档的`DOMContentLoaded`事件在DOM 完全载入后添加事件处理函数，
    // 当事件触发时向指定元素添加您自己的事件处理函数。
    document.addEventListener('DOMContentLoaded', function () {
      document.querySelector('button').addEventListener('click', clickHandler);
    });



.. code-block:: html
  :emphasize-lines: 8

    popup.html:
    ===========

    <!doctype html>
    <html>
      <head>
        <title>我做的很棒的弹出内容！</title>
        <script src="popup.js"></script>
        </script>
      </head>
      <body>
        <button>单击看看会发生什么！</button>
      </body>
    </html>


只有本地脚本和对象资源才会载入    Only local script and and object resources are loaded
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
本与对象资源只能从扩展程序包中载入，不能从网上载入。
这样确保扩展程序只会执行确实允许的代码，避免任何主动的网络攻击者，恶意地重定向您对资源的请求。
不要编写依赖于外部CDN载入的jQuery（或其它库）的代码，而应该考虑将特定版本的jQuery包含在扩展程序包中。
即，不要：

.. code-block:: html
  :emphasize-lines: 5

    <!doctype html>
    <html>
      <head>
        <title>我做的很棒的弹出内容！</title>
        <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
        </script>
      </head>
      <body>
        <button>单击看看会发生什么！</button>
      </body>
    </html>


而应该下载此文件，包括在扩展程序包中，并编写如下代码：

.. code-block:: html
  :emphasize-lines: 5

    <!doctype html>
    <html>
      <head>
        <title>我做的很棒的弹出内容！</title>
        <script src="jquery.min.js"></script>
        </script>
      </head>
      <body>
        <button>单击看看会发生什么！</button>
      </body>
    </html>



放宽默认策略 Relaxing the default policy
------------------------------------------------------------------------------

我们无法放宽限制，允许执行内嵌 `JavaScript`_ 代码。
特别地，设置包含 `unsafe-inline` 的脚本策略不会生效，这是有意为之。

另一方面，如果我们需要某些外部 `JavaScript`_ 代码或对象资源，可以在有限的程度上放宽策略，将特定的HTTPS来源的可接受脚本加入白名单。
将不安全的HTTP资源加入白名单不会生效，这也是有意为之，因为我们希望确保载入的具有扩展程序的更高权限的可执行资源一定是您预期的，而没有被主动的网络攻击者替换。由于 `中间人攻击 <https://en.wikipedia.org/wiki/Man-in-the-middle_attack>`_  非常普遍，并且通过HTTP无法检测到，所以只有HTTPS来源才会被接受。

允许通过HTTPS载入来自 `example.com` 的脚本资源的放宽策略定义如下所示：

::

    {
      ...,
      "content_security_policy": "script-src 'self' https://example.com; object-src 'self'",
      ...
    }

.. note:: 注意

    - `script-src` 与 `object-src` 都由这一策略定义， `Chrome`_浏览器不会接受不将这些值限制为（至少）`'self'`的策略。


利用 :ref:`Google Analytics（分析） <chapter2Analytics>` 是这一种策略定义的典型例子，这样的情况很常见，
所以我们在利用 :ref:`Google Analytics（分析） <chapter2Analytics>` 追踪事件的示例扩展程序中提供了 
:ref:`简单例子 <../_static/examples/tutorials/analytics/>` ，
并在 :ref:`简洁的教程 <chapter2Analytics>` 中提供了更多详情。


- `.../examples/tuturoials/analytics/ <../_static/examples/tutorials/analytics/>`_



使用更严格的策略 Tightening the default policy
------------------------------------------------------------------------------

当然也可以带来更多不变为代价，使用您的扩展程序允许的更严格的策略，增强安全性。
例如，要指定您的扩展程序只能从自己的包中载入所有类型（如图片等）的资源，
可以使用这样的策略： `default_src 'self'` 。

:ref:`Mappy示例扩展程序 <chapter4mappy>` 就是使用的策略比默认设置更加严格的例子。





.. seealso:: (^.^)
    
    原文: `Content Security Policy (CSP) <http://code.google.com/chrome/extensions/crx.html>`_



