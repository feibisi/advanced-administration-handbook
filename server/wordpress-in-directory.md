---
title: "Giving WordPress Its Own Directory"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Server
    - Repos
    - Data
---

# Giving WordPress Its Own Directory

Many people want WordPress to power their website's root (e.g. https://example.com) but they don't want all of the WordPress files cluttering up their root directory. WordPress allows you to install it into a subdirectory, but have your website served from the website root.

**Note:** This guide covers configuration for Apache (`.htaccess`), nginx (server blocks), and IIS (`web.config`).

As of [Version 3.5](https://wordpress.org/documentation/wordpress-version/version-3-5/), Multisite users may use all of the functionality listed below. If you are running a version of WordPress older than 3.5, please update before installing a Multisite WordPress install on a subdirectory.

**Note to theme/plugin developers:** this will not separate your code from WordPress. Themes and plugins will still reside under `wp-content` folder.

## 将根目录安装迁移至独立子目录

假设您已在 `example.com` 安装了 WordPress。现在有两种方法可将 WordPress 安装迁移至子目录：

1. 不更改站点 URL（保持 `example.com`）
2. 更改站点 URL（将重定向至 `example.com/subdirectory`）

## 方法一（不改变 URL）

1. 在根目录安装 WordPress 后，将根目录下的所有内容移动到子目录中。

### Apache (.htaccess)
2. 在根目录创建一个 `.htaccess` 文件，并放入以下内容（只需修改 `example.com` 和 `my_subdir`）：

```
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{HTTP_HOST} ^(www.)?example.com$
RewriteCond %{REQUEST_URI} !^/my_subdir/
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ /my_subdir/$1
RewriteCond %{HTTP_HOST} ^(www.)?example.com$
RewriteRule ^(/)?$ my_subdir/index.php [L] 
</IfModule>
```

### nginx（服务器块）
2. 将此添加到您的 nginx 服务器块中：

```nginx
location /my_subdir/ {
    try_files $uri $uri/ /my_subdir/index.php?$args;
}

location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.*)$;
    fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}
```

就是这样 🙂

## 方法二（修改 URL）

### 迁移步骤

_(注：如果您已在子目录中安装了 WordPress，某些步骤可能已自动完成)。_

1.  为 WordPress 核心文件创建新的存储位置——在示例中我们将使用 `/wordpress`。在 Linux 系统中，请在您的 `www` 目录下使用 `mkdir wordpress` 命令。您可能需要对创建的 `wordpress` 目录使用 `chown apache:apache` 命令。
2.  进入 [常规设置](https://wordpress.org/documentation/article/administration-screens/#settings-configuration-settings) 页面。
3.  在 **WordPress 地址 (URL):** 中，设置您的主要 WordPress 核心文件地址。例如：`https://example.com/wordpress`。
4.  在 **站点地址 (URL):** 中，设置根目录的 URL。例如：`https://example.com`。
5.  点击 **保存更改**。暂时不要理会出现的错误！请继续阅读。
6.  现在，将您的 WordPress 核心文件（从根目录）移动到子目录中。
7.  将 `index.php` 和 `.htaccess` 文件从 WordPress 目录**复制**（而非移动！）到您站点的根目录（博客地址）。`.htaccess` 文件是隐藏的，因此您可能需要设置 FTP 客户端以[显示隐藏文件](https://developer.wordpress.org/advanced-administration/server/file-permissions/#Unhide_the_hidden_files)。如果您没有使用[固定链接](https://wordpress.org/documentation/article/using-permalinks/#using-pretty-permalinks)，那么您可能没有 `.htaccess` 文件。_**如果您在 Windows (IIS) 服务器上运行 WordPress** 并且使用了固定链接，您的 WordPress 目录中将有一个 `web.config` 文件，而不是 `.htaccess` 文件。对于 `index.php` 文件，操作说明相同，将其**复制**（而非移动）到根目录。`web.config` 文件必须与 `.htaccess` 文件区别对待，因此您必须**移动**（而非复制）`web.config` 文件到根目录。_
8.  在[文本编辑器](https://wordpress.org/documentation/article/glossary#text-editor)中打开根目录的 `index.php` 文件。
9.  更改以下内容并保存文件。将这一行：`require dirname( __FILE__ ) . '/wp-blog-header.php';` 更改为以下内容，使用您的 WordPress 核心文件目录名：`require dirname( __FILE__ ) . '/wordpress/wp-blog-header.php';`。
10. 登录到新位置。现在可能是 `https://example.com/wordpress/wp-admin/`。
11. 如果您设置了[固定链接](https://wordpress.org/documentation/article/using-permalinks/)，请前往[固定链接设置](https://wordpress.org/documentation/article/administration-screens/#permalinks)页面并更新您的固定链接结构。如果文件权限适当，WordPress 将自动更新您的 `.htaccess` 文件。如果 WordPress 无法写入您的 `.htaccess` 文件，它将向您显示新的重写规则，您应手动将其复制到您的 `.htaccess` 文件中（与主 `index.php` 文件在同一目录）。

### .htaccess 修改

在某些情况下，有些人喜欢将不同版本安装在子目录中（例如 `/2010`、`/2011`、`/latest` 等），并希望网站（默认情况下）使用最新版本。那么，将 WordPress 安装在子目录中，例如 `/my_subdir`，并在根目录的 .htaccess 文件中添加以下内容（只需根据需要修改文字）：

**Apache (.htaccess):**
```
RewriteEngine On
RewriteCond %{HTTP_HOST} ^(www.)?example.com$
RewriteRule ^(/)?$ my_subdir\[L\]
```

**nginx (server block):**
```nginx
location = / {
    return 301 /my_subdir/;
}

location /my_subdir/ {
    try_files $uri $uri/ /my_subdir/index.php?$args;
}

location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.*)$;
    fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}
```

现在，当用户访问您的根域名（`example.com`）时，它将自动重定向到您指定的子目录。

注意：此代码来自 Site 5 的这篇文章：[如何使用 .htaccess 将您的域名重定向到子文件夹](https://qa.site5.com/advanced/how-to-redirect-your-domain-to-a-subfolder-using-htaccess/)。

## 移动特定 WordPress 文件夹

以下链接解释了如何更改 WordPress 内的特定目录：

* [移动 wp-content 文件夹](https://wordpress.org/documentation/article/editing-wp-config-php/#moving-wp-content-folder)
* [移动插件文件夹](https://wordpress.org/documentation/article/editing-wp-config-php/#moving-plugin-folder)
* [移动主题文件夹](https://wordpress.org/documentation/article/editing-wp-config-php/#moving-themes-folder)
* [移动上传文件夹](https://wordpress.org/documentation/article/editing-wp-config-php/#moving-uploads-folder)

## 另请参阅

* [使用 Caddy 为 WordPress 分配独立目录](https://caddy.community/t/using-caddy-to-give-wordpress-its-own-directory/13185)