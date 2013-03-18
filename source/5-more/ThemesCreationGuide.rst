.. include:: ../LINKS.rst

.. _chapter5TCG:


主题主题皮肤创建手册 
==============================================================================


官方文档中指出主题皮肤创建中要上 `*.cc` 代码中翻找相关信息,对于纯粹的设计人员而言,太囧!
所以,需要一份清晰的手册,指引设计人员快速完成主题皮肤的设计.

所以,有了当前这份:ThemesCreationGuide ~ 主题主题皮肤创建手册!


创建主题皮肤必须作的事儿
---------------------------------------------------------------------


- 一个靠谱的文本编辑器是必须的(至少要有行号,语法主题皮肤等等支持,因为 Chrome 对非良构的 manifest.json是零容忍的, 对于M$用户Notepad++ 是个好选择,笔者推荐新兴跨平台编辑器 Sublime Text 2 )如果对 JSON 文本实在没有感觉,可以尝试在线主题皮肤制作工具,比如: http://www.themebeta.com/chrome-theme-creator-online.html
- 一个靠谱的图像管理器,好工具能帮助你创造好内容,强烈推荐 Photoshop , 同时推荐正版免费软件 Gimp 或是 Paint. 
- 如果你在使用 Photoshop, 就可以下载 `Chrome 窗口设计稿 <http://www.chromium.org/user-experience/visual-design/chrome_0.2_psd.zip>`_ 已经将不同元素整理到不同层,方便进行效果调整.
- 运用一些颜色/模式/设计的创意原则,来控制主题皮肤的整体感观
- 将你的成果打包,并通过以下渠道发布:
    + 直接上传到 `Chrome网上主题皮肤店`_
    + 使用 Chrome 打包,参考: :ref:`发布托管 <chapter1-Hosting>` 或是原文: `Hosting <https://developer.chrome.com/extensions/hosting.html>`_
    + 自行制作 `.crx` , 参考: :ref:`打包 <chapter1-Packaging>` 或是原文: `Packaging <https://developer.chrome.com/extensions/packaging.html>`_




相关工具的配合
---------------------------------------------------------------------



.. _fig_5_t_1:
.. figure:: ../_static/themes/crx-reference-screenhp.jpg

    主题皮肤可定制元素索引

参考以上图片,标出了所有可以进行定制的主题皮肤元素,
我们将使用以上数字逐一说明怎样定制对应UI!

- 首先,创立以主题皮肤名为目录名的空白目录作工程容器.
- 然后,创建两样东西:
  + 将创建的图片(png格式)收集在 `images` 目录
  + 创建最重要的 :ref:`manifest.json <chapter3-manifest>` 清单文件, 这是主题皮肤扩展要求必须有的文件!
    - 使用普通的文本编辑器即可进行编辑
    - 注意,文件名必须全小写
- 最后,就可以进行打包测试了.

在 Chrome 中有很多东西可以主题皮肤化,详细的参考下文 "主题皮肤元素描述" 一节.



主题皮肤可定制元素
---------------------------------------------------------------------

图片 Image Elements
^^^^^^^^^^^^^^^^^^^^^^^    

图片元素在 :ref:`manifest.json <chapter3-manifest>` 中 `images` 一节定义

.. list-table:: 图片元素
   :widths: 5 25 10 10
   :header-rows: 1

   * - 标号
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 1
     - 顶部,Chrome 标签背景区域
     - :ref:`"theme_frame" <TCG-theme_frame>`
     - ∞ x 80
   * - 1.1
     - 区域同上,仅在不活跌时生效
     - :ref:`"theme_frame_inactive" <TCG-theme_frame_inactive>`
     - ~
   * - 1.2
     - 区域同上,仅在"匿名模式"下窗口激活时生效
     - :ref:`"theme_frame_incognito" <TCG-theme_frame_incognito>`
     - ~
   * - 1.3
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 2
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 3
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 3.1
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 4
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 5
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 6
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 6.1
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 7
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 8
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)
   * - 9
     - 说明
     - manifest.json 参数
     - 建议尺寸(宽x高)




Color Elements
^^^^^^^^^^^^^^^^^^^^^^^    

.. list-table:: 颜色元素
   :widths: 5 25 10
   :header-rows: 1

   * - 标号
     - 说明
     - manifest.json 参数
   * - 标号
     - 说明
     - manifest.json 参数


