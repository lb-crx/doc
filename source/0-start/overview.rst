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



文件 Files
------------------------------------------------------------------------------

每个扩展都应该包含下面的文件:
  - `manifest` ~ 声明文件
  - 一个或多个html文件（除非这个应用是一个皮肤）
  - `可选的`: 一个或多个javascript文件
  - `可选的`: 任何需要的其他文件，例如图片

在进行扩展开发时，应该将所有把这些文件都放到同一个目录下.
发布扩展时，这个目录将全部打包为一个特殊的使用 `.crx` 为后綴的 `ZIP`_ 文件.
如果使用 `Chrome Developer Dashboard <https://chrome.google.com/webstore/developer/dashboard>`_ 上传扩展, 则,会自动自动生成 `.crx` 文件

进一步有关扩展的发布,请参考: :ref:`Hosting <chapter1-Hosting>`


文件引用    Referring to files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

任何必要的文件都可以纳入扩展中，但是怎么使用它们呢？
一般而言，同普通的HTML文件中那样通过相对地址来引用文件。
下例演示了如何引用 `images` 子目录下的 `myimage.png`

.. code-block:: html

    <img src="images/myimage.png">



当然,使用 `Google`_ `Chrome`_ 或是 `猎豹浏览器`_ 进行调试时,
扩展中的所有文件,可以使用如下的绝对路径进行访问::

    chrome-extension://<extensionID>/<pathToFile> 

以上 `URL`_ 中, `<extensionID>` 是由扩展系统生成的唯一 `ID` .
我们可以在扩展管理界面 (chrome://extensions) 中看到每个扩展的 `ID`.

`<pathToFile>` 则是文件以扩展目录为根的路径,其实,也是相对路径.

当我们在扩展目录中开发时(即,没有被打包前),
扩展的 ID 是可以变更的.
特别是每当我们从不同的目录中载入解开压缩的扩展时, ID 会变更.
如果我们的扩展代码中,需要包含在扩展中文件的特殊路径,
可以使用预定义变量 `@@extension_id` ,以免在开发过程中使用可能变更的 ID .

当你
When you package an extension (typically, by uploading it with the dashboard), the extension gets a permanent ID, which remains the same even after you update the extension. Once the extension ID is permanent, you can change all occurrences of @@extension_id to use the real ID. 

:ref:`Predefined messages <chapter1-i18n>`


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
