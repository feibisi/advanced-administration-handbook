---
title: "Adminer"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Upgrade
    - Repos
    - Data
---

# Adminer

## 什么是 Adminer？

[Adminer](https://www.adminer.org/)（原名 phpMinAdmin）是一个用 PHP 编写的功能齐全的数据库管理工具。与由多个文件组成的 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/) 不同，Adminer 仅包含一个文件，可直接部署到目标服务器。它支持多种数据库，包括 MySQL、MariaDB、PostgreSQL、SQLite、MS SQL、Oracle、Elasticsearch、MongoDB 等。由于 WordPress 将所有数据存储在 MySQL 数据库中，Adminer 提供了对该数据库中数据、表和字段的“原始”视图。

## Adminer 的优势

- **界面简洁**：与其他数据库管理工具不同，Adminer 提供了清晰且用户友好的界面。
- **直接数据操作**：适用于直接编辑数据库，尤其是在 WordPress 停止工作时。
- **轻量级**：作为单个 PHP 文件，它易于上传、使用和移除。

## 它有什么用处？

Adminer 适用于表维护、数据备份和直接数据库编辑。有时，在[支持论坛](https://wordpress.org/support/welcome/#asking-for-support)中，贡献者会分享一些有用的 SQL 查询，这些查询可以使用 Adminer 等工具执行。

## 我可以在哪里获取它？

许多托管控制面板，例如 cPanel 和 Plesk，都预装了 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/)。如果无法访问数据库，用户可以咨询其托管服务提供商以获取数据库访问权限。
对于更喜欢使用 Adminer 的用户，可以从 [Adminer 项目页面](https://www.adminer.org/)下载它。

## 安装 Adminer

1. 从 [Adminer 下载页面](https://www.adminer.org/en/#download) 下载最新版本的 Adminer。
2. 使用 [FileZilla](https://wordpress.org/documentation/article/using-filezilla/) 等 FTP 工具，将 Adminer PHP 文件上传到 WordPress 根目录（即 `wp-config.php` 文件所在的位置）。
3. 将 Adminer PHP 文件上传到 WordPress 根目录后，您可以在浏览器中通过将文件名添加到 URL 来访问它，例如 `https://example.com/adminer-4.8.1.php`。
4. 数据库登录凭据必须手动填写，可以从 `wp-config.php` 文件中获取。

## 将 Adminer 安装为 WordPress 插件

Adminer 也可能在 [WordPress 插件仓库](https://wordpress.org/plugins/search/database+adminer/) 中以插件形式提供。如果作为 WordPress 插件安装，Adminer 在访问时可能会自动使用 `wp-config.php` 文件中的数据库登录凭据。

## 安全注意事项

为防止未经授权的访问，请确保在使用后移除或保护 Adminer，特别是当它可以公开访问时。一种保护方法是使用 `.htaccess` 文件限制访问。如果您不熟悉 `.htaccess` 文件限制，请考虑寻求 [WordPress 加固指南](https://wordpress.org/documentation/article/hardening-wordpress/) 或在使用后移除 Adminer。