.. include:: ../LINKS.rst

.. _chapter3-manifestVersion:



清单文件版本
==================================================================

扩展程序、主题背景以及应用程序只是打包的资源，另外须附带一
:ref:`manifest.json <chapter3-manifest>` 文件来描述包的内容。
该文件的格式大体上是稳定的，但是偶尔需要重大更改，解决特定的问题。
所以,开发人员应该在清单文件中设置 `manifest_version` 属性，指定扩展包使用的清单文件规范版本。


当前版本
------------------------------------------------------------------


在Chrome 18及更高版本中，开发人员应该指定 `'manifest_version': 2` ： ::

    {
      ...,
      "manifest_version": 2,
      ...
    }


清单文件版本1在Chrome 18及更高版本中 **已弃用** ，但是版本2还不是必需的。 
还没有准备转换至Chrome 18中新的清单文件版本的扩展程序、应用程序以及主题背景既
可以明确地指定 `版本1` ，也可以完全省略该属性。 
在将来的某一时刻，将 **不再支持** 清单文件 `版本1` 。
`Google`_ 将提供充足的警告信息，从载入版本1的未打包扩展程序时的警告开始，并随着时间推移逐步转变。


.. note:: 不建议

    在Chrome 17或更低版本中设置
    `manifest_version 2`
    如果扩展需要在较旧版本的Chrome浏览器中运行，目前请继续使用
    `版本1` ，在 `版本1` 停止工作前 `Google`_ 将会给出充分的警告


版本1与版本2之间的改变
------------------------------------------------------------------

- 内容安全策略默认情况下设置为 `"script-src 'self'; object-src 'self'"`
    这会对开发人员产生一系列影响，在 :ref:`内容安全策略(content_security_policy)文档 <chapter3-CSP>` 中详细描述。

- 扩展程序包的资源默认情况下不再可用于外部网站（作为图片的src属性或者script标签）。
    如果希望网站能够载入我们扩展程序包中的资源，需要通过 
    :ref:`web_accessible_resources <manifest-accessible>`清单文件属性明确将它列入白名单。
    这对于通过插入内容脚本在网页上建立界面的扩展程序特别有用。

- `background_page` 属性已经由 `background` 属性取代，
    它包含 `scripts` 或 `page` 属性中的任意一个，详情在 :ref:`后台页面 <chapter1-BackgroundPages>` 文档中可用。
- 浏览器按钮的更改：

    - 清单文件中的 `browser_actions` 键以及 `chrome.browserActions` 
        API已经消失，请改用单数形式的 :ref:`browser_action <chapter1-BrowserActions>` 以及 `chrome.browserAction` 。
    - `browser_action` 的 `icons` 属性已移除，请改用 :ref:`default_icon <chapter1-BrowserActions>` 属性或者 :ref:`chrome.browserAction.setIcon <BrowserActions-setIcon>`
    - `browser_action` 的 `name` 属性已移除，请改用 :ref:`default_title <chapter1-BrowserActions>` 属性或者 :ref:`chrome.browserAction.setTitle <BrowserActions-setTitle>`
    - `browser_action` 的 `popup` 属性已移除，请改用 :ref:`default_popup <chapter1-BrowserActions>` 属性或者 :ref:`chrome.browserAction.setPopup <BrowserActions-setPopup>`
    - `browser_action` 的 `default_popup` 属性不能再指定为对象，而必须为 `字符串`

- 页面按钮的更改：

    - 清单文件中的 `page_actions` 属性以及 `chrome.pageActions`  API也将消失。
        请改用单数形式的 `page_action` 以及 `chrome.pageAction`
    
    - `page_action` 中的 `icons` 属性已移除，请改用 :ref:`default_icon <chapter1-PageActions>` 属性或 :ref:`chrome.pageAction.setIcon <PageActions-setIcon>`
    - `page_action` 中的 `name` 属性已移除，请改用 :ref:`default_title <chapter1-PageActions>` 属性或 :ref:`chrome.pageAction.setTitle <PageActions-setTitle>`
    - `page_action` 中的 `popup` 属性已移除，请改用 :ref:`default_popup <chapter1-PageActions>` 属性或 :ref:`chrome.pageAction.setPopup <PageActions-setPopup>`
    - `page_action` 中的 `default_popup` 属性不能再指定为对象，而必须为 `字符串`
    - `chrome.self` API已移除，请改用 :ref:`chrome.extension <chapter1-Extension>`

- `chrome.extension.getTabContentses(!!!)`  :sup:`[?]原文就这样的` 与 `chrome.extension.getExtensionTabs` 将消失，请改用
:ref:`chrome.extension.getViews({ "type": "tab" }) <Extension-getViews>`
- `Port.tab` 将消失，请改用 :ref:`Port.sender <Extension-Port>`



.. seealso:: (^.^)
    
    原文: `Manifest Version <http://code.google.com/chrome/extensions/manifestVersion.html>`_

