.. include:: ../LINKS.rst

.. _sample4cld:


CLD
==============================================================================

运用 `background_page` , `browser_action` 以及 `tabs`

展示每个标签的语言


调用:
------------------------------------------------------------------------------

- :ref:`chrome.browserAction.setBadgeText <BrowserActions-setBadgeText>`
- :ref:`chrome.tabs.detectLanguage <Tabs-detectLanguage>`
- :ref:`chrome.tabs.get <Tabs-get>`
- :ref:`chrome.tabs.getSelected <Tabs-getSelected>`
- :ref:`chrome.tabs.onSelectionChanged <Tabs-onSelectionChanged>`
- :ref:`chrome.tabs.onUpdated <Tabs-onUpdated>`


源码:
------------------------------------------------------------------------------


- `manifest.json`


.. code-block:: js
  :emphasize-lines: 6

    {
      "name": "CLD",
      "description": "Displays the language of a tab",
      "version": "0.2",
      "background": {
        "scripts": ["background.js"]
      },
      "permissions": [
        "tabs"
      ],
      "browser_action": {
          "default_name": "Page Language"
      },
      "manifest_version": 2
    }



- `background.js`


.. code-block:: js
  :emphasize-lines: 7,11,15,20,25

    // Copyright (c) 2009 The Chromium Authors. All rights reserved. Use of this
    // source code is governed by a BSD-style license that can be found in the
    // LICENSE file.
    var selectedId = -1;
    function refreshLanguage() {
      chrome.tabs.detectLanguage(null, function(language) {
        console.log(language);
        if (language == " invalid_language_code")
          language = "???";
        chrome.browserAction.setBadgeText({"text": language, tabId: selectedId});
      });
    }

    chrome.tabs.onUpdated.addListener(function(tabId, props) {
      if (props.status == "complete" && tabId == selectedId)
        refreshLanguage();
    });

    chrome.tabs.onSelectionChanged.addListener(function(tabId, props) {
      selectedId = tabId;
      refreshLanguage();
    });

    chrome.tabs.getSelected(null, function(tab) {
      selectedId = tab.id;
      refreshLanguage();
    });





资源下载
------------------------------------------------------------------------------

:download:`cld.zip <../_static/examples/api/i18n/cld.zip>` 


