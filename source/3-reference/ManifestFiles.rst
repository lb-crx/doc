.. include:: ../LINKS.rst

.. _chapter3-manifest:



清单文件格式
======================

所有扩展(包括:可安装的WebApp，皮肤)，
都有一个 `JSON`_ 格式的清单(manifest)文件，叫 `manifest.json` 包含所有重要的信息


字段说明 Field summary
------------------------------------------------------------------------------

下面的脚本演示了所有支持的字段，每个字段都有下述对应的说明。

必须的字段只有: :ref:`name <manifest-name>` 和 :ref:`version <manifest-version>` 。

.. _manifest-summary:


.. compound::

    {
      // 必须 Required

      ":ref:`name <manifest-name>`": "My Extension",

      ":ref:`version <manifest-version>`": "versionString",

      ":ref:`manifest_version <manifest-mversion>`": 2,


      // 建议 Recommended

      ":ref:`description <manifest-description>`": "A plain text description",

      ":ref:`icons <manifest-icons>`: { ... },

      ":ref:`default_locale <manifest-default_locale>`": "en",


      // 选一(或不用) Pick one (or none)

      ":ref:`browser_action <chapter1-BrowserActions>`": {...},

      ":ref:`page_action <chapter1-PageActions>`": {...},

      ":ref:`theme <_chapter5Themes>`": {...},

      ":ref:`app <manifest-app>`": {...},


      // 根据需要 Add any of these that you need

      ":ref:`background <chapter1-BackgroundPages>`": {...},

      ":ref:`chrome_url_overrides <chapter1-OverridePages>`": {...},

      ":ref:`content_scripts <chapter1-ContentScripts>`": [...],

      ":ref:`content_security_policy <chapter3-CSP>`": "policyString",

      ":ref:`file_browser_handlers <api4fileBrowserHandler>`": [...],

      ":ref:`homepage_url <manifest-homepage_url>`": "http://path/to/homepage",

      ":ref:`incognito <manifest-incognito>`": "spanning" or "split",

      ":ref:`key <manifest-key>`": "publicKey",

      ":ref:`minimum_chrome_version <manifest-minimum_chrome_version>`": "versionString",

      ":ref:`nacl_modules <manifest-nacl_modules>`": [...],

      ":ref:`offline_enabled <manifest-offline_enabled>`": true,

      ":ref:`omnibox <chapter1-Omnibox>`": { "keyword": "aString" },

      ":ref:`options_page <chapter1-OptionsPages>`": "aFile.html",

      ":ref:`permissions <manifest-permissions>`": [...],

      ":ref:`plugins <chapter1-NPAPIPlugins>`": [...],

      ":ref:`requirements <manifest-requirements>`": {...},

      ":ref:`update_url <chapter1-Autoupdating>`": "http://path/to/updateInfo.xml",

      ":ref:`web_accessible_resources <manifest-accessible>`": [...]

    }




字段详情 Field details
------------------------------------------------------------------------------

逐一解说各个字段，如果需要完整的字段列表，可以看这个 :ref:`链接 :Field summary <manifest-summary>` 
有整体的描述


.. _manifest-app:

app
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

可安装的webapp，包括打包过的app，需要这个字段来指定app需要使用的url。
最重要的是app的启动页面------当用户在点击app的图标后，浏览器将导航到的地方。

更详细的信息，请参考文档 :ref:`hosted apps <chapter4HostedApps>` 和 :ref:`packaged apps <chapter2packagedapp>` 


.. _manifest-default_locale:

default_locale
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

指定这个扩展保的缺省字符串的子目录： `_lcoales` 。

- 如果扩展有 `_locales` 目录，这个字段是必须的。
- 如果没有 `_locales` 目录，这个字段是必须不存在的。

具体见: :ref:`国际化 <chapter1-i18n>`


.. _manifest-description:

description
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

描述扩展的一段文字（不能是html或者其他格式，不能超过132个字符）。
这个描述必须对扩展的管理界面和 `Chrome网上应用店`_ 都合适。

可以指定本地相关的字符串，具体见: :ref:`国际化 <chapter1-i18n>`


.. _manifest-homepage_url:

homepage_url
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

扩展的主页 `URL`_ .
扩展的管理界面里面将有一个链接指向这个 `URL`_ 。
如果将扩展放在自己的网站上，这个 `URL`_ 就很有用了。
如果你通过了 `Extensions Gallery <https://chrome.google.com/extensions>`_ 和
`Chrome网上应用店`_  来分发扩展，缺省主页就是扩展所属的页面。


.. _manifest-icons:

icons
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