Tint Elements
^^^^^^^^^^^^^^^^^^^^^^^    
.. list-table:: 色调元素
   :widths: 5 25 10
   :header-rows: 1

   * - 标号
     - 说明
     - manifest.json 参数
   * - 标号
     - 说明
     - manifest.json 参数


UI Property Elements
^^^^^^^^^^^^^^^^^^^^^^^    

.. list-table:: 界面属性
   :widths: 5 25 10
   :header-rows: 1

   * - 标号
     - 说明
     - manifest.json 参数
   * - 标号
     - 说明
     - manifest.json 参数



主题皮肤元素描述 Description of Elements
--------------------------------------------------------------------------------------------

Basic Theme Elements
^^^^^^^^^^^^^^^^^^^^^^^    



.. _TCG-theme_frame:

- theme_frame:
  - desc


.. _TCG-theme_toolbar:

- theme_toolbar:
  - desc


.. _TCG-theme_tab_background:

- theme_tab_background:
  - desc



.. _TCG-theme_ntp_background:

- theme_ntp_background:
  - desc


Advanced Theme Elements
^^^^^^^^^^^^^^^^^^^^^^^    




.. _TCG-theme_frame_inactive:

- theme_frame_inactive:
  - desc



.. _TCG-theme_frame_incognito:

- theme_frame_incognito:
  - desc



.. _TCG-theme_frame_incognito_inactive:

- theme_frame_incognito_inactive:
  - desc



.. _TCG-theme_tab_background_incognito:

- theme_tab_background_incognito:
  - desc



.. _TCG-theme_tab_background_v:

- theme_tab_background_v:
  - desc



.. _TCG-theme_frame_overlay_inactive:

- theme_frame_overlay_inactive:
  - desc



.. _TCG-theme_button_background:

- theme_button_background:
  - desc



.. _TCG-theme_ntp_attribution:

- theme_ntp_attribution:
  - desc



.. _TCG-theme_window_control_background:

- theme_window_control_background:
  - desc



.. _TCG-Frame:

- Frame:
  - desc



.. _TCG-Frame_inactive:

- Frame_inactive:
  - desc



.. _TCG-Frame_incognito:

- Frame_incognito:
  - desc



.. _TCG-Frame_incognito_inactive:

- Frame_incognito_inactive:
  - desc



.. _TCG-toolbar:

- toolbar:
  - desc



.. _TCG-tab_text:

- tab_text:
  - desc



.. _TCG-tab_background_text:

- tab_background_text:
  - desc



.. _TCG-bookmark_text:

- bookmark_text:
  - desc



.. _TCG-ntp_background:

- ntp_background:
  - desc



.. _TCG-ntp_text:

- ntp_text:
  - desc



.. _TCG-ntp_link:

- ntp_link:
  - desc



.. _TCG-ntp_link_underline:

- ntp_link_underline:
  - desc



.. _TCG-ntp_header:

- ntp_header:
  - desc



.. _TCG-ntp_section:

- ntp_section:
  - desc



.. _TCG-ntp_section_text:

- ntp_section_text:
  - desc



.. _TCG-ntp_section_link:

- ntp_section_link:
  - desc



.. _TCG-ntp_section_link_underline:

- ntp_section_link_underline:
  - desc




.. _TCG-control_background:

- control_background:
  - desc



.. _TCG-button_background:

- button_background:
  - desc



.. _TCG-buttons:

- buttons:
  - desc



.. _TCG-frame:

- frame:
  - desc



.. _TCG-frame_inactive:

- frame_inactive:
  - desc



.. _TCG-frame_incognito:

- frame_incognito:
  - desc



.. _TCG-frame_incognito_inactive:

- frame_incognito_inactive:
  - desc


.. _TCG-background_tab:

- background_tab:
  - desc


.. _TCG-ntp_background_alignment:

- ntp_background_alignment:
  - desc


.. _TCG-ntp_background_repeat:

- ntp_background_repeat:
  - desc


.. _TCG-ntp_logo_alternate:

- ntp_logo_alternate:
  - desc




Packaging
^^^^^^^^^^^^^^^^^^^^^^^    




.. seealso:: (^.^)
    
    原文: `Theme Creation Guide <https://code.google.com/p/chromium/wiki/ThemeCreationGuide>`_
