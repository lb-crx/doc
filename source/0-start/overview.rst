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
可以使用 :ref:`预定义消息 <chapter1-i18n>`  `@@extension_id` ,以免在开发过程中使用可能变更的 ID .

当你将扩展打包时
(经典的情景是准备从开发者面板上传),
扩展将获得一个 `永久ID` ,
意味着将根据此 `永久ID` 来识别以后扩展的更新.
一但扩展拥有了 `永久ID` ,你就可以将所有 `@@extension_id` 替换成对应的真实ID 了!


.. warning:: (#_#)

    - 注意! `永久ID` 等于扩展作品的电子身份证!
    - 但是! 如果从设计开发到发布到 `Google`_ 的 `web store` 中,整个过程中,如果我们从来没有使用内置的打包工具尝试打包,那么, `永久ID` 将在 `web store` 平台中自动生成,且我们无法下载到本地!
    - 这样,就意识着,我们永远无法将扩展,放到其它网站中发布,除非先发布到 `Google`_ 的 `web store`
    - 所以! 建议一定要先在本地进行扩展的打包,并获得 `.pem` 后綴的 `永久ID`私钥!




引用文件    The manifest file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
引用文件指 `manifest.json` ,
为扩展提供了必要的信息,
是扩展可以发行使用的关键文件.

这儿有份标准的引用文件,声明了从 google.com 获得信息的 `browser action` (浏览器行为)扩展

::

  {
    "name": "My Extension",
    "version": "2.1",
    "description": "Gets information from Google.",
    "icons": { "128": "icon_128.png" },
    "background": {
      "scripts": ["bg.js"]
    },
    "permissions": ["http://*.google.com/", "https://*.google.com/"],
    "browser_action": {
      "default_title": "",
      "default_icon": "icon_19.png",
      "default_popup": "popup.html"
    }
  }


更多细节,参考: :ref:`引用文件 <chapter3-Manifest>`


基本架构 Architecture
------------------------------------------------------------------------------

绝大多数扩展都包含一个后台页面(`background page`),
一个隐藏的页面,包含了扩展的主逻辑.

当然也可以包含其它页面来形成扩展的界面.

如果扩展需要直接嵌入到用户加载页面中(而不仅仅是扩展本身包含的页面),
就需要使用 内容脚本(`content script`).


后台页面    The background page
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

以下插图,展示安装了两个扩展的浏览器,
分别是黄色图标代表的 `browser action` 和蓝色图标代表的 `page action`.

因为,是在 `background.html` 文件里定义了browser action和javascript代码.
故而,在所有窗口里的 `browser action` 都可以工作.

.. figure:: ../_static/images/overview/arch-1.gif


不要使用对我们无用的 `后台页面`.
因为,事实上 `后台页面` 总是打开的,
所以,如果用户安装了很多有 `后台页面` 的扩展,可能告成造成浏览器性能下降.


Here are some examples of extensions that usually do not need a background page:
以下列举一些 `没必要` 使用 `后台页面` 的扩展:
    - 扩展是 `browser action` 的,并且界面仅有弹出页面的(可能只是配置页面)
    - 扩展提供了覆盖页面的 ~ 用以替代标准页面的
    - 扩展是 `内容脚本` (content script) 的,并且不使用本地存储以及扩展接口
    - 扩展除了配置页面,没有其它界面的


详见: :ref:`后台页面 <chapter1-BackgroundPages>`


界面页面    UI pages
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

扩展可以包含原生 `HTML`_ 页面来作界面.
例如: `browser action` 扩展可以包含一个弹窗(popup)，而弹窗就能用html页面实现.
任何扩展也可以拥有配置页面,用以提供给用户定制扩展的工作行为.
另外一种特殊页面就是 :ref:`覆盖页面 <chapter1-OverridePages>`.
还可以使用 `chrome.tabs.create()` 或 `window.open()` 来显示扩展内部的HTML文件。

扩展里面的HTML页面可以互相访问各自DOM树中的全部元素，或者互相调用其中的函数。

下图显示了 :ref:`浏览行为 <chapter1-BrowserActions>` 式扩展的弹窗的架构.
弹窗的内容是由HTML文件（`popup.html`）定义的web页面。
扩展同时也有 `后台页面`,
但是 弹窗页面中 不必复制后台页面(background.html)里的代码,因为它可以直接调用后台页面中的函式。


.. figure:: ../_static/images/overview/arch-2.gif

详见: :ref:`浏览行为 <chapter1-BrowserActions>` , 
:ref:`配置页面 <chapter1-OptionsPages>` , 
:ref:`覆盖页面 <chapter1-OverridePages>` , 
以及下述的 `页间交流` 一节.


内容脚本    Content scripts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

如果扩展需要同(用户的)web页面进行交互，那么就需要使用内容脚本(Content script)。

内容脚本(Content script)脚本是指能够在浏览器已经加载的页面内部运行的javascript脚本。
可以将内容脚本(Content script)看作是网页的一部分，而不是它所在的应用（扩展）的一部分。

内容脚本(Content script)可以获得浏览器所访问的web页面的详细信息，并可以对页面做出修改。
下图显示了一个 内容脚本(Content script) 可以读取并修改当前页面的DOM树。但是它并不能修改它所在应用（扩展）的后台页面的DOM树。

.. figure:: ../_static/images/overview/arch-3.gif

内容脚本(Content script)与它所在的扩展并不是完全没有联系。
一个内容脚本(Content script)脚本可以与所在的扩展交换消息，如下图所示。

例如，当一个内容脚本(Content script)从页面中发现一个RSS种子时，
它可以发送一条消息。或者由后台页面发送一条消息，
要求 内容脚本(Content script) 修改一个网页的内容。

.. figure:: ../_static/images/overview/arch-cs.gif


详见: :ref:`内容脚本 <chapter1-ContentScripts>` , 


使用 chrome.* APIs
------------------------------------------------------------------------------

除了可以访问所有 web 页面可以使用的接口之外,
扩展更是可以访问 `Chrome-专有` 接口来同浏览器更加紧密的进行结合 (之于 `猎豹浏览器`_ 也会公布各种专有接口给大家使用: :ref:`猎豹APIs <chapter3-liebaoapi>`).

例如: 所有扩展或是 `Web app.`_ 都能使用标准的 `window.open()` 来打开一个 `URL`_ .
但是,如果我们想指定某个已经存在的窗口打开 `URL`_ ,
就需要使用 `Chrome-专有` 的 `chrome.tabs.create()` 来达成!





同步对异方法   Asynchronous vs. synchronous methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

多数 `chrome.*` 开头的接口是 `异步`(Asynchronous) 的: 它们立即返回,无需等待操作完成.
如果我们想知道操作的结果,就需要在方法中加入一个回调函式.
回调在主方法执行后稍晚才执行(甚至更加迟).

这有个异步签名方法的实例
::

    chrome.tabs.create(object createProperties, function callback)



其它 `chrome.*` 样儿的方法又都是 `同步`(Synchronous) 的.
同步方法无需回调函式,
因为它们无法没有执行完操作是不会返回的,
一般同步方法总是有个返回类型.
例如方法 `chrome.extensions.getBackgroundPage()`
::

    Window chrome.extension.getBackgroundPage()



此方法没有回调,并返回 `Window` 类型对象,
因为它同步返回后台页面,并不异步执行其它任务.



实例:使用回调    Example: Using a callback
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Say you want to navigate the user's currently selected tab to a new URL. 

假设我们想在用户当前选定的标签页中打开新的 `URL`_ 
首先要获取当前标签的 ID (使用 :ref:`chrome.tabs.getSelected() <Tabs.getSelected>`) ,
然后令其跳转到新的 `URL`_  (使用 :ref:`chrome.tabs.update() <Tabs.update>`).

假如 `getSelected()` 是同步的,我们可能使用如下代码:

.. code-block:: js

    //此代码不工作!
    var tab = chrome.tabs.getSelected(null); //警告!!!
    chrome.tabs.update(tab.id, {url:newUrl});
    someOtherFunction();


这样会出错,因为 `getSelected()` 是异步的，它不等待操作完成就返回了，
所以,事际上并没有返回任何值（尽管有些异步方法会返回信息）。
我们可以从其函式参数中的 `回调` 看出 `getSelected()` 是异步的

::

    chrome.tabs.getSelected(integer windowId, function callback)


修订上面的代码，必须使用那个回调参数。
以下代码显示如何定义一个回调函数，从 `getSelected()` 获得结果（通过参数 `tab`）并调用 `update()`

.. code-block:: js

    //此代码可工作
    1: var tab = chrome.tabs.getSelected(null, function(tab) {
    2:     chrome.tabs.update(tab.id, {url:newUrl});
    3: });
    4: someOtherFunction();

此例子中，以上代码其实是按如下顺序执行的:

- 1
- 4
- 2

只有在当前选定标签的信息可用后，即 `getSelected()`返回后的某一时刻，
浏览器才会调用在 `getSelected` 中指定的回调函数（并且执行第二行）。
另外,尽管 `update()` 是异步的，本例中并没有使用回调参数，
因为我们对于回调的结果并不感兴趣。


更多细节    More details
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

更多细节
有关更多信息，请参见  :ref:`chrome.* API <chapter3-apindex>` 接口索引文档,

并观看以下视频（英文）: `扩展接口的设计 <http://www.youtube.com/embed/bmxr75CV36A?rel=0>`_

.. warning:: (#_#)

    需要翻越...




页间通信 Communication between pages
------------------------------------------------------------------------------

扩展程序中的HTML页面通常需要通信。
好在,同一扩展的所有页面在同一个进程中的同一个线程上执行，
所以,页面可以直接互相调用函数。

要获得扩展程序中的页面，使用 :ref:`chrome.extensions <chapter1-Extension>` 方法，例如
`getViews()` 和 `getBackgroundPage()` 。
一旦页面引用了扩展程序中的其它页面，第一个页面就可以执行其它页面上的函数，并且可以操纵它们的DOM。


数据保存和隐身模式 Saving data and incognito mode
------------------------------------------------------------------------------

扩展程序可以使用HTML5
`网页存储API(web storage API) <http://dev.w3.org/html5/webstorage/>`_
（例如 `localStorage`）或者向服务器发出请求从而保存数据。

每当您要保存任何数据前，首先考虑是否来自隐身窗口。
默认情况下，扩展程序不在隐身窗口中运行，
但是打包应用(`packaged apps`)  `会`在隐身窗口中运行。

当浏览器处于 `隐身模式` 时，需要考虑用户对我们的扩展程序或打包应用(`packaged apps`)的需求。
`隐身模式` 要求不留下任何痕迹。
当处理来自隐身窗口的数据时，尽可能地遵守这一约定。

例如，如果您的扩展程序通常将浏览器历史记录保存至云端，不要保存来自隐身窗口的历史记录。
另一方面，您可以在任何窗口中保存您的扩展程序设置，无论隐身与否。

.. note:: 准则:

  - 如果某些数据可能显示用户在网上的访问记录或者用户所做的事情，千万不要保存来自隐身窗口的这些数据。


要确定窗口是否处于隐身模式，检查相关的 :ref:`Tab <Tabs.Types>` 或 :ref:`Windows <Windows.Types>`对象的 `icognito` 属性。例如：


.. code-block:: js

    var bgPage = chrome.extension.getBackgroundPage();

    function saveTabData(tab, data) {
      if (tab.incognito) {
        bgPage[tab.url] = data;       // 只能留在内存的数据
      } else {
        localStorage[tab.url] = data; // 可以保存的数据
    }




现在? 
------------------------------------------------------------------------------

现在我们已大致了解了扩展程序，应该准备好编写自己的扩展了。

接下来可以参考以下内容:

- :ref:`入门教程 <chapter0hallo>`
- :ref:`调试教程 <chapter2debugging>`
- :ref:`开发者指南 <chapter1index>`
- :ref:`示例 <chapter4index>`






.. seealso:: (^.^)
    
    原文: `Getting Started <http://code.google.com/chrome/extensions/trunk/overview.html>`_
