---
title: "缓存"
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

# 缓存

本文是 [WordPress 优化](https://developer.wordpress.org/advanced-administration/performance/optimization/)系列文章的一部分。

WordPress 缓存是提升性能的最快方式。如果你的网站**此刻**正面临高负载，请立即安装 [W3 Total Cache](https://wordpress.org/plugins/w3-total-cache/)、[WP Super Cache](https://wordpress.org/plugins/wp-super-cache/) 或 [Cache Enabler](https://wordpress.org/extend/plugins/cache-enabler/)。

## 缓存插件 {#caching-plugins}

诸如 [W3 Total Cache](https://wordpress.org/plugins/w3-total-cache/)、[WP Super Cache](https://wordpress.org/plugins/wp-super-cache/) 和 [Cache Enabler](https://wordpress.org/plugins/cache-enabler/) 等插件可以轻松安装，并将您的 WordPress 文章和页面缓存为静态文件。这些静态文件随后会提供给用户，从而减轻服务器的处理负载。对于相当静态的页面，这可以将性能提升数百倍。

当与系统级页面缓存（如 Varnish）结合使用时，效果会非常强大。

如果您的文章/页面包含大量动态内容，配置缓存可能会更加复杂。搜索 "WordPress cache plugin" 以获取更多信息。

## 浏览器缓存 {#browser-caching}

**浏览器缓存** 通过减少每个页面的请求数量，有助于降低服务器负载。例如，通过对不常更改的文件（如图片、CSS、JavaScript 等静态文件）设置正确的文件头，浏览器会在访问者的计算机上缓存这些文件。这项技术使浏览器能够检查文件是否已更改，而不是简单地请求它们。结果是您的 Web 服务器可以返回更多的 304 响应（确认文件未更改），而不是需要发送文件的 200 响应。

了解更多信息，请查阅 HTTP Cache-Control（特别是 **max-age**）和 Expires 头，以及 [实体标签](https://en.wikipedia.org/wiki/HTTP_ETag)。

## 对象缓存 {#object-caching}

WordPress 中的对象缓存是指将数据从检索成本高且速度慢的位置移动到检索成本低且速度快的位置。对象缓存通常还具有持久性，这意味着在一次请求中缓存的数据在后续请求中仍然可用。

除了使数据访问更加容易之外，缓存的数据应始终是可替换和可再生的。如果应用程序遇到数据库损坏（例如 MySQL、Postgres、Couchbase），该数据库将会并应该承担严重后果（但愿有良好的备份计划）。与应用程序的主数据存储不同，如果缓存损坏，应用程序应继续运行，因为缓存数据应能自行再生。虽然缓存再生期间可能会出现一些性能问题，但不会丢失任何数据。

对象缓存的存储引擎可以采用多种技术。流行的对象缓存引擎包括 Redis、Memcached、APC 和文件系统。使用的缓存引擎应根据应用程序的需求来决定。每种引擎都有其优缺点。至少，所使用的引擎应使数据访问比重新生成数据更具性能优势。

## 服务器缓存 {#server-caching}

**Web 服务器缓存**更为复杂，但用于流量极高的站点。可选的方案很多，超出了本文的讨论范围。最简单的方案从服务器本地缓存开始，而更复杂精细的系统可能会在运行 WordPress 应用程序的 Web 服务器"前端"使用多个缓存服务器（也称为反向代理服务器）。

在服务器上添加像 [Opcache](https://www.php.net/manual/en/book.opcache.php) 这样的操作码缓存，或在 IIS 上使用 [WinCache](https://www.iis.net/downloads/microsoft/wincache-extension)，可以将 PHP 性能提升数倍。

当与 W3TC 等 WordPress 缓存插件配合使用时，[Varnish](https://www.varnish-cache.org/) 缓存功能非常强大。

## 延伸阅读 {#further-reading}

* [WordPress 核心缓存概念](https://www.tollmanz.com/core-caching-concepts-in-wordpress/)
* [加速网站的最佳实践](https://developer.yahoo.com/performance/rules.html) – Expires / Cache-Control 标头与 ETags (由 Yahoo! Developer Network 提供)
* [WebSiteOptimization.com: 使用服务器缓存控制提升性能](https://www.websiteoptimization.com/speed/tweak/cache/)