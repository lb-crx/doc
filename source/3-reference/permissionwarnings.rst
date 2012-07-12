
.. include:: ../LINKS.rst

.. _chapter3-permissionwarnings:




权限警告 Permission Warnings
==============================================================================

要想使用大部分 
:ref:`chrome.* API <chapter3-apindex>` 和
:ref:`扩展程序 <chapter1-Extension>` 功能，
扩展程序必须在
:ref:`清单文件 <chapter3-manifest>`
中声明它的意图，通常在 `"permission"` 属性中。

其中一些声明会使用户在安装扩展程序时看到一个警告对话框。

当浏览器尝试自动更新我们的扩展程序时，如果扩展程序请求新的权限，用户可能会看到另一个警告对话框。

这些新的权限可能是因为我们的扩展程序使用了新的API，也可能是因为扩展程序需要访问新的网站。

权限警告的例子 Examples of permission warnings
------------------------------------------------------------------------------

如下是用户在安装扩展程序时可能会看到的典型对话框:

.. figure:: ../_static/images/perms-hw1.png

.. code-block:: js
  :emphasize-lines: 2
  
    "permissions": [
      "http://api.flickr.com/"
    ],


**注意：** 当载入未打包的扩展程序时不会看到权限警告，只有当我们从.crx文件安装扩展程序时才会看到。


如果在自动更新时向扩展程序添加新的权限，用户可能看到新的权限警告。
例如，假设在前一个例子中添加了一个新的站点以及 `"tabs"` 权限：

.. code-block:: js
  :emphasize-lines: 3,4
  
    "permissions": [
      "http://api.flickr.com/", 
      "http://*.flickr.com/", 
      "tabs"
    ],


当扩展程序自动更新时，增加的权限将使扩展程序被禁用，直到用户重新启用。
如下是用户看到的警告：

.. figure:: ../_static/images/perms-hw2-disabled.png


单击重新启用按钮将显示如下警告

.. figure:: ../_static/images/perms-hw2.png


警告以及导致它们的原因 Warnings and their triggers
------------------------------------------------------------------------------


当添加某些权限，例如 `"tabs"` 后，
将显示看上去不相关的警告，比如,说扩展程序可以访问您的浏览器活动，这可能令人惊讶。

这一警告的原因是尽管
:ref:`chrome.tabs <chapter1-Tabs>`
 API可能只用来打开新标签页，它也能够用来查看每一个新打开的标签页相关联的URL
 （使用它们的 :ref:`Tab <chapter1-Tabs>` 对象）

.. note:: 注意

    - 从 `Google`_ `Chrome`_ 7开始，不再需要指定 `"tabs"` 权限
    - 直接调用 :ref:`chrome.tabs.create() <Tabs.create>` 或 :ref:`chrome.tabs.update() <Tabs.update>`


下表列出了用户可能看到的警告消息，以及导致这些警告的清单文件条目。



