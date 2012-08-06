.. include:: ../LINKS.rst

本章收集各种简单的样例,并附可用代码,帮助开发者从一个正确的基础上完成自己的想法...

.. |sample-default| image:: ../_static/images/sample-default-icon.png
.. |fx-icon| image:: ../_static/examples/extensions/fx/icon.png
   :height: 64px




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

   * - |sample-default|
     - :ref:`可接受语言 <sample4accept_language>`

        调用:
            - :ref:`chrome.i18n.getAcceptLanguages <i18n-getAcceptLanguages>`
            - :ref:`chrome.i18n.getMessage <i18n-getMessage>`
        下载:
            - :download:`getMessage.zip <../_static/examples/api/i18n/getMessage.zip>` 

   * - |sample-default|
     - :ref:`页面动画行为 <sample4animated_page_action>`

        调用:
            - :ref:`chrome.pageAction.hide <PageActions-hide>`
            - :ref:`chrome.pageAction.onClicked <PageActions-onClicked>`
            - :ref:`chrome.pageAction.setIcon <PageActions-setIcon>`
            - :ref:`chrome.pageAction.setTitle <PageActions-setTitle>`
            - :ref:`chrome.pageAction.show <PageActions-show>`
            - :ref:`chrome.tabs.get <Tabs-get>`
            - :ref:`chrome.tabs.getSelected <Tabs-getSelected>`
            - :ref:`chrome.tabs.executeScript <Tabs-executeScript>`
        下载:
            - :download:`set_icon.zip <../_static/examples/api/pageAction/set_icon.zip>` 

   * - |sample-default|
     - :ref:`应用加载 <sample4app_launcher>`

        调用:
            - :ref:`chrome.extension.getURL <Extension-getURL>`
            - :ref:`chrome.management.get <management-get>`
            - :ref:`chrome.management.getAll <management-getAll>`
            - :ref:`chrome.management.launchApp <management-launchApp>`
            - :ref:`chrome.tabs.create <Tabs-create>`
        下载:
            - :download:`app_launcher.zip <../_static/examples/extensions/app_launcher.zip>` 

   * - |sample-default|
     - :ref:`空白新标签页 <sample4blank_ntp>`

        下载:
            - :download:`blank_ntp.zip <../_static/examples/api/override/blank_ntp.zip>`

   * - |sample-default|
     - :ref:`拒绝/允许偏好API访问 <sample4enableReferrer>`

        调用:
            - :ref:`chrome.extension.isAllowedIncognitoAccess <Extension-isAllowedFileSchemeAccess>`
        下载:
            - :download:`enableReferrer.zip <../_static/examples/api/preferences/enableReferrer.zip>` 

   * - |sample-default|
     - :ref:`拒绝/允许第三方Cookies <sample4allowThirdPartyCookies>`

        调用:
            - :ref:`chrome.extension.isAllowedIncognitoAccess <Extension-isAllowedIncognitoAccess>`
        下载:
            - :download:`allowThirdPartyCookies.zip <../_static/examples/api/preferences/allowThirdPartyCookies.zip>` 

   * - |sample-default|
     - :ref:`断裂的链接 <sample4broken_links>`

        调用:
            - :ref:`chrome.experimental.devtools.audits.addCategory <devtools_audits-addCategory>`
            - :ref:`chrome.extension.onRequest <Extension-onRequest>`
            - :ref:`chrome.extension.sendRequest <Extension-sendRequest>`
            - :ref:`chrome.tabs.executeScript <Tabs-executeScript>`
            - :ref:`chrome.tabs.sendRequest <Tabs-sendRequest>`
        下载:
            - :download:`broken-links.zip <../_static/examples/api/devtools/audits/broken-links.zip>` 


   * - |sample-default|
     - :ref:`屏蔽喵! <sample4catblock>`

        调用:
            - :ref:`chrome.webRequest.onBeforeRequest <webRequest-onBeforeSendHeaders>`
        下载:
            - :download:`catblock.zip <../_static/examples/extensions/catblock.zip>`  

   * - |sample-default|
     - :ref:`Chrome 元素查询 <sample4chrome_query>`

        下载:
            - :download:`chrome-query.zip <../_static/examples/api/devtools/panels/chrome-query.zip>` 


   * - |fx-icon|
     - :ref:`Chrome 音效 <sample4fx>`

        调用:
            - :ref:`chrome.bookmarks.onCreated <Bookmarks-onCreated>`
            - :ref:`chrome.bookmarks.onMoved <Bookmarks-onMoved>`
            - :ref:`chrome.bookmarks.onRemoved <Bookmarks-onRemoved>`
            - :ref:`chrome.extension.getBackgroundPage <Extension-getBackgroundPage>`
            - :ref:`chrome.extension.onRequest <Extension-onRequest>`
            - :ref:`chrome.extension.sendRequest <Extension-sendRequest>`
            - :ref:`chrome.tabs.get <Tabs-get>`
            - :ref:`chrome.tabs.onAttached <Tabs-onAttached>`
            - :ref:`chrome.tabs.onCreated <Tabs-onCreated>`
            - :ref:`chrome.tabs.onDetached <Tabs-onDetached>`
            - :ref:`chrome.tabs.onMoved <Tabs-onMoved>`
            - :ref:`chrome.tabs.onRemoved <Tabs-onRemoved>`
            - :ref:`chrome.tabs.onSelectionChanged <Tabs-onSelectionChanged>`
            - :ref:`chrome.tabs.onUpdated <Tabs-onUpdated>`
            - :ref:`chrome.windows.onCreated <Windows-onCreated>`
            - :ref:`chrome.windows.onFocusChanged <Windows-onFocusChanged>`
            - :ref:`chrome.windows.onRemoved <Windows-onRemoved>`
        下载:
            - :download:`fx.zip <../_static/examples/extensions/fx.zip>` 




.. seealso:: (^.^)
    
    原文: `Samples <http://code.google.com/chrome/extensions/samples.html>`_

    
    