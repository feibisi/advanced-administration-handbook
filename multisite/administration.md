---
title: "多站点网络管理"
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

# 多站点网络管理

在[创建多站点网络](https://developer.wordpress.org/advanced-administration/multisite/create-network/)后，由于多站点的额外复杂性，您可能需要了解一些高级管理知识。即使您熟悉 WordPress，多站点网络管理的结构和行为起初也可能令人困惑。

## 用户访问与权限 {#user-access-capabilities}

默认情况下，添加到您网络的所有用户都将拥有对**所有站点**的*订阅者*访问权限。若要在各个站点为用户分配不同的默认角色，您需要使用插件。

在 WordPress 多站点网络中，站点管理员角色的权限也有所缩减。站点管理员无法安装新主题或插件，也无法编辑其站点上的用户个人资料。只有网络管理员（又称超级管理员）才具备在 WordPress 多站点网络中执行这些任务的权限。

## 子文件夹安装中的固定链接 {#permalinks-in-subfolder-installs}

虽然固定链接仍能正常工作，但主站点（即第一个创建的站点）会多出一个 `blog` 条目，使你的网址显示为 `domain.com/blog/YYYY/MM/POSTNAME`。

这是有意设计的，旨在防止与子文件夹安装发生冲突。目前没有简便的方法来更改此设置，因为这样做会阻止 WordPress 自动检测主站点与任何子站点之间的冲突。此问题将在未来版本的 WordPress 中得到解决并支持自定义。

还需注意，`blog` 前缀不用于静态页面，静态页面可直接通过基础地址访问，例如 `domain.com/PAGENAME`。如果你尝试在第一个站点中创建一个与网络中现有站点同名的静态页面，该页面的固定链接将获得后缀（例如 `domain.com/PAGENAME-2`）。如果你使用现有静态页面的别名创建新站点，该静态页面将无法再访问。为防止这种情况，你可以将静态页面的名称添加到黑名单中，从而阻止创建具有该名称的站点。

## 上传文件路径 {#uploaded-file-path}

全新安装后，您的第一个站点上传文件将存储在传统位置 `/wp-content/uploads/`，但网络中的所有*后续*站点将位于 `/wp-content/uploads/sites/` 文件夹中，每个站点根据数据库指定的站点编号拥有自己的子文件夹。这些文件可通过该 URL 访问。

这与 Multisite 3.0-3.4.2 版本不同，当时子站点的图片存储在 `/wp-content/blogs.dir/` 中，并通过 https://example.com/files/ 和 https://example.com/sitename/files 等 URL 显示。如果您使用的 Multisite 安装版本早于 3.5，即使图片显示为 `/files/` 的 URL，也*不是*错误。

无论 WordPress 版本如何，站点管理员都无法更改这些位置。只有网络管理员可以在站点设置页面进行更改。不建议在不理解 `ms-files.php` 如何与您的 `.htaccess` 协同工作的情况下更改这些设置，否则很容易导致功能失效。如果 `/files/` 的 URL 无法工作，通常表明服务器上的 `.htaccess` 或 `httpd.conf` 文件配置有误。

## 插件 {#plugins}

插件现在根据其在网络中的实现方式拥有额外的灵活性。所有插件都安装在网络仪表盘的插件页面，可以按站点或整个网络激活。

* **站点特定插件：** 这些插件在单个特定站点的插件页面内激活。某些插件（例如联系表单）在单站点激活时效果最佳，这样它们可以将数据和设置存储在该单个站点的数据库表中，而不是整个网络的表中。要单站点激活/停用的 WordPress 插件存储在插件目录中。
* **网络插件：** 网络管理员可以在网络管理仪表盘中“网络激活”插件。一旦“网络激活”，插件将在所有站点中变为活动状态。“网络激活”的插件在单个站点的仪表盘插件列表中会显示为“网络活动”。某些插件仅在网络激活时才能在多站点环境中运行。网络激活的 WordPress 插件也存储在插件目录中。
* **必须使用插件：** 整个网络中所有站点都要使用的插件也可以作为单个文件或包含子文件夹的文件安装在 mu-plugins 目录中。文件夹内的任何文件都不会被读取。这些文件不会被激活或停用；如果存在，就会被使用。这些插件完全不会出现在按站点的插件列表中。

并非存储库中的所有插件都能在多站点环境中工作。请查阅插件的存储库页面或联系开发者，以了解特定插件是否能在多站点网络中运行。

如果您希望单站点管理员能够为其站点激活/停用站点特定插件，您需要从网络管理的设置 -> 网络设置菜单（“菜单设置”）中为单站点管理员启用插件页面。网络管理员始终可以访问每个站点的插件。单站点的管理员将能够激活和停用未网络激活的插件，但会看到网络激活的插件显示为“网络活动”，且没有停用或设置的选项。

有一些插件可以帮助批量激活/停用单个站点的插件。

## 主题 {#themes}

所有主题均为整个网络安装。如果您编辑某个主题的代码，即为所有使用该主题的站点进行编辑。您可以安装插件 [WP Add Custom CSS](https://wordpress.org/plugins/wp-add-custom-css/) 来允许每个站点调整自己的 CSS，而不影响其他站点。此外，单个站点可以使用主题定制器，其设置将仅存储在其站点的数据库表中。

“网络激活”一个主题并不会使其在每个站点上成为活动主题，而仅仅是使其可在所有单个站点上被激活。要在单个站点的仪表盘中可供激活，主题必须在网络管理中通过“网络激活”或在“编辑站点 - 主题”选项卡中启用。在单个站点中激活主题后，可以网络停用它，而不会影响其保持激活状态的单个站点。

默认情况下，WordPress 将最新的“Twenty ...”主题分配给所有新站点。可以通过在 `wp-config.php` 文件中添加类似 `define('WP_DEFAULT_THEME', 'classic');` 的行来自定义此设置，其中“classic”替换为您主题的文件夹名称。

## 分类与标签 {#categories-and-tags}

WordPress 3.0 不支持全局术语（即在网络中的站点间共享标签和分类）。您可以使用插件在网站门户/首页或网络内的特定页面或站点上整合全局标签，以增强基于微分类内容的导航。

## 站点间内容共享 {#content-sharing-between-sites}

网络中的站点是独立的，默认不共享内容。可以将您的网络视为 WordPress.com 的迷你版。有几款插件可以帮助您在站点间共享内容，[例如这款](https://wordpress.org/plugins/network-posts-extended/)。

## 切换网络类型 {#switching-network-types}

可以在基于域名（子域名）和基于路径（子目录）的多站点安装类型之间进行切换。如果您在单站点上安装 WordPress 已超过一个月，并尝试将该站点激活为网络，系统会提示您使用**子域名**站点。这是为了确保页面（例如 example.com/pagename）和站点（例如 example.com/sitename）之间不会发生冲突。如果您确信不会出现此问题，则可以在完成初始设置后进行更改。

在您的 `wp-config.php` 文件中，需要更改 `SUBDOMAIN_INSTALL` 的定义调用：
对于基于域名的网络（子域名安装）

```
define( 'SUBDOMAIN_INSTALL', true );
```

对于基于路径的网络（子目录安装）

```
define( 'SUBDOMAIN_INSTALL', false );
```

您还需要将 `.htaccess` 文件更改为新的设置。可以转到网络管理员 — 设置 — 网络设置以查找新的 `.htaccess` 规则，或参见下文。

请注意，根据[设置要求](https://developer.wordpress.org/advanced-administration/multisite/prepare-network/#WordPress_Settings_Requirements)，在 `127.0.0.1` 或 `localhost` 上运行时，无法从**子目录**切换到**子域名**。这可能会由于 Cookie 处理导致根站点出现 reauth=1 的无限循环。

## Apache 虚拟主机与 Mod Rewrite {#apache-virtual-hosts-and-mod-rewrite}

要使 mod_rewrite 在 Apache 虚拟主机中正常工作，您可能需要在 DocumentRoot 上设置一些选项。

```
<VirtualHost *:80>
	...
	DocumentRoot /var/www/vhosts/wordpress
	<Directory /var/www/vhosts/wordpress>
		AllowOverride Fileinfo Options
	</Directory>
	...
</VirtualHost>
```

在某些情况下，您需要将 AllowOverride 设置为 All，以确保所有 htaccess 规则生效。

## .htaccess 与 Mod Rewrite {#htaccess-and-mod-rewrite}

与单站点 WordPress 不同，后者可以使用"不美观"的[固定链接](https://wordpress.org/documentation/article/customize-permalinks/)工作，因此不需要 Mod Rewrite，而多站点网络*必须*使用它来格式化子站点的 URL。这就需要使用 .htaccess 文件，如果您使用子文件夹或子域名，其格式会略有不同。以下示例是当 WordPress 安装在网站根目录时，用于 WordPress 子文件夹和子域名的标准 .htaccess 条目。如果 WordPress 安装在它自己的文件夹中，您需要相应地更改 RewriteBase 的值。

提醒一下，这些是**示例**，适用于大多数安装，但并非全部。

**子文件夹示例**

WordPress 3.0 至 3.4+

```
# BEGIN WordPress
RewriteEngine On
RewriteBase /
RewriteRule ^index.php$ - [L]

# 上传的文件
RewriteRule ^([_0-9a-zA-Z-]+/)?files/(.+) wp-includes/ms-files.php?file=$2 [L]

# 为 /wp-admin 添加尾部斜杠
RewriteRule ^([_0-9a-zA-Z-]+/)?wp-admin$ $1wp-admin/ [R=301,L]

RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^ - [L]
RewriteRule ^[_0-9a-zA-Z-]+/(wp-(content|admin|includes).*) $1 [L]
RewriteRule ^[_0-9a-zA-Z-]+/(.*\.php)$ $1 [L]
RewriteRule . index.php [L]
# END WordPress
```

WordPress 3.5+ *仅当您在 3.5 版本上开始使用多站点网络时使用此规则。如果您是从 3.4 升级到 3.5，请使用旧的规则！*

```
# BEGIN WordPress
RewriteEngine On
RewriteBase /
RewriteRule ^index.php$ - [L]

# 为 /wp-admin 添加尾部斜杠
RewriteRule ^([_0-9a-zA-Z-]+/)?wp-admin$ $1wp-admin/ [R=301,L]

RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^ - [L]
RewriteRule ^([_0-9a-zA-Z-]+/)?(wp-(content|admin|includes).*) $2 [L]
RewriteRule ^([_0-9a-zA-Z-]+/)?(.*\.php)$ $2 [L]
RewriteRule . index.php [L]
# END WordPress
```

**子域名示例**

WordPress 3.0 至 3.4+

```
# BEGIN WordPress
RewriteEngine On
RewriteBase /
RewriteRule ^index.php$ - [L]

# 上传的文件
RewriteRule ^files/(.+) wp-includes/ms-files.php?file=$1 [L]

RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^ - [L]
RewriteRule . index.php [L]
# END WordPress
```

WordPress 3.5+

```
# BEGIN WordPress
RewriteEngine On
RewriteBase /
RewriteRule ^index.php$ - [L]

# 为 /wp-admin 添加尾部斜杠
RewriteRule ^wp-admin$ wp-admin/ [R=301,L]

RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^ - [L]
RewriteRule ^(wp-(content|admin|includes).*) $1 [L]
RewriteRule ^(.*\.php)$ wp/$1 [L]
RewriteRule . index.php [L]
# END WordPress
```

**旧版 WPMU 安装的问题**

如果您将 WordPress MU 安装在子文件夹/子目录中（不是通过 ftp 安装在服务器的根目录），并且遇到图片库问题，缩略图和图片不显示，您可能需要手动为您的文件目录添加重写规则，如下所示：

```
RewriteRule ^([_0-9a-zA-Z-]+/)?siteN/files/(.+) wp-content/blogs.dir/N/files/$2 [L]
```

将这些规则放在常规上传文件调用的 _下方_。

## 网络管理员链接位置 {#network-admin-link-location}

网络管理员链接的位置随每个 WordPress 主要版本而变动，因为此功能仍在完善中。根据您使用的 WordPress 版本，该链接可在以下位置找到：

* 3.0 – 名为 _超级管理员_ 的菜单
* 3.1 – 在管理页头 "您好，YOURNAME" 旁
* 3.2 – 在管理页头，作为 "您好，YOURNAME" 下的下拉菜单
* 3.3+ – 在管理工具栏，作为 "我的站点" 下的下拉菜单

## 域名映射 {#domain-mapping}

在 WordPress 4.5 之前，域名映射需要插件支持。自 WordPress 4.5 起，域名映射成为多站点网络的原生功能。请访问 [WordPress 多站点域名映射](https://developer.wordpress.org/advanced-administration/multisite/domain-mapping/) 了解如何使用此功能。

## 迁移多站点 {#moving-multisite}

迁移多站点比迁移单站点安装更为复杂。在继续操作前，请先阅读[迁移 WordPress 多站点](https://developer.wordpress.org/advanced-administration/upgrade/migrating/#moving-wordpress-multisite)。

## 导入到网络 {#importing-into-a-network}

当你创建了用于导入其他站点的 WordPress 网络后，需要查阅 [将多个博客迁移到 WordPress 多站点](https://wordpress.org/documentation/article/migrating-multiple-blogs-into-wordpress-multisite/) 这篇文章。