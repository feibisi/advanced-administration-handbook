---
title: "安装前准备"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Before Install
    - Repos
    - Data
---

# 安装前准备

在安装 WordPress 之前，您需要确认您的网络托管服务提供商满足必要的软件和条件。同时，您必须能够访问服务器并拥有一些工具。

如果您对这些术语感到陌生：**主机**（或**托管服务提供商**）是您付费以将网站上线的公司，而**服务器**是运行 WordPress 的计算机。

## 服务器端要求

* PHP 7.4 或更高版本
* MySQL 5.7 或 MariaDB 10.3 或更高版本
* HTTPS 支持

有关您网络主机的详细要求列表，请参阅[官方要求页面](https://wordpress.org/about/requirements/)和[服务器环境页面](https://make.wordpress.org/hosting/handbook/server-environment/)。

## 本地要求

* 通过 FTP 或 shell 访问服务器的登录账户（用户 ID 和密码）
* 文本编辑器
* FTP 工具
* 您选择的网页浏览器

您需要知道如何使用文本编辑器来编辑主配置文件。如果您是 Windows 用户，记事本即可。如果您是 OS X 用户，可以使用 TextEdit。之后，您可能还需要编辑模板文件（可参考[模板](https://codex.wordpress.org/Templates)）。您可以通过 WordPress [管理界面](https://wordpress.org/documentation/article/administration-screens/)完成此操作，但强烈建议使用一个好的文本编辑器。有关更多信息，请参阅[编辑文件](https://developer.wordpress.org/advanced-administration/wordpress/edit-files/)。

您需要能够使用 FTP 程序来[上传](https://developer.wordpress.org/advanced-administration/upgrade/ftp/filezilla/)文件和[设置文件权限](https://developer.wordpress.org/advanced-administration/server/file-permissions/)（可选）。您可以选择 [FileZilla](https://developer.wordpress.org/advanced-administration/upgrade/ftp/filezilla/) 来完成此任务。

现在您已准备就绪，可以继续[安装](https://developer.wordpress.org/advanced-administration/before-install/howto-install/)了。