.. include:: ../LINKS.rst

.. _chapter1-Autoupdating:



自动更新 Autoupdating
=============================

我们希望扩展程序能够像Google Chrome浏览器一样自动更新:
加入漏洞及安全补丁，增加新特性，增强性能，改善用户界面。

如果仅使用 `Chrome开发者信息中心 <https://chrome.google.com/webstore/developer/dashboard?hl=zh-CN>`_ 发布扩展程序，那么可以 `忽略` 这篇文档。

因为,通过信息中心, Google 会自动通过 网上应用商店( `Chrome Web Store`_ ) 向用户发布扩展的更新版本。

如果想在网上应用店以外的地方托管我们的扩展程序，那么真心应该读完本篇文档.
同时还应该阅读 `托管 <chapter1-Hosting>` 以及 `打包 <chapter1-Packaging>` 。


概述 Overview
------------------------------------------------------------------------------


- 扩展程序的 `清单文件` 可以包含 `update_url` 属性，指向检查更新的位置。
- 检查更新返回的内容为一个 `更新清单` XML文档，列出扩展程序的最新版本。

每隔几小时，浏览器都会检查已安装的扩展程序是否有更新URL。
如果有, 浏览器会对每一个扩展程序的更新URL发出请求，查询更新清单XML文件。
如果更新清单中提及的版本,比已安装的扩展程序更新，浏览器就会下载并安装新版本,
这点和我们手动更新向效果相同，
新的 `.crx` 文件必须与已安装的扩展程序使用同一 `私有密钥` 签名。


更新URL Update URL
------------------------------------------------------------------------------

如果想自行托管发布扩展更新，需要在 `manifest.json <chapter3-Manifest` 文件中添加"update_url"属性，像这样：

::

    {
      "name": "我的扩展程序",
      #...
      "update_url": "http://myhost.com/mytestextension/updates.xml",
      #...
    }


更新清单 Update manifest
------------------------------------------------------------------------------

服务器返回的更新清单应该为一个如下形式的XML文档（ `appid` , `updatecheck` 部分需要修改）：


.. code-block:: xml

    <?xml version='1.0' encoding='UTF-8'?>
    <gupdate xmlns='http://www.google.com/update2/response' protocol='2.0'>
      <app appid='aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa'>
        <updatecheck codebase='http://myhost.com/mytestextension/mte_v2.crx' version='2.0' />
      </app>
    </gupdate>


这一XML格式借用了 Omaha（Google用于更新的基础架构）的格式，有关细节请参见https://code.google.com/p/omaha/（英文）。扩展程序系统使用更新清单中 `<app>` 和 `<updatecheck>` 元素的以下属性：

`appid` :
    - 扩展程序的标识符，基于扩展程序公共密钥的散列值生成，如 `打包 <chapter1-Packaging>` 部分所述。
    - 进入扩展程序页面（chrome://extensions）找到扩展程序标识符。
    - 然而，托管应用程序不会在扩展程序页面中列出，可以通过如下步骤找出任何一个应用程序的标识符：
        - 打开应用程序，可以在新标签页面中单击它的图标。
        - 打开JavaScript控制台，可以单击扳手图标，并选择工具 -> JavaScript 控制台。
        - 在JavaScript控制台中输入如下表达式：`chrome.app.getDetails().id` 控制台将以包含引号的字符串形式显示应用程序标识符。

`codebase` :
    - 指向扩展程序 `.crx` 文件的URL。

`version` :
    - 用于客户端，确定是否应该下载 `codebase` 指定的 `.crx` 文件。这一属性应该与 `.crx` 文件内 `manifest.json` 文件的 `"version"` 属性一致。

更新清单XML文件可以包含多个 `<app>` 元素，描述不同扩展程序的信息。


.. note:: 注意

    - 每个 `<app>` 元素中只能包含一个 `<updatecheck>` 元素，
    - 当我们的扩展程序更新时，请修改现有的 `<updatecheck>` 元素的属性




