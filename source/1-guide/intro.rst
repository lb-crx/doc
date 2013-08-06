

下面这些章节, 是假定你已经把 :ref:`扩展入门 <chapter0hallo>` 和 :ref:`综述 <chapter0overview>` 给啃过了的基础上的. 除非特殊说明, 这里的所有文档适用于扩展以及打包应用.


自定义 Google Chrome 浏览器
-------------------------------------------------------------- 

- :ref:`浏览器操作 <chapter1-BrowserActions>`      加图标到工具栏 (只适用于扩展)
- :ref:`桌面通知 <chapter1-DesktopNotifications>`   通知用户重要的事件
- :ref:`Omnibox <chapter1-Omnibox>`     添关键字到地址栏
- :ref:`选项页 <chapter1-OptionsPages>`   让用户自定义你的扩展
- :ref:`自定义页 <chapter1-OverridePages>`  实现你自己的标准浏览器页面，比如新标签页
- :ref:`页面操作 <chapter1-PageActions>`    在地址栏添加临时图标 (只适用于扩展)
- :ref:`主题皮肤 <chapter1-Themes>`   更改浏览器的整体外观


其他与 Google Chrome 的交互方式
-------------------------------------------------------------- 

- :ref:`书签 <chapter1-Bookmarks>`   新建，组织和管理用户书签
- :ref:`Cookies <chapter1-Cookies>`     浏览和修改浏览器 cookie 
- :ref:`开发者工具 <chapter1-DeveloperTools>`     在开发者工具栏中添加新功能
- :ref:`事件 <chapter1-Events>`  监测是否有感兴趣的事件发生
- :ref:`历史 <chapter1-History>`     交互访问浏览器曾经访问的页面记录
- :ref:`标签页 <chapter1-Tabs>`    创建，修改和重排浏览器标签
- :ref:`窗口 <chapter1-Windows>`     创建，修改和重排浏览器窗口

Implementing the innards of your extension
-------------------------------------------------------------- 

- :ref:`无障碍使用 <chapter1-a11y>`    Make your extension accessible to people with disabilities
- :ref:`后台页面 <chapter1-BackgroundPages>`    Put all the common code for your extension in a single place
- :ref:`内容脚本 <chapter1-ContentScripts>`     Run JavaScript code in the context of web pages
- :ref:`XHR <chapter1-XHR>`    Use XMLHttpRequest to send and receive data from remote servers
- :ref:`国际化 <chapter1-i18n>`    Deal with language and locale
- :ref:`消息传递 <chapter1-MessagePassing>`     Communicate from a content script to its parent extension, or vice versa
- :ref:`可选权限控制 <chapter1-OptionalPermissions>`    Modify your extension's permissions
- :ref:`NPAPI插件 <chapter1-NPAPIPlugins>`   Load native binary code


Finishing and distributing your extension
--------------------------------------------------------------  

- :ref:`自动更新 <chapter1-Autoupdating>`    Update extensions automatically
- :ref:`发布托管 <chapter1-Hosting>`     Host extensions on Google servers or your own
- :ref:`其它部署选项 <chapter1-OtherDeploymentOptions>` Other Deployment Options    Distribute extensions on your network or with other software
- :ref:`Packaging <chapter1-Packaging>`   Create a .crx file so you can distribute your extension 

