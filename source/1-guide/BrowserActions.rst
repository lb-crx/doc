.. include:: ../LINKS.rst

.. _chapter1-BrowserActions:



浏览行为
=============================

.. sidebar:: Manifest

    `UI组件`_

        `图标`_

        `提示信息`_

        `徽章`_

        `弹窗`_

        `提示`_

    `样例`_

    Reference

        Types

            ColorArray
            ImageDataType

        Methods

            setTitle
            getTitle
            setIcon
            setPopup
            getPopup
            setBadgeText
            getBadgeText
            setBadgeBackgroundColor
            getBadgeBackgroundColor
            enable
            disable

    Events

        onClicked

    Sample Extensions



:功能描述:     使用"浏览行为"式的扩展, 可以把图标放到 Google Chrome 主工具栏, 地址框的右边. 除了这个图标外, 一个"浏览行为"式的扩展, 还可以有操作提示, 徽章和弹窗. 
:可用性:    Chrome 5后稳定. 

:Manifest:    "browser_action": {...}

下图里, 在地址栏右侧的彩色正方形, 就是一个"浏览器行为"式的扩展. 它会有一个小弹出窗在下面.

.. figure:: ../_static/images/guide/browser-action.png

如果你想创建一个并不总是可见的图标, 请用 :ref:`页面行为 <chapter1-PageActions>` 扩展, 而不是"浏览行为"式的扩展. 

.. _Manifest:

Manifest:
------------------------------------------------------------------------------

像下面那样在扩展manifest里注册你的"浏览行为"式的扩展::

    {
      "name": "My extension",
      ...
      "browser_action": {
        "default_icon": {                    // 可选
          "19": "images/icon19.png",         // 可选
          "38": "images/icon38.png"          // 可选
        },
        "default_title": "Google Mail",      // 可选; 在提示框显示
        "default_popup": "popup.html"        // 可选
      },
      ...
    }

如果你只是提供19px或38px其中一种尺寸的图标, 扩展系统会根据用户显示的像素密度来缩放. 那将会导致失真或看起来很模糊. 旧的默认图标注册格式还是支持的, 就像下面那样::

    {
      "name": "My extension",
      ...
      "browser_action": {
        ...
        "default_icon": "images/icon19.png"  // 可选
        // equivalent to "default_icon": { "19": "images/icon19.png" }
      },
      ...
    }

.. _UI组件:

UI组件
------------------------------------------------------------------------------

一个"浏览行为"式的扩展可包括一个图标, 操作提示, 徽章和弹出窗.

.. _图标:

图标
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

"浏览行为"式的扩展的图标最大可做成19 dips (跨设备独立像素)  (device-independent pixels) 的正方形. 大图标会缩放调整, 但是为了达到最佳效果, 还是应该使用19-dip的方形图标.

你可以使用两种方式来设置图标：静态图片或者HTML5 canvas元素. 使用静态图片相对简单一些. 但是用 `canvas元素`_ 的话, 可以创作更动态的界面, 比如动画.

静态图片可以是WebKit支持的任何图片格式, 包括BMP, GIF, ICO, JPEG, 或者PNG. 对于没打包的扩展, 图片必须是PNG格式.

可以使用 `Manifest`_ 里 **browser_action** 的 **default_icon** 节点, 或者调用 `setIcon`_ 的方法来设置图片.

在屏幕像素深度( `像素 / 跨设备独立像素的比率` )不等于1的时候, 为了能够正确显示图片, icon可以设定为不同尺寸的图像集. 实际用来显示的图像会从图片集中选取最接近像素大小为19个跨设备独立像素的图标. 现在, 图标集里可以包含像素为19和38的图片.

.. _提示信息:

提示信息
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

要设定提示信息，可以定义 `Manifest`_ 里 **browser_action** 的 **default_title** 字段，或者调用 `setTitle`_ 方法. 你可以指定区域性信息。详情可参考国际化章节.

.. _徽章:

徽章
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

＂浏览行为＂扩展可以显示徽章图案 — 一小段叠在图标上的文字. 使用徽章可以很容易的显示出＂浏览行为＂扩展当前的状态.

因为徽章的空间非常有限，所以它应该最多显示4个字符.

分别通过 `setBadgeText`_ 和 `setBadgeBackgroundColor`_ 两个方法就可以设置徽章的文字和颜色.

.. _弹窗:

弹窗
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

如果一个＂浏览行为＂扩展设置有弹窗，它会在用户点击图标的时候弹出. 弹窗可以包含任意 HTML 内容，而且它可以根据内容来自动调节大小尺寸.

要设定弹窗，必须先创建一个弹窗内容的 HTML 文件，然后在 `Manifest`_ 里 **browser_action** 的 **default_popup** 字段里设定，或者调用 `setPopup`_ 方法.

.. _提示:

提示
----------------------------------------------------------------------------

为了达到最佳的视觉效果，可参考一下规则:

- 只有你的扩展功能对大多数的页面都适用的时候，才使用＂浏览行为＂扩展.
- 如果你的扩展功能只对某些页面适用，请别使用＂浏览行为＂扩展，而是应该使用 :ref:`页面行为 <chapter1-PageActions>` 扩展.
- 要使用尽量可占满 19x19 像素区间的彩色大图标. ＂浏览行为＂扩展的图标应该比起＂页面行为＂扩展的图标要大和深色些.
- 别尝试模仿 Google Chrome 的单色菜单图标. 那样的图标和主题搭配的效果并不好。再说，扩展应该效果鲜明和出众些.
- alpha 透明度为你的图标加上软边框。因为当人们使用主题的时候, 你的图标能在不同的背景色下都好看些.
- 别不断地在图标上加动画效果。那会另用户觉得很烦.

.. _样例:

样例
----------------------------------------------------------------------------
你可以在 `examples/api/browserAction <http://src.chromium.org/viewvc/chrome/trunk/src/chrome/common/extensions/docs/examples/api/browserAction/>`_ 目录下找到一些使用＂浏览行为＂扩展的简单样例. 其他例子和源代码查看帮助请查看 `样例库`_ 

API 手册: chrome.browserAction
------------------------------------------------------------------------------

Types
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ColorArray

ImageDataType

Pixel data for an image. Must be an ImageData object (for example, from a canvas element).


方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _BrowserActions-setBadgeBackgroundColor:

setBadgeBackgroundColor
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. _BrowserActions-setBadgeText:

setBadgeText
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. _setIcon:

setIcon
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. _BrowserActions-setPopup:

setPopup
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. _BrowserActions-setTitle:

setTitle
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Events
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _BrowserActions-onClicked:

onClicked
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""



.. seealso:: (^.^)
    
    原文: `Browser Actions <http://code.google.com/chrome/extensions/browserAction.html>`_


