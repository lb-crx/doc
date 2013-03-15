.. include:: ../LINKS.rst

.. _chapter5TCG:


主题样式创建手册 
==============================================================================


官方文档中指出样式创建中要上 `*.cc` 代码中翻找相关信息,对于纯粹的设计人员而言,太囧!
所以,需要一份清晰的手册,指引设计人员快速完成样式的设计.

所以,有了当前这份:ThemesCreationGuide ~ 主题样式创建手册!


创建样式必须作的事儿
---------------------------------------------------------------------


- 一个靠谱的文本编辑器是必须的(至少要有行号,语法样式等等支持,因为 Chrome 对非良构的 manifest.json是零容忍的, 对于M$用户Notepad++ 是个好选择,笔者推荐新兴跨平台编辑器 Sublime Text 2 )如果对 JSON 文本实在没有感觉,可以尝试在线样式制作工具,比如: http://www.themebeta.com/chrome-theme-creator-online.html
- 一个靠谱的图像管理器,好工具能帮助你创造好内容,强烈推荐 Photoshop , 同时推荐正版免费软件 Gimp 或是 Paint. 
- 如果你在使用 Photoshop, 就可以下载 `Chrome 窗口设计稿 <http://www.chromium.org/user-experience/visual-design/chrome_0.2_psd.zip>`_ 已经将不同元素整理到不同层,方便进行效果调整.
- 运用一些颜色/模式/设计的创意原则,来控制样式的整体感观
- 将你的成果打包,并通过以下渠道发布:
    + 直接上传到 `Chrome网上样式店`_
    + 使用 Chrome 打包,参考: :ref:`发布托管 <chapter1-Hosting>` 或是原文: `Hosting <https://developer.chrome.com/extensions/hosting.html>`_
    + 自行制作 `.crx` , 参考: :ref:`打包 <chapter1-Packaging>` 或是原文: `Packaging <https://developer.chrome.com/extensions/packaging.html>`_




相关工具的配合
---------------------------------------------------------------------


See the help image, you'll see many things highlighted, with a number denoting each element. We will be using the numbers to denote/specify the part of the UI for which we are creating themes.
.. _fig_5_t_1:
.. figure:: ../_static/themes/crx-reference-screenhp.jpg

    主题样式可指


样式元素详述
---------------------------------------------------------------------

Image Elements
^^^^^^^^^^^^^^^^^^^^^^^    

Color Elements
^^^^^^^^^^^^^^^^^^^^^^^    

Tint Elements
^^^^^^^^^^^^^^^^^^^^^^^    

UI Property Elements
^^^^^^^^^^^^^^^^^^^^^^^    



Description of Elements
--------------------------------------------------------------------------------------------

Basic Theme Elements
^^^^^^^^^^^^^^^^^^^^^^^    

Advanced Theme Elements
^^^^^^^^^^^^^^^^^^^^^^^    

Packaging
^^^^^^^^^^^^^^^^^^^^^^^    


.. list-table:: 主题应用中可替换图片备查表
   :widths: 15 30
   :header-rows: 1

   * - 名称
     - 描述
   * - theme_button_background
     - 
   * - theme_frame
     - 
   * - theme_frame_inactive
     - 
   * - theme_frame_incognito
     - 
   * - theme_frame_incognito_inactive
     - 
   * - theme_frame_overlay
     - 
   * - theme_frame_overlay_inactive
     - 
   * - theme_toolbar
     - 
   * - theme_tab_background
     - 
   * - theme_tab_background_incognito
     - 
   * - theme_tab_background_v
     - 
   * - theme_ntp_attribution
     - 
   * - theme_ntp_background
     - 
   * - theme_window_control_background
     - 




.. seealso:: (^.^)
    
    原文: `Theme Creation Guide <https://code.google.com/p/chromium/wiki/ThemeCreationGuide>`_
