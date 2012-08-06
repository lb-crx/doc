.. include:: ../LINKS.rst

.. _sample4fx:


`Chrome`_ 音效
==============================================================================

运用 `background_page` , ` bookmarks` , `options_page` 以及 `tabs`

享受浏览网页时, 更加神奇的身临其境的音效!



调用:
------------------------------------------------------------------------------

- :ref:`chrome.bookmarks.onCreated <Bookmarks-onCreated>`
- :ref:`chrome.bookmarks.onMoved <Bookmarks-onMoved>`
- :ref:`chrome.bookmarks.onRemoved <Bookmarks-onRemoved>`
- :ref:`chrome.extension.getBackgroundPage <Extension-getBackgroundPage>`
- :ref:`chrome.extension.onRequest <Extension-onRequest>`
- :ref:`chrome.extension.sendRequest <Extension-sendRequest>`
- :ref:`chrome.tabs.get <Tabs-get>`
- :ref:`chrome.tabs.onAttached <Tabs-onAttached>`
- :ref:`chrome.tabs.onCreated <Tabs-onCreated>`
- :ref:`chrome.tabs.onDetached <Tabs-onDetached>`
- :ref:`chrome.tabs.onMoved <Tabs-onMoved>`
- :ref:`chrome.tabs.onRemoved <Tabs-onRemoved>`
- :ref:`chrome.tabs.onSelectionChanged <Tabs-onSelectionChanged>`
- :ref:`chrome.tabs.onUpdated <Tabs-onUpdated>`
- :ref:`chrome.windows.onCreated <Windows-onCreated>`
- :ref:`chrome.windows.onFocusChanged <Windows-onFocusChanged>`
- :ref:`chrome.windows.onRemoved <Windows-onRemoved>`





源码:
------------------------------------------------------------------------------

- `manifest.json`

.. code-block:: js
  :emphasize-lines: 6,10-14,16-19

    {
      "name": "Chrome Sounds",
      "version": "1.2",
      "description": "Enjoy a more magical and immersive experience when browsing the web using the power of sound.",
      "background": {
        "scripts": ["bg.js"]
      },
      "options_page": "options.html",
      "icons": { "128": "icon.png" },
      "permissions": [
        "tabs",
        "bookmarks",
        "http://*/*",
        "https://*/*"
      ],
      "content_scripts": [ {
        "matches": ["http://*/*", "https://*/*"],
        "js": ["content.js"],
        "all_frames": true
      }],
      "manifest_version": 2
    }

- `options.html`

.. code-block:: html
  :emphasize-lines: 17

    <!doctype html>
    <html>
    <head>
    <style>
    body {
      font-family: sans-serif;
    }
    #attributions {
      margin-top: 20px;
      color: #666666;
      Xfont-size: 10px;
    }
    .sound {
      cursor: pointer;
    }
    </style>
    <script src="options.js"></script>
    </head>
    <body>
    <div id="sounds"></div>
    <div id="attributions">
      Sounds from:
      <ul>
      <li><a href="http://www.freesound.org">www.freesound.org</a></li>
      <li><a href="http://www.free-samples-n-loops.com/loops.html">www.free-samples-n-loops.com/loops.html</a></li>
      <li>Googlers with microphones.*</li>
      </ul>
      <span style="font-size:10px">* Canadian sound made by actual Canadian.</span>
    </div>
    </body>
    </html>






- `options.js`

.. code-block:: js
  :emphasize-lines: 7,11,19,22,58-59

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    function playSound(id) {
      console.log(id);
      chrome.extension.getBackgroundPage().playSound(id, false);
    }

    function stopSound(id) {
      chrome.extension.getBackgroundPage().stopSound(id);
    }

    function soundChanged(event) {
      var key = event.target.name;
      var checked = event.target.checked;
      if (checked) {
        localStorage.setItem(key, "enabled");
        playSound(event.target.name);
      } else {
        localStorage.setItem(key, "disabled");
        stopSound(event.target.name);
      }
    }

    function showSounds() {
      var sounds = document.getElementById("sounds");
      if (!localStorage.length) {
        sounds.innerText = "";
        return;
      }
      sounds.innerText = "Discovered sounds: (uncheck to disable)";
      var keys = new Array();
      for (var key in localStorage) {
        keys.push(key);
        console.log(key);
      }
      keys.sort();
      for (var index in keys) {
        var key = keys[index];
        var div = document.createElement("div");
        var check = document.createElement("input");
        check.type = "checkbox"
        check.name = key;
        check.checked = localStorage[key] == "enabled";
        check.onchange = soundChanged;
        div.appendChild(check);
        var text = document.createElement("span");
        text.id = key;
        text.innerText = key;
        text.className = "sound";
        text.onclick = function(event) { playSound(event.target.id); };
        div.appendChild(text);
        sounds.appendChild(div);
      }
    }

    document.addEventListener('DOMContentLoaded', showSounds);
    document.addEventListener('focus', showSounds);






