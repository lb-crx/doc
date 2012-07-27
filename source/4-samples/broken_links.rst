.. include:: ../LINKS.rst

.. _sample4broken_links:



断裂的链接
==============================================================================

运用 `background_page` , `devtools_page` `experimental` 以及 `tab`

应用扩展开发工具, 增加审计类别, 搜察页面, 找到断开的链接


调用:
------------------------------------------------------------------------------

- :ref:`chrome.experimental.devtools.audits.addCategory <devtools_audits-addCategory>`
- :ref:`chrome.extension.onRequest <Extension-onRequest>`
- :ref:`chrome.extension.sendRequest <Extension-sendRequest>`
- :ref:`chrome.tabs.executeScript <Tabs-executeScript>`
- :ref:`chrome.tabs.sendRequest <Tabs-sendRequest>`


源码:
------------------------------------------------------------------------------




- messages.json

.. code-block:: js
  :emphasize-lines: 9-14

    {
      "name": "Broken Links",
      "version": "1.1",
      "description": "Extends the Developer Tools, adding an audit category that finds broken links on the inspected page.",
      "background": {
        "scripts": ["background.js"]
      },
      "devtools_page": "devtools.html",
      "permissions":  [
        "experimental",
        "tabs",
        "http://*/*",
        "https://*/*"
      ],
      "manifest_version": 2
    }




- devtools.html

.. code-block:: html
  :emphasize-lines: 4

    <html>
    <head>
    <script src="devtools.js"></script>
    </head>
    </html>










- devtools.js

.. code-block:: js
  :emphasize-lines: 5,8,11,16,22

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    var category = chrome.experimental.devtools.audits.addCategory(
        "Broken links", 1);
    category.onAuditStarted.addListener(function callback(auditResults) {
      chrome.extension.sendRequest({ tabId: webInspector.inspectedWindow.tabId },
          function(results) {
        if (!results.badlinks.length) {
          auditResults.addResult("No broken links",
                                 "There are no broken links on the page!",
                                 auditResults.Severity.Info);
        }
        else {
          var details = auditResults.createResult(results.badlinks.length +
              " links out of " + results.total + " are broken");
          for (var i = 0; i < results.badlinks.length; ++i) {
            details.addChild(auditResults.createURL(results.badlinks[i].href,
                                                    results.badlinks[i].text));
          }
          auditResults.addResult("Broken links found (" +
                                     results.badlinks.length +
                                     ")", "",
                                 auditResults.Severity.Severe,
                                 details);
        }
        auditResults.done();
      });
    });





- background.js

.. code-block:: js
  :emphasize-lines: 11,15,42

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    function validateLinks(links, callback) {
      var results = [];
      var pendingRequests = 0;

      function fetchLink(link) {
        if (!/^http(s?):\/\//.test(link.href))
          return;
        var xhr = new XMLHttpRequest();
        xhr.open("HEAD", link.href, true);
        xhr.onreadystatechange = function() {
          if (xhr.readyState < xhr.HEADERS_RECEIVED || xhr.processed)
            return;
          if (!xhr.status || xhr.status >= 400) {
            results.push({
                href: link.href,
                text: link.text,
                status: xhr.statusText
            });
          }
          xhr.processed = true;
          xhr.abort();
          if (!--pendingRequests) {
            callback({ total: links.length, badlinks: results });
          }
        }
        try {
          xhr.send(null);
          pendingRequests++;
        } catch (e) {
          console.error("XHR failed for " + link.href + ", " + e);
        }
      }

      for (var i = 0; i < links.length; ++i)
        fetchLink(links[i]);
    }

    chrome.extension.onRequest.addListener(function(request, sender, callback) {
      var tabId = request.tabId;
      chrome.tabs.executeScript(tabId, { file: "content.js" }, function() {
        chrome.tabs.sendRequest(tabId, {}, function(results) {
          validateLinks(results, callback);
        });
      });
    });






- content.js

.. code-block:: js
  :emphasize-lines: 2,18

    function getLinks() {
      var links = document.querySelectorAll("a");
      var results = [];
      var seenLinks = {};
      for (var i  = 0; i < links.length; ++i) {
        var text = links[i].textContent;
        if (text.length > 100)
          text = text.substring(0, 100) + "...";
        var link = links[i].href.replace(/(.*)#?/, "$1");
        if (seenLinks[link])
          continue;
        seenLinks[link] = 1;
        results.push({ href: link, text: text });
      }
      return results;
    };

    chrome.extension.onRequest.addListener(function(request, sender, sendResponse) {
      sendResponse(getLinks());
    });



资源下载
------------------------------------------------------------------------------

:download:`broken-links.zip <../_static/examples/api/devtools/audits/broken-links.zip>` 

