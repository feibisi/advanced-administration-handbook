---
title: "安装您语言版本的 WordPress"
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

# 安装您语言版本的 WordPress

_注意：本文介绍如何将 WordPress 管理后台（"后端"）显示为您的语言。如果您想了解如何本地化您的"前端"网站，或定制主题使其可本地化，请参阅 [WordPress 开发者的国际化](https://codex.wordpress.org/I18n_for_WordPress_Developers)（以及主题开发者可选的 [国际化](https://developer.wordpress.org/themes/functionality/internationalization/) 和 [本地化](https://developer.wordpress.org/themes/functionality/localization/)）。如果您对如何构建多语言（例如：法语/英语）WordPress 站点感兴趣，可以从 [这里](https://developer.wordpress.org/advanced-administration/wordpress/multilingual/) 开始。_

虽然 WordPress 默认显示为美式英语，但它内置了支持任何语言的能力。WordPress 社区已将 WordPress 翻译成多种语言，并且许多其他语言也提供了主题、翻译文件和支持（请参阅 [您语言版本的 WordPress](https://developer.wordpress.org/advanced-administration/wordpress/multilingual/)）。

## 从管理面板安装语言文件

自 4.0 版本起，您可以在安装过程中[让 WordPress 自动安装您选择的语言](https://make.wordpress.org/core/2014/09/05/language-chooser-in-4-0/)。

对于 WordPress 4.1 或更高版本，您可以随时[直接从管理后台安装语言包](https://wplang.org/wordpress-4-1-install-language-packs-dashboard/)。WordPress 将下载它们并将管理后台切换为该语言。请导航至 **设置 > 常规 > 站点语言**，并从可用语言列表中选择。对于多站点超级管理员，您可以使用网络管理 **设置** 面板来设置默认语言。

## 手动安装语言文件

以下是安装 WordPress 国际化版本需要遵循的步骤。

**注意：** 如果步骤出错或未指定正确的语言，WordPress 将默认回退到英文。有关安装 WordPress 的更多帮助，请参阅 [安装 WordPress](https://developer.wordpress.org/advanced-administration/before-install/howto-install/) 和 [常见问题：安装](https://wordpress.org/documentation/article/faq-installation/)。

* 下载您所需语言的 `.mo` 语言文件。`.mo` 文件的命名规则基于 ISO-639 语言代码（例如，葡萄牙语为 _pt_），后跟 ISO-3166 国家/地区代码（例如，葡萄牙为 _PT_，巴西为 _BR_）。因此，巴西葡萄牙语文件将命名为 `pt_BR.mo`，而非特定地区的葡萄牙语文件将命名为 `pt.mo`。完整的代码列表可在 [(国家/地区代码)](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/gettext.html#Country-Codes) 和 [(语言代码)](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/gettext.html#Language-Codes) 找到。

## 设置网站语言

### 单站点安装

#### WordPress 4.0 及以上版本

* 在管理设置界面更改语言。路径为：`设置 > 常规 > 站点语言`。

#### WordPress 3.9.2 及以下版本

* 在[文本编辑器](https://wordpress.org/documentation/article/wordpress-glossary/#Text_editor)中打开您的 `wp-config.php` 文件，并搜索：

```
define( 'WPLANG', '' );
```

* 根据您刚刚下载的 `.mo` 文件编辑此行，例如，对于巴西葡萄牙语，您必须添加：

```
define ( 'WPLANG', 'pt_BR' );
```

* 请注意，如果 wp-config.php 中调用的语言代码没有对应的 .mo 和 .po 文件，则不会出现错误消息，但该代码仍会在 language_attributes() 中使用。这对于那些语言与 en_US 足够相似、无需翻译，但又不想在博客中使用 en-US 作为语言标签，而希望使用其他英语变体的人来说很有用。例如：

```
define ( 'WPLANG', 'en_GB' );
```

* 添加语言代码后，保存文件。

### 多站点安装

如果您拥有[站点网络](https://developer.wordpress.org/advanced-administration/multisite/create-network/)（WordPress 多站点），语言将通过 `设置 > 常规` 子面板中的“站点语言”选项按博客进行设置。

您可以在 `网络管理 > 设置` 界面（“默认语言”）下为整个网络设置默认语言。

### 添加翻译

如果您希望在安装后为仍显示为英文的术语添加翻译，请访问 [translate.wordpress.org](https://translate.wordpress.org) 并选择您的语言。要开始使用，请参阅 [翻译者手册](https://make.wordpress.org/polyglots/handbook/) 中的 [此页面](https://make.wordpress.org/polyglots/handbook/tools/glotpress-translate-wordpress-org/)。