---
title: "Apache HTTPD / .htaccess"
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

# Apache HTTPD / .htaccess

## .htaccess

`.htaccess` 是一个分布式配置文件，Apache 通过它实现基于目录的配置变更。

WordPress 利用此文件来调整 Apache 从其根目录及子目录提供文件的方式。最显著的是，WP 会修改此文件以支持美观的固定链接功能。

本页面可用于恢复损坏的 `.htaccess` 文件（例如因插件异常导致的问题）。

### Basic WP

```
# BEGIN WordPress

RewriteEngine On
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]

# END WordPress
```

### 多站点

#### WordPress 3.5 及以上版本

如果您在 WordPress 3.5 或更高版本上启用了多站点功能，请使用以下方法之一。

##### WordPress >=3.5 Subfolder Example

```
# BEGIN WordPress Multisite
# Using subfolder network type: https://wordpress.org/documentation/article/htaccess/#multisite

RewriteEngine On
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
RewriteBase /
RewriteRule ^index\.php$ - [L]

# add a trailing slash to /wp-admin
RewriteRule ^([_0-9a-zA-Z-]+/)?wp-admin$ $1wp-admin/ [R=301,L]

RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^ - [L]
RewriteRule ^([_0-9a-zA-Z-]+/)?(wp-(content|admin|includes).*) $2 [L]
RewriteRule ^([_0-9a-zA-Z-]+/)?(.*\.php)$ $2 [L]
RewriteRule . index.php [L]

# END WordPress Multisite
```

##### WordPress >=3.5 SubDomain Example

```
# BEGIN WordPress Multisite
# Using subdomain network type: https://wordpress.org/documentation/article/htaccess/#multisite

RewriteEngine On
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
RewriteBase /
RewriteRule ^index\.php$ - [L]

# add a trailing slash to /wp-admin
RewriteRule ^wp-admin$ wp-admin/ [R=301,L]

RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^ - [L]
RewriteRule ^(wp-(content|admin|includes).*) $1 [L]
RewriteRule ^(.*\.php)$ $1 [L]
RewriteRule . index.php [L]

# END WordPress Multisite
```

#### WordPress 3.4 及更低版本

如果您最初安装的是 3.4 或更旧版本的 WordPress 并启用了多站点功能，则需要使用以下配置之一：

##### WordPress <=3.4 子文件夹示例

WordPress 3.0 至 3.4.2

```
# BEGIN WordPress Multisite
# 使用子文件夹网络类型：https://wordpress.org/documentation/article/htaccess/#multisite

RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]

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

# END WordPress Multisite
```

##### WordPress <=3.4 SubDomain Example

```
# BEGIN WordPress Multisite
# Using subdomain network type: https://wordpress.org/documentation/article/htaccess/#multisite

RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]

# uploaded files
RewriteRule ^files/(.+) wp-includes/ms-files.php?file=$1 [L]

RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^ - [L]
RewriteRule . index.php [L]

# END WordPress Multisite
```

### 通用示例

#### 选项

任何以 **+** 开头的选项会被添加到当前生效的选项中，而任何以 **–** 开头的选项会从当前生效的选项中移除。

