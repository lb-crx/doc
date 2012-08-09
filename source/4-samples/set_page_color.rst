.. include:: ../LINKS.rst

.. _sample4set_page_color:


browser action:使用弹出窗口改变页面颜色
==============================================================================

运用 `browser_action` and `tabs`

调用:
------------------------------------------------------------------------------

- :ref:`chrome.tabs.executeScript <Tabs-executeScript>`




源码:
------------------------------------------------------------------------------

- `manifest.json`


.. code-block:: js
  :emphasize-lines: 5,7-10

    {
      "name": "A browser action with a popup that changes the page color.",
      "version": "1.0",
      "permissions": [
        "tabs", "http://*/*", "https://*/*"
      ],
      "browser_action": {
          "default_title": "Set this page's color.",
          "default_icon": "icon.png",
          "default_popup": "popup.html"
      },
      "manifest_version": 2
    }


- `popup.html`


.. code-block:: html
  :emphasize-lines: 47

    <!doctype html>
    <html>
      <head>
        <title>Set Page Color Popup</title>
        <style>
        body {
          overflow: hidden;
          margin: 0px;
          padding: 0px;
          background: white;
        }

        div:first-child {
          margin-top: 0px;
        }

        div {
          cursor: pointer;
          text-align: center;
          padding: 1px 3px;
          font-family: sans-serif;
          font-size: 0.8em;
          width: 100px;
          margin-top: 1px;
          background: #cccccc;
        }
        div:hover {
          background: #aaaaaa;
        }
        #red {
          border: 1px solid red;
          color: red;
        }
        #blue {
          border: 1px solid blue;
          color: blue;
        }
        #green {
          border: 1px solid green;
          color: green;
        }
        #yellow {
          border: 1px solid yellow;
          color: yellow;
        }
        </style>
        <script src="popup.js"></script>
      </head>
      <body>
        <div id="red">red</div>
        <div id="blue">blue</div>
        <div id="green">green</div>
        <div id="yellow">yellow</div>
      </body>
    </html>


- `popup.js`

.. code-block:: js
  :emphasize-lines: 6,11-12

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    function click(e) {
      chrome.tabs.executeScript(null,
          {code:"document.body.style.backgroundColor='" + e.target.id + "'"});
      window.close();
    }

    document.addEventListener('DOMContentLoaded', function () {
      var divs = document.querySelectorAll('div');
      for (var i = 0; i < divs.length; i++) {
        divs[i].addEventListener('click', click);
      }
    });




资源下载
------------------------------------------------------------------------------

:download:`set_page_color.zip <../_static/examples/api/browserAction/set_page_color.zip>` 

