---
title: "Php"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Performance
    - Repos
    - Data
---

## PHP

PHP（PHP：超文本预处理器）是互联网上一种流行的编程语言。PHP 将动态内容（如 WordPress 中的内容）转换为网页浏览器可以读取的 HTML、CSS 和 JavaScript。WordPress 主要用 PHP 编写，服务器必须安装 PHP 才能运行 WordPress。

由于 PHP 是一种解释型语言，其版本和配置对 WordPress 的运行效果乃至能否运行都有很大影响。

### 版本

在可能的情况下，应使用 PHP 7.4 或更高版本来运行 WordPress。截至本文撰写时，PHP 7.4 是 WordPress 官方支持的版本，而 PHP 8.0 和 8.1 为“兼容但有例外”，PHP 8.2 处于“测试支持”阶段。PHP 8 是唯一仍在积极开发和维护的 PHP 主要版本。PHP 团队会定期停止对旧版本 PHP 的支持，且不保证会为旧版本提供安全更新。

同时，新版本的 PHP 包含安全和性能改进，并伴有新功能和错误修复，但这些更新不保证向后兼容。然而，升级 PHP 版本时必须极其谨慎。虽然 WordPress 与最新的 PHP 版本兼容，但基于旧版 PHP 构建的网站可能因其包含的插件和主题而不兼容。

如果无法立即升级到 PHP 8，应尽快升级到 PHP 7.4。虽然 WordPress *可能* 在旧版 PHP 上运行，但这些版本已达到官方生命周期终止状态，运行过时的 PHP 安装**可能使您的网站面临安全漏洞**。

