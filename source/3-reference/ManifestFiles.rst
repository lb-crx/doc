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

下面的例子显示了两个 `.nmf` 文件处理三个 `MIME`_ 类型的扩展。


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


.. list-table:: 扩展可指定权限
   :widths: 15 50
   :header-rows: 1

   * - 权限
     - 描述
   * - 匹配表达式
     - 指定主机权限。如果扩展程序需要与页面上运行的代码交互则必须指定该权限。许多扩展程序的能力，例如 :ref:`跨站XMLHttpRequest <chapter1-XHR>` 、以编程方式插入 :ref:`内容脚本 <chapter1-ContentScripts>` 以及 :ref:`Cookie API <api4cookies>` 需要主机权限。有关语法上的细节，请参见 :ref:`匹配表达式 <chapter3-MatchPatterns>`
   * - "background"
     - 让Chrome很早就启动很晚才退出，以便应用程序和扩展程序可以有更长的生命周期。 :
        当任何已安装的托管应用程序、打包应用程序或扩展程序拥有 `"background"` 权限时，Chrome浏览器在用户登录计算机时就（不可见地）运行，那时用户还没有亲自执行Chrome浏览器。 `"background"` 权限也使Chrome浏览器继续运行（即使在最后一个窗口已经关闭后），直到用户主动退出Chrome浏览器。
        
        **注意：** 已禁用的应用程序以及扩展程序以未安装对待。
        
        通常您与 :ref:`后台页面 <chapter1-BackgroundPages>` 或者（对于托管应用程序） `后台窗口 <https://code.google.com/chrome/apps/docs/background.html>`_ （英文）一起使用 `"background"` 权限。

   * - "bookmarks"
     - 如果扩展程序使用 :ref:`chrome.bookmarks <chapter1-BrowserActions>` 模块则必须指定该权限。
   * - "chrome://favicon/"
     - 如果扩展程序使用chrome://favicon/url机制来显示页面的收藏夹图标则必须指定该权限。

        例如，要显示http://www.google.com/的收藏夹图标，您声明"chrome://favicon/"权限，并使用如下所示的HTML代码：

        `<img src="chrome://favicon/http://www.google.com/">`


   * - "clipboardRead"
     - 如果扩展程序使用 `document.execCommand('paste')`则必须指定该权限
   * - "clipboardWrite"
     - 指定应用程序或扩展程序可以使用 `document.execCommand('copy')` 或 `document.execCommand('cut')` 。 `托管` 应用程序 **必须** 指定该权限，同时建议扩展程序和打包应用程序指定该权限。
   * - "contentSettings"
     - 如果扩展程序使用 :ref:`chrome.contentSettings <api4contentSettings>` 模块则必须指定该权限
   * - "contextMenus"
     - 如果扩展程序使用 :ref:`chrome.contextMenus <chapter1-ContextMenus>` 模块则必须指定该权限
   * - "cookies" 
     - 如果扩展程序使用 :ref:`chrome.cookies <api4cookies>` 模块则必须指定该权限
   * - "experimental" 
     - 如果扩展程序使用 :ref:`chrome.experimental.* APIs <api4experimental>` 模块则必须指定该权限
   * - "fileBrowserHandler"
     - 如果扩展程序使用 :ref:`chrome.fileBrowserHandler <api4fileBrowserHandler>` 模块则必须指定该权限
   * - "geolocation"
     - 允许扩展程序使用提出的HTML5 `地理位置API <http://dev.w3.org/geo/api/spec-source.html>`_ 而不用提示用户
   * - "history"
     - 如果扩展程序使用 :ref:`chrome.history <api4history>` 模块则必须指定该权限
   * - "idle"
     - 如果扩展程序使用 :ref:`chrome.idle <api4idle>` 模块则必须指定该权限
   * - "management"
     - 如果扩展程序使用 :ref:`chrome.management <api4management>` 模块则必须指定该权限
   * - "notifications"
     - 允许扩展程序使用提出的HTML5 `通知API <http://www.chromium.org/developers/design-documents/desktop-notifications/api-specification>`_ 而不需要调用方法请求权限（例如: `checkPermission()` ）。有关更多信息请参见 :ref:`桌面通知 <chapter1-DesktopNotifications>`
   * - "pageCapture"
     - 如果扩展程序使用 :ref:`chrome.pageCapture <api4pageCapture>` 模块则必须指定该权限
   * - "privacy"
     - 如果扩展程序使用 :ref:`chrome.privacy <api4privacy>` 模块则必须指定该权限
   * - "proxy"
     - 如果扩展程序使用 :ref:`chrome.proxy <api4proxy>` 模块则必须指定该权限
   * - "tabs"
     - 如果扩展程序使用 :ref:`chrome.tabs <chapter1-Tabs>` 或 :ref:`chrome.windows <chapter1-Windows>` 模块则必须指定该权限
   * - "tts"
     - 如果扩展程序使用 :ref:`chrome.tts <api4tts>` 模块则必须指定该权限
   * - "ttsEngine"
     - 如果扩展程序使用 :ref:`chrome.tts <api4ttsEngine>` 模块则必须指定该权限
   * - "unlimitedStorage"
     - 提供无限的存储空间，保存HTML5客户端数据，例如数据库以及本地存储文件。如果没有这一权限，扩展程序的本地存储将限制在5MB以内。

        **注意：** 该权限仅应用于Web SQL数据库以及应用程序缓存（参见问题 `58985 <http://crbug.com/58985>`_ ）。另外，当前还不能使用含有通配符的子域名，例如 `http://*.example.com`
   * - "webNavigation"
     - 如果扩展程序使用 :ref:`chrome.webNavigation <api4webNavigation>` 模块则必须指定该权限
   * - "webRequest"
     - 如果扩展程序使用 :ref:`chrome.webRequest <api4webRequest>` 模块则必须指定该权限
   * - "webRequestBlocking"
     - 如果扩展程序使用 :ref:`chrome.webRequest <api4webRequest>` 模块则必须指定该权限



