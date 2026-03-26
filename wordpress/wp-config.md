---
title: "编辑 wp-config.php 文件"
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

# 编辑 wp-config.php 文件

WordPress 安装中最重要的文件之一是 `wp-config.php` 文件。该文件位于 WordPress 文件目录的根目录，包含网站的基本配置信息，例如数据库连接信息。

首次下载 WordPress 时，`wp-config.php` 文件并不包含在内。WordPress 安装过程会根据您在[安装](https://developer.wordpress.org/advanced-administration/before-install/howto-install/)过程中提供的信息为您创建 `wp-config.php` 文件。

非开发人员通常不需要编辑 wp-config.php 文件，但如果您正在按照技术人员或网络主机提供的故障排除步骤操作，此[页面](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/)应该会有所帮助。

# wp-config.php

临时说明：这可能链接到简单部分，指向：
* https://developer.wordpress.org/advanced-administration/wordpress/wp-config/
* https://developer.wordpress.org/advanced-administration/debug/debug-wordpress/

## 高级选项 {#advanced-options}

以下部分可能包含高级信息，某些更改可能导致不可预见的问题。在修改这些设置前，请确保您已进行[定期备份](https://developer.wordpress.org/advanced-administration/security/backup/)并了解如何恢复。

### table_prefix {#table-prefix}

**$table_prefix** 是放置在数据库表名称前的值。如果您不想使用 **wp_** 作为数据库表前缀，请更改此值。通常在以下情况会修改此值：在同一数据库中[安装多个 WordPress 站点](https://developer.wordpress.org/advanced-administration/before-install/multiple-instances/)，例如使用多站点功能时。

如果为每个安装设置唯一的前缀，则可以在一个数据库中拥有多个安装。如果选择这样做，请务必注意安全性。

```
$table_prefix = 'example123_'; // 请仅使用数字、字母和下划线！
```

### WP_SITEURL {#wp-siteurl}

WP_SITEURL 允许定义 WordPress 地址（URL）。所定义的值是 WordPress 核心文件所在的地址。它也应包含 https:// 部分。末尾不要加斜杠 "**/**"。在 `wp-config.php` 中设置此值会覆盖 **siteurl** 在 [wp_options 表](https://codex.wordpress.org/Database_Description#Table:_wp_options) 中的值。定义这些常量会将您网站的 URL 配置硬编码，从而提高可移植性，并有助于防止因仪表盘中的拼写错误或配置错误导致的问题。**注意：** 这**不会**更改数据库中存储的值。如果从 `wp-config.php` 中移除此行，URL 将恢复为旧的数据库值。[使用 **RELOCATE** 常量](https://developer.wordpress.org/advanced-administration/upgrade/migrating/) 来更改数据库中的 **siteurl** 值。

如果 WordPress 安装在 [域名](https://en.wikipedia.org/wiki/Domain_name_system) example.com 下名为 "wordpress" 的目录中，请像这样定义 `WP_SITEURL`：

```
define( 'WP_SITEURL', 'https://example.com/wordpress' );
```

基于 $_SERVER['HTTP_HOST'] 动态设置 WP_SITEURL

```
define( 'WP_SITEURL', 'https://' . $_SERVER['HTTP_HOST'] . '/path/to/wordpress' );
```

**注意：** HTTP_HOST 是由 PHP 根据请求中 HTTP HOST 标头的值动态创建的，因此可能允许 [文件包含漏洞](https://en.wikipedia.org/wiki/File_inclusion_vulnerability)。SERVER_NAME 也可能是动态创建的。但是，当 Apache 配置为 UseCanonicalName "on" 时，SERVER_NAME 由服务器配置设置，而不是动态生成。在这种情况下，使用 SERVER_NAME 比 HTTP_HOST 更安全。

基于 `$_SERVER['SERVER_NAME']` 动态设置 `WP_SITEURL`

```
define( 'WP_SITEURL', 'https://' . $_SERVER['SERVER_NAME'] . '/path/to/wordpress' );
```

### 博客地址 (URL) {#blog-address-url}

与 WP_SITEURL 类似，WP_HOME _会覆盖数据库 [wp_options 表](https://codex.wordpress.org/Database_Description#Table:_wp_options) 中_ home _的值，但不会在数据库中更改它。_ **home** 是您希望用户在浏览器中输入以访问您的 WordPress 博客的地址。它应包含 https:// 部分，并且末尾不应有斜杠 "**/**"。定义这些常量会硬编码您网站的 URL 配置，这可以提高可移植性，并有助于防止因仪表盘中的拼写错误或配置错误导致的问题。

```
define( 'WP_HOME', 'https://example.com/wordpress' );
```

如果您正在使用 [为 WordPress 设置独立目录](https://developer.wordpress.org/advanced-administration/server/wordpress-in-directory/) 中描述的技术，请遵循以下示例。请记住，如果您使用类似这样的设置，您还需要在网站根目录中放置一个 `index.php` 文件。

```
define( 'WP_HOME', 'https://example.com' );
```

基于 `$_SERVER['HTTP_HOST']` 动态设置 `WP_HOME`

```
define( 'WP_HOME', 'https://' . $_SERVER['HTTP_HOST'] . '/path/to/wordpress' );
```

### 移动 wp-content 文件夹 {#moving-wp-content-folder}

您可以将存放主题、插件和上传文件的 `wp-content` 目录移出 WordPress 应用程序目录。

将 WP_CONTENT_DIR 设置为该目录的完整**本地路径**（末尾不加斜杠），例如：

```
define( 'WP_CONTENT_DIR', dirname(__FILE__) . '/blog/wp-content' );
```

将 WP_CONTENT_URL 设置为该目录的完整**URL**（末尾不加斜杠），例如：

```
define( 'WP_CONTENT_URL', 'https://example/blog/wp-content' );
```

### 移动插件文件夹 {#moving-plugin-folder}

将 WP_PLUGIN_DIR 设置为该目录的完整**本地路径**（不带尾部斜杠），例如：

```
define( 'WP_PLUGIN_DIR', dirname(__FILE__) . '/blog/wp-content/plugins' );
```

将 WP_PLUGIN_URL 设置为该目录的完整**URI**（不带尾部斜杠），例如：

```
define( 'WP_PLUGIN_URL', 'https://example/blog/wp-content/plugins' );
```

如果插件存在兼容性问题，请将 PLUGINDIR 设置为该目录的完整**本地路径**（不带尾部斜杠），例如：

```
define( 'PLUGINDIR', dirname(__FILE__) . '/blog/wp-content/plugins' );
```

### 移动主题文件夹 {#moving-themes-folder}

您无法移动主题文件夹，因为其路径是相对于 `wp-content` 文件夹硬编码的：

```
$theme_root = WP_CONTENT_DIR . '/themes';
```

但是，您可以使用 [register_theme_directory](https://developer.wordpress.org/reference/functions/register_theme_directory/) 注册额外的主题目录。

了解如何[移动 wp-content](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件夹。有关主题文件夹如何确定的更多详细信息，请参阅 `wp-includes/theme.php`。

### 移动上传文件夹 {#moving-uploads-folder}

将 UPLOADS 设置为：

```
define( 'UPLOADS', 'blog/wp-content/uploads' );
```

此路径不能是绝对路径。它始终相对于 ABSPATH，因此不需要前导斜杠。

### 修改自动保存间隔 {#modify-autosave-interval}

在编辑文章时，WordPress 会使用 Ajax 自动保存修订版本。您可能希望增加此设置以延长自动保存之间的间隔，或减少此设置以确保不会丢失更改。默认值为 60 秒。

```
define( 'AUTOSAVE_INTERVAL', 180 ); // 秒
```

### 文章修订版本 {#post-revisions}

默认情况下，WordPress 会保存对文章或页面所做的每次编辑的副本，从而允许恢复到该文章或页面的先前版本。可以禁用修订版本的保存，也可以指定每个文章或页面的最大修订版本数。

#### 禁用文章修订版本 {#disable-post-revisions}

如果您**不**设置此值，WordPress 默认会将 WP_POST_REVISIONS 设为 _true_（启用文章修订版本）。如果您想禁用这个强大的修订功能，请使用此设置：

```
define( 'WP_POST_REVISIONS', false );
```

注意：部分用户需要将此命令移至 `wp-config.php` 文件初始块注释后的第一行才能使其生效。

#### 指定文章修订版本数量 {#specify-the-number-of-post-revisions}

若要指定 WordPress 存储的修订版本最大数量，请将 _false_ 更改为整数/数字（_例如_，3 或 12）。

```
define( 'WP_POST_REVISIONS', 3 );
```

注意：部分用户需要将此命令移至 `wp-config.php` 文件初始块注释后的首行，方可使其生效。

### 设置 Cookie 域名 {#set-cookie-domain}

可为具有特殊域名配置的用户指定 WordPress cookie 中设置的域名。例如，如果使用子域名提供静态内容，您可以将 cookie 域名设置为仅限您的非静态域名，以防止每次向子域上的静态内容发送请求时附带 WordPress cookie。

```
define( 'COOKIE_DOMAIN', 'www.example.com' );
```

### 启用多站点/网络功能 {#enable-multisite-network-ability}

WP_ALLOW_MULTISITE 是用于启用多站点功能的特性。如果 `wp-config.php` 文件中缺少此设置，则默认值为 false。

```
define( 'WP_ALLOW_MULTISITE', true );
```

### 重定向不存在的博客 {#redirect-nonexistent-blogs}

如果访问者尝试访问不存在的子域名或子文件夹，可以使用 NOBLOGREDIRECT 来重定向浏览器。

```
define( 'NOBLOGREDIRECT', 'https://example.com' );
```

### 致命错误处理程序 {#wp-disable-fatal-error-handler}

WordPress 5.2 引入了[恢复模式](https://make.wordpress.org/core/2019/04/16/fatal-error-recovery-mode-in-5-2/)，当插件导致致命错误时，会显示错误信息而非白屏。

_网站遇到技术困难。请检查您的站点管理员邮箱收件箱以获取说明。_

白屏和 PHP 错误信息不再向用户显示。但在开发环境中，如果您想启用 WP_DEBUG_DISPLAY，则必须通过将 WP_DISABLE_FATAL_ERROR_HANDLER 设为 true 来禁用恢复模式。

```
define( 'WP_DISABLE_FATAL_ERROR_HANDLER', true ); // 5.2 及更高版本
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_DISPLAY', true );
```

### WP_DEBUG {#wp-debug}

[WP_DEBUG](https://developer.wordpress.org/advanced-administration/debug/debug-wordpress/) 选项控制某些错误和警告的报告，并启用 WP_DEBUG_DISPLAY 和 WP_DEBUG_LOG 设置。默认布尔值为 false。

```
define( 'WP_DISABLE_FATAL_ERROR_HANDLER', true ); // 5.2 及更高版本
define( 'WP_DEBUG', true );
```

[仅当 WP_DEBUG 设置为 true 时才会打印数据库错误](https://trac.wordpress.org/ticket/5473)。数据库错误由 [wpdb](https://developer.wordpress.org/reference/classes/wpdb/) 类处理，不受 [PHP 错误设置](https://www.php.net/errorfunc) 影响。

将 WP_DEBUG 设置为 true 还会将 [错误报告级别](https://www.php.net/manual/en/errorfunc.configuration.php#ini.error-reporting) 提升至 E_ALL，并在使用已弃用的函数或文件时激活警告；否则，WordPress 会将错误报告级别设置为 E_ALL ^ E_NOTICE ^ E_USER_NOTICE。

### WP_ENVIRONMENT_TYPE {#wp-environment-type}

WP_ENVIRONMENT_TYPE 选项控制网站的环境类型：`local`、`development`、`staging` 和 `production`。

环境类型的值按以下顺序处理，每种后续方法会覆盖之前的值：WP_ENVIRONMENT_TYPE [PHP 环境变量](https://www.php.net/manual/en/reserved.variables.environment.php) 和 WP_ENVIRONMENT_TYPE 常量。

对于这两种方法，如果提供的环境类型值不在允许的环境类型列表中，将返回默认值 `production`。

最简单的设置方法可能是通过定义常量：

```
define( 'WP_ENVIRONMENT_TYPE', 'staging' );
```

注意：当 [wp_get_environment_type()](https://developer.wordpress.org/reference/functions/wp_get_environment_type/) 返回 `development` 时，如果网站的 `wp-config.php` 文件中未定义 WP_DEBUG，它将被设置为 `true`。

### SCRIPT_DEBUG {#script-debug}

[SCRIPT_DEBUG](https://developer.wordpress.org/advanced-administration/debug/debug-wordpress/) 是一个相关常量，它会强制 WordPress 使用 `wp-includes/js`、`wp-includes/css`、`wp-admin/js` 和 `wp-admin/css` 目录中的“开发”版本脚本和样式表，而不是加载 `.min.css` 和 `.min.js` 版本。如果你计划修改 WordPress 内置的 JavaScript 或层叠样式表，应在配置文件中添加以下代码：

```
define( 'SCRIPT_DEBUG', true );
```

### 禁用 JavaScript 合并 {#disable-javascript-concatenation}

为了提升管理后台页面的加载速度，所有 JavaScript 文件会被[合并](https://en.wikipedia.org/wiki/Concatenation)至一个 URL。若管理后台页面的 JavaScript 功能失效，可尝试禁用此功能：

```
define( 'CONCATENATE_SCRIPTS', false );
```

### 配置错误日志记录 {#configure-error-logging}

配置错误日志记录可能有点棘手。首先，默认的 PHP 错误日志和显示设置是在 php.ini 文件中配置的，你可能有权访问也可能无权访问该文件。如果你有权访问，则应将其设置为面向公众的线上 PHP 页面所需的设置。强烈建议不要向公众显示错误消息，而是将其路由到错误日志。此外，错误日志不应位于服务器可公开访问的部分。推荐的 php.ini 错误设置示例：

```
error_reporting = 4339
display_errors = Off
display_startup_errors = Off
log_errors = On
error_log = /home/example.com/logs/php_error.log
log_errors_max_len = 1024
ignore_repeated_errors = On
ignore_repeated_source = Off
html_errors = Off
```

**关于错误报告值 4339** 这是一个自定义值，仅记录影响网站功能的问题，并忽略可能甚至不是错误的通知等。有关 1000011110011（即等于 4339 的二进制数）每个二进制位含义的信息，请参阅 [PHP 错误常量](https://www.php.net/manual/en/errorfunc.constants.php)。最左边的 1 表示报告任何 E_RECOVERABLE_ERROR。下一个 0 表示不报告 E_STRICT（当使用草率但功能正常的代码时会抛出），依此类推。你可以随意确定自己的自定义错误报告编号以替代 4339。

显然，你会希望为开发环境设置不同的配置。如果你的测试副本在同一服务器上，或者你无法访问 `php.ini`，则需要在运行时覆盖默认设置。你是希望错误记录到日志文件，还是希望立即收到任何错误通知，或者两者兼而有之，这取决于个人偏好。以下是一个立即报告所有错误的示例，你可以将其插入到 `wp-config.php` 文件中：

```
@ini_set( 'log_errors', 'Off' );
@ini_set( 'display_errors', 'On' );
define( 'WP_DISABLE_FATAL_ERROR_HANDLER', true ); // 5.2 及更高版本
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', false );
define( 'WP_DEBUG_DISPLAY', true );
```

因为 `wp-config.php` 会为每个未从缓存文件加载的页面视图加载，所以它是设置控制 PHP 安装的 `php.ini` 设置的绝佳位置。如果你无法访问 `php.ini` 文件，或者只是想动态更改某些设置，这会很有用。一个例外是 'error_reporting'。当 WP_DEBUG 定义为 true 时，无论你在 wp-config.php 中尝试设置什么，WordPress 都会将 'error_reporting' 设置为 E_ALL。如果你确实需要将 'error_reporting' 设置为其他值，则必须在 `wp-settings.php` 加载后执行，例如在插件文件中。

如果你启用了错误日志记录，请记得之后删除该文件，因为它通常位于可公开访问的位置，任何人都可以访问你的日志。

以下是一个开启 PHP 错误日志记录并将其保存到指定文件的示例。如果 WP_DEBUG 被定义为 true，错误也会被记录到这个文件中。只需将此代码放在任何 _require_once_ 或 _include_ 命令之前。

```
@ini_set( 'log_errors', 'On' );
@ini_set( 'display_errors', 'Off' );
@ini_set( 'error_log', '/home/example.com/logs/php_error.log' );
/* 就这些，停止编辑！祝您博客愉快。 */
```

另一个记录错误的示例，由 Mike Little 在 [wp-hackers 邮件列表](https://lists.automattic.com/pipermail/wp-hackers/2010-September/034830.html) 中建议：

```
/**
 * 这会将所有错误、通知和警告记录到 wp-content 目录下名为 debug.log 的文件中
 * （如果 Apache 没有写入权限，您可能需要先创建该文件并设置适当的权限（例如使用 666））
 */
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', true );
define( 'WP_DEBUG_DISPLAY', false );
@ini_set( 'display_errors', 0 );
```

来自 Mike Little 在 [曼彻斯特 WordPress 用户组](https://groups.google.com/g/manchester-wordpress-user-group/c/tHJxMGhcnZs/m/dn-8yjYIq9wJ) 的一个改进版本：

```
/**
 * 这仅在 WP_DEBUG 为 true 时，将所有错误、通知和警告记录到 wp-content 目录下名为 debug.log 的文件中。
 * 如果 Apache 没有写入权限，您可能需要先创建该文件并设置适当的权限（例如使用 666）。
 */
define( 'WP_DEBUG', true ); // 或 false
if ( WP_DEBUG ) {
	define( 'WP_DEBUG_LOG', true );
	define( 'WP_DEBUG_DISPLAY', false );
	@ini_set( 'display_errors', 0 );
}
```

令人困惑的是，WordPress 有三个 (3) 看起来功能相似的常量。首先，请记住，如果 WP_DEBUG 为 false，它和另外两个 WordPress DEBUG 常量将不起作用。PHP 指令（无论是什么）将起主导作用。除了 'error_reporting'，如果 WP_DEBUG 被定义为 false，WordPress 会将其设置为 4983。其次，即使 WP_DEBUG 为 true，其他常量也只有在它们也被设置为 true 时才起作用。如果它们被设置为 false，PHP 指令将保持不变。例如，如果您的 `php.ini` 文件中有指令 ('display_errors' = 'On')；但您在 `wp-config.php` 文件中有语句 define( 'WP_DEBUG_DISPLAY', false );，错误仍然会显示在屏幕上，即使您试图通过将 WP_DEBUG_DISPLAY 设置为 false 来阻止它，因为这是 PHP 配置的行为。这就是为什么在任何相关的 WP 常量被设置为 false 的情况下，将 PHP 指令设置为您需要的值非常重要。为了安全起见，请明确设置/定义这两种类型。关于 WP 常量的更详细描述，请参阅 [WordPress 中的调试](https://developer.wordpress.org/advanced-administration/debug/debug-wordpress/)。

对于您公开的生产环境 WordPress 安装，您可能考虑在 `wp-config.php` 文件中放置以下内容，即使它可能部分冗余：

```
@ini_set( 'log_errors', 'On' );
@ini_set( 'display_errors', 'Off' );
define( 'WP_DISABLE_FATAL_ERROR_HANDLER', false ); // 5.2 and later
define( 'WP_DEBUG', false );
define( 'WP_DEBUG_LOG', false );
define( 'WP_DEBUG_DISPLAY', false );
```

The default debug log file is `/wp-content/debug.log`. Placing error logs in publicly accessible locations is a security risk. Ideally, your log files should be placed above you site's public root directory. If you can't do this, at the very least, set the log file permissions to 600 and add this entry to the `.htaccess` file in the root directory of your WordPress installation:

```
<Files debug.log>
  Order allow,deny
  Deny from all
</Files>
```

This prevents anyone from accessing the file via HTTP. You can always view the log file by retrieving it from your server via FTP.

### 增加分配给 PHP 的内存 {#increasing-memory-allocated-to-php}

**WP_MEMORY_LIMIT** 选项允许你指定 PHP 可以消耗的最大内存量。当你收到类似“允许的内存大小 xxxxxx 字节已耗尽”的消息时，可能需要此设置。

此设置仅针对 WordPress 增加 PHP 内存，不影响其他应用程序。默认情况下，WordPress 会尝试将分配给 PHP 的内存增加到 40MB（代码位于 `/wp-includes/default-constants.php` 开头），多站点则为 64MB。因此，`wp-config.php` 中的设置应反映高于 40MB 或 64MB 的值，具体取决于你的配置。

在使用此功能之前，WordPress 会自动检查 PHP 分配的内存是否少于输入的值。例如，如果 PHP 已分配 64MB，则无需将此值设置为 64M，因为 WordPress 会在需要时自动使用全部 64MB。

注意：某些主机不允许自动增加 PHP 内存限制。在这种情况下，请联系你的主机提供商以增加 PHP 内存限制。此外，许多主机将 PHP 限制设置为 8MB。

调整 WordPress 内存限制也可能带来问题。随着你添加更多插件或功能，你可能会掩盖问题的根源，导致问题在以后发生。

如果你在提高了内存限制后仍然遇到内存不足问题，你应该正确调试你的安装。很可能你有太多内存密集型函数绑定到特定操作，应该将这些函数移动到定时任务中。

将 PHP 内存增加到 64MB

```
define( 'WP_MEMORY_LIMIT', '64M' );
```

将 PHP 内存增加到 96MB

```
define( 'WP_MEMORY_LIMIT', '96M' );
```

管理任务可能需要比常规操作更多的内存。在管理区域时，可以通过定义 WP_MAX_MEMORY_LIMIT 来从 WP_MEMORY_LIMIT 增加或减少内存。

```
define( 'WP_MAX_MEMORY_LIMIT', '128M' );
```

注意：这必须放在包含 wp-settings.php 之前。

### 缓存 {#cache}

**WP_CACHE** 设置项若为 true，则在执行 `wp-settings.php` 时会包含 `wp-content/advanced-cache.php` 脚本。

```
define( 'WP_CACHE', true );
```

### 自定义用户与用户元数据表 {#custom-user-and-usermeta-tables}

**CUSTOM_USER_TABLE** 与 **CUSTOM_USER_META_TABLE** 用于指定 WordPress 通常使用的用户表和用户元数据表将被替换，转而使用这些值/表来存储用户信息。

```
define( 'CUSTOM_USER_TABLE', $table_prefix.'my_users' );
define( 'CUSTOM_USER_META_TABLE', $table_prefix.'my_usermeta' );
```

注意：即使手动设置了 'CUSTOM_USER_META_TABLE'，系统仍会为每个数据库创建一个用户元数据表，并为每个实例分配相应的权限。默认情况下，WordPress 安装程序会为第一个用户（ID #1）添加权限。您还需要通过插件或自定义函数来管理每个站点的权限。如果未进行此设置，将会遇到权限错误和登录问题。

CUSTOM_USER_TABLE 在初始设置第一个 WordPress 实例时最容易采用。第一个实例的 `wp-config.php` 中的 define 语句指向 `wp_users` 数据默认存储的位置。完成第一个站点设置后，将可正常工作的 `wp-config.php` 复制到下一个实例仅需更改 `$table_prefix` 变量。请勿使用原始安装中已使用的电子邮件地址。完成设置过程后，使用自动生成的管理员账户和密码登录。接着，将您的普通账户提升为管理员级别并退出管理员账户。重新以您自己的账户登录，删除管理员账户并根据需要提升其他用户账户的权限。

### 语言与语言目录 {#language-and-language-directory}

WordPress [4.0 版本](https://wordpress.org/documentation/wordpress-version/version-4-0/) 允许您在 WordPress [管理界面](https://wordpress.org/documentation/article/administration-screens/) 中更改语言。要更改管理设置界面的语言，请前往 [设置](https://wordpress.org/documentation/article/administration-screens/#settings-configuration-settings) > [常规](https://wordpress.org/documentation/article/settings-general-screen/) 并选择站点语言。

#### WordPress v3.9.6 及更低版本 {#wordpress-v3-9-6-and-below}

**WPLANG** 定义了语言翻译 (.mo) 文件的名称。**WP_LANG_DIR** 定义了 WPLANG .mo 文件所在的目录。如果未定义 WP_LANG_DIR，WordPress 会先在 wp-content/languages 中查找，然后在 `wp-includes/languages` 中查找由 WPLANG 定义的 .mo 文件。

```
define( 'WPLANG', 'de_DE' );
define( 'WP_LANG_DIR', dirname(__FILE__) . 'wordpress/languages' );
```

要查找 WPLANG 语言代码，请[参考此处](https://make.wordpress.org/polyglots/teams/)。WP Local 列中的代码就是您需要的。

### 保存查询以供分析 {#save-queries-for-analysis}

**SAVEQUERIES** 定义会将数据库查询保存到一个数组中，并且可以显示该数组以帮助分析这些查询。该信息会保存每条查询、调用它的函数以及查询执行所花费的时间。**注意：** 这会对您的网站性能产生影响，因此请确保在非调试时关闭此功能。

首先，将此添加到 `wp-config.php` 文件中：

```
define( 'SAVEQUERIES', true );
```

然后在您主题的页脚中添加此代码：

```
if ( current_user_can( 'administrator' ) ) {
  global $wpdb;
  echo "<pre>";
  print_r( $wpdb->queries );
  echo "</pre>";
}
```

或者，可以考虑使用 [Query Monitor](https://wordpress.org/plugins/query-monitor/)

### 覆盖默认文件权限 {#override-of-default-file-permissions}

**FS_CHMOD_DIR** 和 **FS_CHMOD_FILE** 定义语句允许覆盖默认文件权限。这两个变量的开发是为了解决核心更新功能在运行于 [suexec](https://en.wikipedia.org/wiki/SuEXEC) 环境下的主机上失败的问题。如果主机对所有用户文件使用限制性文件权限（例如 400），并拒绝访问设置了组或全局权限的文件，这些定义可以解决该问题。

```
define( 'FS_CHMOD_DIR', ( 0755 & ~ umask() ) );
define( 'FS_CHMOD_FILE', ( 0644 & ~ umask() ) );
```

提供 setgid 的示例：

```
define( 'FS_CHMOD_DIR', ( 02755 & ~umask() ) );
```

注意：'**0755′** 和 '**02755**' 是八进制值。八进制值必须以 0 为前缀，并且不使用单引号（'）分隔。另请参阅：[更改文件权限](https://developer.wordpress.org/advanced-administration/server/file-permissions/)

### WordPress 升级常量 {#wordpress-upgrade-constants}

**注意：请根据解决更新问题的实际需要，尽可能少地定义以下常量。**

需要定义这些常量最常见的原因是：

*   主机运行在涉及符号链接的特殊安装配置下。您可能需要定义与路径相关的常量（FTP_BASE、FTP_CONTENT_DIR 和 FTP_PLUGIN_DIR）。通常，仅定义基础路径就足够了。
*   某些 PHP 安装附带的 PHP FTP 扩展与某些 FTP 服务器不兼容。在这些罕见情况下，您可能需要将 FS_METHOD 定义为 "ftpsockets"。

以下是适用于 WordPress 更新的有效常量：

*   **FS_METHOD** 强制指定文件系统方法。它只能是 "direct"、"ssh2"、"ftpext" 或 "ftpsockets"。通常，只有在遇到更新问题时才应更改此设置。如果更改后没有帮助，**请改回或移除它**。在大多数情况下，如果自动选择的方法无效，将其设置为 'ftpsockets' 会起作用。
    *   **（首选）"direct"** 强制使用 PHP 内部的直接文件 I/O 请求，这在配置不当的主机上容易引发安全问题，在适当时会自动选择此方法。
    *   **（次选）"ssh2"** 强制使用已安装的 SSH PHP 扩展。
    *   **（第三选择）"ftpext"** 强制使用 FTP PHP 扩展进行 FTP 访问。
    *   **（第四选择）"ftpsockets"** 利用 PHP Sockets 类进行 FTP 访问。
*   **FTP_BASE** 是 WordPress 安装的"基础"（ABSPATH）文件夹的完整路径。
*   **FTP_CONTENT_DIR** 是 WordPress 安装的 wp-content 文件夹的完整路径。
*   **FTP_PLUGIN_DIR** 是 WordPress 安装的 plugins 文件夹的完整路径。
*   **FTP_PUBKEY** 是您的 SSH 公钥的完整路径。
*   **FTP_PRIKEY** 是您的 SSH 私钥的完整路径。
*   **FTP_USER** 是 FTP 或 SSH 用户名。很可能两者相同，但请根据您希望执行的更新类型使用相应的用户名。
*   **FTP_PASS** 是为 **FTP_USER** 输入的用户名的密码。如果使用 SSH 公钥认证，可以省略此项。
*   **FTP_HOST** 是您的 SSH/FTP 服务器的 主机名:端口 组合。默认 FTP 端口是 21，默认 SSH 端口是 22。这些无需特别说明。
*   **FTP_SSL** 如果**底层传输支持**（并非所有服务器都可用），则设为 TRUE 以使用 SSL 连接。这用于"安全 FTP"，而非 SSH SFTP。

```
define( 'FS_METHOD', 'ftpext' );
define( 'FTP_BASE', '/path/to/wordpress/' );
define( 'FTP_CONTENT_DIR', '/path/to/wordpress/wp-content/' );
define( 'FTP_PLUGIN_DIR ', '/path/to/wordpress/wp-content/plugins/' );
define( 'FTP_PUBKEY', '/home/username/.ssh/id_rsa.pub' );
define( 'FTP_PRIKEY', '/home/username/.ssh/id_rsa' );
define( 'FTP_USER', 'username' );
define( 'FTP_PASS', 'password' );
define( 'FTP_HOST', 'ftp.example.org' );
define( 'FTP_SSL', false );
```

Some configurations should set FTP_HOST to localhost to avoid 503 problems when trying to update plugins or WP itself.

#### 启用 SSH 升级访问 {#enabling-ssh-upgrade-access}

有两种使用 SSH2 进行升级的方法。

第一种是使用 [SSH SFTP 更新器支持插件](https://wordpress.org/plugins/ssh-sftp-updater-support/)。第二种是使用内置的 SSH2 升级器，这需要安装 pecl SSH2 扩展。

要安装 pecl SSH2 扩展，您需要执行类似以下的命令，或者联系您的网络托管提供商进行安装：

```
pecl install ssh2
```

安装 pecl ssh2 扩展后，您需要修改 PHP 配置以自动加载此扩展。

在大多数 Linux 发行版中，pecl 由 pear 包提供。要在 Redhat/Fedora/CentOS 中安装 pecl：

```
yum -y install php-pear
```

要在 Debian/Ubuntu 中安装 pecl：

```
apt-get install php-pear
```

建议使用不受密码短语保护的私钥。有许多报告称受密码短语保护的私钥无法正常工作。如果您决定尝试使用受密码短语保护的私钥，则需要将私钥的密码短语作为 FTP_PASS 输入，或者在安装更新时在显示的凭据字段的“密码”字段中输入。

### 替代性 Cron 任务调度 {#alternative-cron}

在某些情况下可能需要为 WP 使用替代性 Cron。最常见的原因是预定发布的文章未能按预期发布。这种替代方法采用重定向机制：当需要执行 cron 时，用户浏览器会收到重定向指令，使其立即返回网站，而 cron 则在他们刚中断的连接中继续运行。此方法存在特定风险，因为它依赖于非原生的 WordPress 服务。

```
define( 'ALTERNATE_WP_CRON', true );
```

### 禁用 Cron 和 Cron 超时设置 {#disable-cron-and-cron-timeout}

通过将 DISABLE_WP_CRON 设置为 true 来完全禁用 cron。

```
define( 'DISABLE_WP_CRON', true );
```

确保 cron 进程在 WP_CRON_LOCK_TIMEOUT 秒内不会重复运行。

```
define( 'WP_CRON_LOCK_TIMEOUT', 60 );
```

### 其他可定义常量 {#additional-defined-constants}

以下是其他可以定义的常量。除非已尝试过其他方法，否则可能不应设置这些常量。如果您有特殊的域名设置，Cookie 定义可能会特别有用。

```
define( 'COOKIEPATH', preg_replace( '|https?://[^/]+|i', '', get_option( 'home' ) . '/' ) );
define( 'SITECOOKIEPATH', preg_replace( '|https?://[^/]+|i', '', get_option( 'siteurl' ) . '/' ) );
define( 'ADMIN_COOKIE_PATH', SITECOOKIEPATH . 'wp-admin' );
define( 'PLUGINS_COOKIE_PATH', preg_replace( '|https?://[^/]+|i', '', WP_PLUGIN_URL ) );
define( 'TEMPLATEPATH', get_template_directory() );
define( 'STYLESHEETPATH', get_stylesheet_directory() );
```

### 清空回收站 {#empty-trash}

此常量控制 WordPress 在多少天后将永久删除回收站中的文章、页面、附件和评论。默认值为 30 天：

```
define( 'EMPTY_TRASH_DAYS', 30 ); // 30 天
```

要禁用回收站功能，请将天数设置为零。

```
define( 'EMPTY_TRASH_DAYS', 0 ); // 零天
```

**注意：** 使用此设置时，当有人点击“永久删除”时，WordPress 将不会要求确认。

### 自动数据库优化 {#automatic-database-optimizing}

系统支持自动数据库修复功能，您可以通过在 `wp-config.php` 文件中添加以下定义来启用。

注意：此功能仅在需要时启用，问题解决后应禁用。启用后，用户无需登录即可访问该功能，因为其主要目的是修复损坏的数据库，而数据库损坏时用户通常无法登录。

```
define( 'WP_ALLOW_REPAIR', true );
```

该脚本位于 `{$your_site}/wp-admin/maint/repair.php`。

### DO_NOT_UPGRADE_GLOBAL_TABLES {#do-not-upgrade-global-tables}

**DO_NOT_UPGRADE_GLOBAL_TABLES** 常量定义可阻止 [dbDelta()](https://developer.wordpress.org/reference/functions/dbdelta/) 和升级函数对全局表执行高开销查询。

对于拥有大型全局表（特别是用户表和用户元数据表）的站点，以及那些与 bbPress 或其他 WordPress 安装共享用户表的站点，可以通过将 **DO_NOT_UPGRADE_GLOBAL_TABLES** 定义为 true 来防止升级过程中修改这些表。由于 ALTER 操作或无限制的 DELETE 或 UPDATE 操作可能需要很长时间才能完成，大型站点通常希望避免在升级过程中运行这些查询，以便自行处理。此外，如果多个 bbPress 和 WordPress 安装之间共享用户表，您可能希望指定一个站点作为升级主控。

```
define( 'DO_NOT_UPGRADE_GLOBAL_TABLES', true );
```

### 查看所有已定义的常量 {#view-all-defined-constants}

PHP 提供了一个函数，可以返回包含所有当前已定义常量及其值的数组。

```
print_r( @get_defined_constants() );
```

### 禁用插件和主题文件编辑器 {#disable-the-plugin-and-theme-file-editor}

有时您可能希望禁用插件或主题文件编辑器，以防止过度热心的用户编辑敏感文件并可能导致网站崩溃。如果黑客获得了高权限用户账户的访问权限，禁用这些功能还能提供额外的安全层。

```
define( 'DISALLOW_FILE_EDIT', true );
```

**注意**：某些插件的功能可能会因其代码中使用 `current_user_can('edit_plugins')` 而受到影响。插件作者应避免检查此权限，或至少检查是否设置了此常量并显示适当的错误消息。请注意，如果插件无法正常工作，这可能是原因。

### 禁用插件和主题更新与安装 {#disable-plugin-and-theme-update-and-installation}

这将阻止用户在 WordPress 管理区域使用插件和主题的安装/更新功能。设置此常量也会禁用插件和主题文件编辑器（即您无需同时设置 DISALLOW_FILE_MODS 和 DISALLOW_FILE_EDIT，因为单独设置 DISALLOW_FILE_MODS 会产生相同效果）。

```
define( 'DISALLOW_FILE_MODS', true );
```

### 为管理员和登录强制使用 SSL {#require-ssl-for-admin-and-logins}

**注意：** WordPress [4.0 版本](https://wordpress.org/documentation/wordpress-version/version-4-0/)已弃用 FORCE_SSL_LOGIN。请使用 FORCE_SSL_ADMIN。

FORCE_SSL_ADMIN 用于保护登录和管理区域，确保密码和 Cookie 永远不会以明文形式发送。更多详情请参阅 [HTTPS](https://developer.wordpress.org/advanced-administration/security/https/)。

```
define( 'FORCE_SSL_ADMIN', true );
```

### 阻止外部 URL 请求 {#block-external-url-requests}

通过将 WP_HTTP_BLOCK_EXTERNAL 定义为 true 来阻止外部 URL 请求，这将只允许本地主机和您的博客发出请求。常量 WP_ACCESSIBLE_HOSTS 将允许额外的主机通过以进行请求。WP_ACCESSIBLE_HOSTS 常量的格式是以逗号分隔的允许主机名列表，支持通配符域名，例如 `*.wordpress.org` 将允许联系 wordpress.org 的所有子域名。

```
define( 'WP_HTTP_BLOCK_EXTERNAL', true );
define( 'WP_ACCESSIBLE_HOSTS', 'api.wordpress.org,*.github.com' );
```

### 禁用 WordPress 自动更新 {#disable-wordpress-auto-updates}

网站可能因某些原因（如自定义修改或主机提供的更新）需要禁用自动更新。在重大版本发布前，也可先禁用自动更新，以便在开发或测试环境中进行充分测试，再在生产站点执行更新。

```
define( 'AUTOMATIC_UPDATER_DISABLED', true );
```

### 禁用 WordPress 核心更新 {#disable-wordpress-core-updates}

控制核心更新的最简单方法是使用 `WP_AUTO_UPDATE_CORE` 常量：

**禁用所有核心更新：**
```
define( 'WP_AUTO_UPDATE_CORE', false );
```

**启用所有核心更新，包括次要版本和主要版本：**
```
define( 'WP_AUTO_UPDATE_CORE', true );
```

**仅为次要版本启用核心更新（默认设置）：**
```
define( 'WP_AUTO_UPDATE_CORE', 'minor' );
```

参考：[在 WordPress 3.7 中禁用自动更新](https://make.wordpress.org/core/2013/10/25/the-definitive-guide-to-disabling-auto-updates-in-wordpress-3-7/)

### 清理图片编辑 {#cleanup-image-edits}

默认情况下，WordPress 每次编辑图片时都会创建一组新的图片，并且在恢复原始图片时，会将所有编辑版本保留在服务器上。将 IMAGE_EDIT_OVERWRITE 定义为 true 可以改变此行为。系统只会创建一组图片编辑版本，并且在恢复原始图片时，编辑版本会从服务器上移除。

```
define( 'IMAGE_EDIT_OVERWRITE', true );
```

## 保存前请仔细检查 {#double-check-before-saving}

_**请务必检查您输入的上述任何值前后是否有空格，并且不要删除单引号！**_

在保存文件之前，请务必**仔细检查**您是否意外删除了参数值周围的任何单引号。确保文件中的 PHP 结束标签后没有任何内容。文件的最后内容应为 **?>**，别无其他。没有空格。

要保存文件，请选择**文件 > 另存为 > wp-config.php**，并将文件保存在 WordPress 安装的根目录中。将文件上传到您的 Web 服务器，您就可以安装 WordPress 了！