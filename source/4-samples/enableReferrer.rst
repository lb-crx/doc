.. include:: ../LINKS.rst

.. _sample4enableReferrer:


拒绝/允许偏好API访问
==============================================================================

运用 `browser_action` , `privacy`


样例,展示了如何访问偏好配置


调用:
------------------------------------------------------------------------------

- :ref:`chrome.extension.isAllowedIncognitoAccess <Extension-isAllowedFileSchemeAccess>`


源码:
------------------------------------------------------------------------------



- `manifest.json`


.. code-block:: js
  :emphasize-lines: 5

    {
      "name" : "Block/allow referrer API example extension",
      "version" : "0.1",
      "description" : "Sample extension which demonstrates how to access a preference.",
      "permissions": [ "privacy" ],
      "browser_action": {
         "default_icon": "advicedog.jpg",
         "default_popup": "popup.html"
      },
      "manifest_version": 2
    }


- `popup.html`

.. code-block:: html
  :emphasize-lines: 4-5,10

    <!DOCTYPE html>
    <html>
    <head>
      <link href="popup.css" rel="stylesheet" type="text/css">
      <script src="popup.js"></script>
    </head>
    <body>

    <div id="container">
      <input type="checkbox" id="regularValue" />
      Enable referrers
      <div id="incognito">
        <input type="checkbox" id="useSeparateIncognitoSettings" />
        Use separate setting for incognito mode:
        <br>
        <input type="checkbox" id="incognitoValue" disabled="disabled"/>
        Enable referrers in incognito sessions
      </div>
      <div id="incognito-forbidden">
        Select "Allow in incognito" to access incognito preferences
      </div>
    </div>

    </body>
    </html>


- `popup.js`

.. code-block:: js
  :emphasize-lines: 6,64,67,75

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.


    var pref = chrome.privacy.websites.referrersEnabled;

    function $(id) {
      return document.getElementById(id);
    }

    /**
     * Returns whether the |levelOfControl| means that the extension can change the
     * preference value.
     *
     * @param levelOfControl{string}
     */
    function settingIsControllable(levelOfControl) {
      return (levelOfControl == 'controllable_by_this_extension' ||
              levelOfControl == 'controlled_by_this_extension');
    }

    /**
     * Updates the UI to reflect the state of the preference.
     *
     * @param settings{object} A settings object, as returned from |get()| or the
     * |onchange| event.
     */
    function updateUI(settings) {
      var disableUI = !settingIsControllable(settings.levelOfControl);
      document.getElementById('regularValue').disabled = disableUI;
      document.getElementById('useSeparateIncognitoSettings').disabled = disableUI;
      if (settings.hasOwnProperty('incognitoSpecific')) {
        var hasIncognitoValue = settings.incognitoSpecific;
        document.getElementById('useSeparateIncognitoSettings').checked =
            hasIncognitoValue;
        document.getElementById('incognitoValue').disabled =
            disableUI || !hasIncognitoValue;
        document.getElementById('incognitoValue').checked = settings.value;
      } else {
        document.getElementById('regularValue').checked = settings.value;
      }
    }

    /**
     * Wrapper for |updateUI| which is used as callback for the |get()| method and
     * which logs the result.
     * If there was an error getting the preference, does nothing.
     *
     * @param settings{object} A settings object, as returned from |get()|.
     */
    function updateUIFromGet(settings) {
      if (settings) {
        console.log('pref.get result:' + JSON.stringify(settings));
        updateUI(settings);
      }
    }

    /**
     * Wrapper for |updateUI| which is used as handler for the |onchange| event
     * and which logs the result.
     *
     * @param settings{object} A settings object, as returned from the |onchange|
     * event.
     */
    function updateUIFromOnChange(settings) {
      console.log('pref.onChange event:' + JSON.stringify(settings));
      updateUI(settings);
    }

    /*
     * Initializes the UI.
     */
    function init() {
      chrome.extension.isAllowedIncognitoAccess(function(allowed) {
        if (allowed) {
          pref.get({'incognito': true}, updateUIFromGet);
          $('incognito').style.display = 'block';
          $('incognito-forbidden').style.display = 'none';
        }
      });
      pref.get({}, updateUIFromGet);
      pref.onChange.addListener(updateUIFromOnChange);

      $('regularValue').addEventListener('click', function () {
        setPrefValue(this.checked, false);
      });
      $('useSeparateIncognitoSettings').addEventListener('click', function () {
         setUseSeparateIncognitoSettings(this.checked);
      });
      $('incognitoValue').addEventListener('click', function () {
        setPrefValue(this.checked, true);
      });
    }

    /**
     * Called from the UI to change the preference value.
     *
     * @param enabled{boolean} The new preference value.
     * @param incognito{boolean} Whether the value is specific to incognito mode.
     */
    function setPrefValue(enabled, incognito) {
      var scope = incognito ? 'incognito_session_only' : 'regular';
      pref.set({'value': enabled, 'scope': scope});
    }

    /**
     * Called from the UI to change whether to use separate settings for
     * incognito mode.
     *
     * @param value{boolean} whether to use separate settings for
     * incognito mode.
     */
    function setUseSeparateIncognitoSettings(value) {
      if (!value) {
        pref.clear({'incognito': true});
      } else {
        // Explicitly set the value for incognito mode.
        pref.get({'incognito': true}, function(settings) {
          pref.set({'incognito': true, 'value': settings.value});
        });
      }
      document.getElementById('incognitoValue').disabled = !value;
    }

    // Call `init` to kick things off.
    document.addEventListener('DOMContentLoaded', init);



- `popup.css`

.. code-block:: css

    /**
     * Copyright (c) 2011 The Chromium Authors. All rights reserved.
     * Use of this source code is governed by a BSD-style license that can be
     * found in the LICENSE file.
     */

    #container {
      width: 300px;
    }

    #incognito {
      display: none;
    }




资源下载
------------------------------------------------------------------------------

:download:`enableReferrer.zip <../_static/examples/api/preferences/enableReferrer.zip>` 