您可以在 [PHP 兼容性与 WordPress 版本](https://make.wordpress.org/core/handbook/references/php-compatibility-and-wordpress-versions/) 页面找到与您的 WordPress 版本兼容的 PHP 版本。

有关 PHP 支持版本的更多信息，可随时查阅 [PHP 支持版本页面](https://www.php.net/supported-versions.php)。

升级 PHP 时，最好在升级前测试网站的兼容性。如果您提供多个环境（例如暂存环境和生产环境），应为每个环境单独配置 PHP 版本。这将允许用户在非生产环境中测试新版本的 PHP，并在升级生产环境的 PHP 版本前解决任何问题。

有一个有用的 [WP-CLI 命令](https://github.com/danielbachhuber/php-compat-command) 可用于执行通用兼容性检查，但请注意它并非 100% 准确。

### 配置

PHP 主要通过配置文件 `php.ini` 进行配置，PHP 在运行时从中读取所有设置和配置。这通常通过 CGI/FastCGI 或 PHP-FPM 等进程管理器实现。

某些服务器环境可能允许使用其他文件（如 `.htaccess` 或 `.user.ini` 文件）来自定义 PHP 配置。

您可以在 [官方 PHP 文档](https://www.php.net/manual/en/ini.core.php) 中查看每个指令的详细信息。

#### 超时设置

系统中有多个超时设置，用于限制请求的不同方面。配置超时值时，选择能良好协同工作的数值非常重要。例如，如果 Web 服务器（如 Apache）的超时时间低于 PHP 服务的脚本执行超时时间，那么设置非常高的 PHP 脚本执行超时就没有意义——在这种情况下，如果请求耗时过长，无论 PHP 超时设置如何，Web 服务器都会终止该请求。

请注意，进程所需时间因服务器负载而异，设置这些限制是为了确保服务器正常运行。如果服务器负载较高，进程可能需要更长时间才能完成，从而导致级联效应，进一步增加服务器负载。因此，需要在给予脚本足够编译时间与确保服务器负载正常之间取得平衡。

主要的 PHP 超时可以通过 [`max_execution_time`](https://www.php.net/manual/en/info.configuration.php#ini.max-execution-time) `php.ini` 指令设置。此设置限制代码执行时间，但不限制系统库调用或 MySQL 查询，[Windows 系统除外](https://www.php.net/manual/en/function.set-time-limit.php)，在 Windows 上它确实限制这些操作。

从 Web 服务器到 PHP 数据传输允许的最长时间由 [`max_input_time`](https://www.php.net/manual/en/info.configuration.php#ini.max-input-time) `php.ini` 指令指定。它通常用于限制文件上传时间。需要注意的是，此时间与 `max_execution_time` 是分开的，它定义了从 Web 服务器调用 PHP 到执行开始之间的时间。

请注意，这些超时设置通常是按服务器配置的，如果您使用的是共享主机账户，可能无法修改它们。最佳方法是联系您的主机公司技术支持，询问是否可以修改这些设置以满足您的需求。

#### 内存限制

PHP 在每次页面渲染时允许使用的最大内存量由 [`memory_limit`](https://www.php.net/manual/en/ini.core.php#ini.memory-limit) `php.ini` 指令指定。

除了在 PHP 内部设置内存限制外，WordPress 还有两个内存配置常量，可以在 **wp-config.php** 文件中修改。如果 WordPress 有权限，它会将 PHP 的 `memory_limit` 提升到这些值，但如果 `php.ini` 指定的值更高，WordPress 不会降低允许的内存量。

选项 `WP_MEMORY_LIMIT` 声明了 WordPress 为渲染网站前端应请求的内存量。WordPress 默认值为 40 MB，WordPress 多站点默认值为 64 MB。

```
define( 'WP_MEMORY_LIMIT', '128M' );
```

选项 `WP_MAX_MEMORY_LIMIT` 声明了 WordPress 为渲染网站后端应请求的内存量。WordPress 默认值为 256 MB。

```
define( 'WP_MAX_MEMORY_LIMIT', '256M' );
```

由于 WordPress 后端通常需要更多内存，因此有一个单独的设置用于为已登录用户指定内存量。这主要是媒体上传所必需的。你可以将其设置为高于前端限制，以确保后端拥有所需的所有资源。通常，`WP_MEMORY_LIMIT <= WP_MAX_MEMORY_LIMIT`。

#### 文件上传大小限制

通过 WordPress 管理后台上传媒体文件和其他内容时，WordPress 使用 PHP 处理上传。PHP 的配置包含对可通过 PHP 上传的文件大小限制，以及可发送到 Web 服务器进行处理的请求大小限制。这些设置需要与上文讨论的服务器超时设置保持一致。

单个文件上传的大小限制可通过 [`upload_max_filesize`](https://www.php.net/manual/en/ini.core.php#ini.upload-max-filesize) `php.ini` 指令进行配置。

从 Web 服务器发送到 PHP 进行处理的完整请求大小限制可通过 [`post_max_size`](https://www.php.net/manual/en/ini.core.php#ini.post-max-size) `php.ini` 指令进行配置。`post_max_size` 的值必须大于或等于 `upload_max_filesize` 的值。PHP 不会处理超过 `post_max_size` 设定值的请求。

请注意 `post_max_size` 适用于所有 PHP 请求而不仅仅是上传，因此如果网站需要处理请求中包含的大量其他数据，可能需要单独考虑此设置。

请注意在共享主机账户中，这些限制通常在服务器级别设置，您可能无法修改它们或将其提高到特定值以上。此外，不同环境修改上述值的方式也不同。如需进一步帮助，请联系您的主机提供商技术支持。

#### 替换 WordPress 的 Cron 触发器

`wp-cron.php` 脚本负责调度和执行某些自动任务。每次有人访问您的网站时，`wp-cron.php` 都会检查是否到了执行任务的时间。尽管这些检查量小且快速，但它们会消耗时间并产生负载。因此，值得考虑设置 [`DISABLE_WP_CRON` 常量](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/#disable-cron-and-cron-timeout)并使用替代方法来触发 WordPress 的 cron 系统。但请注意，WordPress cron 系统在设计时考虑了性能，运行所需资源极少，因此除非确实需要，否则不必替换它。