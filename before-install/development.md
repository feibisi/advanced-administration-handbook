---
title: "运行 WordPress 开发副本"
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

# 运行 WordPress 开发副本

拥有一个 WordPress 开发实例是在不中断线上版本的情况下更新、开发和修改网站的好方法。设置 WordPress 开发副本有多种方式，本文将介绍基础知识、最佳实践、技巧以及一些工具，让运行 WordPress 开发副本变得更加容易。

## Installing WordPress on your computer

Use these instructions to set up a local server environment for testing and development.

Installing WordPress locally is usually meant for development. Those interested in development can follow the instructions below to download and install WordPress locally.
- [wp-env](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/) - a free, open-source development environment maintained by the WordPress core developer community.
- [VVV or Varying Vagrant Vagrants](https://varyingvagrantvagrants.org/) - free, open-source local development environment maintained by members of the WordPress community.
- [XAMPP](https://www.apachefriends.org/) - free and open-source local development environment maintained by Apache Friends
- [MAMP](https://www.mamp.info/en/mac/) - free local development environment that everything you need to install WordPress locally.
- [DDEV](https://ddev.readthedocs.io/en/stable/users/quickstart/#wordpress) - free, open-source, development environment. Seamlessly share local sites over public domains, includes a database editor, Xdebug, and other performance profiling tools.
- [Lando](https://lando.dev/) – free, open-source development environment that offers a [plugin to install WordPress locally](https://docs.lando.dev/plugins/wordpress/).
- [AMPPS](https://ampps.com/downloads/) – free WAMP/MAMP/LAMP stack with Softaculous Installer built in. It can 1-click install and upgrade WordPress and others as well.
- [Bitnami package for WordPress](https://bitnami.com/stack/wordpress) and [Bitnami package for WordPress Multisite](https://bitnami.com/stack/wordpress-multisite) - Bitnami packages for WordPress that provide a one-click install solution for WordPress or WordPress Multisite on your local computer or in the cloud.
- [Instant WordPress](https://instantwp.com/) - free, standalone, portable WordPress development environment for Windows that will run from a USB key.
- [WordPress Studio](https://developer.wordpress.com/studio/) - free, open-source app to install and manage multiple WordPress sites locally.

### 软件设备 - 开箱即用

您可能会发现，使用预集成的[软件设备](https://en.wikipedia.org/wiki/Software_appliance)是快速启动并运行 WordPress 的绝佳方式，尤其是在与虚拟机软件（如 VMWare、VirtualBox、Xen HVM、KVM）结合使用时。

Parallels 是另一款可用的软件。与虚拟机软件不同，它需要付费。它允许您在计算机上同时运行 Mac 和 Windows 系统。

软件设备让用户无需手动安装 WordPress 及其依赖项，而是可以在几分钟内部署一个自包含的系统，几乎无需设置。

- [TurnKey WordPress 设备](https://www.turnkeylinux.org/wordpress)：一个基于 Debian 的免费设备，即开即用。它捆绑了一系列流行的 WordPress 插件，具有占用空间小、自动安全更新、SSL 支持和 Web 管理界面等特点。提供 ISO、虚拟机镜像格式，或可在云端直接启动。

### 在 Ubuntu Server 16.04 LTS 上无人值守/自动化安装 WordPress

## 使用一个数据库的两个 WordPress 安装

**注意：** 如果您计划进行数据库开发，则**不推荐**使用此方法。

运行您实时站点的本地副本的一个流行方法是使用相同的本地和实时数据库。使用相同的数据库将允许您在本地副本上工作，并将更改从本地推送到生产环境，而不会中断正常运行时间。

**本地副本的设置**

一旦您设置好本地文件，您必须修改本地安装根目录中的 wp-config.php 文件。

```
define('WP_HOME',  "https://{$_SERVER['HTTP_HOST']}");
define('WP_SITEURL', "https://{$_SERVER['HTTP_HOST']}");

ob_start( 'ob_replace_home_url' );
function ob_replace_home_url( $content ) {
    $home_urls = array(
        'https://site.testing.example.com',
        'https://site.example.com',
        'https://site.authoring.testing.example.com',
        'https://site.authoring.example.com',
    );

    $content = str_replace( $home_urls, WP_HOME, $content );

    return $content;
}
```

### Using a Drop-In

What if we don’t want to hack core code? Avoiding changes to core code is a good practice for easy upgrading and code-sharing. There is even a filter for this (`pre_option_siteurl` and `pre_option_home`) but there’s a problem: within **wp-settings.php**,

- the filter can’t be defined until after line 65 when `functions.php` is included
- WordPress makes calls to `get_option` on line 155 of (via `wp_plugin_directory_constants()`)
- plugins aren’t defined until later down around line 194.

However, between lines 65 and 155, there is something we can use, namely the loading of the drop-in `db.php`; the filter can be safely defined there. (However, this is perhaps only halfway towards “not core” code.) Check if you already have an existing wp-content/db.php before trying this technique. Plugins like W3 Total Cache use it for similar reasons.

```
<?php
// paste this in a (new) file, wp-content/db.php
add_filter ( 'pre_option_home', 'test_localhosts' );
add_filter ( 'pre_option_siteurl', 'test_localhosts' );
function test_localhosts( ) {
  if (... same logic as before to see if on dev site ...) {
     return "https://my.example.com/dev";
  }
  else return false; // act as normal; will pull main site info from db
}
```