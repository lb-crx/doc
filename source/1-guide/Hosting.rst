.. include:: ../LINKS.rst

.. _chapter1-Hosting:



发布托管 Hosting
=============================

本文分享如何在自己的服务器上托管发布 `.crx` 文件。

如果完全是通过 `Chrome网上应用店 <https://chrome.google.com/webstore?hl=zh-CN>`_ 发布扩展程序、应用程序或主题背景，则不需要关注本文，
而应该参考 `应用店帮助 <https://www.google.com/support/chrome_webstore/?hl=zh-Hans>`_ 和 `文档 <https://code.google.com/chrome/webstore/index.html>`_  :sup:`(英文)` 

通常扩展程序、可安装的网上应用程序和主题背景都以 `.crx` 文件的形式提供，无论通过 `Chrome网上应用店 <https://chrome.google.com/webstore?hl=zh-CN>`_  还是自定义的服务器。

当使用 `Chrome开发者信息中心 <https://chrome.google.com/webstore/developer/dashboard?hl=zh-CN>`_ 上传ZIP文件时，信息中心会为我们创建 `.crx` 文件。

如果不通过信息中心发布扩展程序，需要自己创建 `.crx` 文件，这在 :ref:`打包 <chapter1-Packaging>` 中进行了描述。

进一步的,也可以指定 :ref:`自动更新 <chapter1-Autoupdating>` 信息，确保用户能自动获得最新的 `.crx` 文件。
(如果不指定,将默认尝试向 `Chrome网上应用店 <https://chrome.google.com/webstore?hl=zh-CN>`_ 查询是否有更新, 参考: :ref:`自动更新 <chapter1-Autoupdating>` )

托管 `.crx` 文件的服务器必须使用合适的HTTP头，以便用户可以通过单击链接来安装这些文件。
如果以下 `任何一个` 条件成立，`Google`_ `Chrome`_ 及兼容浏览器将认为文件可安装：

- 文件的内容类型为 `application/x-chrome-extension`
- 文件的后缀为 `.crx` 并且以下条件 `全` 都成立：
    - 文件 `没有` 如下HTTP头: ``X-Content-Type-Options: nosniff``
    - 文件的内容类型为以下的某一种：
        - 空字符串
        - "text/plain"
        - "application/octet-stream"
        - "unknown/unknown"
        - "application/unknown"
        - "*/*"

不能识别可安装文件的最常见原因是服务器发送了 `X-Content-Type-Options: no sniff` 头，第二个最常见的原因是服务器发送了不在以上列表中 `未知的内容类型` 。要解决HTTP头的有关问题，要么更改服务器的配置文件，要么尝试在另一个服务器上托管 `.crx` 文件。


.. note:: (~_~)

    - 不过,猎豹浏览器的扩展托管,可以更加简单
    - 因为我们可以通过对内核的定制,智能感知到是 `.crx` 文件,从而尝试下载后安装!



.. seealso:: (^.^)
    
    原文: `Hosting <http://code.google.com/chrome/extensions/hosting.html>`_