[Options 指令](https://httpd.apache.org/docs/trunk/mod/core.html#options) 的可能值是以下任意组合：

**None**

所有选项均被关闭。

**All**

除 MultiViews 外的所有选项。这是默认设置。

**ExecCGI**

允许使用 mod_cgi 执行 CGI 脚本。

**FollowSymLinks**

服务器将跟踪此目录中的符号链接。

**Includes**

允许使用 mod_include 提供的服务器端包含。

**IncludesNOEXEC**

允许服务器端包含，但禁用 #exec cmd 和 #exec cgi。

**Indexes**

URL 映射到一个目录，且没有 DirectoryIndex 时，显示目录的格式化列表。

**MultiViews**

允许使用 mod_negotiation 进行内容协商的“MultiViews”。

**SymLinksIfOwnerMatch**

仅当目标与链接具有相同用户 ID 时才跟踪符号链接。

以下示例将禁用所有选项，然后仅启用 FollowSymLinks（这是 mod\_rewrite 所必需的）。

```
Options None
Options FollowSymLinks
```

#### DirectoryIndex

[DirectoryIndex 指令](https://httpd.apache.org/docs/trunk/mod/mod_dir.html#directoryindex) 用于设置当请求一个目录时，Apache 将提供的文件。

可以指定多个 URL，服务器将返回它找到的第一个文件。

```
DirectoryIndex index.php index.html /index.php
```

#### DefaultLanguage

[DefaultLanguage 指令](https://httpd.apache.org/docs/trunk/mod/mod_mime.html#defaultlanguage) 将使所有未关联特定语言标签的文件使用此默认语言。

```
DefaultLanguage en
```

#### 默认字符集

设置 HTTP 头中发送的默认字符编码。请参阅[在 .htaccess 中设置字符集信息](https://www.w3.org/International/questions/qa-htaccess-charset)

```
AddDefaultCharset UTF-8
```

**为特定文件设置字符集**

```
AddType 'text/html; charset=UTF-8' .html
```

**为特定文件设置**

```
AddCharset UTF-8 .html
```

#### ServerSignature

[ServerSignature 指令](https://httpd.apache.org/docs/trunk/mod/core.html#serversignature) 允许配置服务器生成文档的页脚行。可选择性地在服务器生成的页面（内部错误文档、FTP 目录列表、mod_status 和 mod_info 输出等，但不包括 CGI 生成的文档或自定义错误文档）中添加一行，包含服务器版本和虚拟主机名。

**On**

添加一行，包含服务器版本号和提供服务的虚拟主机的 ServerName

**Off**

隐藏页脚行

**Email**

创建一个指向被引用文档的 ServerAdmin 的 "mailto:" 链接

```
SetEnv SERVER_ADMIN admin@site.com
ServerSignature Email
```

#### 强制文件下载

以下设置将导致任何以指定扩展名结尾的文件请求不会在浏览器中显示，而是强制弹出“另存为”对话框，以便客户端可以下载。

```
AddType application/octet-stream .avi .mpg .mov .pdf .xls .mp4
```

#### HTTP 压缩

[AddOutputFilter 指令](https://httpd.apache.org/docs/trunk/mod/mod_mime.html#addoutputfilter) 将文件扩展名映射到过滤器，这些过滤器会在服务器响应发送给客户端之前对其进行处理。这是对其他地方定义的过滤器（包括 `SetOutputFilter` 和 `AddOutputFilterByType`）的补充。此映射会与任何已生效的映射合并，并覆盖同一扩展名的任何现有映射。

另请参阅 [启用压缩](https://developers.google.com/speed/docs/insights/EnableCompression)

```
AddOutputFilterByType DEFLATE text/html text/plain text/xml application/xml application/xhtml+xml text/javascript text/css application/x-javascript
BrowserMatch ^Mozilla/4 gzip-only-text/html
BrowserMatch ^Mozilla/4\.0[678] no-gzip
BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
```

**强制压缩特定文件**

```
SetOutputFilter DEFLATE
```

#### 发送自定义 HTTP 头

[Header 指令](https://httpd.apache.org/docs/trunk/mod/mod_headers.html#header)允许您为每个请求或特定文件发送 HTTP 头。您可以使用 [Firebug](https://getfirebug.com/)、[Chrome Dev Tools](https://developer.chrome.com/docs/devtools/)、[Wireshark](https://www.wireshark.org/) 或 [Advanced HTTP Request / Response Headers](https://www.askapache.com/online-tools/http-headers-tool/) 查看站点的 HTTP 头。

```
Header set X-Pingback "https://example.com/xmlrpc.php"
Header set Content-Language "en-US"
```

#### 取消设置 HTTP 头信息

这将取消设置 HTTP 头信息，使用 **always** 会额外努力地尝试移除它们。

```
Header unset Pragma
Header always unset WP-Super-Cache
Header always unset X-Pingback
```

#### 密码保护登录

此功能对保护 `wp-login.php` 文件非常有用。你可以使用这个 [高级 Htpasswd/Htdigest 文件生成器](https://www.askapache.com/online-tools/htpasswd-generator/)。

**基本身份验证**

```
AuthType Basic
AuthName "Password Protected"
AuthUserFile /full/absolute/path/to/.htpasswd
Require valid-user
Satisfy All
```

**摘要式身份验证**

```
AuthType Digest
AuthName "Password Protected"
AuthDigestDomain /wp-login.php https://example.com/wp-login.php
AuthUserFile /full/absolute/path/to/.htpasswd
Require valid-user
Satisfy All
```

#### 要求特定 IP

这是一种仅允许所列 IP 地址访问的方法。注意此处使用 RequireAny 而非 RequireAll。

```
<RequireAny>
  Require ip 192.0.2.123
  Require ip 2001:0DB8:1111:2222:3333:4444:5555:6666
</RequireAny>
```

#### 保护敏感文件

此设置将禁止所有网络访问您的 wp-config 文件、htaccess/htpasswd 以及 WordPress debug.log。对于已安装的站点，建议同时添加 install.php。

```
<FilesMatch "^(wp-config\.php|\.htaccess|\.htpasswd|debug\.log)$">
  Require all denied
</FilesMatch>
```

#### 强制使用 SSL

这将强制使用 SSL，并要求精确匹配主机名，否则将重定向到 SSL 版本。适用于 `/wp-admin/.htaccess` 文件。

```
SSLOptions +StrictRequire
SSLRequireSSL
SSLRequire %{HTTP_HOST} eq "www.example.com"
ErrorDocument 403 https://www.example.com
```

### 外部资源

* [Apache HTTP 服务器官方教程：.htaccess 文件](https://httpd.apache.org/docs/trunk/howto/htaccess.html)
* [官方 Htaccess 指令快速参考](https://httpd.apache.org/docs/trunk/mod/quickreference.html)
* [Htaccess 教程](https://www.askapache.com/htaccess/)
* [Google PageSpeed 开发者指南](https://developers.google.com/speed/docs/insights/rules)
* [愚蠢的 Htaccess 技巧](https://perishablepress.com/stupid-htaccess-tricks/)
* [高级 Mod_Rewrite](https://www.askapache.com/htaccess/crazy-advanced-mod_rewrite-tutorial/)

### 另请参阅

* [子目录的 htaccess](https://codex.wordpress.org/htaccess%20for%20subdirectories)
* [使用固定链接](https://wordpress.org/documentation/article/customize-permalinks/)
* [更改文件权限](https://wordpress.org/documentation/article/changing-file-permissions/)
* [UNIX Shell 技能](https://codex.wordpress.org/UNIX%20Shell%20Skills)
* [Rewrite API](https://codex.wordpress.org/Rewrite%20API)