.. list-table:: 权限警告清单对照表
   :widths: 15 10 30
   :header-rows: 1

   * - 警告消息
     - 导致这一警告的清单文件条目
     - 备注
   * - 访问您计算机上的所有数据以及您在所有网站上的数据
     - "`plugins`"权限
     - :ref:`NPAPI插件 <chapter1-NPAPIPlugins>` 要求 "`plugins`" 权限
   * - 读取和修改您的书签
     - "`bookmarks`"权限
     - :ref:`chrome.bookmarks <chapter1-Bookmarks>` 要求 "`bookmarks`" 权限
   * - 读取和修改您的浏览历史记录
     - "`history`"权限
     - :ref:`chrome.history <chapter1-History>` 要求 "`history`" 权限

   * - 访问您的标签页和浏览活动
     - 如下的任何一个权限:

        - "`tabs`"
        - "`webNavigation`"

     - :ref:`chrome.tabs <chapter1-Tabs>` 和 :ref:`chrome.windows <chapter1-Windows>` 模块要求"`tabs`"权限。 :ref:`chrome.webNavigation模块 <api4webNavigation>` 要求"`webNavigation`"权限。
   * - 修改用于指定网站是否可以使用Cookie、JavaScript和插件等功能的设置
     - "`contentSettings`"权限
     - :ref:`chrome.contentSettings <api4contentSettings>`_ 要求"`contentSettings`"权限
   * - 访问您在所有网站上的数据
     - 如下的任何一个权限：

        - "`debugger`"权限
        - "`proxy`"权限
        - 在"`permissions`"字段中的匹配表达式，匹配所有主机
        - "`content_scripts`"字段中"`matches`"项匹配所有主机
        - "`devtools_page`"
     - 实验性的 :ref:`调试器模块 <api4debugger>` 要求"`debugger`"权限。 :ref:`chrome.proxy模块 <api4proxy>` 要求"`proxy`"权限。 如下任何一个URL匹配所有主机：

        - `http://*/*`
        - `https://*/*`
        - `*://*/*`
        - `<all_urls>`

   * - 访问您在{列出的网站}上的数据
     - 在"`permissions`"字段中的匹配表达式，指定了一个或多个，但不是所有主机
     - 最多会列出3个站点的名称。子域名不会特别对待。例如，`a.com` 和 `b.a.com` 将作为不同站点列出。在自动更新时，如果扩展程序添加或更改了站点，用户将会看到权限警告。例如，从 `a.com,b.com` 到 `a.com,b.com,c.com` 导致一个警告。从b.a.com到a.com或者反过来也都会导致警告。
   * - 访问您打开的页面内容
     - "`pageCapture`"权限
     - :ref:`chrome.pageCapture <api4pageCapture>` 模块要求"`pageCapture`"权限
   * - 管理您的应用程序、扩展程序和主题
     - "`management`"权限
     - :ref:`chrome.management <api4management>` 模块要求"`management`"权限
   * - 检测您的实际位置
     - "`geolocation`"权限
     - 允许扩展程序使用提出的HTML5 `地理位置API <http://dev.w3.org/geo/api/spec-source.html>`_ 而不用提示用户就允许访问
   * - 访问您复制和粘贴的数据
     - "`clipboardRead`"权限
     - 允许扩展程序通过 `document.execCommand()` 使用以下编辑命令：

        - "copy"
        - "cut"

   * - 修改隐私相关的设置
     - "`privacy`"权限
     - :ref:`chrome.privacy <api4privacy>` 模块要求"`privacy`"权限
   * - 访问所有使用语音合成朗读的文字
     - "`ttsEngine`"权限
     - :ref:`chrome.ttsEngine <api4ttsEngine>` 模块要求"`ttsEngine`"权限
   * - 警告消息
     - 导致这一警告的清单文件条目
     - 备注



不导致警告的权限
------------------------------------------------------------------------------


如下权限不会导致警告：

- "browsingData"
- "chrome://favicon/"
- "clipboardWrite"
- "contextMenus"
- "cookies"
- "experimental"
- "idle"
- "notifications"
- "storage"
- "unlimitedStorage"
- "webRequest"
- "webRequestBlocking"



测试权限警告 Testing permission warnings
------------------------------------------------------------------------------


- 如果想看到用户会得到怎样的警告，将扩展程序 :ref:`打包 <chapter1-Packaging>` 为.crx文件并安装。
- 如果想看到在扩展程序自动更新时用户会看到的警告，这要稍微麻烦一些，可以设置自动更新服务器。

    - 要这么做，首先创建一个更新清单文件，并在您的扩展程序中通过"`update_url`"属性（参见 :ref:`自动更新 <chapter1-Autoupdating>` ）指向它。
    - 接着，将扩展程序 :ref:`打包 <chapter1-Packaging>` 成新的 `.crx` 文件，并从这一 `.crx` 文件安装。
    - 现在，更改扩展程序的`清单文件`，以包含新的权限，并 :ref:`重新打包扩展程序 <chapter1-Packaging>` 。
    - 最后，单击 `chrome://extensions` 页面中的 `立即更新` 扩展程序按钮来更新扩展程序（以及所有其它有更新的扩展程序）。



API
------------------------------------------------------------------------------

您可以使用 :ref:`chrome.management.getPermissionWarnings() <api4management-getPermissionWarningsByManifest>` 获得任意清单文件的权限警告列表



.. seealso:: (^.^)
    
    原文: `Permission Warnings <http://code.google.com/chrome/extensions/trunk/permission_warnings.html>`_