测试  Testing
------------------------------------------------------------------------------
默认更新检查频率为几个小时，但是您可以使用扩展程序页面中的 `立即更新扩展程序` 按钮强制更新。

另一个选项是使用 `--extensions-update-frequency` 命令行参数，以秒为单位，设置更频繁的更新间隔。
例如，要每隔45秒就检查更新，像这样运行Google Chrome浏览器：

::

    chrome.exe --extensions-update-frequency=45


.. note:: 注意

    这会影响到所有已安装的扩展程序，所以您应该考虑到这会带来的带宽以及服务器负载。您可能需要临时卸载除了您正在测试的扩展程序以外的所有其它扩展程序，并且在正常的浏览器使用过程中不应该以这一选项运行浏览器


高级用法：请求参数   Advanced usage: request parameters
------------------------------------------------------------------------------



基本的自动更新机制使得服务器端的工作十分简单，只要在任何普通的Web服务器（例如Apache）上加入一个静态XML文件，并在发布您的扩展程序的新版本时更新此XML文件即可。
更高级的开发者可能希望利用我们请求更新清单时附加的参数，指示扩展程序的标识符和版本。这样，就可以对所有扩展程序都使用相同的更新URL，指向运行动态服务器端代码而不是静态XML文件的URL。
请求参数的格式为 ::

    ?x=<extension_data>


其中 `<extension_data>` 为如下格式的内容经过URL编码后所得的字符串：::

    id=<id>&v=<version>


例如，假设您有两个扩展程序，都指向相同的更新URL（http://test.com/extension_updates.php）：

- 扩展程序1
    - 标识符："aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
    - 版本："1.1"
- 扩展程序2
    - 标识符："bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb"
    - 版本："0.4"


则向每一个单独的扩展程序发出的请求分别为：::

    http://test.com/extension_updates.php?x=id%3Daaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa%26v%3D1.1
    http://test.com/extension_updates.php?x=id%3Dbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb%26v%3D0.4

对于每一个唯一的更新URL，多个扩展程序可以在一个请求中列出。对于上述例子，如果用户安装了这两个扩展程序，则两个请求合并为一个请求：::

    http://test.com/extension_updates.php
        ?x=id%3Daaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa%26v%3D1.1
        &x=id%3Dbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb%26v%3D0.4

 
如果已安装的扩展程序数目太多，使用相同的更新URL使得GET请求URL太长（大约2000个字符以上），更新检查程序会发送必要的附加GET请求。


.. note:: 注意

   将来可能会用发出单个POST请求包含请求参数，来替代发出多个GET请求。



高级用法：最小浏览器版本 Advanced usage: minimum browser version
------------------------------------------------------------------------------

`Google`_ 不断地在向浏览器中添加更多的扩展API，可能我们发布的更新版本的扩展程序只能在更新版本的浏览器中运行。

尽管 Google `Chrome`_ 浏览器会自动更新(目前 :sup:`120619` `猎豹浏览器`_ :sub:`1.0.3.2292` 还不支持自动升级,不过,一定会的!-)，大部分用户更新至给定的新版本仍然需要几天时间。

要确保给定的扩展程序更新仅应用于指定的版本或更高版本的Google Chrome 及兼容浏览器，
您可以在您的更新清单中向<app>元素添加 `"prodversionmin"` 属性。例如：


.. code-block:: xml

        <?xml version='1.0' encoding='UTF-8'?>
        <gupdate xmlns='http://www.google.com/update2/response' protocol='2.0'>
          <app appid='aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa'>
            <updatecheck codebase='http://myhost.com/mytestextension/mte_v2.crx' 
            version='2.0' 
            prodversionmin='3.0.193.0'/>
          </app>
        </gupdate>


这样就确保只用用户正在运行Google Chrome 3.0.193.0或更新版本时才会自动更新到这一扩展程序的2.0版本。


.. seealso:: (^.^)
    
    原文: `Autoupdating <http://code.google.com/chrome/extensions/trunk/autoupdate.html>`_

