.. include:: ../LINKS.rst

.. _chapter1-OtherDeploymentOptions:



其它发布选项 Other Deployment Options
==============================================================================
~ （外部扩展程序）

通常，用户自己安装扩展程序。然而有时候可能希望自动安装扩展程序，如下是两个典型案例：

- 扩展程序与其它软件有联系，并且用户安装软件时也应该同时安装扩展程序，在用户卸载软件时扩展程序也应该同时被卸载。
- 网络管理员希望在整个公司安装同样的扩展程序。

自动安装的扩展程序称为 `外部扩展程序` 。 `Google`_  `Chrome`_ 浏览器支持两种安装外部扩展程序的方式：

- 使用首选项 `JSON`_ 文件
- 使用Windows注册表（仅用于Windows）

这两种方式都支持在用户的计算机上安装来自 `.crx` 文件的扩展程序，
首选项JSON文件还支持安装已托管在 :ref:`更新URL <chapter1-Autoupdating>` 的扩展程序。

有关托管扩展程序的详情，请参见 :ref:`托管 <chapter1-Hosting>` 。



开始之前 Before you begin
------------------------------------------------------------------------------

首先，将扩展程序 :ref:`打包 <chapter1-Packaging>` 为 `.crx` 文件并确保它能够成功安装。
如果想从 :ref:`更新URL <chapter1-Autoupdating>` 安装，
确保扩展程序已正确 :ref:`托管 <chapter1-Hosting>` 。
然后，在编辑首选项文件或者注册表前，注意以下几个问题：

- 扩展程序 `.crx` 文件的 `位置` ，或者更新URL。
- 扩展程序的 `版本` （来自清单文件或者chrome://extensions页面）
扩展程序的 `标识符`（当载入已打包的扩展程序后可以从chrome://extensions页面获得）

如下例子假定版本为 `1.0` ，标识符为 `aaaaaaaaaabbbbbbbbbbcccccccccc`



使用选项文件 Using a preferences file
------------------------------------------------------------------------------


Windows用户注意:
    在 `bug 41902 <http://crbug.com/41902>`_ 修复之前，
    可能需要使用 :ref:`Windows注册表 <chapter1-OtherDeploymentOptions-winreg`_ ，而不是首选项文件。




.. note:: 注意：

    - 以前版本的 `Chrome`_ 浏览器使用 `external_extensions.json` 文件来指定要安装的扩展程序，
    - `该文件已弃用`!
    - 建议为每一个扩展程序使用单独的 `.json` 文件。


#. 如果从文件安装，确保 `.crx` 扩展程序文件在我们需要安装扩展程序的计算机上可用
    （将它复制到本地目录或者网络共享，例如 `\\server\share\extension.crx` 或 `/home/share/extension.crx` ）
#. 在下面列出的某一个文件夹中创建下列名称的文件： `aaaaaaaaaabbbbbbbbbbcccccccccc.json`
    ，其中文件名（不包括扩展名）对应于我们的扩展程序标志符。位置取决与操作系统。

    - **Windows** :

        - ``chrome_root\Application\chrome_version\Extensions\``
        - 例子： ``c:\Users\Me\AppData\Local\Google\Chrome\Application\6.0.422.0\Extensions\``

    - **Mac OS X** :

        - 用于某个特定用户: `~USERNAME/Library/Application Support/Google/Chrome/External Extensions/`
        - 用于所有用户： `/Library/Application Support/Google/Chrome/External Extensions/`
        - 只有路径中每一个目录的所有者都是root或者属于wheel或管理员组，并且不是所有人都具有写入权限，才会读取用于所有用户的外部扩展程序文件。
        - 另外，路径还不能包含符号链接。这些限制确保未授权的用户不能使扩展程序为所有用户安装。
            有关详情请参见 :ref:`疑难解答 <chapter1-OtherDeploymentOptions-mac>` 。

        .. note:: 注意

            - 以上用于所有用户的路径是在Chrome 16中追加的，之前的版本使用另一个路径
            - `/Applications/Google Chrome.app/Contents/Extensions/`
            - 该路径在版本17中弃用，在版本20中不再支持。
            - 请使用以上列出的路径形式

    - **Linux** :

        - `/opt/google/chrome/extensions/`
        - `/usr/share/google-chrome/extensions/`


        .. note:: 注意

            - 如果必要的话, 用 `chmod` 改变权限组,确保所有人都能访问
            - `extensions/aaaaaaaaaabbbbbbbbbbcccccccccc.json`

#. 果从文件安装的话，通过"`external_crx`"与"`external_version`"字段在以上创建的文件中指定扩展程序的位置与版本

    - 例如  ::

        {
          "external_crx": "/home/share/extension.crx",
          "external_version": "1.0"
        }

    .. note:: 注意

        - 在位置中必须为所有 ``\`` 字符转义。
        - 例如， ``\\server\share\extension.crx``
        - 表示为" ``\\\\server\\share\\extension.crx`` "

    - 如果从更新URL安装，在名为"external_update_url"的字段中指定扩展程序的更新URL。
    - 例如 ::

        {
          "external_update_url": "http://myhost.com/mytestextension/updates.xml"
        }

    - 如果只是为某些浏览器语言安装扩展程序，可以在名为"`supported_locales`"的字段中列出支持的语言。可以指定诸如"`en`"之类的语言，这样扩展程序将为所有像"`en-US`"、"`en-GB`"等这样的英语语言安装。
    - 选择了扩展程序不支持的另一种语言，外部扩展程序将会被卸载。
    - 如果没有"`supported_locales`"列表，扩展程序将为所有语言安装。
    - 例如 ::

        {
          "external_update_url": "http://myhost.com/mytestextension/updates.xml",
          "supported_locales": [ "en", "fr", "de" ]
        }

