.. include:: ../LINKS.rst

.. _chapter1-Tabs:


Tabs
==============================================================================

Manifest
------------------------------------------------------------------------------

Examples
------------------------------------------------------------------------------


Methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Tabs-captureVisibleTab:


    captureVisibleTab
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-connect:

    connect
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-create:

    create
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-detectLanguage:

    detectLanguage
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-executeScript:

    executeScript
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-get:

    get
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""



.. _Tabs-getSelected:

getSelected
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. warning:: (#_#)

    - 已经废除!





.. _Tabs-getCurrent:

getCurrent
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-highlight:

    highlight
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-insertCSS:

    insertCSS
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-insertCSS:

    insertCSS
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-query:

    query
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-reload:

    reload
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-remove:

    remove
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""




.. _Tabs-sendMessage:

sendMessage
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""



chrome.tabs.sendMessage(integer tabId, any message, function responseCallback)

Sends a single message to the content script(s) in the specified tab, with an optional callback to run when a response is sent back. The chrome.extension.onMessage event is fired in each content script running in the specified tab for the current extension.
Parameters

tabId
( integer )
message
( any )
responseCallback
( optional function )
    Parameters

    response
    ( any )
        The JSON response object sent by the handler of the message. If an error occurs while connecting to the specified tab, the callback will be called with no arguments and chrome.extension.lastError will be set to the error message.

Callback function

If you specify the callback parameter, it should specify a function that looks like this:

function(any response) {...};

response
( any )
    The JSON response object sent by the handler of the message. If an error occurs while connecting to the specified tab, the callback will be called with no arguments and chrome.extension.lastError will be set to the error message.



.. _Tabs-update:

    update
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. js:function:: chrome.tabs.update(integer tabId, object updateProperties, function callback)

    Modifies the properties of a tab. Properties that are not specified in updateProperties are not modified. Note: This function can be used without requesting the 'tabs' permission in the manifest.

    :param tabId: ( optional integer )
        Defaults to the selected tab of the current window.
    :param updateProperties:( object )

    url
    ( optional string )
        A URL to navigate the tab to.
    active
    ( optional boolean )
        Whether the tab should be active.
    highlighted
    ( optional boolean )
        Adds or removes the tab from the current selection.
    pinned
    ( optional boolean )
        Whether the tab should be pinned.
    openerTabId
    ( optional integer )
        The ID of the tab that opened this tab. If specified, the opener tab must be in the same window as this tab.

callback
( optional function )

Callback function

If you specify the callback parameter, it should specify a function that looks like this:

function(Tab tab) {...};

tab
( optional Tab )
    Details about the updated tab, or null if the 'tabs' permission has not been requested.



Events
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. _Tabs-onActivated:

    onActivated
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""



.. _Tabs-onAttached:

    onAttached
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-onCreated:

    onCreated
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


.. _Tabs-onDetached:

    onDetached
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""



.. _Tabs-onHighlighted:

    onHighlighted
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""



.. _Tabs-onMoved:

    onMoved
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""



.. _Tabs-onRemoved:

    onRemoved
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""



.. _Tabs-onUpdated:

    onUpdated
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""




Types
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Tabs.Types:



.. _Tabs-Tab:

Tab
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""




.. seealso:: (^.^)
    
    原文: `Types <http://code.google.com/chrome/extensions/trunk/tabs.html>`_




