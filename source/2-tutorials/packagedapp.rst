.. include:: ../LINKS.rst

.. _chapter2packagedapp:



打包应用程序 Packaged App
============================================

本文讨论打包应用程序——如何实现它们以及它们与扩展程序和普通网上应用的区别

概述
------------------------------------------------------------------------------

`打包的应用程序` 是指包装在一个 `.crx` 文件中,并可使用Chrome浏览器扩展程序功能的网上应用。
我们建立 `打包的应用程序` 就像建立扩展程序一样，唯一的区别是打包的应用程序不能包含
:ref:`浏览器按钮 <chapter1-BrowserActions>` 或 :ref:`页面按钮 <chapter1-PageActions>` 。
作为替代, `打包的应用程序` 必须在它的.crx文件中包含至少一个HTML文件，来提供用户界面。

打包应用程序是一种 :ref:`可安装的应用程序 <iwebapps0index>` ——可以在Chrome浏览器中安装的网络应用，
:ref:`可安装的应用程序 <iwebapps0index>` 的另一种类型是 :ref:`托管应用程序 <iwebapps1hosted>` ，即一个普通的应用程序加上一点额外的元数据

如果您正在为Chrome网上应用店开发网上应用，如果以下任一条件为真，您可能会使用打包的应用程序，而不是托管的应用程序：

#. 您不想运行服务托管您的应用程序。
#. 您想建立能够在离线状态下很好地运行的应用程序。
#. 您想更好地与Chrome整合，使用扩展程序API。


要想了解网上应用与网站、扩展程序与打包应用程序、打包应用程序与托管应用程序之间的区别，请参见：

- `选择应用程序类型（英文） <https://code.google.com/chrome/webstore/docs/choosing.html>`_
- `Web应用编程思想 <https://code.google.com/intl/zh-CN/chrome/apps/articles/thinking_in_web_apps.html>`_
- `Chrome Web Store 中的扩展程序、打包应用程序和托管应用程序（英文） <https://code.google.com/intl/zh-CN/chrome/webstore/articles/apps_vs_extensions.html>`_



清单文件
------------------------------------------------------------------------------
打包应用程序的清单文件可以包含可用于扩展程序的任何属性，除了"browser_action"和"page_action"。此外，打包应用程序的清单文件 `必须` 包含"app"属性。如下是打包应用程序清单文件的典型例子

.. code-block:: js

    {
      "name": "My Awesome Racing Game",
      "description": "Enter a world where a Vanagon can beat a Maserati",
      "version": "1",
      "app": {
        "launch": {
          "local_path": "main.html"
        }
      },
      "icons": {
        "16": "icon_16.png",
        "128": "icon_128.png"
      }
    }



"app"域有一个子域"launch"，指定应用程序的启动页面——当用户单击新标签页面中应用程序图标时浏览器进入的页面（打包在.crx文件中的HTML文件）。"launch"域包含如下内容：

local_path:
    `必需` , 以 `.crx` 包中的相对路径指定启动页面。
container:
    容器类型。"panel"（面板）使应用程序出现在应用面板中。默认情况下，或者您指定"tab"（标签页）时，应用程序出现在标签页中。
height:
    如果容器类型设置为"panel"，这一整数指定面板的高度，以像素为单位。例如，您可能指定 `"height":400` 。注意，高度值不需要加引号。该属性指定显示内容的区域的高度，窗口样式会增加一些像素至总高度。如果容器类型不是面板，将忽略这一属性。
width:
    与"height"类似，但是指定面板的宽度。

打包应用程序通常提供一个16×16大小的图标用于包含应用程序页面的收藏夹图标，
另外还应该提供一个128×128大小的图标，而不是48×48大小的图标。

- 有关更多信息请参见清单文件文档中的 :ref:`icons属性 <chapter3-Manifest-icons>`  。
- 有关打包应用程序的清单文件可以包含的内容的进一步细节，请参见 :ref:`清单文件文档 <chapter3-Manifest>`  。




接下来?
------------------------------------------------------------------------------

查阅: :ref:`概述 <chapter0overview>`  了解有关扩展程序的基本概念.

