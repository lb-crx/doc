.. include:: ../LINKS.rst

.. _chapter0overview:


综述
===================

当读完了这个综述和 :ref:`入门扩展 <chapter0hallo>` 之后，就可以开始创建应用（扩展）和 `PackagedApp` 了。



.. note:: 

    `PackagedApp` 是通过应用（扩展）的方式实现的，所以除非特别声明，本页所有内容都适用于 `PackagedApp`



基本概念 The basics
------------------------------------------------------------------------------

一个应用（扩展）其实是压缩在一起的一组文件，包括HTML，CSS，Javascript脚本，图片文件，以及其它任何需要的文件。 应用（扩展）本质上来说就是web页面，它们可以使用所有的 :ref:`浏览器提供的各种API <chapter3othersapi>` ，从XMLHttpRequest到JSON到HTML5全都有。


应用（扩展）可以与Web页面交互，或者通过
:ref:`内容脚本 <chapter1-ContentScripts>`
或 :ref:`Cross-Origin XMLHttpRequest <chapter1-CrossOriginXMLHttpRequest>` 与服务器交互。应用（扩展）还可以访问浏览器提供的内部功能，
例如 :ref:`标签 <chapter1-Tabs>` 或 :ref:`书签 <chapter1-Bookmarks>` 等。




扩展的界面    Extension UIs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
很多应用（不包括WebApp）会以
:ref:`浏览行为(browser action) <chapter1-BrowserActions>`
或 :ref:`页面行为(page action) <chapter1-PageActions>` 的形式在浏览器界面上展现出来。
每个应用（扩展）最多可以有一个
:ref:`浏览行为(browser action) <chapter1-BrowserActions>`
或  :ref:`页面行为(page action) <chapter1-PageActions>`
。

当应用（扩展）的图标是否显示出来是取决于单个的页面时，应当选择 :ref:`页面行为(page action) <chapter1-PageActions>` ；

当其它情况时可以选择 :ref:`浏览行为(browser action) <chapter1-BrowserActions>` 。


.. |browser-action| image:: ../_static/images/overview/browser-action.png
.. |page-action| image:: ../_static/images/overview/page-action.png
.. |browser-action-with-popup| image:: ../_static/images/overview/browser-action-with-popup.png


.. list-table:: 扩展的表现形态
   :widths: 20 20 20
   :header-rows: 1

   * - |browser-action|
     - |page-action|
     - |browser-action-with-popup|
   * - :ref:`邮件扩展 <chapter4gmail>` 用 `browser action` (图标在工具栏)
     - :ref:`地图扩展 <chapter4mappy>` 用 `page action` (图标在地址栏) 及 `content script` (脚本注入到了页面)
     - :ref:`新闻扩展 <chapter4news>` 用 `browser action` 点击时显示 `popup`


扩展也可以通过其它方式提供界面，比如加入到上下文菜单，
提供一个选项页面或者用 `content script` 改变页面的显示等。可以在" :ref:`开发指南 <chapter1index>` "中找到应用（扩展）特性的完整列表以及实现的细节。


界面    Packaged app UIs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

一个WebApp通常会打包一个包含了主要功能的html页面进来。
例如下图中这个WebApp在HTML页面中显示了一个flash文件。

.. figure:: ../_static/images/overview/flash-app.png


.. seealso:: (^.^)
    
    详细参考: :ref:`Packaged App <chapter2packagedapp>`



Files
------------------------------------------------------------------------------

    Referring to files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

    The manifest file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Architecture
------------------------------------------------------------------------------

    The background page
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

    UI pages
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

    Content scripts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Using the chrome.* APIs
------------------------------------------------------------------------------

    Asynchronous vs. synchronous methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

    Example: Using a callback
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

    More details
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Communication between pages
------------------------------------------------------------------------------

Saving data and incognito mode
------------------------------------------------------------------------------

现在? 
------------------------------------------------------------------------------



.. seealso:: (^.^)
    
    原文: `Getting Started <http://code.google.com/chrome/extensions/trunk/overview.html>`_
