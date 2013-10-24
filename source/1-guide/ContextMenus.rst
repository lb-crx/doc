.. include:: ../LINKS.rst

.. _chapter1-ContextMenus:




右键菜单
=============================

.. sidebar:: Manifest

    `用法`_

    `样例`_

    Reference

        Types

            OnClickData

        Methods

            create

            update

            remove

            removeAll

        Events

            onClicked

        Sample Extensions



:功能描述:     使用 chrome.contextMenus API 可以添加选项到 Google Chrome 的右键菜单中. 你可以指定添加项的类别，比如图片，超链接，或者页面.
:可用性:    Chrome 5后稳定.
:权限:     "contextMenus"

.. _用法:

右键选项可以出现在任何页面里面（或者是页面里的 frame 里头），即使是 file:// 或者 chrome:// 这些开头的页面. 当你在调用 create() 或者 update() 方法的时候，指定 documentUrlPatterns 的值，就可以作相应的控制.

你可以创建任何数量的右键菜单，但是如果从你的扩展里同时展现出来的话, Google Chrome 会自动把它们合并到一个父菜单项上.


.. _Manifest:

Manifest:
------------------------------------------------------------------------------

你必须在你的扩展 manifest 里声明具有 "contextMenus" 的权限才可以使用这个 API. 同时，你还要制定一个 16x16 像素大小的图标用以显示在你的菜单选项旁. 比如::

    {
      "name": "My extension",
      ...
      "permissions": [
        "contextMenus"
      ],
      "icons": {
        "16": "icon-bitty.png",
        "48": "icon-small.png",
        "128": "icon-large.png"
      },
      ...
    }


.. _样例:

你可以在样例页面找到些 API 使用实例.

.. seealso:: (^.^)

    原文: `Context Menus <http://code.google.com/chrome/extensions/contextMenus.html>`_


