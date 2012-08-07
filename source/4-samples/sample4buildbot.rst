.. include:: ../LINKS.rst

.. _sample4buildbot:


`Chromium`_ `Buildbot`_ 监察器
==============================================================================

运用 `background_page` ,`browser_action` , ` notifications` 以及 ` options_page`

在工具栏中显示 `Chromium`_ 的 `Buildbot`_ 自动构建状态,并通过点击的弹窗观察细节!

调用:
------------------------------------------------------------------------------

- :ref:`chrome.browserAction.setBadgeBackgroundColor <BrowserActions-setBadgeBackgroundColor>`
- :ref:`chrome.browserAction.setBadgeText <BrowserActions-setBadgeText>`
- :ref:`chrome.browserAction.setTitle <BrowserActions-setTitle>`
- :ref:`chrome.extension.getURL <Extension-getURL>`




源码:
------------------------------------------------------------------------------

- `manifest.json`










.. code-block:: js
  :emphasize-lines: 7,9-12,17,19

    {
      "name": "Chromium Buildbot Monitor",
      "version": "0.7.7",
      "description": "Displays the status of the Chromium buildbot in the toolbar.  Click to see more detailed status in a popup.",
      "icons": { "128": "icon.png" },
      "background": {
        "scripts": ["bg.js"]
      },
      "permissions": [
        "notifications",
        "http://build.chromium.org/",
        "http://chromium-status.appspot.com/"
      ],
      "browser_action": {
        "default_title": "",
        "default_icon": "chromium.png",
        "default_popup": "popup.html"
      },
      "options_page": "options.html",

      "manifest_version": 2
    }



- `options.html`

.. code-block:: html
  :emphasize-lines: 5

    <!doctype html>
    <html>
    <head>
      <title>Chromium Buildbot Monitor Options</title>
      <script src='options.js'></script>
    </head>
    <body>
      <h2>Options for Chromium Buildbot Monitor</h2>
      <br>
      <label>
        Use desktop notifications:
        <input id="notifications" type="checkbox">
      </label>
    </body>
    </html>










- `options.js`

.. code-block:: js
  :emphasize-lines: 7,8,13-16

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    function save() {
      var prefs = JSON.parse(localStorage.prefs);
      prefs.use_notifications = document.getElementById('notifications').checked;
      localStorage.prefs = JSON.stringify(prefs);
    }

    // Make sure the checkbox checked state gets properly initialized from the
    // saved preference.
    document.addEventListener('DOMContentLoaded', function () {
      var prefs = JSON.parse(localStorage.prefs);
      document.getElementById('notifications').checked = prefs.use_notifications;
      document.getElementById('notifications').addEventListener('click', save);
    });


- `popup.html`

.. code-block:: html
  :emphasize-lines: 99

    <!doctype html>
    <html>
      <head>
        <title>Chromium Buildbot Monitor Popup</title>
        <script src='popup.js'></script>
        <style>
        body {
          font-family: sans-serif;
          font-size: 0.8em;
          overflow: hidden;
        }

        #links {
          background-color: #efefef;
          border: 1px solid #cccccc;
          border-radius: 5px;
          margin-top: 1px;
          padding: 3px;
          white-space: nowrap;
          text-align: center;
        }

        a {
          text-decoration: underline;
          color: #444;
        }

        a:hover {
          color: black;
          cursor: pointer;
        }

        body.big .bot {
          -webkit-transition: all .5s ease-out;
          margin: 20px;
        }

        body.small .bot {
          -webkit-transition: all .5s ease-out;
        }

        .bot {
          cursor: pointer;
          border-radius: 5px;
          margin-top: 1px;
          padding: 3px;
          white-space: nowrap;
        }

        .bot:hover {
          border: 2px solid black;
          padding: 2px;
        }

        .open {
          color: green;
        }

        .closed {
          color: red;
        }

        .running {
          background-color:  rgb(255, 252, 108);
          border: 1px solid rgb(197, 197, 109);
        }

        .notstarted {
          border: 1px solid rgb(170, 170, 170);
        }

        .failure {
          background-color: rgb(233, 128, 128);
          border: 1px solid rgb(167, 114, 114);
        }

        .warnings {
          background-color: rgb(255, 195, 67);
          border: 1px solid rgb(194, 157, 70);
        }

        .success {
          background-color: rgb(143, 223, 95);
          border: 1px solid rgb(79, 133, 48);
        }

        .exception {
          background-color: rgb(224, 176, 255);
          border: 1px solid rgb(172, 160, 179);
        }
      </style>
    </head>
    <body>
    <div id="links">
    <a href="" id='console'>console</a> -
    <a href="" id='try'>try</a> -
    <a href="" id='fyi'>fyi</a>
    </div>
    <div id="bots">Loading....</div>
    </body>





- `popup.js`

