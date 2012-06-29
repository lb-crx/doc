.. include:: ../LINKS.rst

.. _chapter2debugging:

.. |hello-world-small| image:: ../_static/examplex/tutorials/getstarted/icon.png
   :alt: 你好世界图标

.. |playbutton| image:: ../_static/images/play-button.gif
   :alt: 播放按钮

.. |consolebutton| image:: ../_static/images/console-button.gif
   :alt: 控制按钮


调试 Debugging
===================

这一教程介绍如何使用Google Chrome浏览器内建的开发人员工具来交互性地调试扩展程序

查看扩展程序信息
------------------------------------------------------------------------------

要使用这一教程,需要 :ref:`入门 <chapter0hallo>` 部分的Hello World扩展程序. 

我们将会载入扩展程序,并在扩展程序页面中查看它的信息:

#. 如果还没运行的话载入Hello World扩展程序. 如果扩展程序已经运行,您将看到浏览器地址栏右边的Hello World图标. 

    如果Hello World扩展程序还没有运行,找到扩展程序文件并将它们载入. 
    如果您手边没有这些文件,从这一 See :download:`ZIP文件 <../_static/examples/tutorials/getstarted.zip>` 解压缩. 
    如果您需要有关载入扩展程序的指导请参见 :ref:`入门 <chapter0hallo>` . 

#. 进入扩展程序页面(chrome://extensions),并确保该页面处于开发人员模式
#. 在该页面中查看Hello World扩展程序的信息,您可以看到一些细节,例如扩展程序的名称,描述以及标识符



审查弹出内容
------------------------------------------------------------------------------

只要浏览器在开发者模式,就可以方便地审查弹出内容

#. 进入扩展程序页面(chrome://extensions),并确保开发人员模式处于启用状态. 进行以下工作时不需要扩展程序页面一直开着. 浏览器将记住设置,即使页面没有显示. 
#. 右击 |hello-world-small| 图标,并选择 `审查弹出内容` (Inspect popup)菜单项. 弹出内容将会出现,并且如下图所示的开发人员工具窗口将会显示 `popup.html` 的代码. 

    .. figure:: ../_static/images/devtools-1.gif

    只要开发者窗口工具开着,弹出内容也将一直保持打开状态. 

#. 如果 `Scripts` 按钮还未选中则单击它. 
#. 单击控制台按钮 |consolebutton| (在开发人员工具窗口的底部,从左边开始的第二个),这样您可以同时看到代码和控制台. 






使用调试器
------------------------------------------------------------------------------
在这一部分,弹出页面自己添加图片时您将跟踪它的执行. 

#. 搜索 `img.src` ,单击它出现的行号(例如,`37`),在添加图片的循环中设置断点. 

    .. figure:: ../_static/images/devtools-2.gif

#. 确保您可以看到 `popup.html` 的内容,它应该显示20个"hello world"图片. 
#. 在控制台提示符中,输入 `location.reload(true)` 重新载入弹出页面 ::

    > location.reload(true)

    弹出页面在重新载入时会变成空白,并且执行将在第37行停止. 

#. 在工具窗口的右上角,您应该看到本地变量. 这一部分显示当前作用域内所有变量的值. 例如,在如下屏幕截图中i的值为0,photos是包含一些元素的节点列表. (事实上,它包含20个元素,索引从0至19,每一个代表一幅图. )

    .. figure:: ../_static/images/devtools-localvars.gif

#. 单击开始/暂停 |playbutton| (Play/Pause)按钮(在开发人员工具窗口的右上角),执行一次图片处理循环. 您每一次单击该按钮,i的值就会递增,并且弹出页面中出现另一个图标. 例如,当i为10时,弹出页面如下图所示:

    .. figure:: ../_static/images/devtools-3.gif

#. 使用开始/暂停旁边的按钮来进行逐过程,逐语句调试或完成函数调用. 要让页面完成载入,单击第37行删除断点,并按下开始/暂停按钮继续执行. 






总结!
------------------------------------------------------------------------------

教程演示了可以用来调试扩展程序的一些技术:

- 在扩展程序页面(chrome://extensions)中找到扩展程序标识符以及指向其页面的链接. 
- 使用 `chrome-extension://extensionId/filename` 查看较难到达的页面(以及您的扩展程序中的其它任何文件). 
- 使用开发人员工具审查并单步调试页面的 `JavaScript`_ 代码. 
- 从控制台中使用 `location.reload(true)` 重新载入当前审查中的页面. 


然后?
------------------------------------------------------------------------------

现在我们已经熟悉了调试过程,如下是有关接下来做什么的建议:

- 观看扩展程序视频 `开发和调试 <https://www.youtube.com/watch?v=IP0nMv_NI1s&feature=PlayList&p=CA101D6A85FE9D4B&index=5>`_ (英文,墙外). 
- 尝试安装和阅读其它扩展程序,例如示例. 
- 尝试在您的扩展程序的 `JavaScript`_ 代码中使用常见的调试API: `console.log()` 和 `console.error()`. 例如::

    console.log("Hello, world!")

- 遵循 :ref:`开发人员工具教程 <devtool0index>` ,浏览 `开发人员工具站点 <http://www.chromium.org/devtools>`_ (英文),并观看一些视频教程. 

有关更多想法,请参见入门中的" :ref:`然后? <chapter0overview4now>` "部分. 



.. seealso:: (^.^)
    
    原文: `Tutorial: Google Analytics <http://code.google.com/chrome/extensions/tut_debugging.html>`_

