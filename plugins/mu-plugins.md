---
title: "必用插件"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Plugins
    - Repos
    - Data
---

# 必用插件

必用插件（又称 mu-plugins）是安装在内容文件夹内特殊目录中的插件，它们会在安装的所有站点上自动启用。必用插件不会显示在 wp-admin 插件页面的默认插件列表中（尽管它们会出现在一个特殊的必用插件部分），并且无法禁用，除非从必用插件目录中删除插件文件，该目录默认位于 **wp-content/mu-plugins**。对于网络主机而言，mu-plugins 通常用于添加对主机特定功能的支持，尤其是那些缺失可能导致网站崩溃的功能。

要手动更改默认目录，请在 [wp-config.php](https://wordpress.org/documentation/article/editing-wp-config-php/) 中定义 `WPMU_PLUGIN_DIR` 和 `WPMU_PLUGIN_URL`。

## 功能特性
* 始终启用，无需管理员手动开启，用户也无法意外禁用。
* 只需将文件上传至 mu-plugins 目录即可启用，无需登录操作。
* 由 PHP 按字母顺序在常规插件之前加载，这意味着 mu-plugin 中添加的 API 钩子将应用于所有其他插件，即使它们在全局命名空间中运行钩子函数。

## Caveats
Despite its suitability for many special cases, the mu-plugins system is not always ideal and has several downsides that make it inappropriate in certain circumstances. Below are several important caveats to keep in mind:

* Plugins in the must-use directory will not appear in the update notifications nor show their update status on the plugins page, so you are responsible for learning about and performing updates on your own.
* Activation hooks are not executed in plugins added to the must-use plugins folder. These hooks are used by many plugins to run installation code that sets up the plugin initially and/or uninstall code that cleans up when the plugin is deleted. Plugins depending on these hooks may not function in the mu-plugins folder, and as such all plugins should be carefully tested specifically in the mu-plugins directory before being deployed to a live site.
* WordPress only looks for PHP files right inside the mu-plugins directory, and (unlike for normal plugins) not for files in subdirectories. You may want to create a proxy PHP loader file inside the mu-plugins directory:

```
<?php // mu-plugins/load.php
require WPMU_PLUGIN_DIR.'/my-plugin/my-plugin.php';
```

## 历史与命名

_mu-plugins_ 目录最初由 WPMU（多用户版）实现，旨在为站点管理员提供一种简便方式，默认在农场中的所有博客上激活插件。当时需要此功能，因为多用户专用代码无法通过站点管理区域实现此效果（如今更名为“WordPress 多站点”的功能已支持在管理界面内管理插件）。

处理 /mu-plugins/ 的代码于 2009 年 3 月 7 日通过[此变更集](https://core.trac.wordpress.org/changeset/10737)合并到 WordPress 主代码库中，比 wpmu 代码库首次合并早了整整 10 个月。此后所有 WordPress 站点均可利用自动加载插件功能，无论是否启用了 MU/多站点。该功能在不同情况下对所有类型的 WordPress 安装都有用，因此此举合乎逻辑。

在此过程中，“mu plugins”这一名称变得名不副实，因为它不仅适用于多站点安装，且“MU”已不再用于指代多博客的 WordPress 安装。尽管如此，该名称得以保留，并**被重新解释为“必须使用的插件”**，即这些插件必须始终使用，因此无论 wp-admin 插件面板中的设置如何，它们都会在所有站点上自动加载。

因此，“Must-Use”实际上是一个[逆向首字母缩写](https://en.wikipedia.org/wiki/Backronym)，类似于 [PHP](https://wordpress.org/documentation/article/wordpress-glossary/#PHP)（最初意为“个人主页”，后来被重新解释为“PHP 超文本预处理器”，这也是一个[递归首字母缩写](https://en.wikipedia.org/wiki/Recursive_acronym)）。

## 源代码
* `get_mu_plugins()` 函数位于 [wp-admin/includes/plugin.php](https://core.trac.wordpress.org/browser/tags/4.5.3/src/wp-admin/includes/plugin.php#L0)。
* `wp_get_mu_plugins()` 函数位于 [wp-includes/load.php](https://core.trac.wordpress.org/browser/tags/4.5.3/src/wp-includes/load.php#L0)。