扩展，web app，和皮肤 需要至少一个图标来表示。
- 通常我们可以提供一个128x128的图标，用以在 `Chrome网上应用店`_展示
- 扩展需要一个48x48的图标，管理页面需要这个图标。
- 同时，还可以提供给一个16x16的图标作为扩页面的 `Favicon`_ ~ 网页图标 
- 这个16x16的图标，还将显示在实验性的 :ref:`扩展infobar <api4infobars>` 特性上。

图标要求是png格式，因为这是对透明支持最好的格式。
也可以用其他 `WebKit`_ 支持的格式，如BMP,GIF,ICON和JPEG。下面有个例子：

::

    "icons": { "16": "icon16.png",
               "48": "icon48.png",
              "128": "icon128.png" },


.. note:: 注意:请只使用文档说明的图标大小

    - 可能已注意到了，`Chrome`_ 有时候会将这些图标尺寸变小，比如，安装对话框将128像素图标缩小为69像素了。
    - 然而， `Chrome`_ 的界面细节可能每个版本都不一样，但每次变动都假设开发者使用的是文档标注过的尺寸。
    - 如果你使用了其他的尺寸，图标就可能在将来的某个版本中看起来很丑


如果使用 `Chrome开发者信息中心`_  上传的扩展、app、皮肤，
需要上传附带的图片，包括至少一张扩展的缩略图。

更多信息请参考 ： :ref:`Chrome 应用店开发手册 <chapter5webstore>` 

.. _manifest-incognito:

incognito
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

可选值："spanning"和"split"，指定当扩展在允许隐身模式下运行时如何响应。

扩展的缺省值是 "spanning" ，这意味着扩展将在一个共享的进程里面运行。
隐身标签页的事件和消息都会发送到这个共享进程，来源通过incognito标志来区分。

可安装的webapp的缺省值是 "split"，意思是隐身模式下的webapp都将运行在他们自己的隐身进程中。
如果app或扩展有背景页面，也将运行在隐身进程中。
隐身进程和普通进程一样，只是cookie保存在内存中而已。
每个进程只可看到和自己相关的事件和消息（比如，隐身进程只能看到隐身标签也更新）。
`这些进程之间不能互相通信` 。

根据经验:

- 如果扩展或app需要在隐身浏览器里面开一个标签页，使用 `split`
- 如果你的扩展或app需要登记录到远程服务器或者本地永久配置，用 `spanning`


.. _manifest-intents:


intents
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

指定这一扩展程序或应用程序提供的所有 `Intent` 处理程序的词典，词典中的每一个键指定这一扩展程序处理的动作谓语。
以下例子为动作谓语"http://webintents.org/share"指定两个处理程序。

.. code-block:: js

    {
      "name": "测试",
      "version": "1",
      "intents": {
        "http://webintents.org/share": [
          { 
            "type": ["text/uri-list"],
            "href": "/services/sharelink.html",
            "title" : "Sample Link Sharing Intent",
            "disposition" : "inline"
          },
          {
            "type": ["image/*"],
            "href": "/services/shareimage.html",
            "title" : "Sample Image Sharing Intent",
            "disposition" : "window"
          }
        ]
      }  
    }

`"type"` 的值为这一处理程序支持的 `MIME`_ 类型数组，
`"href"` 指示处理该 `Intent` 的页面 `URL_ 。
对于托管应用程序，这些 `URL`_ 必须包含在允许的 `URL`_ 中。
对于扩展程序，所有 `URL`_ 必须在扩展程序中，并且认为相对于扩展程序根 `URL`_ 。

当用户执行这一处理程序的操作时 `"title"` 将在 `Intent` 选择器用户界面中显示。
`"disposition" 可以为 `"inline"` 或 `"window"` 。
使用 `"window"` 方式的 `Intent` 调用时将打开新标签页，而使用 `"inline"` 方式调用时会显示在 `Intent` 选择器中。

更多信息，请参考 `Web Intents规范 <http://dvcs.w3.org/hg/web-intents/raw-file/tip/spec/Overview.html>`  与 `webintents.org <http://www.webintents.org/>`


通过Intent处理内容类型:
    - Web Intents可以注册为内容类型的查看器，如果要这么做，动作谓语必须为"http://webintents.org/view"，内容类型必须为白名单中的 `MIME`_ 类型。
    - 加入白名单中的MIME类型:

        - application/rss+xml
        - application/atom+xml



.. _manifest-key:

key
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
在开发阶段，这个值可以作为扩展、app和皮肤的唯一标示来使用。


.. note:: 注意

    - 其实并不需要使用这个值。
    - 相反，应该使用 :ref:`relative paths<relative-urls>` 和 :ref:`chrome.extension.getURL() <Extension-getURL>`  来编写不依赖这一值的代码

要获得合适的键值，首先从 `.crx` 文件安装扩展程序（可能需要上传您的扩展程序或手工打包）。
然后，在配置文件目录中，查看如下文件：
`Default/Extensions/<extensionId>/<versionString>/manifest.json` 
将会看到填在此处的键值。


