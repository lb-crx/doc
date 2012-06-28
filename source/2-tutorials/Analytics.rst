.. include:: ../LINKS.rst

.. _chapter2Analytics:


Google Analytics（分析）
==============================================================================

本教程演示如何使用Google Analytics（分析）来追踪您的扩展程序的使用情况

要求
------------------------------------------------------------------------------

本教程要求对编写 `Google`_ `Chrome`_ 浏览器的扩展程序有一定的了解。
如果需要有关如何编写扩展程序的信息，
请阅读 :ref:`入门教程 <chapter0hallo>` 。

还需要设置一个
`Google Analytics （分析）帐户 <http://www.google.com/analytics>`_
来追踪您的扩展程序。


.. note:: 注意

    - 当设置账户时，在网站URL域中可以使用任何值
    - 因为扩展程序没有自己的URL


.. figure:: ../_static/images/tut_analytics/screenshot01.png


.. warning:: (#_#)

    - 另外注意Google Analytics（分析）至少需要版本 `4.0.302.2` 以上的Google Chrome浏览器才能正常工作，
    - 使用更早版本Google Chrome浏览器的用户不会在您的Google Analytics（分析）报告中出现。
    - 在 :ref:`常见问题 <chapterFAQ>` 中进一步查阅,了解如何确定Google Chrome浏览器的每一个版本都部署到哪一个平台上了。



Installing the tracking code
------------------------------------------------------------------------------


.. code-block:: js


Tracking page views
------------------------------------------------------------------------------

.. figure:: ../_static/images/tut_analytics/screenshot02.png


Monitoring analytics requests
------------------------------------------------------------------------------

.. figure:: ../_static/images/tut_analytics/screenshot03.png


Tracking events
------------------------------------------------------------------------------

.. figure:: ../_static/images/tut_analytics/screenshot04.png


Sample code
------------------------------------------------------------------------------



