---
title: "常见的 WordPress 错误"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Wordpress
    - Repos
    - Data
---

# 常见的 WordPress 错误

如果你遇到 WordPress 错误信息或白屏，请不要惊慌。很可能之前已经有人遇到过相同的问题，并且可以轻松解决。

本页面列出了 WordPress 用户最常遇到的错误，并提供了修复它们的起点。在 [WordPress 支持](https://wordpress.org/documentation/)页面，你还可以找到指向更详细页面或论坛的链接，那里会有志愿者提供帮助。

## 白屏死机 {#the-white-screen-of-death}

PHP 错误和数据库错误都可能表现为白屏，即没有任何信息的空白屏幕，这在 WordPress 社区中通常被称为 _WordPress 白屏死机_ (WSOD)。

在采取极端措施之前，WordPress 白屏死机有多种原因：

* **插件导致兼容性问题**。如果您可以访问[管理屏幕](https://wordpress.org/documentation/article/administration-screens/)，请尝试停用所有插件，然后逐个重新激活。如果无法访问管理屏幕，请通过 [FTP](https://developer.wordpress.org/advanced-administration/upgrade/ftp/) 登录您的网站。找到 `wp-content/plugins` 文件夹，并将插件文件夹重命名为 `plugins_old`。这将停用所有插件。您可以在[故障排除常见问题解答](https://wordpress.org/documentation/article/faq-troubleshooting/#how-to-deactivate-all-plugins-when-not-able-to-access-the-administrative-menus)中阅读更多关于手动停用插件的信息。
* **您的主题可能导致问题**。如果您在刚刚激活新主题或在 WordPress 网络中创建新站点后遇到白屏死机，这种情况尤其可能发生。登录 WordPress 管理屏幕并激活默认的 WordPress 主题（例如 Twenty Twenty-One）。如果您使用的是 WordPress 5.8 及以下版本，请切换到 Twenty Twenty-One 主题，因为 Twenty Twenty-Two 主题需要 5.9 及以上版本。如果无法访问管理屏幕，请通过 FTP 访问您的网站并导航到 `/wp-content/themes/` 文件夹。重命名当前活动主题的文件夹。

[WP_DEBUG 功能](https://developer.wordpress.org/advanced-administration/debug/debug-wordpress/#wp_debug)通常能提供额外信息。

## 内部服务器错误 {#internal-server-error}

[![](https://wordpress.org/documentation/files/2018/11/internalservererror2.jpg)](https://wordpress.org/documentation/files/2018/11/internalservererror2.jpg)
内部服务器错误消息

导致内部服务器错误的原因有很多。以下是一些可以尝试的解决方法：

* 最可能的问题是 `.htaccess` 文件损坏。使用 FTP 登录到您的网站根目录，将 `.htaccess` 文件重命名为 `.htaccess_old`。然后尝试加载您的网站，看看问题是否解决。如果有效，请务必访问 [设置](https://wordpress.org/documentation/article/administration-screens/#settings-configuration-settings) > [固定链接](https://wordpress.org/documentation/article/administration-screens/#permalinks) 并重置您的 [固定链接](https://wordpress.org/documentation/article/using-permalinks/)。这将为您生成一个新的 `.htaccess` 文件。
* 尝试停用所有插件，以检查是否是插件问题。如果您无法访问 WordPress 管理后台，请按照 [这些说明](https://wordpress.org/documentation/article/faq-troubleshooting/#how-to-deactivate-all-plugins-when-not-able-to-access-the-administrative-menus) 通过 FTP 停用插件。
* 将主题切换到 WordPress 默认主题（例如 Twenty Twenty-One），以排除主题相关的问题。如果您使用的是 WordPress 5.8 及以下版本，请切换到 Twenty Twenty-One 主题，因为 Twenty Twenty-Two 主题需要 5.9 及以上版本。
* 增加 [PHP 内存限制](https://wordpress.org/documentation/article/editing-wp-config-php/#increasing-memory-allocated-to-php)
* 尝试从 [全新安装的 WordPress](https://wordpress.org/download/) 重新上传 `wp-admin` 和 `wp-includes` 文件夹。

## 建立数据库连接时出错 {#error-establishing-database-connection}

如果您看到页面显示“建立数据库连接时出错”的消息，这意味着连接到您的数据库时出现了问题，可能有多种原因。以下是可能的原因和解决方案。

### 错误的 wp-config.php 信息 {#incorrect-wp-config-php-information}

"建立数据库连接时出错"通常是由您的 [wp-config.php](https://wordpress.org/documentation/article/editing-wp-config-php/) 文件中的错误引起的。通过 FTP 客户端访问您的站点。打开 `wp-config.php` 并确保以下信息正确：

* 数据库名称
* 数据库用户名
* 数据库密码
* 数据库主机

了解更多关于 [编辑 wp-config.php](https://wordpress.org/documentation/article/editing-wp-config-php/) 的信息。

如果您确定配置正确，可以 [尝试手动重置 MySQL 密码](https://developer.wordpress.org/advanced-administration/before-install/howto-install/#common-installation-problems)。

### 您的网络主机问题 {#problems-with-your-web-host}

下一步是联系您的网络主机。以下主机问题可能导致此问题：

* 您的数据库已达到配额并已关闭。
* 服务器已关闭。

联系您的主机提供商，查看是否是这些问题导致了您的问题。

### 网站被入侵 {#compromised-website}

如果您已检查 `wp-config.php` 文件是否存在错误，并已向主机提供商确认无托管问题，那么您的网站可能已被黑客入侵。

请使用 [Sucuri SiteCheck](https://sitecheck.sucuri.net/) 扫描您的网站，以确保其未被入侵。如果确实被入侵，您应该查阅 [我的网站被黑了](https://wordpress.org/documentation/article/faq-my-site-was-hacked/)。

## 自动升级失败 {#failed-auto-upgrade}

在某些情况下，WordPress 自动更新功能可能会失败。常见症状包括：

* 出现空白白屏且无任何信息。
* 提示更新失败的警告。
* 显示 PHP 错误信息。

WordPress 自动升级功能失败的原因可能是：与主 WordPress 文件连接时出现故障、升级过程中网络连接问题，或是 [文件权限](https://developer.wordpress.org/advanced-administration/server/file-permissions/) 设置不正确。

如需手动更新 WordPress 站点，请参阅 [手动更新文章](https://wordpress.org/documentation/article/updating-wordpress/#manual-update)。

## 连接超时 {#connection-timed-out}

当您的网站尝试执行超出服务器处理能力的事务时，会出现连接超时错误。这在内存限制严格的共享主机上尤为常见。您可以尝试以下方法：

* **停用所有插件。** 如果停用您站点上的所有 WordPress 插件能解决问题，请逐个重新激活它们以找出导致问题的插件。如果您无法访问管理后台，[请阅读如何手动停用插件](https://wordpress.org/documentation/article/faq-troubleshooting/#how-to-deactivate-all-plugins-when-not-able-to-access-the-administrative-menus)。
* **切换到默认 WordPress 主题。** 如果您使用的是 WordPress 5.8 及以下版本，请切换到 Twenty Twenty-One 主题，因为 Twenty Twenty-Two 主题需要 5.9 及以上版本。这应能排除与主题相关的问题。
* **增加 [wp-config.php 中的内存限制](https://wordpress.org/documentation/article/editing-wp-config-php/#increasing-memory-allocated-to-php)。** 如果您使用的是共享主机，可能需要联系您的主机提供商为您增加内存限制。
* **增加 [php.ini](https://php.net/manual/en/ini.core.php) 文件中的最大执行时间。** 这不是 WordPress 核心文件，因此如果您不确定如何编辑，请联系您的主机提供商，请求他们增加最大执行时间。请参阅下方关于增加最大执行时间的说明。

## 升级后的维护模式 {#maintenance-mode-following-upgrade}

[![](https://wordpress.org/documentation/files/2018/11/maintenancemode1.jpg)](https://wordpress.org/documentation/files/2018/11/maintenancemode1.jpg)

当 WordPress 更新时，它会自动安装一个 `.maintenance` 文件。升级后，您可能会看到一条消息，提示“正在进行计划维护，暂时无法访问。请稍后再试。” 这可能是因为维护文件未被正确移除。

要清除此消息，请执行以下操作：

1.  使用您的 FTP 程序登录到您的网站
2.  删除位于您网站根目录下的 `.maintenance` 文件。

阅读更多关于[维护模式问题](https://wordpress.org/documentation/article/faq-troubleshooting/#how-to-clear-the-briefly-unavailable-for-scheduled-maintenance-message-after-doing-automatic-upgrade)的信息。

## 你做了更改但没有任何变化 {#you-make-changes-and-nothing-happens}

如果你对网站进行了修改，但在浏览器中看不到变化，可能需要清除[浏览器缓存](https://wordpress.org/support/topic/i-make-changes-and-nothing-happens/)。浏览器会存储你访问过的网站信息，这样再次访问时加载速度更快，因为浏览器只需重新加载已存储在电脑上的信息，而无需重新下载。

如果你对网站做了更改，但浏览器认为改动不重要，它就会直接从缓存加载数据，导致你看不到变化。要解决这个问题，只需[清空浏览器缓存](https://wordpress.org/support/topic/i-make-changes-and-nothing-happens/)或关闭标签页后重新打开链接。

## 美化固定链接 404 错误与图片无法显示问题 {#pretty-permalinks-404-and-images-not-working}

如果您在使用美化[固定链接](https://wordpress.org/documentation/article/using-permalinks/)时遇到 404 错误，并且上传图片时出现白屏，可能是 Apache 默认未启用 [mod_rewrite](https://wordpress.org/documentation/article/glossary#mod_rewrite)。Mod_rewrite 是 [Apache 网页服务器](https://wordpress.org/documentation/article/glossary#apache)软件的扩展模块，能够实时“重写”[URL](https://en.wikipedia.org/wiki/Url)。这是实现美化固定链接功能所必需的。

[WordPress 多站点](https://wordpress.org/documentation/article/glossary#multisite)网络通常会遇到此问题，但在共享主机提供商处或[网站迁移或服务器搬迁](https://wordpress.org/documentation/article/moving-wordpress/)后也可能发生。

请通过 **设置 > 固定链接** 重置您的固定链接。如果这不起作用，您可能需要手动编辑 `.htaccess` 文件。

```
# BEGIN WordPress  
<IfModule mod\_rewrite.c>  
RewriteEngine On  
RewriteBase /  
RewriteRule ^index\.php$ - [L]  
RewriteCond %{REQUEST_FILENAME} !-f  
RewriteCond %{REQUEST_FILENAME} !-d  
RewriteRule . /index.php [L]  
</IfModule>  
# END WordPress
```

如果您不熟悉如何编辑 `.htaccess` 文件，请联系您的主机提供商，请求他们启用 mod_rewrite 规则。更多关于[美化固定链接的信息可在 WordPress Codex 中找到](https://wordpress.org/documentation/article/using-permalinks/#using-pretty-permalinks)。

## 自定义文章类型 404 错误 {#custom-post-type-404-errors}

您可能会遇到 404 错误和[自定义文章类型](https://wordpress.org/documentation/article/post-types/#custom-types)相关的问题。请尝试以下步骤：

1.  确保您的自定义文章类型与单页面的名称没有重复。如果有，请重命名单页面，包括其[别名](https://wordpress.org/documentation/article/glossary/#post-slug)。
2.  登录 WordPress 管理后台，导航至 **设置 > 固定链接**。选择默认固定链接并保存。然后重新选择您偏好的固定链接。这将刷新重写规则，通常可以解决问题。

## 关键错误消息 {#critical-error-message}

如果您看到消息“此网站上发生了一个关键错误。请检查您的站点管理员电子邮件收件箱以获取说明。”，这表明您网站中的某些内容导致了关键错误，阻止了 WordPress 按预期运行。

首先，请检查您的电子邮件，查看 WordPress 是否已发送有关该错误的任何详细信息。接下来，在您的 [wp-config.php](https://developer.wordpress.org/advanced-administration/debug/debug-wordpress/#example-wp-config-php-for-debugging) 文件中启用调试模式。

启用后，请查看位于 wp-content/debug.log 的错误日志，以找出具体出了什么问题。

您可能发现的一些最常见原因包括：插件之间的冲突、与您的主题的兼容性问题、运行不兼容的 PHP 版本、达到内存限制或 WordPress 文件损坏。

## 特定错误信息 {#specific-error-messages}

您的错误日志中可能会出现多种不同的错误。要访问错误日志，您需要启用[调试模式](https://wordpress.org/documentation/article/editing-wp-config-php/#wp_debug)，然后通过 FTP 找到错误日志文件。以下信息将帮助您解读一些常见的错误信息。

### PHP 错误 {#php-errors}

以下是一些常见的 PHP 错误信息。

#### 致命错误与警告 {#fatal-errors-and-warnings}

##### 无法修改头部信息 – 头部信息已发送

如果您收到 WordPress 无法修改头部信息且头部信息已发送的警告，通常意味着您在开始标签之前或结束标签之后存在空格或字符。请阅读如何[修复头部信息已发送错误](https://wordpress.org/documentation/article/faq-troubleshooting/#how-do-i-solve-the-headers-already-sent-warning-problem)。

如果您在刚安装 WordPress 时遇到此问题，可能是 `wp-config.php` 文件中引入了语法错误。[这些说明将帮助您修复错误](https://developer.wordpress.org/advanced-administration/before-install/howto-install/#common-installation-problems)。

##### 调用未定义函数

出现“调用未定义函数”错误可能意味着某个 WordPress 插件正在尝试查找代码中不存在或无法访问的文件或数据。原因包括：

* 尝试自动安装或自动升级插件时出错。请尝试[手动安装或升级插件](https://wordpress.org/documentation/article/managing-plugins/#manual-plugin-installation)。
* 尝试自动安装或自动升级主题时出错。请尝试[手动安装或升级主题](https://wordpress.org/documentation/article/using-themes/#adding-new-themes)。
* 您可能正在使用[不兼容的 WordPress 插件](https://wordpress.org/documentation/article/managing-plugins/#plugin-compatibility)或不兼容的主题。这可能在旧版 WordPress 与新插件同时使用时发生，或尝试在单站点安装中使用 WordPress 多站点插件。升级 WordPress 以解决此问题。
* 您可能正在尝试调用不存在的函数。请检查 `functions.php` 中是否存在拼写错误。

尝试停用导致错误出现的 WordPress 插件或更换 WordPress 主题。如果无法通过管理界面操作，您可能需要[通过 FTP 手动处理](https://wordpress.org/documentation/article/faq-troubleshooting/#how-to-deactivate-all-plugins-when-not-able-to-access-the-administrative-menus)。

##### 允许的内存大小已耗尽

允许的内存大小已耗尽错误意味着您的 WordPress 安装没有足够的内存来完成您想要的操作。您可以尝试以下步骤：

* 在 [wp-config.php 中增加内存限制](https://wordpress.org/documentation/article/editing-wp-config-php/#increasing-memory-allocated-to-php)
* 通过编辑 `php.ini` 来增加内存限制。这不是 WordPress 自带的文件，因此如果您不熟悉它，应联系您的主机提供商来增加内存限制。

##### 超出最大执行时间

您可能会收到类似"超出 30 秒最大执行时间"或"超出 60 秒最大执行时间"的消息。这意味着某个进程完成所需时间过长，导致超时。有多种方法可以修复此错误。

**编辑 `.htaccess` 文件**

**编辑前请务必备份 `.htaccess` 文件。**

在 `.htaccess` 文件中添加以下行：

```
php_value max_execution_time 60
```

**编辑 `php.ini` 文件**

在 `php.ini` 文件中添加以下内容：

```
max_execution_time = 60
```

如果您不确定如何进行这些更改，或者您使用的是禁止自行修改的共享主机，则应联系您的主机提供商，请求他们增加最大执行时间。

#### 解析错误 {#parse-errors}

##### 语法错误

语法错误意味着您在创建 PHP 结构时出现了错误。例如，您可能：

* 在单行末尾缺少 `;`。
* 使用了花引号。
* 缺少花括号。

当此错误出现时，它会告诉您错误出现在哪个文件（例如 `functions.php`）以及代码中大约哪一行（可能不总是精确的行，因此请务必检查该行前后）。

##### 意外错误

如果你收到一个显示“解析错误：意外”的错误信息，这通常意味着你忘记包含某个字符。最常见的情况有：

* **意外的 '='**：引用变量时忘记包含 $ 符号
* **意外的 ')'**：忘记包含开括号 (
* **意外的 '('**：忘记包含闭括号 )
* **意外的 T_STRING**：忘记在前一行末尾添加引号或分号
* **意外的 T_ELSE**：存在 else 语句但没有对应的 if 语句

#### 使用未定义的常量 {#use-of-an-undefined-constant}

与解析错误类似，“使用未定义的常量”意味着您缺少某个字符。可能是以下情况之一：

* 引用变量时缺少 `$` 符号
* 数组键周围缺少引号

### 数据库错误 {#database-errors}

以下错误可能与您的 WordPress 数据库有关。

#### 错误 13 – 无法创建/写入文件 {#error-13-cannot-create-write-to-file}

出现此错误可能有多种原因。

**MySQL 无法创建临时文件。**

当使用 PHP 访问 MySQL 时，MySQL 变量 `tmpdir` 被设置为一个无法写入的目录。要验证这一点，请在命令行中输入 MySQL 并键入 `show variables`。您将看到一个很长的列表，其中一项会显示：**tmpdir = /某个目录/**（无论您的设置是什么）。

要解决此问题，请修改 **tmpdir** 变量，使其指向一个可写目录。

1.  找到 **my.cnf** 文件。在 *nix 系统上，该文件通常位于 **/etc/** 中。在 Windows 系统上，找到 **my.ini** 文件。
2.  找到后，用简单的文本编辑器打开它，并找到 **[mysqld]** 部分。
3.  在此部分下，找到 **tmpdir** 行。如果该行被注释掉了（开头有 **#**），请删除 **#** 并编辑该行，使其显示为：**tmpdir = /可写目录**，其中 **/可写目录** 是您可以写入的目录。有些人使用 **/tmp**，您也可以尝试 **/var/tmp** 或 **/usr/tmp**。在 Windows 上，使用 **C:/Windows/tmp**。
4.  保存文件。
5.  通过键入 `mysqlshutdown -u -p shutdown` 来关闭 MySQL。
6.  转到 MySQL 目录并键入 `./bin/safe_mysqld &` 来启动 MySQL。在 Linux 系统上，MySQL 目录通常在 **/usr/local** 中，有时在 **/usr/** 中。

**[文件权限](https://developer.wordpress.org/advanced-administration/server/file-permissions/) 不正确**

请更正[文件权限](https://developer.wordpress.org/advanced-administration/server/file-permissions/)。

如果以上内容您都不理解，并且有人为您管理系统，请将上述内容展示给他们，他们应该能够解决。

#### 用户被拒绝执行 CREATE 命令 {#create-command-denied-to-user}

当分配给数据库的用户没有足够的权限在数据库中执行创建列和表的操作时，会发生此错误。您需要登录 [CPanel](https://wordpress.org/documentation/article/using-cpanel/) 或 [Plesk](https://www.plesk.com/) 来授予数据库用户足够的权限。

或者，您可以[创建一个新用户并分配给数据库](https://wordpress.org/documentation/article/using-cpanel/#step-3-add-user-to-database)。如果创建了新用户，则需要确保在 `[wp-config.php](https://wordpress.org/documentation/article/using-cpanel/#editing-the-wordpress-config-file)` 文件中[更新此信息](https://wordpress.org/documentation/article/using-cpanel/#editing-the-wordpress-config-file)。

#### 错误 {#error-28}

可能的原因包括：

* `/tmp` 目录（无论 `tmpdir` 指向何处）空间不足，或
* `/tmp` 目录中文件过多（即使有大量可用空间），或
* 服务器上的缓存已满

这是一个 MySQL 错误，与 WordPress 没有直接关系；您应该就此问题联系您的主机服务商。一些用户报告称，在 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/) 中运行“修复表”命令解决了该问题。

#### 错误 145 {#error-145}

这表明您数据库中的某个表已损坏。如果您熟悉使用 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/)，可以按照[这些关于修复 MySQL 数据库表的说明](https://wordpress.org/documentation/article/faq-troubleshooting-2/#how-do-you-repair-a-mysql-database-table)进行操作。

**在对数据库执行任何操作之前，请务必[备份您的数据库](https://wordpress.org/documentation/article/backing-up-your-database/)。**

如果您以前从未使用过 phpMyAdmin，或对此感到不熟悉，请联系您的主机提供商，请他们对您的数据库运行 CHECK/REPAIR 命令。

#### 未知列 {#unknown-column}

未知列错误可能是由于数据库中缺少列引起的。如果您刚刚升级了 WordPress，请尝试再次手动升级。要手动更新您的 WordPress 站点，请参阅[更新文章](https://wordpress.org/documentation/article/updating-wordpress/#manual-update)。

如果您在运行数据库查询时遇到此错误，则可能是为标识符引号字符使用了不正确的引号。此[Stack Overflow 上的问题提供了更多详细信息](https://stackoverflow.com/questions/1346209/unknown-column-in-field-list-error-on-mysql-update-query)。另请参阅[MySQL 文档](https://dev.mysql.com/doc/refman/8.2/en/identifiers.html)。

## 资源 {#resources}

* [MySQL 错误代码与消息](https://dev.mysql.com/doc/refman/en/error-messages-server.html)