- `content.js`

.. code-block:: js
  :emphasize-lines: 8,40

    /*
     * Content script for Chrome Sounds.
     * Tracks in-page events and notifies the background page.
     */

    function sendEvent(event, value) {
      console.log("sendEvent: " + event + "," + value);
      chrome.extension.sendRequest({eventName: event, eventValue: value});
    }

    // Timers to trigger "stopEvent" for coalescing events.
    var timers = {};

    function stopEvent(type) {
      timers[type] = 0;
      sendEvent(type, "stopped");
    }

    // Automatically coalesces repeating events into a start and a stop event.
    // |validator| is a function which should return true if the event is
    // considered to be a valid event of this type.
    function handleEvent(event, type, validator) {
      if (validator) {
        if (!validator(event)) {
          return;
        }
      }
      var timerId = timers[type];
      var eventInProgress = (timerId > 0);
      if (eventInProgress) {
        clearTimeout(timerId);
        timers[type] = 0;
      } else {
        sendEvent(type, "started");
      }
      timers[type] = setTimeout(stopEvent, 300, type);
    }

    function listenAndCoalesce(target, type, validator) {
      target.addEventListener(type, function(event) {
        handleEvent(event, type, validator);
      }, true);
    }

    listenAndCoalesce(document, "scroll");

    // For some reason, "resize" doesn't seem to work with addEventListener.
    if ((window == window.top) && document.body && !document.body.onresize) {
      document.body.onresize = function(event) {
        sendEvent("resize", "");
      };
    }

    listenAndCoalesce(document, "keypress", function(event) {
      if (event.charCode == 13)
        return false;

      // TODO(erikkay) This doesn't work in gmail's rich text compose window.
      return event.target.tagName == "TEXTAREA" ||
             event.target.tagName == "INPUT" ||
             event.target.isContentEditable;
    });


- `bg.js`

