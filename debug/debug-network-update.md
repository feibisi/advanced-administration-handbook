---
title: "调试 WordPress 多站点网络"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Debug
    - Repos
    - Data
---

# 调试 WordPress 多站点网络

如果您访问到此页面，很可能是在设置或运行 [WordPress 多站点网络](https://wordpress.org/documentation/article/multisite-network-administration/)时遇到了错误。这些错误通常发生在 WordPress 无法在数据库中找到网络所需的一个或多个全局表，或者 Web 服务器配置不正确时。

本指南概述了 WordPress 多站点网络在设置或运行过程中最常见的问题原因及解决方法。

---

## 开始之前 {#before-you-begin}
- 在进行更改前，**请备份您的数据库和文件**。
- 确保您的主机环境满足[多站点网络的技术要求](https://developer.wordpress.org/advanced-administration/multisite/create-network/)。
- 向您的主机服务商确认是否允许安装网络/多站点（某些共享主机可能会限制此功能）。

---

## 如果您刚刚安装了网络 {#new-install}

### 验证 wp-config.php 文件 {#verify-wp-config}
检查您的 [wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件，确认以下内容：

- 正确的数据库信息 (`DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_HOST`)
- `MULTISITE` 常量
- `SUBDOMAIN_INSTALL` 常量（根据设置填写 true/false）
- `$base` 变量
- 数据表前缀 (`$table_prefix`)

确保在以下代码块之后没有放置任何代码：

```php
/* That's all, stop editing! Happy publishing. */

/** WordPress目录的绝对路径。 */
if ( ! defined('ABSPATH') ) {
    define('ABSPATH', __DIR__ . '/');
}

/** 设置WordPress变量和包含文件。 */
require_once ABSPATH . 'wp-settings.php';
```

如果您在 `require_once` 行*之后*发现了自定义代码，请将其移动到“stop editing”注释之前。

---

### Mod_rewrite 或重写规则不工作 {#rewrites}

如果主站点能加载但子站点返回 404 错误，可能是您的重写规则未生效。

#### Apache
在 Ubuntu/Debian 系统上：

```bash
sudo a2enmod rewrite
sudo nano /etc/apache2/sites-available/000-default.conf
```

将两处 `AllowOverride None` 都改为 `AllowOverride All`，然后重启 Apache：

```bash
sudo systemctl restart apache2
```

#### Nginx
对于 Nginx，请确保您的服务器块包含以下配置：

```nginx
location / {
    try_files $uri $uri/ /index.php?$args;
}
```

重启 Nginx：

```bash
sudo systemctl restart nginx
```

---

### 检查数据库 {#check-database}

多站点网络需要额外的数据库表。请确认以下表存在（使用您的前缀，例如 `wp_`）：

- `wp_blogs`
- `wp_blogmeta`
- `wp_blog_versions`
- `wp_registration_log`
- `wp_signups`
- `wp_site`
- `wp_sitemeta`

如果表缺失，或者 `wp_site`/`wp_blogs` 表为空，您可能需要手动插入数据行。**请先备份！**

示例 SQL（请调整前缀、域名、路径、用户和日期）：

```sql
INSERT INTO wp_site VALUES (1, 'example.com', '/');
INSERT INTO wp_blogs VALUES (1, 1, 'example.com', '/', '2025-01-01', '2025-01-01', 1, 0, 0, 0, 0, 0);
INSERT INTO wp_sitemeta VALUES (1, 1, 'site_admins', 'a:1:{i:1;s:5:"admin";}');
```

⚠️ 请将 `"admin"` 更改为您的实际用户名，并调整 `s:5` 以匹配其字符长度。

---

## 如果新站点创建突然停止工作 {#site-creation-stopped}
- 确认数据库服务器主机名未更改（检查 `wp-config.php` 中的 `DB_HOST`）。
- 检查数据库用户是否对数据库拥有**所有权限**。
- 查看 Web 服务器错误日志中与 `.htaccess` 或重写相关的问题。

---

## 其他问题 {#other-issues}
- **数据库排序规则冲突**：如今已很少见，但从 WordPress MU 升级的遗留系统可能因排序规则不匹配导致站点创建错误。如果从非常旧的版本升级，请检查数据库排序规则设置。
- **Apache 重写限制**：如果看到如下错误：

  ```
  Options FollowSymLinks or SymLinksIfOwnerMatch is off which implies that RewriteRule directive is forbidden
  ```

  这可能会阻止重写并导致 WordPress 报告缺少数据表。请更新 Apache 配置以允许重写。

---

## 相关文章 {#related-articles}
- [WordPress 多站点网络：完整指南](https://multilingualpress.org/wordpress-multisite-network/)
- [强化 WordPress 多站点安全](https://developer.wordpress.org/advanced-administration/security/hardening/)

## 外部链接 {#external-links}
- [WordPress 支持 → 多站点论坛](https://wordpress.org/support/forum/multisite/)

---

## 遗留说明 {#legacy-notes}
部分问题可追溯至从 WordPress MU（3.0 版之前）迁移而来。这些问题在现代网络中已很罕见，但在运行非常古老的站点时仍可能出现。如有可能，请考虑迁移至全新的多站点安装。