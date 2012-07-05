.. include:: LINKS.rst


嗨!
------------

本手册是有关如何开发 `Google`_ `Chrome`_ 浏览器扩展应用的.
因为扩展上手快, 所以 APIs以及文档 都号称可以 `扩展` 任何东西!


.. sidebar:: 注意

    除非特殊说明,
    这里的所有文档适用于扩展以及打包应用(`packaged apps.`)



从哪儿开始?
------------------------------------

开始编程前,建议阅读.

:ref:`扩展入门 <chapter0hallo>` :
    如何在5分钟里创建一个 `Hello World` 的扩展
:ref:`综述 <chapter0overview>` :
    对整个扩展体系进行要点介绍

同时查阅以下:
    - :ref:`开发手册 <chapter1index>`
    - :ref:`扩展样例 <chapter4index>`
    - `Stack Overflow [google-chrome-extension] tag <http://stackoverflow.com/questions/tagged/google-chrome-extension>`_
    - `Group: chromium-extensions <http://groups.google.com/a/chromium.org/group/chromium-extensions>`_
    - `Chrome Web Store <http://chrome.google.com/webstore>`_
    - `Chrome Web Store 开发文档 <chapter5webstore>`_


文档版本
------------------------------------

通常文档发布在 `http://code.google.com/chrome/extensions/<filename>`
(比如: http://code.google.com/chrome/extensions/overview.html)

但是,如果我们想查阅最新文档,或者我们使用的是不同的 Chrome 版本,
那么,可能就需要查阅不同的地址
(比如: .../extensions/ `dev` /overview.html)
以下表格说明不同 `URL`_ 的版本含义:

.. list-table:: 技术手册的版本约定
   :widths: 20 50
   :header-rows: 1

   * - URL
     - 版本
   * - `.../extensions/... <http://code.google.com/chrome/extensions/overview.html>`_
     - 最常用地址,指稳定版本接口文档
   * - `.../extensions/beta/... <http://code.google.com/chrome/extensions/beta/overview.html>`_
     - 文档对应 `Beta` 分支的 `Chrome`_
   * - 
     - `注意!` `Beta` 分支的接口可能会改变
   * - `.../extensions/dev/... <http://code.google.com/chrome/extensions/dev/overview.html>`_
     - 文档对应 `Dev` 分支的 `Chrome`_ ,这类版本经常是进行问题修复,或是还未集成到正式版本的特性的增补
   * - 
     - `注意!` `Dev` 分支的接口可能会改变
   * - `.../extensions/trunk/... <http://code.google.com/chrome/extensions/trunk/overview.html>`_
     - 最新版本文档,如果使用 `Canary <http://tools.google.com/dlpage/chromesxs>`_ 或是 `Chromium <http://dev.chromium.org/>`_ 想体验最新特性的,可以参考,这类版本也可能包含问题修复,或是还未集成到正式版本的特性的增补
   * - 
     - `注意!` `trunk` 版本的文档是很可能包含错误的!


`好吧,我们的文档是以稳定版本文档为主的 ;-)`

.. seealso:: (^.^)
    
    原文: `Hello There! <http://code.google.com/chrome/extensions/docs.html>`_

