.. include:: ../LINKS.rst

.. _chapter1-Packaging:



打包 Packaging
=============================

本文描述如何将我们的扩展程序打包。

如 :ref:`概述 <chapter0overview>` 中所说，
扩展程序打包为已签名的 `ZIP`_ 文件，扩展名为“crx”，例如: `myextension.crx`

.. note:: 注意

    - 可能我们不需要为专门进行打包。
    - 如果使用 `Chrome开发者信息中心 <https://chrome.google.com/webstore/developer/dashboard>`_ 进行发布

        - 那么不需要特意创建 `.crx` 文件
        - 除非我们需要发布非公开版本，例如用于测试人员。
    
    - 可以在Chrome网上应用店的 :ref:`入门教程 <chapter5webstoreGettingStarted>` 中从 :ref:`步骤5：为应用创建ZIP文件 <chapter5Started-5>` 这一部分开始，找到发布扩展程序和应用程序的有关信息。


当我们为扩展程序打包时，将获得唯一的 `密钥对`_ ~ 扩展程序的基于公钥的散列标识符， 
`私钥`_ 由我们单独保存，用来为将来所有版本的扩展程序签名。


创建包 Creating a package
------------------------------------------------------------------------------


为扩展程序打包的步骤:

#. 进入以下URL，打开扩展程序管理页面： `chrome://extensions`
#. 如果 `开发人员模式` 右边有一个加号，单击加号（在新界面中，如果 `开发人员模式` 左边的复选框未选中，单击选中它）。
#. 单击 `打包扩展程序` 按钮，出现一个对话框。
#. 在 `扩展程序根目录` 字段中，指定扩展程序所在文件夹的路径，例如， ``c:\myext``  。（忽略其它字段，第一次为扩展程序打包时不需要指定私钥文件。）
#. 单击 `打包扩展程序` 。打包程序将创建两个文件：一个 `.crx` 文件，即可以安装的扩展程序；另一个是 `.pem` 文件，包含私钥。

`不要丢失私钥！` 确保 `.pem` 文件保密，并存放在安全的地方。
如果以后进行如下事情，我们需要这一文件：

#. :ref:`更新 <chapter1-Packaging-Updat>` 扩展程序
#. :ref:`上传 <chapter1-Packaging-Upload>` 将扩展程序至 `Chrome网上应用店`_

如果扩展程序打包成功，您会看到如下对话框，告诉您.crx文件与.pem文件的位置：

.. figure:: ../_static/images/package-success.gif


.. _chapter1-Packaging-Updat:

更新扩展程序 Updating a package
------------------------------------------------------------------------------

创建扩展更新版本的步骤：

#. 增加 `manifest.json` 中的版本号。
#. 进入如下URL，打开扩展程序管理页面： `chrome://extensions`
#. 单击 `打包扩展程序` 按钮，出现一个对话框
#. 在 `扩展程序根目录` 字段中指定扩展程序所在文件夹，例如 ``c:\myext``
#. 在 `私有密钥文件` 字段中，指定已生成的用于该扩展程序的 `.pem` 文件位置，例如 ``c:\myext.pem``
#. 单击 `打包扩展程序` 

如果已更新的扩展程序打包成功，将会看到如下对话框：

.. figure:: ../_static/images/update-success.gif


.. _chapter1-Packaging-Upload:

将已经打包的扩展程序上传到Chrome网上应用店
------------------------------------------------------------------------------

可以使用 `Chrome开发者信息中心`_ 来上传之前自己打包的扩展程序。

然而，除非进行如下步骤，
否则! `Chrome网上应用店`_ 中扩展程序 `标识符` 将与我们有本地创建的扩展程序包不同。

如果我们已经通过其它渠道发布了扩展程序包，
不同的标识符可能会出问题，
因为这样将导致用户可以同时安装扩展程序的多个版本, 而且分别具有单独的本地数据

如果想要保持扩展程序的标识符不变，遵循以下步骤：

#. 将创建 `.crx` 文件时生成的私有 `密钥`_ 文件重命名为 `key.pem`
#. 将 `key.pem` 文件放在扩展程序根目录中
#. 将这一目录压缩为ZIP文件。
#. 使用 `Chrome开发者信息中心`_ 上传该ZIP文件。



在命令行中打包 Packaging at the command line
------------------------------------------------------------------------------


为扩展程序打包的另一种方式是在命令行中执行 `chrome.exe` 。
使用 `--pack-extension` 参数指定扩展程序所在文件夹位置，
使用 `--pack-extension-key` 指定扩展程序的私有密钥文件位置。

例如 Windows 环境下，在 CMD 中输入如下命令： 
::

    chrome.exe --pack-extension=c:\myext --pack-extension-key=c:\myext.pem

Linux 环境下，使用如下命令：
::

    google-chrome --pack-extension=/home/glow/myext --pack-extension-key=/home/glow/myext.pem


如果不希望看到对话框，请在命令行中添加 `--no-message-box` 参数。



扩展程序包的格式和脚本 Package format and scripts
------------------------------------------------------------------------------

有关格式的更多信息以及您可以用来创建.crx文件的脚本，请参见 :ref:`CRX包的格式 <chapter3-crx>` 。



.. seealso:: (^.^)
    
    原文: `Hosting <http://code.google.com/chrome/extensions/hosting.html>`_