#. 保存 `JSON`_ 文件
#. 运行 `Google`_ `Chrome`_ 浏览器，进入 `chrome://extensions` ，应该会看到列出的扩展程序



.. _chapter1-OtherDeploymentOptions-mac:

Mac OS权限问题疑难解答    Troubleshooting Mac OS permissions problems
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Mac OS中，只有系统权限阻止未授权的用户更改它时，才会读取外部扩展程序文件。
如果 `Chrome`_ 浏览器运行后没有看见已安装的外部扩展程序文件，那么可能外部扩展程序首选项文件有权限问题。
要确定是否是这一问题，请遵循如下步骤：

#. 运行控制台程序。可在应用程序/实用工具/控制台找到它。
#. 如果控制台最左边的图标为“显示日志列表”，单击该图标，将会在左边出现新的一列。
#. 单击左侧窗格中的“控制台消息”。
#. 搜索 `无法读取外部扩展程序（Can not read external extensions）` 这一字符串。

    - 如果读取外部扩展程序文件过程中发生问题，就将记录成错误消息。
    - 查阅其上方的另一个错误消息，应该包含问题的描述。
    - 例如，如果看到了如下错误：“路径 /Library/Application Support/Google/Chrome 的所有者不正确”，就需要使用 `chgrp` 或 `Finder` 的信息对话框更改目录的所有者为管理员组。

#. 修复这一问题后，重启 `Chrome`_ 浏览器，看看外部扩展程序现在是否己安装。

    - 有可能前一权限错误使 `Chrome`_ 浏览器不能检测到第二个错误。
    - 所以,如果外部扩展程序还没有安装，要重复以上步骤，直到在控制台应用程序中看不到错误。




.. _chapter1-OtherDeploymentOptions-winreg:

使用Windows注册表 Using the Windows registry
------------------------------------------------------------------------------


#. 确保 `.crx` 扩展程序文件在需要安装扩展程序的计算机上可用。
    （将它复制到本地目录或者网络共享，例如 `\\server\share\extension.crx` ）

#. 在注册表中寻找或创建如下键：

    - 32位Windows： `HKEY_LOCAL_MACHINE\Software\Google\Chrome\Extensions`
    - 64位Windows： `HKEY_LOCAL_MACHINE\Software\Wow6432Node\Google\Chrome\Extensions`

#. 在 **Extensions** 键下创建新键（文件夹），名称与您的扩展程序标识符相同
    （例如， `aaaaaaaaaabbbbbbbbbbcccccccccc` ）。

#. 创建两个字符串值（ `REG_SZ` ），分别名为 `path` 和 `version` ，并分别设置为扩展程序的位置和版本。例如：

    - path： `\\server\share\extension.crx`
    - version： `1.0`

#. 运行浏览器，进入 chrome://extensions 应该看到列出的扩展程序。




更新和卸载 Updating and uninstalling
------------------------------------------------------------------------------

`Google`_ `Chrome`_ 浏览器在
每一次启动时扫描首选项文件中的元数据项以及注册表，对已安装的外部扩展程序做出必要的更改。

- 要将我们的扩展程序更新到新的版本，需要更新相应的文件，并更新首选项文件或注册表中的版本。
- 要卸载我们的扩展程序（例如，如果依赖的软件卸载了），

    - 请删除首选项文件（ `aaaaaaaaaabbbbbbbbbbcccccccccc.json` 
    - 或从旧版本 `Chrome`_ 浏览器中的 `external_extensions.json` 文件中删除元数据）
    - 或者从注册表中删除元数据。




常见问题 FAQ
------------------------------------------------------------------------------

回答部分有关外部扩展程序的常见问题。

我能否将URL指定为指向外部扩展程序的路径？ :
    可以，只要您使用了首选项JSON文件，
    扩展程序必须按照托管部分描述的那样正确 :ref:`托管 <chapter1-Hosting>`  。
    使用"external_update_url"属性指向更新清单，包含您的扩展程序的URL。

使用首选项文件安装时的常见问题有哪些？:
    - 没有指定与 `.crx` 文件中相同的标识符或版本
    - `.json` 文件（ `aaaaaaaaaabbbbbbbbbbcccccccccc.json` ）[或者老版本 `Chrome`_ 浏览器中 `external_extensions.json` ]的位置不正确
    - `JSON`_ 文件中有语法错误（忘记用逗号分隔项目或者某处遗漏行尾的逗号）
    - `JSON`_ 文件项指向的 `.crx` 路径不正确（或者指定的路径不包含文件名）
    - UNC路径中的反斜杠没有转义（例如， " ``\\server\share\file`` "是错误的，应该为" ``\\\\server\\share\\extension`` "）
    - 网络共享的权限问题

使用注册表安装时的常见问题有哪些？ :
    - 没有指定与 `.crx` 文件中相同的标识符或版本
    - 键在注册表中创建的位置不对
    - 注册表项指向的 `.crx` 文件路径不正确（或者指定的路径不包含文件名）
    - 网络共享的权限问题

如果用户卸载了扩展程序怎么办？:
    如果用户通过用户界面卸载了扩展程序，在启动时将不会再安装或更新这一扩展程序。换句话说，这一扩展程序进入了黑名单。

我应该如何从黑名单中去掉我的扩展程序？:
    如果用户卸载了我们的扩展程序，应该尊重这一决定。
    然而，如果我们（开发者）意外地通过用户界面卸载了自己的扩展程序，
    可以通过用户界面正常安装扩展程序，然后卸载，从而移除黑名单标记。




