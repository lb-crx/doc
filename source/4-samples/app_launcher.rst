.. include:: ../LINKS.rst

.. _sample4app_launcher:


应用加载
==============================================================================

运用 `browser_action` and `management `

调用:
------------------------------------------------------------------------------

- :ref:`chrome.extension.getURL <Extension-getURL>`
- :ref:`chrome.management.get <management-get>`
- :ref:`chrome.management.getAll <management-getAll>`
- :ref:`chrome.management.launchApp <management-launchApp>`
- :ref:`chrome.tabs.create <Tabs-create>`




源码:
------------------------------------------------------------------------------

- `manifest.json`


.. code-block:: js
  :emphasize-lines: 4

    {
      "name": "App Launcher",
      "version": "0.7.3",
      "permissions": ["management"],
      "browser_action": {
        "default_icon": "browser_action_icon.png",
        "default_title": "App Launcher",
        "default_popup": "popup.html"
      },
      "icons": {
        "48": "icon.png"
      },
      "manifest_version": 2
    }




- `pop.css`

.. code-block:: css

    // Copyright (c) 2010 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    html {
      overflow-x: hidden;
    }

    body {
      font-family: Helvetica, Arial, sans-serif;
    }

    #search_container {
      text-align: center;
    }

    #search {
      width: 85%;
    }

    .app {
      width: 100%;
      margin-top: 7px;
      margin-bottom: 7px;
      margin-left: 2px;
      margin-right: 20px;
      white-space: nowrap;
    }

    .app img {
      vertical-align: middle;
      height: 38px;
      width: 38px;
    }

    .app_title {
      vertical-align: middle;
      margin-left: 5px;
    }

    .app:hover {
      background-color: rgb(250,250,250);
      cursor:pointer;
      outline: 1px dotted rgb(100,100,200);
    }

    .app_selected,.app_selected:hover {
      background-color: rgb(230,230,230);
    }

    #appstore_link {
      display: none;
    }


- `pop.js`

