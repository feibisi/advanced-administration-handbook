---
title: "网络管理员设置界面"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Multisite
    - Repos
    - Data
---

# 网络管理员设置界面

**网络管理员设置**是网络管理员为整个网络设置和修改配置的地方。网络中的第一个站点是主站点，网络设置是从该原始站点的选项中提取的。

![](https://i0.wp.com/wordpress.org/documentation/files/2020/02/superadmin-options.png?fit=1024%2C751&ssl=1)

## 运行设置

**网络名称**

您希望为此网站命名的名称。

**网络管理员邮箱**

注册和支持邮件将从此地址发出。建议使用类似 _support@example.com_ 的地址。

## 注册设置

**允许新用户注册**

禁用或启用注册功能，以及允许注册的对象类型。（默认设置为禁用。）

* 禁用注册功能。（默认）
* 允许注册用户账户。
* 允许已登录用户注册新站点。
* 允许注册站点和用户账户。

**注册通知**

每当有人注册站点或用户账户时，向网络管理员发送电子邮件通知。

**添加新用户**

允许站点管理员通过“用户 -> 添加新用户”页面向其站点添加新用户。

**禁止使用的名称**

用户不允许注册这些站点名称。请用空格分隔各个名称。

**限制邮箱注册**

如果您希望将站点注册限制在特定的邮箱域名。请每行输入一个域名。

**禁止的邮箱域名**

如果您希望禁止某些域名用于站点注册。请每行输入一个域名。

## 新站点设置

**欢迎邮件**

```
发送给新站点所有者的欢迎邮件。

亲爱的用户：

您的新 SITE_NAME 博客已成功创建于：
BLOG_URL

您可以使用以下信息登录管理员账户：
用户名：USERNAME
密码：PASSWORD
在此登录：BLOG_URLwp-login.php

希望您喜欢您的新博客。
谢谢！

--SITE_NAME 团队
```

**欢迎用户邮件**

```
发送给新用户的欢迎邮件。

亲爱的用户：

您的新账户已设置完成。

您可以使用以下信息登录：
用户名：USERNAME
密码：PASSWORD
LOGINLINK

谢谢！

--SITE_NAME 团队
```

**首篇文章**

新站点上的第一篇文章。

```
欢迎来到 <a href="SITE_URL">SITE_NAME</a>。这是您的第一篇文章。编辑或删除它，然后开始写博客吧！
```

**首个页面**

新站点上的第一个页面。

**首条评论**

新站点上的第一条评论。

**首条评论作者**

新站点上第一条评论的作者。

**首条评论网址**

新站点上第一条评论的网址。

## 上传设置

**站点上传空间**

限制上传文件总大小为 [ 50 ] MB。

**上传文件类型**

默认为 `jpg jpeg png gif mp3 mov avi wmv midi mid pdf m2ts`。

注意：添加任意文件类型将不会生效，除非同时通过 [upload_mimes](https://developer.wordpress.org/reference/hooks/upload_mimes/) 过滤器挂载了相应的功能。当前默认支持的 MIME 类型/文件扩展名集，请参阅 [wp-includes/functions.php](https://github.com/WordPress/WordPress/blob/master/wp-includes/functions.php) 中的 `wp_get_mime_types` 函数。在“上传文件类型”字段中添加默认集中未列出的 MIME 类型将**不会**生效，除非您已使用 [upload_mimes](https://developer.wordpress.org/reference/hooks/upload_mimes/) 过滤器添加了它们！上传不支持 MIME 类型的文件（未使用过滤器添加）将失败，并显示消息“抱歉，出于安全原因，不允许此文件类型”。

**最大上传文件大小**

默认为 [ 1500 ] KB。

## 语言设置

**默认语言**

默认语言为英语。

## 菜单设置

**启用管理菜单**

*   插件

在 WordPress 多站点网络中，插件的默认设置是禁用的。这意味着您的用户将无法在其仪表板内访问插件管理面板，除非您首先在网络范围内启用对插件的访问权限。