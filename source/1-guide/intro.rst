

下面这些章节, 是假定你已经把 :ref:`扩展入门 <chapter0hallo>` 和 :ref:`综述 <chapter0overview>` 给啃过了的基础上的. 除非特殊说明, 这里的所有文档适用于扩展以及打包应用.


自定义 Google Chrome 浏览器
-------------------------------------------------------------- 

- :ref:`浏览器操作 <chapter1-BrowserActions>`      加图标到工具栏 (只适用于扩展)
- :ref:`桌面通知 <chapter1-DesktopNotifications>`   通知用户重要的事件
- :ref:`Omnibox <chapter1-Omnibox>`     添关键字到地址栏
- Options Pages   Let users customize your extension
- Override Pages  Implement your own version of standard browser pages such as the New Tab page
- Page Actions    Add temporary icons inside the address bar (只适用于扩展)
- Themes  Change the overall appearance of the browser


Interacting with Google Chrome in other ways
-------------------------------------------------------------- 

- Bookmarks   Create, organize, and otherwise manipulate the user's bookmarks
- Cookies     Explore and modify the browser's cookie system
- :ref:`Developer Tools <chapter1-DeveloperTools>`     Add features to Chrome Developer Tools
- Events  Detect when something interesting happens
- History     Interact with the browser's record of visited pages
- Tabs    Create, modify, and rearrange tabs in the browser
- Windows     Create, modify, and rearrange windows in the browser

Implementing the innards of your extension
-------------------------------------------------------------- 

- Accessibility (a11y)    Make your extension accessible to people with disabilities
- Background Pages    Put all the common code for your extension in a single place
- Content Scripts     Run JavaScript code in the context of web pages
- Cross-Origin XHR    Use XMLHttpRequest to send and receive data from remote servers
- Internationalization    Deal with language and locale
- Message Passing     Communicate from a content script to its parent extension, or vice versa
- Optional Permissions    Modify your extension's permissions
- NPAPI Plugins   Load native binary code


Finishing and distributing your extension
--------------------------------------------------------------  

- :ref:`自动更新 <chapter1-Autoupdating>`    Update extensions automatically
- :ref:`发布托管 <chapter1-Hosting>`     Host extensions on Google servers or your own
- :ref:`其它部署选项 <chapter1-OtherDeploymentOptions>` Other Deployment Options    Distribute extensions on your network or with other software
- :ref:`Packaging <chapter1-Packaging>`   Create a .crx file so you can distribute your extension 

