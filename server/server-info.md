---
title: "查找服务器信息"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Server
    - Repos
    - Data
---

# 查找服务器信息

您正在使用哪个版本的 [PHP](https://wordpress.org/documentation/article/glossary#php)？您的网站主机使用什么服务器软件？您拥有哪个版本的 [MySQL](https://wordpress.org/documentation/article/glossary#mysql)？您的网站主机使用什么操作系统？

![php73win_phpinfo](https://user-images.githubusercontent.com/1508963/201365720-3a13ccab-c44c-43f2-8326-e3a997c5acfa.jpg)

ALT= PHP 信息测试文件结果顶部

这些是 WordPress 用户在准备[安装 WordPress](https://developer.wordpress.org/advanced-administration/before-install/howto-install/) 时经常被问到的问题。其他时候，这些问题是在排查 WordPress 安装问题时被问到的。但不必花费大量时间在您的网站或主机网站上搜索答案——有一种非常简单的方法可以获取这些信息。

收集信息最简单的方法是使用一个名为 phpinfo() 的 PHP 函数。phpinfo() 函数将查询您（或您主机）的服务器，并生成一份包含大量数据的报告。注意：请记住收藏此页面，因为将来 [WordPress 支持论坛](https://www.wordpress.org/support/forums/)的志愿者可能会要求您使用此方法获取信息，以协助他们排查您在支持论坛上提出的问题。

**警告：** 此文件将包含一些关于您服务器的中等敏感信息，这些信息可能帮助攻击者访问您的服务器。请确保为文件起一个不易猜测的文件名，并在完成后立即删除它。

在文本编辑器中，复制并粘贴以下命令：

```
<?php phpinfo(); ?>
```

确保命令前后没有空格，只有命令本身，然后将文件保存为一个不易猜测的名称，例如 sffdsajk234.php。使文件名难以被黑客猜测很重要，因为该文件包含的信息可能帮助他们入侵您的服务器。

将文件上传到您网站的根目录。然后在浏览器中输入该文件的地址：

```
https://example.com/sffdsajk234.php
```

结果将长达数页，并包含大量信息。虽然您的数据顺序可能不同，但大部分情况下，您只需要列出以下内容的摘要项：

|                |                                                               |
| -------------- | ------------------------------------------------------------- |
| PHP            | 版本 7.3.0                                                    |
| 系统           | Windows NT DESKTOP-LK01DAN 10.0 build 17763 (Windows 10) i586 |
| 构建日期       | 2018年12月6日 01:51:18                                        |
| 服务器 API     | Apache 2.0 处理器                                             |
| Apache 版本    | Apache/2.4.37 (Win32) OpenSSL/1.1.1a PHP/7.3.0                |

就是这样。请务必记得在使用完毕后删除该文件，因为将其留在服务器上可能会帮助黑客入侵您的系统。

## Information and Resources

- [PHP.net’s phpinfo Manual](https://www.php.net/phpinfo)
- [Zend’s PHP Manual on phpinfo](https://www.zend.com/manual/function.phpinfo.php)
- [WordPress Environment PHP library](https://github.com/abelcallejo/wordpress-environment)