.. include:: ../LINKS.rst

.. _chapter1BrowserActions:

.. _canvas元素: http://www.whatwg.org/specs/web-apps/current-work/multipage/the-canvas-element.html

浏览行为
=============================

.. sidebar:: Manifest
    Parts of the UI
        Icon
        Tooltip
        Badge
        Popup
        Tips
    Examples
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

:功能描述:     使用"浏览行为"式的扩展, 可以把图标放到Google Chrome主工具栏, 地址框的右边. 除了这个图标外, 一个"浏览行为"式的扩展, 还可以有操作提示,  a badge和弹窗. 
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

UI组件
------------------------------------------------------------------------------

一个"浏览行为"式的扩展可包括一个图标, 操作提示, 徽章和弹出窗.

.. _BrowserActions-Icon:

    图标
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

"浏览行为"式的扩展的图标最大可做成19 dips (跨设备独立像素)  (device-independent pixels) 的正方形. 大图标会缩放调整, 但是为了达到最佳效果, 还是应该使用19-dip的方形图标.

你可以使用两种方式来设置图标：静态图片或者HTML5 canvas元素. 使用静态图片相对简单一些. 但是用 `canvas元素`_ 的话, 可以创作更动态的界面, 比如动画.

静态图片可以是WebKit支持的任何图片格式, 包括BMP, GIF, ICO, JPEG, 或者PNG. 对于没打包的扩展, 图片必须是PNG格式.

可以使用 `Manifest`_ 里 **browser_action** 的 **default_icon** 节点, 或者调用 `setIcon`_ 的方法来设置图片.

在屏幕像素深度( `像素 / 跨设备独立像素的比率` )不等于1的时候, 为了能够正确显示图片, icon可以设定为不同尺寸的图像集. 实际用来显示的图像会从图片集中选取最接近像素大小为19个跨设备独立像素的图标. 现在, 图标集里可以包含像素为19和38的图片.

.. _BrowserActions-Tooltip:

    Tooltip
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _BrowserActions-Badge:

    Badge
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _BrowserActions-Popup:

    Popup
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Tips
------------------------------------------------------------------------------

Examples
------------------------------------------------------------------------------

API reference: chrome.browserAction
------------------------------------------------------------------------------

    Methods
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


