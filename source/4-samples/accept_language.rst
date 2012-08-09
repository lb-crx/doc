.. include:: ../LINKS.rst

.. _sample4accept_language:


可接受语言 AcceptLanguage
==============================================================================

运用 `browser_action`

调用:
------------------------------------------------------------------------------

- :ref:`chrome.i18n.getAcceptLanguages <i18n-getAcceptLanguages>`
- :ref:`chrome.i18n.getMessage <i18n-getMessage>`


源码:
------------------------------------------------------------------------------

- `_locales/en_US/messages.json`

.. code-block:: js
  :emphasize-lines: 21-23

    {
      "chrome_extension_name": {
        "message": "AcceptLanguage"
      },
      "chrome_extension_description": {
        "message": "Returns accept languages of the browser"
      },
      "click_here": {
        "message": "Left click to list acceptLanguages."
      },
      "browser_action_title": {
        "message": "Click Me"
      },
      "chrome_accept_languages": {
        "message": "$CHROME$ accepts $languages$ languages",
        "placeholders": {
          "chrome": {
            "content": "Chrome",
            "example": "Chrome"
          },
          "languages": {
            "content": "$1",
            "example": "en-US,sr,de"
          }
        }
      }
    }


- `_locales/es/messages.json` (`西班牙`)

.. code-block:: js
  :emphasize-lines: 21-23

    {
      "chrome_extension_name": {
        "message": "AcceptLanguage"
      },
      "chrome_extension_description": {
        "message": "Devuelve los idiomas aceptados por el navegador"
      },
      "click_here": {
        "message": "Click con botón izquierdo para mostrar la lista de acceptLanguages."
      },
      "browser_action_title": {
        "message": "Haz click aquí"
      },
      "chrome_accept_languages": {
        "message": "$CHROME$ acepta los idiomas $languages$",
        "placeholders": {
          "chrome": {
            "content": "Chrome",
            "example": "Chrome"
          },
          "languages": {
            "content": "$1",
            "example": "en-US,sr,de"
          }
        }
      }
    }


- `_locales/sr/messages.json` (`塞尔维亚`)

.. code-block:: js
  :emphasize-lines: 18-20

    {
      "chrome_extension_name": {
        "message": "Прихватљиви језици"
      },
      "chrome_extension_description": {
        "message": "Језици које прегледач прихвата"
      },
      "click_here": {
        "message": "Кликните да излистате дозвољене језике."
      },
      "chrome_accept_languages": {
        "message": "$CHROME$ прихвата $languages$ језике.",
        "placeholders": {
          "chrome": {
            "content": "Chrome",
            "example": "Chrome"
          },
          "languages": {
            "content": "$1",
            "example": "en-US,sr,de"
          }
        }
      }
    }



- `manifest.json`


.. code-block:: js
  :emphasize-lines: 5

    {
      "name": "__MSG_chrome_extension_name__",
      "description": "__MSG_chrome_extension_description__",
      "version": "0.2",
      "default_locale": "en_US",
      "browser_action": {
          "default_title": "__MSG_browser_action_title__",
          "default_icon": "icon.png",
          "default_popup": "popup.html"
      },
      "manifest_version": 2
    }


- `popup.html`

.. code-block:: html
  :emphasize-lines: 18-19

    <!--
    Copyright (c) 2012 The Chromium Authors. All rights reserved. Use of this
    source code is governed by a BSD-style license that can be found in the
    LICENSE file.
    -->

    <html>
      <head>
        <style>
          body {
            color: black;
            width: 300px;
          }
        </style>
        <script src="popup.js"></script>
      </head>
      <body>
        <div id="accept_lang">
          <span id="languageSpan"></span>
        </div>
      </body>
    </html>




- `popup.js`

.. code-block:: js
  :emphasize-lines: 10,14-17

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    function setChildTextNode(elementId, text) {
      document.getElementById(elementId).innerText = text;
    }

    function init() {
      setChildTextNode('languageSpan', chrome.i18n.getMessage("click_here"));
    }

    function getAcceptLanguages() {
      chrome.i18n.getAcceptLanguages(function(languageList) {
        var languages = languageList.join(",");
        setChildTextNode('languageSpan',
            chrome.i18n.getMessage("chrome_accept_languages", languages));
      })
    }

    document.addEventListener('DOMContentLoaded', function() {
      document.querySelector('#accept_lang').addEventListener(
          'click', getAcceptLanguages);
    });



资源下载
------------------------------------------------------------------------------

:download:`getMessage.zip <../_static/examples/api/i18n/getMessage.zip>` 