.. _manifest-requirements:

requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

应用程序或扩展程序要求的技术。托管站点（例如 `Chrome网上应用店`_ ）可能会使用这一列表建议用户不要安装不能在他们的计算机上工作的应用程序或者扩展程序。

目前唯一支持的要求为"3D"，代表GPU硬件加速。对于这一要求，可以如下面的例子所示，列出应用程序要求的3D相关的功能：

::

    "requirements": {
      "3D": {
        "features": ["css3d", "webgl"]
      }
    }


`"css3d"` 要求指的是 `CSS 3D变换规范 <https://sites.google.com/>`_（英文）， 
`"webgl"` 要求指的是 `WebGL API <https://sites.google.com/>`_（英文）。
有关Chrome浏览器对3D图形支持的更多信息，请参见有关
`WebGL和3D图形 <https://support.google.com/chrome/bin/answer.py?hl=zh-Hans&answer=1220892>`_
的帮助文章。
对于更多不同要求的检查可能会在将来增加。


.. _manifest-version:

version
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

一至四个用点（半角英文句号）分开的整数，标识扩展程序的版本。这些整数应该符合这两条规则：范围在0-65535之间（包括0和65535），非零整数不能以0开头。例如99999和032都是无效的。
如下是一些有效的版本号的例子：

- `"version": "1"`
- `"version": "1.0"`
- `"version": "2.10.2"`
- `"version": "3.1.2.4567"`

自动更新系统比较版本号来确定已安装的某个扩展程序是否需要更新。如果已发布的扩展程序比已安装的扩展程序有更新的版本字符串，该扩展程序将会自动更新。
比较将从最左边的整数开始。如果相等，比较右边的整数，以此类推。例如，1.2.0比1.1.9.9999新。
省略整数的等于零，例如1.1.9.9999比1.1新。
有关更多信息，请参见 :ref:`自动更新 <chapter1-Autoupdating>`


.. _manifest-mversion:

manifest_version
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

指定扩展程序包要求的清单文件格式的版本。
从Chrome 18开始，开发人员应该指定2（不加引号），使用本文档描述的格式： ::

    "manifest_version": 2

从Chrome 18开始，认为清单文件版本1已弃用，版本2目前还不是必需的，
但在不远的将来某一时刻，将不再支持使用已弃用清单文件版本的扩展程序包。

还没有准备转换至Chrome 18中新的清单文件版本的扩展程序、应用程序以及主题背景既可以明确地指定版本1，也可以完全省略该属性。

