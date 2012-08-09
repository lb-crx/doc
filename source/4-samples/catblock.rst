.. include:: ../LINKS.rst

.. _sample4catblock:

屏蔽喵!
==============================================================================

运用 `browser_action` , `webRequest` 以及 ` webRequestBlocking`

.. I can't has cheezburger!

偷偷将喵的图片替换成汪的!


调用:
------------------------------------------------------------------------------

- :ref:`chrome.webRequest.onBeforeRequest <webRequest-onBeforeSendHeaders>`


源码:
------------------------------------------------------------------------------

- `messages.json`

.. code-block:: js
  :emphasize-lines: 9

    {
      "name": "CatBlock",
      "version": "1.0",
      "description": "I can't has cheezburger!",
      "permissions": ["webRequest", "webRequestBlocking",
                      "http://icanhascheezburger.files.wordpress.com/*",
                      "http://chzmemebase.files.wordpress.com/*"],
      "background": {
        "scripts": ["loldogs.js", "background.js"]
      },

      "manifest_version": 2
    }




- `loldogs.js`

.. code-block:: js

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    var loldogs = [
    "http://ihasahotdog.files.wordpress.com/2011/08/funny-dog-pictures-yoo-bin-warndid.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/08/funny-dog-pictures-it-juzz-liek-ezploded-or-sumfin.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/08/funny-dog-pictures-cool-story-bro-got-anymore-bacon.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/08/funny-dog-pictures-ohaii-iz-n-ur-livin-room-defyin-ur-gravaties.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/08/cue-puppy-pictures-spike-is-not-sleeping-on-duty-hes-lulling-you-into-a-false-sense-of-security.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/08/funny-dog-pictures-pens-too-mainstream.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/08/funny-dog-pictures-tyrannodoggus-rex.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/08/funny-dog-pictures-sniper-dog-in-position.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-i-see-wat-yur-doin-must-put-it-on-internet.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/cute-puppy-pictures-fresh-squeed.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-that-sir-is-not-my-issue.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-trippin.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-oh-gurl-diz-iz-my-jam.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-get-off-teh-lawn-we-haz-no-komment-on-teh-allejed-mailman-insident.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-sowwy-i-kin-still-feel-da-pea.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-hang-upz-ai-haz-dem.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-i-haz-a-happy-cuz-im-going-to-mai-fer-ebber-home.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-the-guitar-its-a-les-paw.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-if-you-need-me-ill-just-be-watching-the-game.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-camouflage-complete.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/cute-puppy-pictures-its-okay-i-tuck-myself-in.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-transformers-more-than-meets-the-eye.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/cute-puppy-pictures-a-long-night-of-pwnage.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/raised-in-the-woods-sos-he-knew-every-tree-killed-him-a-bear-when-he-was-only-three.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/this-be-ten-times-betteh-den-laska.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-wut-did-u-say-mai-heering-iz-spotty.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/07/funny-dog-pictures-albark-einstein-discusses-relativity.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-goggie-ob-teh-week-i-love-you.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-oh-lord-take-me-now-i-broke-my-chew-toy.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-i-has-a-safe.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-corgi-choir.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-goggie-ob-teh-week-snaaaaacks-i-love-snaaaaacks.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-porky-pug-bath-club-est.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-so-wuts-so-tuff-abowt.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-taek-teh-pitcher-awreddy.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-hip-um-pot-o-mush-impresshun-iz-good.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-if-it-touchez-floor-itz-mine-no-secund-rule.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-pug-life.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-readz-us-a-storee.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-green-light-red-light-green-light.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/648800e7-f305-43e8-83ed-06a5325923c8.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/cute-puppy-pictures-why-ai-always-gets-picked-last.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/cute-puppy-pictures-corgagram.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/cute-puppy-pictures-shes-got-a-chicken-to-ride-and-she-dont-care.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-caring-is-for-the-birds.jpg",
    "http://ihasahotdog.files.wordpress.com/2011/06/funny-dog-pictures-goggie-ob-teh-week-pudge-is-teh-boss.jpg",
    ];


- `background.js`

.. code-block:: js
  :emphasize-lines: 8

    // Copyright (c) 2012 The Chromium Authors. All rights reserved.
    // Use of this source code is governed by a BSD-style license that can be
    // found in the LICENSE file.

    // Simple extension to replace lolcat images from
    // http://icanhascheezburger.com/ with loldog images instead.

    chrome.webRequest.onBeforeRequest.addListener(
      function(info) {
        console.log("Cat intercepted: " + info.url);
        // Redirect the lolcal request to a random loldog URL.
        var i = Math.round(Math.random() * loldogs.length);
        return {redirectUrl: loldogs[i]};
      },
      // filters
      {
        urls: [
          "http://icanhascheezburger.files.wordpress.com/*",
          "http://chzmemebase.files.wordpress.com/*",
        ],
        types: ["image"]
      },
      // extraInfoSpec
      ["blocking"]);




资源下载
------------------------------------------------------------------------------

:download:`catblock.zip <../_static/examples/extensions/catblock.zip>` 
