.. include:: ../LINKS.rst

.. _sample4animated_page_action:


页面动画行为
==============================================================================

运用 `background_page` ,`browser_action` and `tabs`

在工具栏中加入动画效果.

调用:
------------------------------------------------------------------------------

- :ref:`chrome.pageAction.hide <PageActions-hide>`
- :ref:`chrome.pageAction.onClicked <PageActions-onClicked>`
- :ref:`chrome.pageAction.setIcon <PageActions-setIcon>`
- :ref:`chrome.pageAction.setTitle <PageActions-setTitle>`
- :ref:`chrome.pageAction.show <PageActions-show>`
- :ref:`chrome.tabs.get <Tabs-get>`
- :ref:`chrome.tabs.getSelected <Tabs-getSelected>`
- :ref:`chrome.tabs.executeScript <Tabs-executeScript>`




源码:
------------------------------------------------------------------------------

- `manifest.json`


.. code-block:: js
  :emphasize-lines: 5

    {
      "name": "Animated Page Action",
      "description": "This extension adds an animated browser action to the toolbar.",
      "version": "1.1",
      "permissions": ["tabs"],
      "background": {
        "page": "background.html"
      },
      "page_action": {
        "default_title": "First icon"
      },
      "manifest_version": 2
    }




- `background.html`


.. code-block:: html
  :emphasize-lines: 7,12,18,23,25,39,49,51-62

    <html>
    <head>
    <script>
      var lastTabId = 0;
      var tab_clicks = {};

      chrome.tabs.onSelectionChanged.addListener(function(tabId) {
        lastTabId = tabId;
        chrome.pageAction.show(lastTabId);
      });

      chrome.tabs.getSelected(null, function(tab) {
        lastTabId = tab.id;
        chrome.pageAction.show(lastTabId);
      });

      // Called when the user clicks on the page action.
      chrome.pageAction.onClicked.addListener(function(tab) {
        var clicks = tab_clicks[tab.id] || 0;
        chrome.pageAction.setIcon({path: "icon" + (clicks + 1) + ".png",
                                   tabId: tab.id});
        if (clicks % 2) {
          chrome.pageAction.show(tab.id);
        } else {
          chrome.pageAction.hide(tab.id);
          setTimeout(function() { chrome.pageAction.show(tab.id); }, 200);
        }
        chrome.pageAction.setTitle({title: "click:" + clicks, tabId: tab.id});

        // We only have 2 icons, but cycle through 3 icons to test the
        // out-of-bounds index bug.
        clicks++;
        if (clicks > 3)
          clicks = 0;
        tab_clicks[tab.id] = clicks;
      });

      var i = 0;
      window.setInterval(function() {
        var clicks = tab_clicks[lastTabId] || 0;

        // Don't animate while in "click" mode.
        if (clicks > 0) return;

        // Don't do anything if we don't have a tab yet.
        if (lastTabId == 0) return;

        i++;
        chrome.pageAction.setIcon({imageData: draw(i*2, i*4), tabId: lastTabId});
      }, 50);

      function draw(starty, startx) {
        var canvas = document.getElementById('canvas');
        var context = canvas.getContext('2d');
        context.clearRect(0, 0, canvas.width, canvas.height);
        context.fillStyle = "rgba(0,200,0,255)";
        context.fillRect(startx % 19, starty % 19, 8, 8);
        context.fillStyle = "rgba(0,0,200,255)";
        context.fillRect((startx + 5) % 19, (starty + 5) % 19, 8, 8);
        context.fillStyle = "rgba(200,0,0,255)";
        context.fillRect((startx + 10) % 19, (starty + 10) % 19, 8, 8);
        return context.getImageData(0, 0, 19, 19);
      }
    </script>
    </head>
    <body>
    <canvas id="canvas" width="19" height="19"></canvas>
    </body>
    </html>




资源下载
------------------------------------------------------------------------------

:download:`set_icon.zip <../_static/examples/api/pageAction/set_icon.zip>` 
