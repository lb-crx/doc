.. include:: ../LINKS.rst

.. _chapter3-crx:



CRX包的格式
======================


CRX文件是具有特殊头信息的ZIP文件，并且文件扩展名为 `.crx`

包头信息
------------------------------------------------------------------------------

头信息包含作者的公共密钥和扩展程序的签名，签名以SHA-1算法使用作者的私有密钥从ZIP文件生成。

头信息要求“ `低低高高` ”的字节顺序以及4字节的对齐。


.. list-table:: 按顺序描述.crx的头信息
   :widths: 15 10 10 10 30
   :header-rows: 1

   * - 字段
     - 类型
     - 长度
     - 值
     - 描述
   * - magic number
     - `char[]`
     - 32位
     - Cr24
     - Chrome要求每一个.crx包的开头包含此常量
   * - version
     - `unsigned int`
     - 32位
     - 2
     - 使用的.crx文件格式的版本（当前为2）
   * - public key length
     - `unsigned int`
     - 32位
     - pubkey.length
     - RSA公共密钥的长度，以字节为单位
   * - signature length
     - `unsigned int`
     - 32位
     - sig.length
     -  签名的长度，以字节为单位
   * - public key
     - `byte[]`
     - pubkey.length
     - pubkey.contents
     - 以 `X509 SubjectPublicKeyInfo` 块的格式表示的作者的 `RSA`_ 公共密钥的内容
   * - signature
     - `byte[]`
     - sig.length
     - sig.content
     - `ZIP`_ 内容使用作者私有密钥的签名，该签名使用 `RSA`_ 算法， `SHA-1`_ 散列函数





扩展程序内容 Extension contents
------------------------------------------------------------------------------

扩展程序的 `ZIP`_ 文件附加到 `*.crx` 包的头信息之后，
这应该是生成头信息中的签名的同一个 `ZIP`_ 文件

例子
------------------------------------------------------------------------------

以下是一个 `.crx` 文件开头内容的十六进制表示： 
::

    43 72 32 34   # "Cr24" -- the magic number
    02 00 00 00   # 2 -- the crx format version number
    A2 00 00 00   # 162 -- length of public key in bytes
    80 00 00 00   # 128 -- length of signature in bytes
    ...........   # the contents of the public key
    ...........   # the contents of the signature
    ...........   # the contents of the zip file



用于打包的脚本 Packaging scripts
------------------------------------------------------------------------------

社区成员编写各种脚本来产生 `.crx` 文件


    Ruby
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`github: crxmake <http://github.com/Constellation/crxmake>`_


    Bash
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash -e
    #
    # Purpose: Pack a Chromium extension directory into crx format

    if test $# -ne 2; then
      echo "Usage: crxmake.sh <extension dir> <pem path>"
      exit 1
    fi

    dir=$1
    key=$2
    name=$(basename "$dir")
    crx="$name.crx"
    pub="$name.pub"
    sig="$name.sig"
    zip="$name.zip"
    trap 'rm -f "$pub" "$sig" "$zip"' EXIT

    # zip up the crx dir
    cwd=$(pwd -P)
    (cd "$dir" && zip -qr -9 -X "$cwd/$zip" .)

    # signature
    openssl sha1 -sha1 -binary -sign "$key" < "$zip" > "$sig"

    # public key
    openssl rsa -pubout -outform DER < "$key" > "$pub" 2>/dev/null

    byte_swap () {
      # Take "abcdefgh" and return it as "ghefcdab"
      echo "${1:6:2}${1:4:2}${1:2:2}${1:0:2}"
    }

    crmagic_hex="4372 3234" # Cr24
    version_hex="0200 0000" # 2
    pub_len_hex=$(byte_swap $(printf '%08x\n' $(ls -l "$pub" | awk '{print $5}')))
    sig_len_hex=$(byte_swap $(printf '%08x\n' $(ls -l "$sig" | awk '{print $5}')))
    (
      echo "$crmagic_hex $version_hex $pub_len_hex $sig_len_hex" | xxd -r -p
      cat "$pub" "$sig" "$zip"
    ) > "$crx"
    echo "Wrote $crx"



.. seealso:: (^.^)
    
    原文: `Tutorial: Google Analytics <http://code.google.com/chrome/extensions/crx.html>`_


