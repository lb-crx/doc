.. include:: ../LINKS.rst

.. _sample4make_page_red:


browser action:无图标令页面背景变红
==============================================================================

运用 `background_page` ,`browser_action` and `tabs`

调用:
------------------------------------------------------------------------------

- :ref:`chrome.browserAction.onClicked <BrowserActions-onClicked>`
- :ref:`chrome.browserAction.setBadgeBackgroundColor <BrowserActions-setBadgeBackgroundColor>`
- :ref:`chrome.browserAction.setBadgeText <BrowserActions-setBadgeText>`
- :ref:`chrome.tabs.executeScript <Tabs-executeScript>`




源码:
------------------------------------------------------------------------------

- `manifest.json`


.. code-block:: js
  :emphasize-lines: 4,7

    {
      "name": "A browser action with no icon that makes the page red",
      "version": "1.0",
      "background": { "scripts": ["background.js"] },
      "permissions": [
        "tabs", "http://*/*"
      ],
      "browser_action": {
        "name": "Make this page red",
        "icons": ["icon.png"]
      },
      "manifest_version": 2
    }




- `background.js`

.. code-block:: js
  :emphasize-lines: 5,7,10,13

    // Copyright (c) 2011 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    // Called when the user clicks on the browser action.
    chrome.browserAction.onClicked.addListener(function(tab) {
      chrome.tabs.executeScript(
          null, {code:"document.body.style.background='red !important'"});
    });

    chrome.browserAction.setBadgeBackgroundColor({color:[0, 200, 0, 100]});

    var i = 0;
    window.setInterval(function() {
      chrome.browserAction.setBadgeText({text:String(i)});
      i++;
    }, 10);




资源下载
------------------------------------------------------------------------------

:download:`make_page_red.zip <../_static/examples/api/browserAction/make_page_red.zip>` 
