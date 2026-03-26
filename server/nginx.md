---
title: "Nginx"
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

# Nginx

虽然 LAMP 堆栈（Linux + Apache + MySQL + PHP）在驱动 WordPress 方面非常流行，但使用 Nginx 也是可行的。WordPress 支持 Nginx，一些大型 WordPress 站点（例如 WordPress.com）就是由 Nginx 驱动的。

在讨论 Nginx 时，重要的是要了解有多种实现 Nginx 的方式。它可以设置为 Apache 前面的反向代理，这是一种非常强大的设置，允许您使用 Apache 的所有功能和优势，同时受益于 Nginx 的速度。大多数报告使用 Nginx 作为服务器的网站（基于从 HTTP 响应头收集的统计数据），实际上是 Apache 与 Nginx 作为反向代理一起运行。（显示“Nginx”的 HTTP 响应头是由反向代理报告的，而不是服务器本身。）

**本指南指的是独立的 Nginx 设置，其中 Nginx 被用作主要服务器，而不是 Apache。** 需要注意的是，Nginx 并非 Apache 的完全可互换替代品。在继续之前，您需要了解一些影响 WordPress 实现的关键差异：

- 使用 Nginx 时，没有像 Apache 的 .htaccess 或 IIS 的 web.config 文件那样的目录级配置文件。所有配置都必须在服务器级别由管理员完成，WordPress 无法像在 Apache 或 IIS 中那样修改配置。
- 运行 Nginx 时，美观固定链接功能略有不同。
- 由于 Nginx 没有 .htaccess 类型的功能，并且 WordPress 无法自动为您修改服务器配置，因此它无法为您生成重写规则。
- 如果不修改您的安装，“index.php”将被添加到您的固定链接中。（可以通过插件（见下文）和/或在子主题的 functions.php 中添加自定义代码来缓解此问题。）
- 但是，如果您确实希望拥有一些（有限的）.htaccess 功能，从技术上讲，可以通过安装 [PHP 的 htscanner PECL 扩展](https://www.php.net/manual/en/book.htscanner.php) 来实现。（然而，这不是一个完美的解决方案，因此请务必在使用前在生产站点上进行彻底测试和调试。）

本指南不会涵盖如何安装和配置 Nginx，因此假设您已经安装了 Nginx 并对其操作和调试有基本的了解。

## 通用与多站点支持

要让 WordPress 在 Nginx 上运行，您需要配置后端 php-cgi。可用的选项是 `fastcgi` 或 `php-fpm`。这里使用 php-fpm，因为它包含在 PHP 5.3+ 中，所以安装起来很简单。

Nginx 配置已拆分为五个独立的文件，并添加了大量注释，以便更容易理解每个选项。[作者](https://wordpress.org/support/users/bigsite/)也尽力遵循 Nginx 配置的“最佳实践”。

### Main (generic) startup file

This is equivalent to /etc/nginx/nginx.conf (or /etc/nginx/conf/nginx.conf if you’re using Arch Linux).

```
# Generic startup file.
user {user} {group};

#usually equal to number of CPUs you have. run command "grep processor /proc/cpuinfo | wc -l" to find it
worker_processes  auto;
worker_cpu_affinity auto;

error_log  /var/log/nginx/error.log;
pid        /var/run/nginx.pid;

# Keeps the logs free of messages about not being able to bind().
#daemon     off;

events {
    worker_connections  1024;
}

http {
    #rewrite_log on;

    include mime.types;
    default_type       application/octet-stream;
    access_log         /var/log/nginx/access.log;
    sendfile           on;
    #tcp_nopush         on;
    keepalive_timeout  3;
    #tcp_nodelay        on;
    #gzip               on;
    #php max upload limit cannot be larger than this
    client_max_body_size 13m;
    index              index.php index.html index.htm;

    # Upstream to abstract backend connection(s) for PHP.
    upstream php {
        #this should match value of "listen" directive in php-fpm pool
        server unix:/tmp/php-fpm.sock;
        # server 127.0.0.1:9000;
    }

    include sites-enabled/*;
}
```

### Per Site configuration

```
# Redirect everything to the main site. We use a separate server statement and NOT an if statement - see https://www.nginx.com/resources/wiki/start/topics/depth/ifisevil/

server {
    server_name  _;
    return 302 $scheme://example.com$request_uri;
}

server {
    server_name example.com;
    root /var/www/example.com;

    index index.php;

    include global/restrictions.conf;

    # Additional rules go here.

    # Only include one of the files below.
    include global/wordpress.conf;
    # include global/wordpress-ms-subdir.conf;
    # include global/wordpress-ms-subdomain.conf;
}
```

Splitting sections of the configuration into multiple files allows the same logic to be reused over and over. A ‘global’ subdirectory is used to add extra rules for general purpose use (either /etc/nginx/conf/global/ or /etc/nginx/global/ depending on how your nginx install is set up).

### Global restrictions file

```
# Global restrictions configuration file.
# Designed to be included in any server {} block.
location = /favicon.ico {
    log_not_found off;
    access_log off;
}

location = /robots.txt {
    allow all;
    log_not_found off;
    access_log off;
}

# Deny all attempts to access hidden files such as .htaccess, .htpasswd, .DS_Store (Mac).
# Keep logging the requests to parse later (or to pass to firewall utilities such as fail2ban)
location ~ /\. {
    deny all;
}

# Deny access to any files with a .php extension in the uploads directory
# Works in sub-directory installs and also in multisite network
# Keep logging the requests to parse later (or to pass to firewall utilities such as fail2ban)
location ~* /(?:uploads|files)/.*\.php$ {
    deny all;
}
```

### General WordPress rules

For single site installations, here is the `global/wordpress.conf` file:

```
# WordPress single site rules.
# Designed to be included in any server {} block.
# Upstream to abstract backend connection(s) for php
upstream php {
    server unix:/tmp/php-cgi.socket;
    server 127.0.0.1:9000;
}

server {
    ## Your website name goes here.
    server_name domain.tld;
    ## Your only path reference.
    root /var/www/wordpress;
    ## This should be in your http block and if it is, it's not needed here.
    index index.php;

    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }

    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }

    location / {
        # This is cool because no php is touched for static content.
        # include the "?$args" part so non-default permalinks doesn't break when using query string
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        #NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
        include fastcgi.conf;
        fastcgi_intercept_errors on;
        fastcgi_pass php;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
        expires max;
        log_not_found off;
    }
}
```

This is more up-to-date example for Nginx: https://www.nginx.com/resources/wiki/start/topics/recipes/wordpress/

### WordPress 多站点

对于多站点安装，请根据启用多站点时使用的 WordPress 版本以及域名/子目录配置，在 `global/wordpress.conf` 文件中使用以下相应部分。

#### WordPress 3.5 及以上版本

如果您在 WordPress 3.5 或更高版本上启用了多站点功能，请使用以下配置之一。

##### WordPress 3.5 及以上版本子目录配置示例

```
# 适用于 WP 3.5 及以上版本的 WordPress 多站点子目录配置文件。
server {
    server_name example.com ;

    root /var/www/example.com/htdocs;
    index index.php;

    if (!-e $request_filename) {
        rewrite /wp-admin$ $scheme://$host$request_uri/ permanent;
        rewrite ^(/[^/]+)?(/wp-.*) $2 last;
        rewrite ^(/[^/]+)?(/.*\.php) $2 last;
    }

    location / {
        try_files $uri $uri/ /index.php?$args ;
    }

    location ~ \.php$ {
        try_files $uri =404;
        include fastcgi_params;
        fastcgi_pass php;
    }

    # 在此处为静态内容添加一些过期头规则
}
```

##### WordPress 3.5 and up Subdomains Examples

```
# WordPress multisite subdomain config file for WP 3.5 and up.
server {
    server_name example.com *.example.com ;

    root /var/www/example.com/htdocs;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$args ;
    }

    location ~ \.php$ {
        try_files $uri =404;
        include fastcgi_params;
        fastcgi_pass php;
    }

    #add some rules for static content expiry-headers here
}
```

#### WordPress 3.4 and below

If you originally activated Multisite with WordPress with 3.4 or older, you need to use one of these:

##### WordPress <=3.4 Subdirectory Examples

```
# WordPress multisite subdirectory config file for WP 3.4 and below.

map $uri $blogname{
    ~^(?P<blogpath>/[^/]+/)files/(.*)       $blogpath ;
}

map $blogname $blogid{
    default -999;

    #Ref: https://wordpress.org/extend/plugins/nginx-helper/
    #include /var/www/wordpress/wp-content/plugins/nginx-helper/map.conf ;
}

server {
    server_name example.com ;

    root /var/www/example.com/htdocs;
    index index.php;

    location ~ ^(/[^/]+/)?files/(.+) {
        try_files /wp-content/blogs.dir/$blogid/files/$2 /wp-includes/ms-files.php?file=$2 ;
        access_log off;     log_not_found off; expires max;
    }

    #avoid php readfile()
    location ^~ /blogs.dir {
        internal;
        alias /var/www/example.com/htdocs/wp-content/blogs.dir ;
        access_log off;     log_not_found off; expires max;
    }

    if (!-e $request_filename) {
        rewrite /wp-admin$ $scheme://$host$request_uri/ permanent;
        rewrite ^(/[^/]+)?(/wp-.*) $2 last;
        rewrite ^(/[^/]+)?(/.*\.php) $2 last;
    }

    location / {
        try_files $uri $uri/ /index.php?$args ;
    }

    location ~ \.php$ {
        try_files $uri =404;
        include fastcgi_params;
        fastcgi_pass php;
    }

    #add some rules for static content expiry-headers here
}
```

NGINX provides 2 special directive: X-Accel-Redirect and map. Using these 2 directives, one can eliminate performance hit for static-file serving on WordPress multisite network.

##### WordPress <=3.4 Subdomains Examples

```
# WordPress multisite subdomain config file for WP 3.4 and below.
map $http_host $blogid {
    default       -999;

    #Ref: https://wordpress.org/extend/plugins/nginx-helper/
    #include /var/www/wordpress/wp-content/plugins/nginx-helper/map.conf ;

}

server {
    server_name example.com *.example.com ;

    root /var/www/example.com/htdocs;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$args ;
    }

    location ~ \.php$ {
        try_files $uri =404;
        include fastcgi_params;
        fastcgi_pass php;
    }

    #WPMU Files
        location ~ ^/files/(.*)$ {
            try_files /wp-content/blogs.dir/$blogid/$uri /wp-includes/ms-files.php?file=$1 ;
            access_log off; log_not_found off; expires max;
        }

    #WPMU x-sendfile to avoid php readfile()
    location ^~ /blogs.dir {
        internal;
        alias /var/www/example.com/htdocs/wp-content/blogs.dir;
        access_log off;     log_not_found off;      expires max;
    }

    #add some rules for static content expiry-headers here
}
```

Ref: https://www.nginx.com/resources/wiki/start/topics/recipes/wordpress/

### HTTPS in Nginx

Enabling HTTPS in Nginx is relatively simple.

```
server {
    # listens both on IPv4 and IPv6 on 443 and enables HTTPS and HTTP/2 support.
    # HTTP/2 is available in nginx 1.9.5 and above.
    listen *:443 ssl;
    listen [::]:443 ssl;
    http2 on;

    # indicate locations of SSL key files.
    ssl_certificate /srv/www/ssl/ssl.crt;
    ssl_certificate_key /srv/www/ssl/ssl.key;
    ssl_dhparam /srv/www/master/ssl/dhparam.pem;

    # indicate the server name
    server_name example.com *.example.com;

    # Enable HSTS. This forces SSL on clients that respect it, most modern browsers. The includeSubDomains flag is optional.
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains";

    # Set caches, protocols, and accepted ciphers. This config will merit an A+ SSL Labs score as of Sept 2015.
    ssl_session_cache shared:SSL:20m;
    ssl_session_timeout 10m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_ciphers 'ECDH+AESGCM:ECDH+AES256:ECDH+AES128:DH+3DES:!ADH:!AECDH:!MD5';
}
```

Mozilla offers an [excellent SSL config generation tool](https://mozilla.github.io/server-side-tls/ssl-config-generator/) as well.

### WP Super Cache Rules

```
# WP Super Cache rules.
# Designed to be included from a 'wordpress-ms-...' configuration file.

set $cache_uri $request_uri;

# POST requests and urls with a query string should always go to PHP
if ($request_method = POST) {
    set $cache_uri 'null cache';
}

if ($query_string != "") {
    set $cache_uri 'null cache';
}

# Don't cache uris containing the following segments
if ($request_uri ~* "(/wp-admin/|/xmlrpc.php|/wp-(app|cron|login|register|mail).php|wp-.*.php|/feed/|index.php|wp-comments-popup.php|wp-links-opml.php|wp-locations.php|sitemap(_index)?.xml|[a-z0-9_-]+-sitemap([0-9]+)?.xml)") {
    set $cache_uri 'null cache';
}

# Don't use the cache for logged in users or recent commenters
if ($http_cookie ~* "comment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_logged_in") {
    set $cache_uri 'null cache';
}

# START MOBILE
# Mobile browsers section to server them non-cached version. COMMENTED by default as most modern wordpress themes including twenty-eleven are responsive. Uncomment config lines in this section if you want to use a plugin like WP-Touch
# if ($http_x_wap_profile) {
    # set $cache_uri 'null cache';
#}

#if ($http_profile) {
    # set $cache_uri 'null cache';
#}

#if ($http_user_agent ~* (2.0\ MMP|240x320|400X240|AvantGo|BlackBerry|Blazer|Cellphone|Danger|DoCoMo|Elaine/3.0|EudoraWeb|Googlebot-Mobile|hiptop|IEMobile|KYOCERA/WX310K|LG/U990|MIDP-2.|MMEF20|MOT-V|NetFront|Newt|Nintendo\ Wii|Nitro|Nokia|Opera\ Mini|Palm|PlayStation\ Portable|portalmmm|Proxinet|ProxiNet|SHARP-TQ-GX10|SHG-i900|Small|SonyEricsson|Symbian\ OS|SymbianOS|TS21i-10|UP.Browser|UP.Link|webOS|Windows\ CE|WinWAP|YahooSeeker/M1A1-R2D2|iPhone|iPod|Android|BlackBerry9530|LG-TU915\ Obigo|LGE\ VX|webOS|Nokia5800)) {
    # set $cache_uri 'null cache';
#}

#if ($http_user_agent ~* (w3c\ |w3c-|acs-|alav|alca|amoi|audi|avan|benq|bird|blac|blaz|brew|cell|cldc|cmd-|dang|doco|eric|hipt|htc_|inno|ipaq|ipod|jigs|kddi|keji|leno|lg-c|lg-d|lg-g|lge-|lg/u|maui|maxo|midp|mits|mmef|mobi|mot-|moto|mwbp|nec-|newt|noki|palm|pana|pant|phil|play|port|prox|qwap|sage|sams|sany|sch-|sec-|send|seri|sgh-|shar|sie-|siem|smal|smar|sony|sph-|symb|t-mo|teli|tim-|tosh|tsm-|upg1|upsi|vk-v|voda|wap-|wapa|wapi|wapp|wapr|webc|winw|winw|xda\ |xda-)) {
    # set $cache_uri 'null cache';
#}
#END MOBILE

# Use cached or actual file if they exists, otherwise pass request to WordPress
location / {
    try_files /wp-content/cache/supercache/$http_host/$cache_uri/index.html $uri $uri/ /index.php?$args ;
}
```

**Experimental modifications:**

If you are using HTTPS, the latest development version of WP Super Cache may use a different directory structure to differentiate between HTTP and HTTPS. try_files line may look like below:

```
location / {
    try_files /wp-content/cache/supercache/$http_host/$cache_uri/index-https.html $uri $uri/ /index.php?$args ;
}
```

### W3 Total Cache 规则

W3 Total Cache 根据 WordPress 配置，对基于磁盘的缓存存储使用不同的目录结构。

缓存验证检查将保持通用，如下所示：

```
#W3 TOTAL CACHE 检查
set $cache_uri $request_uri;

# POST 请求和带有查询字符串的 URL 应始终交给 PHP 处理
if ($request_method = POST) {
    set $cache_uri 'null cache';
}
if ($query_string != "") {
    set $cache_uri 'null cache';
}

# 不缓存包含以下片段的 URI
if ($request_uri ~* "(/wp-admin/|/xmlrpc.php|/wp-(app|cron|login|register|mail).php|wp-.*.php|/feed/|index.php|wp-comments-popup.php|wp-links-opml.php|wp-locations.php|sitemap(_index)?.xml|[a-z0-9_-]+-sitemap([0-9]+)?.xml)") {
    set $cache_uri 'null cache';
}

# 不对登录用户或最近评论者使用缓存
if ($http_cookie ~* "comment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_logged_in") {
    set $cache_uri 'null cache';
}
#从上面的 WP SUPER CACHE 部分添加移动端规则

#从下方追加一个代码块...
```

**对于普通 WordPress（非多站点）**

使用以下配置：

```
# 如果存在缓存文件或实际文件则使用，否则将请求传递给 WordPress
location / {
    try_files /wp-content/w3tc/pgcache/$cache_uri/_index.html $uri $uri/ /index.php?$args ;
}
```

**对于使用子目录的多站点**
使用以下配置：

```
if ( $request_uri ~* "^/([_0-9a-zA-Z-]+)/.*" ){
    set $blog $1;
}

set $blog "${blog}.";

if ( $blog = "blog." ){
    set $blog "";
}

# 如果存在缓存文件或实际文件则使用，否则将请求传递给 WordPress
location / {
    try_files /wp-content/w3tc-$blog$host/pgcache$cache_uri/_index.html $uri $uri/ /index.php?$args ;
}
```

**对于使用子域名/域名映射的多站点**
使用以下配置：

```
location / {
    try_files /wp-content/w3tc-$host/pgcache/$cache_uri/_index.html $uri $uri/ /index.php?$args;
}
```

注意事项

- Nginx 可以自动处理 gzip 和浏览器缓存，因此最好将这部分交给 nginx。
- W3 Total Cache 的 Minify 规则与上述配置配合使用不会有任何问题。

## Nginx fastcgi_cache

Nginx can perform caching on its own end to reduce load on your server. When you want to use Nginx’s built-in fastcgi_cache, you better compile nginx with [fastcgi_cache_purge](https://github.com/FRiCKLE/ngx_cache_purge) module. It will help nginx purge cache for a page when it gets edited. On the WordPress side, you need to install a plugin like [Nginx Helper](https://wordpress.org/plugins/nginx-helper/) to utilize fastcgi_cache_purge feature.

Config will look like below:

**Define a Nginx cache zone in http{…} block, outside server{…} block**

```
#move next 3 lines to /etc/nginx/nginx.conf if you want to use fastcgi_cache across many sites
fastcgi_cache_path /var/run/nginx-cache levels=1:2 keys_zone=WORDPRESS:500m inactive=60m;
fastcgi_cache_key "$scheme$request_method$host$request_uri";
fastcgi_cache_use_stale error timeout invalid_header http_500;
```

**For WordPress site config, in server{..} block add a cache check block as follow**

```
#fastcgi_cache start
set $no_cache 0;

# POST requests and urls with a query string should always go to PHP
if ($request_method = POST) {
    set $no_cache 1;
}
if ($query_string != "") {
    set $no_cache 1;
}

# Don't cache uris containing the following segments
if ($request_uri ~* "(/wp-admin/|/xmlrpc.php|/wp-(app|cron|login|register|mail).php|wp-.*.php|/feed/|index.php|wp-comments-popup.php|wp-links-opml.php|wp-locations.php|sitemap(_index)?.xml|[a-z0-9_-]+-sitemap([0-9]+)?.xml)") {
        set $no_cache 1;
}

# Don't use the cache for logged in users or recent commenters
if ($http_cookie ~* "comment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_no_cache|wordpress_logged_in") {
        set $no_cache 1;
}
```

**Then make changes to PHP handling block**

Just add this to the following php block. Note the line fastcgi_cache_valid 200 60m; which tells nginx only to cache 200 responses(normal pages), which means that redirects are not cached. This is important for multilanguage sites where, if not implemented, nginx would cache the main url in one language instead of redirecting users to their respective content according to their language.

```
fastcgi_cache_bypass $no_cache;
fastcgi_no_cache $no_cache;

fastcgi_cache WORDPRESS;
fastcgi_cache_valid 200 60m;
```

Such that it becomes something like this

```
location ~ [^/]\.php(/|$) {
    fastcgi_split_path_info ^(.+?\.php)(/.*)$;
    if (!-f $document_root$fastcgi_script_name) {
        return 404;
    }
    # This is a robust solution for path info security issue and works with "cgi.fix_pathinfo = 1" in /etc/php.ini (default)

    include fastcgi.conf;
    fastcgi_index index.php;
    # fastcgi_intercept_errors on;
    fastcgi_pass php;

    fastcgi_cache_bypass $no_cache;
    fastcgi_no_cache $no_cache;

    fastcgi_cache WORDPRESS;
    fastcgi_cache_valid 200 60m;
}
```

**Finally add a location for conditional purge**

```
location ~ /purge(/.*) {
    # Uncomment the following two lines to allow purge only from the webserver
    # allow 127.0.0.1;
    # deny all;

    fastcgi_cache_purge WORDPRESS "$scheme$request_method$host$1";
}
```

If you get an ‘unknown directive “fastcgi_cache_purge”‘ error check that your Nginx installation has fastcgi_cache_purge module.

## 提升多站点静态文件性能（WP <= 3.4）

默认情况下，在 3.5 版本之前激活的多站点网络中，静态文件请求会调用 PHP 处理，即通过 `ms-files.php` 文件。使用 Nginx 的 `Map{..}` 指令可以获得更好的性能。

在您站点的 Nginx 配置中，于 `server{..}` 块上方添加如下部分：

```
map $http_host $blogid {
    default               0;

    example.com           1;
    site1.example.com     2;
    site1.com             2;
}
```

这只是一个站点名称与博客 ID 的对应列表。您可以使用 [Nginx helper](https://wordpress.org/extend/plugins/nginx-helper) 插件来获取这样的站点名称/博客 ID 对列表。该插件还会生成一个 `map.conf` 文件，您可以直接将其包含在 `map{}` 部分中，如下所示：

```
map $http_host $blogid {
    default               0;

    include /path/to/map.conf ;
}
```

创建 `map{..}` 部分后，您只需在 Nginx 配置中再做一个更改，以便对 /files/ 的请求首先使用 nginx `map{..}` 处理：

```
location ~ ^/files/(.*)$ {
    try_files /wp-content/blogs.dir/$blogid/$uri /wp-includes/ms-files.php?file=$1 ;
    access_log off; log_not_found off; expires max;
}
```

注意事项

- 每当创建新站点、删除站点或将额外域名映射到现有站点时，Nginx helper 会自动更新 map.conf 文件，但您仍需要手动重新加载 Nginx 配置。您可以稍后随时进行。在此之前，只有新站点的文件将通过 php-fpm 提供。
- 此方法不会生成任何符号链接。因此，不会出现因意外删除或备份脚本跟随符号链接而导致的问题。
- 对于大型网络，这将具有良好的扩展性，因为只有一个 map.conf 文件。

最后几点重要说明：整个设置假设站点的根目录是博客，并且所有引用的文件都位于主机上。如果您将博客放在子目录中（例如 /blog），则必须修改规则。也许有人可以借鉴这些规则，使其能够实现，例如，在主 'server' 块中使用：

```
set $wp_subdir "/blog";
```

指令，并使其自动应用于通用的 WordPress 规则。

## 警告

[全局限制文件](https://developer.wordpress.org/advanced-administration/server/web-server/nginx/#global-restrictions-file)中的拼写错误可能造成安全漏洞。要测试您的“uploads”目录是否真正受到保护，请创建一个包含某些内容的 PHP 文件（例如：<?php phpinfo(); ?>），将其上传到“uploads”目录（或其子目录），然后尝试从浏览器访问（执行）它。

## 资源

### 参考

- [nginx + php-fpm + PHP APC + WordPress 多站点（子目录）+ WP Super Cache](https://wordpress.org/support/topic/nginx-php-fpm-php-apc-wordpress-multisite-subdirectory-wp-super-cache/)。

### External Links

- [Nginx WordPress wiki page](https://www.nginx.com/resources/wiki/start/topics/recipes/wordpress/)
- [LEMP guides on Linode’s Library](https://www.linode.com/docs/guides/web-servers/lemp/)
- [Various guides about Nginx on Linode’s Library](https://www.linode.com/docs/guides/web-servers/nginx/)
- [Lightning fast WordPress with Php-fpm and Nginx](https://www.sitepoint.com/lightning-fast-wordpress-with-php-fpm-and-nginx/)
- [Virtual Hosts Examples](https://wiki.nginx.org/VirtualHostExample)
- [List of 20+ WordPress-Nginx Tutorials for common situations](https://rtcamp.com/wordpress-nginx/tutorials/)
- [An introduction to Nginx configuration](https://blog.martinfjordvald.com/nginx-primer/)
- [A comprehensive blog series on hosting WordPress yourself using Nginx](https://deliciousbrains.com/hosting-wordpress-setup-secure-virtual-server/)
- [WordPress Installation CentminMod](https://centminmod.com/nginx_configure_wordpress.html)
- [Nginx WordPress Installation Guide](https://thecustomizewindows.com/2015/12/nginx-wordpress-installation-guide-steps/)

### 脚本与工具

对于 WordPress 的 Nginx 脚本化安装，CentOS 系统可使用 [CentminMod](https://centminmod.com/nginx_configure_wordpress.html)。

### 加固 Nginx

- [加固 Nginx 与 PHP](http://kbeezie.com/securing-nginx-php/)
- [配置 PHP-FastCGI 与 nginx？别轻信教程：检查你的配置！](https://nealpoole.com/blog/2011/04/setting-up-php-fastcgi-and-nginx-dont-trust-the-tutorials-check-your-configuration/)