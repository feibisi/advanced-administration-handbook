---
title: "显示错误"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Security
    - Repos
    - Data
---

# 显示错误

## 什么是 display_errors？

`display_errors` 是 PHP 中的一个指令，位于 php.ini 文件中。通过此选项，PHP 决定是否将错误直接打印在页面上。

## 为什么需要禁用 display_errors？

根据 [PHP 文档](https://www.php.net/manual/en/errorfunc.configuration.php#ini.display-errors)，在生产环境或线上站点上绝不应启用此功能。

虽然 `display_errors` 在调试场景中可能提供有用信息，但如果启用它，则需考虑潜在的安全问题。[参见 OWASP 关于错误处理不当的文章。](https://owasp.org/www-community/Improper_Error_Handling)

然而，一些托管公司默认启用了 `display_errors`。这可能是由于配置错误，例如尝试使用在托管环境中无效的配置来禁用它，例如 PHP 不是作为模块运行，而是使用 PHP FastCGI 进程管理器（PHP-FPM）。

## 如何禁用 display_errors

检查您的托管控制面板以禁用 `display_errors`，或联系您的托管服务提供商。

如果您的 PHP 作为 Apache 模块运行，您可以使用以下 .htaccess 配置来禁用 display_errors：

`<IfModule mod_php8.c> php_flag display_errors off </IfModule>`

如果您的服务器使用 FastCGI/PHP-FPM，可以通过确保 .user.ini 文件包含以下内容来禁用 display_errors：

`display_errors = 0`

如果这些示例对您无效，或者您需要更多指导，请联系您的托管服务提供商。