清单文件格式版本1与版本2之间的改变在 :ref:`manifest_version文档 <chapter3-manifestVersion>` 中详细描述。


.. warning:: (#_#)

    - 不建议在Chrome 17或更低版本中设置 `manifest_version 2`
    - 如果扩展程序需要在较旧版本的Chrome浏览器中运行，目前请继续使用版本1，在版本1停止工作前将会给出充分的警告。



.. _manifest-accessible:

web_accessible_resources
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

字符串数组，指定扩展程序包内可以在网页中使用的资源路径（相对于扩展程序包的根目录）。
例如，为了在example.com上建立自定义界面，而插入内容脚本的扩展程序可以将界面需要的所有资源
（图片、图标、样式表、脚本等等）加入白名单，如下所示：

::

    {
      ...
      "web_accessible_resources": [
        "images/my-awesome-image1.png",
        "images/my-amazing-icon1.png",
        "style/double-rainbow.css",
        "script/double-rainbow.js"
      ],
      ...
    }

这些资源将可以通过 `URL`_ `chrome-extension://[扩展程序包的标识符]/[路径]` 使用，
`URL`_ 可以通过 :ref:`chrome.extension.getURL <Extension-getURL>` 方法产生。
加入白名单的资源将以合适的 `CORS <http://www.w3.org/TR/cors/>`_ 头信息提供，
所以它们可以通过XHR之类的机制使用。

插入的内容脚本本身不需要加入白名单。


默认情况:
  - 使用 `manifest_version 2` 或更高的扩展程序包内的资源 **默认情况下被阻止** ，必须通过这一属性加入白名单。
  - 使用 `manifest_version 1` 的扩展程序包内的资源默认情况下可用，但是只要您设置了这一属性，它将以所有加入白名单资源的完整列表来对待，没有列出的资源将被阻止



sandbox
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`新:` `r140689 <https://src.chromium.org/viewvc/chrome?view=rev&sortby=rev&sortdir=down&revision=140689>`_



要在受沙箱保护的唯一来源中载入的页面路径（相对于扩展程序包根目录）列表，还可以指定可选的内容安全策略来一起使用。
在沙箱中意味着如下两点：

#. 受沙箱保护的页面不能访问扩展程序或应用程序API，也不能直接访问沙箱外的页面（但是可以通过postMessage()进行通信）。
#. 受沙箱保护的页面不受应用程序或扩展程序其余部分使用的 :ref:`内容安全策略（CSP） <chapter3-CSP>` 所限制。这就意味着，它可以使用内嵌脚本与 `eval`

例如，如下代码指定两个扩展程序页面将在沙箱中载入，并指定了自定义的
:ref:`CSP <chapter3-CSP>`  ： ::

    {
      ...
      "sandbox": {
        "pages": [
          "page1.html",
          "directory/page2.html"
        ]
        // 内容安全策略是可选的。
        "content_security_policy":
            "sandbox allow-scripts; script-src https://www.google.com"
      ],
      ...
    }


如果没有指定的话，默认的 `content_security_policy` 值为
`sandbox allow-scripts allow-forms` 。
可以指定自己的CSP值来更进一步限制沙箱，但是它必须包含sandbox指示符，
并且 **不能** 包含 `allow-same-origin` 记号
（有关可能的沙箱记号，请参考: `相应的HTML5规范 <http://www.whatwg.org/specs/web-apps/current-work/multipage/the-iframe-element.html#attr-iframe-sandbox>`_ ）。


.. note:: 注意

    - 只需要列出我们期望在窗口或框架中载入的页面。
    - 受沙箱保护的页面所使用的资源（例如样式表或 `JavaScript`_ 源文件）不需要出现在 `sandbox` 列表中，它们会使用内嵌它们的页面所使用的沙箱。
    - 只有使用 `manifest_version 2` 或更高版本时才能指定受沙箱保护的页面。




.. seealso:: 子页面
    
    - :ref:`内容安全策略（CSP） <chapter3-CSP>`
    - :ref:`清单文件版本 <chapter3-manifestVersion>`


.. seealso:: (^.^)
    
    原文: `Formats: Manifest File <http://code.google.com/chrome/extensions/manifest.html>`_