.. _manifest-minimum_chrome_version:

minimum_chrome_version
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

如果我们的扩展，app或皮肤依赖指定版本的chrome话
,在此指定。
这个字符串的格式和 :ref:`version <manifest-version>` 字段一样。


.. _manifest-name:

name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

用来标示扩展的简短文本。
这个字段将用在安装对话框，扩展管理界面，和 `Chrome网上应用店`里 。

可以为这个字段指定一个本地相关的字符串，
具体参考： :ref:`国际化 <chapter1-i18n>`

.. _manifest-nacl_modules:

nacl_modules
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

设定本地客户端模块处理各种MIME类型的映射.

例如，下面代码片段中高亮部分, 为电子表格注册MIME类型,并指定本机OpenOffice的程序客户端作为内容处理模块


.. code-block:: js
    :emphasize-lines: 5-8
  
    {
      "name": "Native Client OpenOffice Spreadsheet Viewer",
      "version": "0.1",
      "description": "Open OpenOffice spreadsheets, right in your browser.",
      "nacl_modules": [{
        "path": "OpenOfficeViewer.nmf",
        "mime_type": "application/vnd.oasis.opendocument.spreadsheet"
      }]
    }


"path" 的值是原生客户端清单文件(Native Client manifest, `.nmf` 为后綴的文件file) 在扩展中的路径.

更多 `.nmf` 的信息, 参考: `Native Client Technical Overview <http://code.google.com/chrome/nativeclient/docs/technical_overview.html>`_


每一 `MIME`_ 类型可以与一个 `.nmf`文件结合.
不过,一个 `.nmf` 文件可处理多个 `MIME`_ 类型.

下面的例子显示了两个 `.nmf`文件处理三个 `MIME`_ 类型的扩展。


.. code-block:: js

    {
      "name": "Spreadsheet Viewer",
      "version": "0.1",
      "description": "Open OpenOffice and Excel spreadsheets, right in your browser.",
      "nacl_modules": [{
        "path": "OpenOfficeViewer.nmf",
        "mime_type": "application/vnd.oasis.opendocument.spreadsheet"
      },
      {
        "path": "OpenOfficeViewer.nmf",
        "mime_type": "application/vnd.oasis.opendocument.spreadsheet-template"
      },
      {
        "path": "ExcelViewer.nmf",
        "mime_type": "application/excel"
      }]
    }


.. note:: 注意

    - 可以不指定“nacl_modules” 而使用Native Client的扩展模块
    - 当且仅当,需要浏览器使用本地客户端显示特定类型内容时,才应使用“nacl_modules”


.. _manifest-offline_enabled:

offline_enabled
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

应用程序或者扩展程序能否在离线状态下工作。
当 `Chrome`_ 浏览器检测到处于离线状态时，
这一属性设为 `true` 的应用程序会在新标签页面中高亮显示



.. _manifest-permissions:

permissions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

包含该扩展程序或应用程序需要使用的权限的数组。
每一个权限既可以是已知字符串列表中的某一个（例如 `"geolocation"`），
或者授予访问一个或多个主机权限的匹配表达式。

权限可以帮助我们在扩展程序或应用程序受到攻击时尽可能减小损失，
某些权限也会在安装前向用户显示，这些将在 :ref:`权限警告 <chapter3-permissionwarnings>` 中详细描述。

如果某个扩展程序API需要清单文件中声明某个权限，它的文档会告诉我们如何去做。
例如， :ref:`标签页 <chapter1-Tabs>` 页面会演示如何声明 `"tabs"` 权限。


.. note:: 注意

    - 从 `Chrome`_ 16开始，一些权限可以是可选的，有关细节请参见 :ref:`可选权限 <chapter1-OptionalPermissions>` 

如下是一个扩展程序清单文件的权限部分的例子：

::

    "permissions": [
      "tabs",
      "bookmarks",
      "http://www.blogger.com/",
      "http://*.google.com/",
      "unlimitedStorage"
    ],


下表列出了扩展程序或打包应用程序可以使用的权限。

.. note:: 注意

    托管应用程序可以使用 `"background"` 、 `"clipboardRead"` 、
    `"clipboardWrite"` 、 `"geolocation"` 、 `"notification"` 
    和 `"unlimitedStorage"` 权限，但不能使用下表列出的其它任何权限。



.. _manifest-requirements:

requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _manifest-version:

version
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _manifest-mversion:

manifest_version
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _manifest-accessible:

web_accessible_resources
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^





.. seealso:: (^.^)
    
    原文: `Formats: Manifest File <http://code.google.com/chrome/extensions/manifest.html>`_


