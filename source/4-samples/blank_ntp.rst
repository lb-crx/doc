.. include:: ../LINKS.rst

.. _sample4blank_ntp:


空白新标签页
==============================================================================

运用 `chrome_url_overrides`




源码:
------------------------------------------------------------------------------

- `manifest.json`


.. code-block:: js
  :emphasize-lines: 6

    {
      "name": "Blank new tab page",
      "version": "0.2",
      "incognito": "split",
      "chrome_url_overrides": {
        "newtab": "blank.html"
      },
      "manifest_version": 2
    }





- `blank.html`


.. code-block:: html
  :emphasize-lines: 15-16

    <html>
     <head>
      <title>Blank New Tab</title>
      <style>
      div {
        color: #cccccc;
        vertical-align: 50%;
        text-align: center;
        font-family: sans-serif;
        font-size: 300%;
      }
      </style>
     </head>
     <body>
      <div style="height:40%"></div>
      <div>Blank New Tab&trade;</div>
     </body>
    </html>




资源下载
------------------------------------------------------------------------------

:download:`blank_ntp.zip <../_static/examples/api/override/blank_ntp.zip>` 
