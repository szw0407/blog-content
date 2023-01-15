# 以下是如果要做接入MS API所需要参考的文档。

需求概述：

前端提供一个界面允许登录到Microsoft账户，并在登录完成后提供成功（或者失败）的提示框，拉取到相关的key/token总之这类东西（具体如何表述我说不清），并**如果有可能**留一个简单的设置界面，定下来参数（一个输入同步间隔，两个选择框表示同步平台）。这部分内容存为json。

后端读取前端留下来的json获得需要的信息，调用Microsoft Graph API，将列表中的任务每隔一段时间（比如10分钟）提交到Microsoft ToDo或者Outlook日历（根据前端的设置参数）。提交时，ToDo任务提前一天设置提醒；日历项目提醒提前1小时（当然如果前端继续给GUI完成设置最好，前端太难做了也可以通过手动改json文件解决，不是大问题）。

## 1. 注册应用

真的有手就行，略。目前本人已经能轻易完成（而且只能完成到）这一步

## 2. 用户访问权限的获取

登录获取：https://learn.microsoft.com/zh-cn/graph/auth-v2-user

非登录获取：https://learn.microsoft.com/zh-cn/graph/auth-v2-service

## 3. 使用API

综述：https://learn.microsoft.com/zh-cn/graph/use-the-api

调用：https://learn.microsoft.com/zh-cn/graph/call-api

## 附录：相关API的信息

Outlook API综述：https://learn.microsoft.com/zh-cn/previous-versions/office/office-365-api/api/version-2.0/use-outlook-rest-api

*看了几个小时文档后，我已经准备入土了。*

Outlook REST API基于Microsoft Graph。文档比较分散，而且都不大能看懂。官方建议尽可能使用Microsoft Graph API。

如果觉得这个东西还有可行性可言，参考下面的链接。
这个需求的模块或许可以单独成立出一个项目了。

Outlook 日历 API-创建事件：https://learn.microsoft.com/zh-cn/previous-versions/office/office-365-api/api/version-2.0/calendar-rest-operations#CreateEvents

Microsoft Graph API-创建事件：https://learn.microsoft.com/zh-cn/graph/api/user-post-events?view=graph-rest-1.0&tabs=http

Outlook API 完成ToDo任务创建：https://learn.microsoft.com/zh-cn/previous-versions/office/office-365-api/api/version-2.0/task-rest-operations#create-tasks

Microsoft Graph API 完成ToDo任务的创建：https://learn.microsoft.com/zh-cn/graph/api/todotasklist-post-tasks

## 相关的教程

使用 Microsoft Graph 生成 Python 应用：https://learn.microsoft.com/zh-cn/graph/tutorials/python

使用 Microsoft Graph 生成 Node.js Express 应用：https://learn.microsoft.com/zh-cn/graph/tutorials/node

针对单页web应用的文档：https://learn.microsoft.com/zh-cn/azure/active-directory/develop/index-spa

针对web应用的文档：https://learn.microsoft.com/zh-cn/azure/active-directory/develop/index-web-app

针对服务器后端脚本的文档：https://learn.microsoft.com/zh-cn/azure/active-directory/develop/index-service

*个人最开始认为作为在服务器后端脚本，此功能可能开发难度比较低。*

*该功能即使开发完成、部署下去，也可能存在别的问题。*

其中一个问题在于，注册在Azure AD的应用程序接管在谁手上。尤其是当采用非登陆方法调用API的时候，需要应用程序管理员负责这些秘钥的保存。

暂且写这些内容，目前这个需求完成开发的可能性一般，对于我这种看文档感觉有点吃力的菜鸟，可能短期完成开发比较困难。何况这个需求可能真的只是个人的需求，毕竟微软Outlook这套生态的用户真的不多，虽然不可否认这个是接入多平台系统日历的比较简单的解决方案了，不需要操作各种系统的接口。
