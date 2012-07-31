.. include:: ../LINKS.rst

.. _sample4chrome_query:

`Chrome`_ 元素查询
==============================================================================

运用 `devtools_page`

扩展开发工具,追加侧边栏显示选定的DOM元素的jQuery数据



源码:
------------------------------------------------------------------------------

- `manifest.json`


.. code-block:: js
  :emphasize-lines: 5,6

    {
      "name": "Chrome Query",
      "version": "1.1",
      "description": "Extends the Developer Tools, adding a sidebar that displays the jQuery data associated with the selected DOM element.",
      "devtools_page": "devtools.html",
      "manifest_version": 2
    }



- `devtools.html`

.. code-block:: html
  :emphasize-lines: 4

    <html>
    <body>
    <script src="devtools.js"></script>
    </body>
    </html>

- `devtools.js`



.. code-block:: js
  :emphasize-lines: 7,10,17,24

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    // The function below is executed in the context of the inspected page.
    var page_getProperties = function() {
      var data = window.jQuery && $0 ? jQuery.data($0) : {};
      // Make a shallow copy with a null prototype, so that sidebar does not
      // expose prototype.
      var props = Object.getOwnPropertyNames(data);
      var copy = { __proto__: null };
      for (var i = 0; i < props.length; ++i)
        copy[props[i]] = data[props[i]];
      return copy;
    }

    chrome.devtools.panels.elements.createSidebarPane(
        "jQuery Properties",
        function(sidebar) {
      function updateElementProperties() {
        sidebar.setExpression("(" + page_getProperties.toString() + ")()");
      }
      updateElementProperties();
      chrome.devtools.panels.elements.onSelectionChanged.addListener(
          updateElementProperties);
    });


资源下载
------------------------------------------------------------------------------

:download:`chrome-query.zip <../_static/examples/api/devtools/panels/chrome-query.zip>` 

