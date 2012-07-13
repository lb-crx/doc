.. include:: ../LINKS.rst

.. _sample4set_icon_path:


A browser action which changes its icon when clicked.
==============================================================================

Uses `background_page`, `browser_action` and `tabs`

调用:
------------------------------------------------------------------------------

- :ref:`chrome.browserAction.onClicked <BrowserActions-onClicked>`
- :ref:`chrome.browserAction.setIcon <BrowserActions-setIcon>`

源码:
------------------------------------------------------------------------------

- background.js

.. code-block:: js
  :emphasize-lines: 2,10,17,18

    // Copyright (c) 2011 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    var min = 1;
    var max = 5;
    var current = min;

    function updateIcon() {
      chrome.browserAction.setIcon({path:"icon" + current + ".png"});
      current++;

      if (current > max)
        current = min;
    }

    chrome.browserAction.onClicked.addListener(updateIcon);
    updateIcon();


- manifest.json


.. code-block:: js
  :emphasize-lines: 2-4,6

    {
      "name": "A browser action which changes its icon when clicked.",
      "version": "1.1",
      "background": { "scripts": ["background.js"] },
      "permissions": [
        "tabs", "http://*/*"
      ],
      "browser_action": {
          "name": "Click to change the icon's color"
      },
      "manifest_version": 2
    }


资源下载
------------------------------------------------------------------------------

:download:`set_icon_path.zip <../_static/examples/api/browserAction/set_icon_path.zip>` 


