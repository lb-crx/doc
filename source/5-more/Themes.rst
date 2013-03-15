.. include:: ../LINKS.rst

.. _chapter5Themes:


皮肤样式
==============================================================================

.. |themes1| image:: ../_static/images/themes-1.gif
.. |themes2| image:: ../_static/images/themes-2.gif
.. |themes3| image:: ../_static/images/themes-3.gif

皮肤样式是一种特殊的扩展程序，可以改变浏览器的外观。
皮肤样式的打包与普通的扩展程序类似，只是不含 `JavaScript`_ 或 `HTML`_ 代码。

您可以在 `Chrome网上样式店`_ 中寻找与尝试各种主题背景:

=========== =========== ===========
 |themes1|   |themes2|   |themes3|
=========== =========== ===========



清单文件 Manifest
------------------------------------------------------------------------------
以下是用于主题背景的manifest.json的例子：

.. code-block:: js
  :emphasize-lines: 4-26

    {
      "version": "2.6",
      "name": "camo theme",
      "theme": {
        "images" : {
          "theme_frame" : "images/theme_frame_camo.png",
          "theme_frame_overlay" : "images/theme_frame_stripe.png",
          "theme_toolbar" : "images/theme_toolbar_camo.png",
          "theme_ntp_background" : "images/theme_ntp_background_norepeat.png",
          "theme_ntp_attribution" : "images/attribution.png"
        },
        "colors" : {
          "frame" : [71, 105, 91],
          "toolbar" : [207, 221, 192],
          "ntp_text" : [20, 40, 0],
          "ntp_link" : [36, 70, 0],
          "ntp_section" : [207, 221, 192],
          "button_background" : [255, 255, 255]
        },
        "tints" : {
          "buttons" : [0.33, 0.5, 0.47]
        },
        "properties" : {
          "ntp_background_alignment" : "bottom"
        }
      }
    }



颜色   
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
颜色以RGB形式表示。
可以在
`theme_service.cc <http://src.chromium.org/viewvc/chrome/trunk/src/chrome/browser/themes/theme_service.cc>`_
文件中 `kColor*` 字串部分找到可以在此使用的字符串。

:download:`theme_service.cc <_static/themes/theme_service.cc>`.




图像   
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

图像资源使用相对于扩展根目录的路径。
可替换 `theme_service.cc <http://src.chromium.org/viewvc/chrome/trunk/src/chrome/browser/themes/theme_service.cc>`_
文件中 `kThemeableImages` 指定的任何图片，只要将“IDR_”删除并将剩余字符转换为小写。
例如，`IDR_THEME_NTP_BACKGROUND`
（ `kThemeableImages` 用来指定新标签栏的背景）
对应"theme_ntp_background"。

主题样式扩展中,应用中可替换图片备查,参考:

- :ref:`样式创建导引 <chapter5TCG>`
- :ref:`样式规格手册 <chapter5TReference>`



属性
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

这一字段让我们可以指定诸如背景对齐、背景重复、替代标志等属性。

有关进一步的名称以及其值，请参见
`theme_service.cc <http://src.chromium.org/viewvc/chrome/trunk/src/chrome/browser/themes/theme_service.cc>`_


色调    tints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

我们也可以指定应用于用户界面某些部分的色调，例如按钮框架和后台标签页。
Google Chrome浏览器支持色调而不是图片，因为图片不一定能跨平台使用，并且在增加新按钮时不适用。
有关您可以在"tints"中使用的字符串，在 `theme_service.cc中` 寻找 `kTint*`字符串。


色调以色调-饱和度-亮度（HSL）的格式指定，使用0-1.0之间的浮点数：

- `色调` 为绝对值，0和1为红色。
- `饱和度` 相对于当前提供的图片。0.5表示 `没有变化`，0表示 `完全不饱和` ，1表示 `完全饱和` 。
- `亮度` 也是相对的，0.5表示没有变化，0表示 `所有像素` 均为黑色，1表示 `所有像素` 均为白色。
  
还可以对任意HSL值使用 `-1.0` 表示没有变化。


其它文档 Additional documentation 
------------------------------------------------------------------------------

由社区撰写的相关帮助文档在这里（英文）:

- `Theme Creation Guide <http://code.google.com/p/chromium/wiki/ThemeCreationGuide>`_
- `Chrome Theme Reference <https://docs.google.com/Doc?docid=0Aa86IE02TBXPZGtzZDU0NV85ZnFocnQzZGo>`_
    
翻译:

- :ref:`样式创建导引 <chapter5TCG>`
- :ref:`样式规格手册 <chapter5TReference>`


.. seealso:: (^.^)
    
    原文: `Themes <http://code.google.com/chrome/extensions/themes.html>`_