.. code-block:: js
  :emphasize-lines: 12,81-92

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    /*
     * Background page for Chrome Sounds extension.
     * This tracks various events from Chrome and plays sounds.
     */

    // Map of hostname suffixes or URLs without query params to sounds.
    // Yeah OK, some of these are a little cliche...
    var urlSounds = {
      "http://www.google.ca/": "canadian-hello.mp3",
      "about:histograms": "time-passing.mp3",
      "about:memory": "transform!.mp3",
      "about:crash": "sadtrombone.mp3",
      "chrome://extensions/": "beepboop.mp3",
      "http://www.google.com.au/": "didgeridoo.mp3",
      "http://www.google.com.my/": "my_subway.mp3",
      "http://www.google.com/appserve/fiberrfi/": "dialup.mp3",
      "lively.com": "cricket.mp3",
      "http://www.google.co.uk/": "mind_the_gap.mp3",
      "http://news.google.com/": "news.mp3",
      "http://www.bing.com/": "sonar.mp3",
    };

    // Map of query parameter words to sounds.
    // More easy cliches...
    var searchSounds = {
      "scotland": "bagpipe.mp3",
      "seattle": "rain.mp3",
    };

    // Map of tab numbers to notes on a scale.
    var tabNoteSounds = {
      "tab0": "mando-1.mp3",
      "tab1": "mando-2.mp3",
      "tab2": "mando-3.mp3",
      "tab3": "mando-4.mp3",
      "tab4": "mando-5.mp3",
      "tab5": "mando-6.mp3",
      "tab6": "mando-7.mp3",
    };

    // Map of sounds that play in a continuous loop while an event is happening
    // in the content area (e.g. "keypress" while start and keep looping while
    // the user keeps typing).
    var contentSounds = {
      "keypress": "typewriter-1.mp3",
      "resize": "harp-transition-2.mp3",
      "scroll": "shepard.mp3"
    };

    // Map of events to their default sounds
    var eventSounds = {
      "tabCreated": "conga1.mp3",
      "tabMoved": "bell-transition.mp3",
      "tabRemoved": "smash-glass-1.mp3",
      "tabSelectionChanged": "click.mp3",
      "tabAttached": "whoosh-15.mp3",
      "tabDetached": "sword-shrill.mp3",
      "tabNavigated": "click.mp3",
      "windowCreated": "bell-small.mp3",
      "windowFocusChanged": "click.mp3",
      "bookmarkCreated": "bubble-drop.mp3",
      "bookmarkMoved": "thud.mp3",
      "bookmarkRemoved": "explosion-6.mp3",
      "windowCreatedIncognito": "weird-wind1.mp3",
      "startup": "whoosh-19.mp3"
    };

    var soundLists = [urlSounds, searchSounds, eventSounds, tabNoteSounds,
        contentSounds];

    var sounds = {};

    // Map of event names to extension events.
    // Events intentionally skipped:
    // chrome.windows.onRemoved - can't suppress the tab removed that comes first
    var events = {
      "tabCreated": chrome.tabs.onCreated,
      "tabMoved": chrome.tabs.onMoved,
      "tabRemoved": chrome.tabs.onRemoved,
      "tabSelectionChanged": chrome.tabs.onSelectionChanged,
      "tabAttached": chrome.tabs.onAttached,
      "tabDetached": chrome.tabs.onDetached,
      "tabNavigated": chrome.tabs.onUpdated,
      "windowCreated": chrome.windows.onCreated,
      "windowFocusChanged": chrome.windows.onFocusChanged,
      "bookmarkCreated": chrome.bookmarks.onCreated,
      "bookmarkMoved": chrome.bookmarks.onMoved,
      "bookmarkRemoved": chrome.bookmarks.onRemoved
    };

    // Map of event name to a validation function that is should return true if
    // the default sound should be played for this event.
    var eventValidator = {
      "tabCreated": tabCreated,
      "tabNavigated": tabNavigated,
      "tabRemoved": tabRemoved,
      "tabSelectionChanged": tabSelectionChanged,
      "windowCreated": windowCreated,
      "windowFocusChanged": windowFocusChanged,
    };

    var started = false;

    function shouldPlay(id) {
      // Ignore all events until the startup sound has finished.
      if (id != "startup" && !started)
        return false;
      var val = localStorage.getItem(id);
      if (val && val != "enabled") {
        console.log(id + " disabled");
        return false;
      }
      return true;
    }

    function didPlay(id) {
      if (!localStorage.getItem(id))
        localStorage.setItem(id, "enabled");
    }

    function playSound(id, loop) {
      if (!shouldPlay(id))
        return;

      var sound = sounds[id];
      console.log("playsound: " + id);
      if (sound && sound.src) {
        if (!sound.paused) {
          if (sound.currentTime < 0.2) {
            console.log("ignoring fast replay: " + id + "/" + sound.currentTime);
            return;
          }
          sound.pause();
          sound.currentTime = 0;
        }
        if (loop)
          sound.loop = loop;

        // Sometimes, when playing multiple times, readyState is HAVE_METADATA.
        if (sound.readyState == 0) {  // HAVE_NOTHING
          console.log("bad ready state: " + sound.readyState);
        } else if (sound.error) {
          console.log("media error: " + sound.error);
        } else {
          didPlay(id);
          sound.play();
        }
      } else {
        console.log("bad playSound: " + id);
      }
    }

    function stopSound(id) {
      console.log("stopSound: " + id);
      var sound = sounds[id];
      if (sound && sound.src && !sound.paused) {
        sound.pause();
        sound.currentTime = 0;
      }
    }

    var base_url = "http://dl.google.com/dl/chrome/extensions/audio/";

    function soundLoadError(audio, id) {
      console.log("failed to load sound: " + id + "-" + audio.src);
      audio.src = "";
      if (id == "startup")
        started = true;
    }

    function soundLoaded(audio, id) {
      console.log("loaded sound: " + id);
      sounds[id] = audio;
      if (id == "startup")
        playSound(id);
    }

    // Hack to keep a reference to the objects while we're waiting for them to load.
    var notYetLoaded = {};

    function loadSound(file, id) {
      if (!file.length) {
        console.log("no sound for " + id);
        return;
      }
      var audio = new Audio();
      audio.id = id;
      audio.onerror = function() { soundLoadError(audio, id); };
      audio.addEventListener("canplaythrough",
          function() { soundLoaded(audio, id); }, false);
      if (id == "startup") {
        audio.addEventListener("ended", function() { started = true; });
      }
      audio.src = base_url + file;
      audio.load();
      notYetLoaded[id] = audio;
    }

    // Remember the last event so that we can avoid multiple events firing
    // unnecessarily (e.g. selection changed due to close).
    var eventsToEat = 0;

    function eatEvent(name) {
      if (eventsToEat > 0) {
        console.log("ate event: " + name);
        eventsToEat--;
        return true;
      }
      return false;
    }

    function soundEvent(event, name) {
      if (event) {
        var validator = eventValidator[name];
        if (validator) {
          event.addListener(function() {
            console.log("handling custom event: " + name);

            // Check this first since the validator may bump the count for future
            // events.
            var canPlay = (eventsToEat == 0);
            if (validator.apply(this, arguments)) {
              if (!canPlay) {
                console.log("ate event: " + name);
                eventsToEat--;
                return;
              }
              playSound(name);
            }
          });
        } else {
          event.addListener(function() {
            console.log("handling event: " + name);
            if (eatEvent(name)) {
              return;
            }
            playSound(name);
          });
        }
      } else {
        console.log("no event for " + name);
      }
    }

    var navSound;

    function stopNavSound() {
      if (navSound) {
        stopSound(navSound);
        navSound = null;
      }
    }

    function playNavSound(id) {
      stopNavSound();
      navSound = id;
      playSound(id);
    }

    function tabNavigated(tabId, changeInfo, tab) {
      // Quick fix to catch the case where the content script doesn't have a chance
      // to stop itself.
      stopSound("keypress");

      //console.log(JSON.stringify(changeInfo) + JSON.stringify(tab));
      if (changeInfo.status != "complete") {
        return false;
      }
      if (eatEvent("tabNavigated")) {
        return false;
      }

      console.log(JSON.stringify(tab));

      if (navSound)
        stopSound(navSound);

      var re = /https?:\/\/([^\/:]*)[^\?]*\??(.*)/i;
      match = re.exec(tab.url);
      if (match) {
        if (match.length == 3) {
          var query = match[2];
          var parts = query.split("&");
          for (var i in parts) {
            if (parts[i].indexOf("q=") == 0) {
              var q = decodeURIComponent(parts[i].substring(2));
              q = q.replace("+", " ");
              console.log("query == " + q);
              var words = q.split(" ");
              for (j in words) {
                if (searchSounds[words[j]]) {
                  console.log("searchSound: " + words[j]);
                  playNavSound(words[j]);
                  return false;
                }
              }
              break;
            }
          }
        }
        if (match.length >= 2) {
          var hostname = match[1];
          if (hostname) {
            var parts = hostname.split(".");
            if (parts.length > 1) {
              var tld2 = parts.slice(-2).join(".");
              var tld3 = parts.slice(-3).join(".");
              var sound = urlSounds[tld2];
              if (sound) {
                playNavSound(tld2);
                return false;
              }
              sound = urlSounds[tld3];
              if (sound) {
                playNavSound(tld3);
                return false;
              }
            }
          }
        }
      }

      // Now try a direct URL match (without query string).
      var url = tab.url;
      var query = url.indexOf("?");
      if (query > 0) {
        url = tab.url.substring(0, query);
      }
      console.log(tab.url);
      var sound = urlSounds[url];
      if (sound) {
        playNavSound(url);
        return false;
      }

      return true;
    }

    var selectedTabId = -1;

    function tabSelectionChanged(tabId) {
      selectedTabId = tabId;
      if (eatEvent("tabSelectionChanged"))
        return false;

      var count = 7;
      chrome.tabs.get(tabId, function(tab) {
        var index = tab.index % count;
        playSound("tab" + index);
      });
      return false;
    }

    function tabCreated(tab) {
      if (eatEvent("tabCreated")) {
        return false;
      }
      eventsToEat++;  // tabNavigated or tabSelectionChanged
      // TODO - unfortunately, we can't detect whether this tab will get focus, so
      // we can't decide whether or not to eat a second event.
      return true;
    }

    function tabRemoved(tabId) {
      if (eatEvent("tabRemoved")) {
        return false;
      }
      if (tabId == selectedTabId) {
        eventsToEat++;  // tabSelectionChanged
        stopNavSound();
      }
      return true;
    }

    function windowCreated(window) {
      if (eatEvent("windowCreated")) {
        return false;
      }
      eventsToEat += 3;  // tabNavigated, tabSelectionChanged, windowFocusChanged
      if (window.incognito) {
        playSound("windowCreatedIncognito");
        return false;
      }
      return true;
    }

    var selectedWindowId = -1;

    function windowFocusChanged(windowId) {
      if (windowId == selectedWindowId) {
        return false;
      }
      selectedWindowId = windowId;
      if (eatEvent("windowFocusChanged")) {
        return false;
      }
      return true;
    }

    function contentScriptHandler(request) {
      if (contentSounds[request.eventName]) {
        if (request.eventValue == "started") {
          playSound(request.eventName, true);
        } else if (request.eventValue == "stopped") {
          stopSound(request.eventName);
        } else {
          playSound(request.eventName);
        }
      }
      console.log("got message: " + JSON.stringify(request));
    }


    //////////////////////////////////////////////////////

    // Listen for messages from content scripts.
    chrome.extension.onRequest.addListener(contentScriptHandler);

    // Load the sounds and register event listeners.
    for (var list in soundLists) {
      for (var id in soundLists[list]) {
        loadSound(soundLists[list][id], id);
      }
    }
    for (var name in events) {
      soundEvent(events[name], name);
    }







资源下载
------------------------------------------------------------------------------

:download:`fx.zip <../_static/examples/extensions/fx.zip>` 