.. code-block:: js
  :emphasize-lines: 12,25

    // Copyright (c) 2011 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    function $(id) {
      return document.getElementById(id);
    }

    // Returns the largest size icon, or a default icon, for the given |app|.
    function getIconURL(app) {
      if (!app.icons || app.icons.length == 0) {
        return chrome.extension.getURL('icon.png');
      }
      var largest = {size:0};
      for (var i = 0; i < app.icons.length; i++) {
        var icon = app.icons[i];
        if (icon.size > largest.size) {
          largest = icon;
        }
      }
      return largest.url;
    }

    function launchApp(id) {
      chrome.management.launchApp(id);
      window.close(); // Only needed on OSX because of crbug.com/63594
    }

    // Adds DOM nodes for |app| into |appsDiv|.
    function addApp(appsDiv, app, selected) {
      var div = document.createElement('div');
      div.className = 'app' + (selected ? ' app_selected' : '');

      div.onclick = function() {
        launchApp(app.id);
      };

      var img = document.createElement('img');
      img.src = getIconURL(app);
      div.appendChild(img);

      var title = document.createElement('span');
      title.className = 'app_title';
      title.innerText = app.name;
      div.appendChild(title);

      appsDiv.appendChild(div);
    }

    // The list of all apps & extensions.
    var completeList = [];

    // A filtered list of apps we actually want to show.
    var appList = [];

    // The index of an app in |appList| that should be highlighted.
    var selectedIndex = 0;

    function reloadAppDisplay() {
      var appsDiv = $('apps');

      // Empty the current content.
      appsDiv.innerHTML = '';

      for (var i = 0; i < appList.length; i++) {
        var item = appList[i];
        addApp(appsDiv, item, i == selectedIndex);
      }
    }

    // Puts only enabled apps from completeList into appList.
    function rebuildAppList(filter) {
      selectedIndex = 0;
      appList = [];
      for (var i = 0; i < completeList.length; i++){
        var item = completeList[i];
        // Skip extensions and disabled apps.
        if (!item.isApp || !item.enabled) {
          continue;
        }
        if (filter && item.name.toLowerCase().search(filter) < 0) {
          continue;
        }
        appList.push(item);
      }
    }

    // In order to keep the popup bubble from shrinking as your search narrows the
    // list of apps shown, we set an explicit width on the outermost div.
    var didSetExplicitWidth = false;

    function adjustWidthIfNeeded(filter) {
      if (filter.length > 0 && !didSetExplicitWidth) {
        // Set an explicit width, correcting for any scroll bar present.
        var outer = $('outer');
        var correction = window.innerWidth - document.documentElement.clientWidth;
        var width = outer.offsetWidth;
        $('spacer_dummy').style.width = width + correction + 'px';
        didSetExplicitWidth = true;
      }
    }

    // Shows the list of apps based on the search box contents.
    function onSearchInput() {
      var filter = $('search').value;
      adjustWidthIfNeeded(filter);
      rebuildAppList(filter);
      reloadAppDisplay();
    }

    function compare(a, b) {
      return (a > b) ? 1 : (a == b ? 0 : -1);
    }

    function compareByName(app1, app2) {
      return compare(app1.name.toLowerCase(), app2.name.toLowerCase());
    }

    // Changes the selected app in the list.
    function changeSelection(newIndex) {
      if (newIndex >= 0 && newIndex <= appList.length - 1) {
        selectedIndex = newIndex;
        reloadAppDisplay();

        var selected = document.getElementsByClassName('app_selected')[0];
        var rect = selected.getBoundingClientRect();
        if (newIndex == 0) {
          window.scrollTo(0, 0);
        } else if (newIndex == appList.length - 1) {
          window.scrollTo(0, document.height);
        }  else if (rect.top < 0) {
          window.scrollBy(0, rect.top);
        } else if (rect.bottom > innerHeight) {
          window.scrollBy(0, rect.bottom - innerHeight);
        }
      }
    }

    var keys = {
      ENTER : 13,
      ESCAPE : 27,
      END : 35,
      HOME : 36,
      LEFT : 37,
      UP : 38,
      RIGHT : 39,
      DOWN : 40
    };

    // Set up a key event handler that handles moving the selected app up/down,
    // hitting enter to launch the selected app, as well as auto-focusing the
    // search box as soon as you start typing.
    window.onkeydown = function(event) {
      switch (event.keyCode) {
        case keys.DOWN:
          changeSelection(selectedIndex + 1);
          break;
        case keys.UP:
          changeSelection(selectedIndex - 1);
          break;
        case keys.HOME:
          changeSelection(0);
          break;
        case keys.END:
          changeSelection(appList.length - 1);
          break;
        case keys.ENTER:
          var app = appList[selectedIndex];
          if (app) {
            launchApp(app.id);
          }
          break;
        default:
          // Focus the search box and return true so you can just start typing even
          // when the search box isn't focused.
          $('search').focus();
          return true;
      }
      return false;
    };


    // Initalize the popup window.
    document.addEventListener('DOMContentLoaded', function () {
      chrome.management.getAll(function(info) {
        var appCount = 0;
        for (var i = 0; i < info.length; i++) {
          if (info[i].isApp) {
            appCount++;
          }
        }
        if (appCount == 0) {
          $('search').style.display = 'none';
          $('appstore_link').style.display = '';
          return;
        }
        completeList = info.sort(compareByName);
        onSearchInput();
      });

      $('search').addEventListener('input', onSearchInput);

      // Opens the webstore in a new tab.
      document.querySelector('#appstore_link button').addEventListener('click',
          function () {
            chrome.tabs.create({
              'url':'https://chrome.google.com/webstore',
              'selected':true
            });
            window.close();
          });
    });



- `pop.html`

.. code-block:: html
  :emphasize-lines: 10,14,17

    <!doctype html>
    <!--
     * Copyright (c) 2010 The Chromium Authors. All rights reserved.
     * Use of this source code is governed by a BSD-style license that can be
     * found in the LICENSE file.
    -->
    <html>
    <head>
    <link rel="stylesheet" type="text/css" href="popup.css">
    <script src="popup.js"></script>
    </head>
    <body>

    <div id="spacer_dummy"></div>
    <div id="outer">

    <div id="appstore_link">
      <p>No apps installed.</p><p>
      <button>Go get some</button></p>
    </div>

    <div id="search_container">
      <input id="search" type="text" placeholder="type to search" spellcheck="false">
    </div>

    <div id="apps"></div>

    </div>

    </body>
    </html>






资源下载
------------------------------------------------------------------------------

:download:`app_launcher.zip <../_static/examples/extensions/app_launcher.zip>` 