.. code-block:: js
  :emphasize-lines: 5-6,24,25,148

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    var botRoot = "http://build.chromium.org/p/chromium";
    var waterfallURL = botRoot + "/waterfall/bot_status.json?json=1";
    var botList;
    var checkinResults;
    var bots;
    var failures;

    function updateBotList(text) {
      var results = (new RegExp('(.*)<\/body>', 'g')).exec(text);
      if (!results || results.index < 0) {
        console.log("Error: couldn't find bot JSON");
        console.log(text);
        return;
      }
      var data;
      try {
        // The build bot returns invalid JSON. Namely it uses single
        // quotes and includes commas in some invalid locations. We have to
        // run some regexps across the text to fix it up.
        var jsonString = results[1].replace(/'/g, '"').replace(/},]/g,'}]');
        data = JSON.parse(jsonString);
      } catch (e) {
        console.dir(e);
        console.log(text);
        return;
      }
      if (!data) {
        throw new Error("JSON parse fail: " + results[1]);
      }
      botList = data[0];
      checkinResults = data[1];

      failures = botList.filter(function(bot) {
        return (bot.color != "success");
      });
      displayFailures();
    }

    function displayFailures() {
      bots.innerText = "";

      if (failures.length == 0) {
        var anchor = document.createElement("a");
        anchor.addEventListener("click", showConsole);
        anchor.className = "open";
        anchor.innerText = "The tree is completely green.";
        bots.appendChild(anchor);
        bots.appendChild(document.createTextNode(" (no way!)"));
      } else {
        var anchor = document.createElement("a");
        anchor.addEventListener("click", showFailures);
        anchor.innerText = "failures:";
        var div = document.createElement("div");
        div.appendChild(anchor);
        bots.appendChild(div);

        failures.forEach(function(bot, i) {
          var div = document.createElement("div");
          div.className = "bot " + bot.color;
          div.addEventListener("click", function() {
            // Requires closure for each iteration to retain local value of |i|.
            return function() { showBot(i); }
          }());
          div.innerText = bot.title;
          bots.appendChild(div);
        });
      }
    }

    function showURL(url) {
      window.open(url);
      window.event.stopPropagation();
    }

    function showBot(botIndex) {
      var bot = failures[botIndex];
      var url = botRoot + "/waterfall/waterfall?builder=" + bot.name;
      showURL(url);
    }

    function showConsole() {
      var url = botRoot + "/waterfall/console";
      showURL(url);
    }

    function showTry() {
      var url = botRoot + "/try-server/waterfall";
      showURL(url);
    }

    function showFyi() {
      var url = botRoot + "/waterfall.fyi/console";
      showURL(url);
    }

    function showFailures() {
      var url = botRoot +
          "/waterfall/waterfall?show_events=true&failures_only=true";
      showURL(url);
    }

    function requestURL(url, callback) {
      console.log("requestURL: " + url);
      var xhr = new XMLHttpRequest();
      try {
        xhr.onreadystatechange = function(state) {
          if (xhr.readyState == 4) {
            if (xhr.status == 200) {
              var text = xhr.responseText;
              //console.log(text);
              callback(text);
            } else {
              bots.innerText = "Error.";
            }
          }
        }

        xhr.onerror = function(error) {
          console.log("xhr error: " + JSON.stringify(error));
          console.dir(error);
        }

        xhr.open("GET", url, true);
        xhr.send({});
      } catch(e) {
        console.log("exception: " + e);
      }
    }

    function toggle_size() {
      if (document.body.className == "big") {
        document.body.className = "small";
      } else {
        document.body.className = "big";
      }
    }

    document.addEventListener('DOMContentLoaded', function () {
      toggle_size();

      bots = document.getElementById("bots");

      // XHR from onload winds up blocking the load, so we put it in a setTimeout.
      window.setTimeout(requestURL, 0, waterfallURL, updateBotList);

      // Setup event handlers
      document.querySelector('#console').addEventListener('click', showConsole);
      document.querySelector('#try').addEventListener('click', showTry);
      document.querySelector('#fyi').addEventListener('click', showFyi);
    });







- `bg.js`

.. code-block:: js
  :emphasize-lines: 5,9,21,29,34-35,60-61

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    var statusURL = "http://chromium-status.appspot.com/current?format=raw";

    if (!localStorage.prefs) {
      // Default to notifications being on.
      localStorage.prefs = JSON.stringify({ "use_notifications": true });
    }

    var lastOpen = null;
    var lastNotification = null;
    function notifyIfStatusChange(open, status) {
      var prefs = JSON.parse(localStorage.prefs);
      if (lastOpen && lastOpen != open && prefs.use_notifications) {
        if (lastNotification) {
          lastNotification.cancel();
        }
        var notification = webkitNotifications.createNotification(
            chrome.extension.getURL("icon.png"), "Tree is " + open, status);
        lastNotification = notification;
        notification.show();
      }
      lastOpen = open;
    }

    function updateStatus(status) {
      chrome.browserAction.setTitle({title:status});
      var open = /open/i;
      if (open.exec(status)) {
        notifyIfStatusChange("open", status);
        //chrome.browserAction.setBadgeText("\u263A");
        chrome.browserAction.setBadgeText({text:"\u2022"});
        chrome.browserAction.setBadgeBackgroundColor({color:[0,255,0,255]});
      } else {
        notifyIfStatusChange("closed", status);
        //chrome.browserAction.setBadgeText("\u2639");
        chrome.browserAction.setBadgeText({text:"\u00D7"});
        chrome.browserAction.setBadgeBackgroundColor({color:[255,0,0,255]});
      }
    }

    function requestStatus() {
      requestURL(statusURL, updateStatus);
      setTimeout(requestStatus, 30000);
    }

    function requestURL(url, callback) {
      //console.log("requestURL: " + url);
      var xhr = new XMLHttpRequest();
      try {
        xhr.onreadystatechange = function(state) {
          if (xhr.readyState == 4) {
            if (xhr.status == 200) {
              var text = xhr.responseText;
              //console.log(text);
              callback(text);
            } else {
              chrome.browserAction.setBadgeText({text:"?"});
              chrome.browserAction.setBadgeBackgroundColor({color:[0,0,255,255]});
            }
          }
        }

        xhr.onerror = function(error) {
          console.log("xhr error: " + JSON.stringify(error));
          console.dir(error);
        }

        xhr.open("GET", url, true);
        xhr.send({});
      } catch(e) {
        console.log("exception: " + e);
      }
    }

    window.onload = function() {
      window.setTimeout(requestStatus, 10);
    }




资源下载
------------------------------------------------------------------------------

:download:`buildbot.zip <../_static/examples/extensions/buildbot.zip>` 

