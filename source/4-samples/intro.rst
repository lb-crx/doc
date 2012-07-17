.. include:: ../LINKS.rst

本章收集各种简单的样例,并附可用代码,帮助开发者从一个正确的基础上完成自己的想法...

.. |sample-default| image:: ../_static/images/sample-default-icon.png



.. list-table:: 样例s
   :widths: 5 60
   :header-rows: 0

   * - |sample-default|
     - :ref:`browser action:点击时改变图标 <sample4set_icon_path>`

        调用:
            - :ref:`chrome.browserAction.onClicked <BrowserActions-onClicked>`
            - :ref:`chrome.browserAction.setIcon <BrowserActions-setIcon>`
        下载:
            - :download:`set_icon_path.zip <../_static/examples/api/browserAction/set_icon_path.zip>`
   * - |sample-default|
     - :ref:`browser action:使用弹出窗口改变页面颜色 <sample4set_page_color>`

        调用:
            - :ref:`chrome.tabs.executeScript <Tabs-executeScript>`
        下载:
            - :download:`set_page_color.zip <../_static/examples/api/browserAction/set_page_color.zip>` 

   * - |sample-default|
     - :ref:`browser action:无图标令页面背景变红 <sample4make_page_red>`

        调用:
            - :ref:`chrome.browserAction.onClicked <BrowserActions-onClicked>`
            - :ref:`chrome.browserAction.setBadgeBackgroundColor <BrowserActions-setBadgeBackgroundColor>`
            - :ref:`chrome.browserAction.setBadgeText <BrowserActions-setBadgeText>`
            - :ref:`chrome.tabs.executeScript <Tabs-executeScript>`
        下载:
            - :download:`make_page_red.zip <../_static/examples/api/browserAction/make_page_red.zip>` 






.. seealso:: (^.^)
    
    原文: `Samples <http://code.google.com/chrome/extensions/samples.html>`_

    
    