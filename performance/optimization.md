---
title: "优化"
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

# 优化

无论您运行的是高流量 WordPress 站点，还是低成本共享主机上的小型博客，都应优化 WordPress 和服务器以实现最高效运行。本文提供 WordPress 优化的全面概述及具体推荐方案，但不对每个方面进行详细技术说明。

如需快速解决方案，请直接跳转至缓存部分，您将以最小代价获得最大收益。若想立即开始更全面的优化流程，请参阅[如何提升 WordPress 性能](#how-to-improve-performance-in-wordpress)。

下方[性能影响因素](#performance-factors)和[性能测试工具](#performance-testing-tools)部分包含性能主题的全面概述。本文讨论的许多技术同样适用于 WordPress 多站点（MU）。

## 性能影响因素

多个因素可能影响您的 WordPress 博客（或网站）的性能。这些因素包括但不限于：托管环境、WordPress 配置、软件版本、图片数量及其文件大小。

本文档将探讨并解决大部分导致性能下降的因素。

### 托管

您可用的优化技术将取决于您的托管设置。

#### 共享托管

这是最常见的托管类型。您的网站将与许多其他网站一起托管在同一台服务器上。托管公司为您管理网络服务器，因此您对服务器设置等的控制权非常有限。在大多数共享托管中，用户可以通过 SFTP 访问网站根目录的文件系统，并通过[网络托管控制面板](https://developer.wordpress.org/advanced-administration/server/control-panel/)执行许多常见的域名/托管任务。

与此类托管最相关的领域是：[缓存](#caching)、[WordPress 性能](#optimizing-your-wordpress-website)和[内容卸载](#content-offloading)。

#### 托管主机

托管主机类似于共享主机，但更局限于用户可在特定使用场景下运行的一组软件栈。因此，托管提供商为用户管理软件栈，但条件是限制软件选择。用户通常无法（或无需）访问文件系统并通过[网络托管控制面板](https://developer.wordpress.org/advanced-administration/server/control-panel/)管理任务。一些托管提供商会在更高层级的托管方案中提供更多软件或插件选择。

如今网络上的大多数内容平台（例如博客/社交媒体）都属于托管主机的一种形式，因为它们旨在为特定使用场景提供平台。

#### 虚拟专用服务器与专用服务器

在此托管方案中，您拥有对服务器的完全控制权：包括完整的文件系统、SSH 访问权限，以及能够在专用于该服务器的独立操作系统上安装/配置任意软件。该服务器可能是专用硬件设备，也可能是共享同一物理硬件的多个虚拟服务器之一。

关键在于，您拥有服务器设置的控制权。除了上述领域（缓存与 WordPress 性能）外，这里需要重点关注的核心领域是：[优化软件](#optimize-software) 与 [内容分流](#content-offloading)。

#### 服务器数量

在处理极高流量场景时，可能需要部署多台服务器。若已达到此级别，您应当已应用上文列出的所有适用技术。

WordPress 数据库可轻松迁移至独立服务器，仅需在配置文件中进行微小调整。同样，图片等静态文件也可转移至其他服务器（参见[内容分流](#content-offloading)章节）。

[负载均衡器](https://en.wikipedia.org/wiki/Load_balancing_(computing))可协助将流量分散至多个网络服务器，但需要更高专业能力。若需部署多台数据库服务器，[HyperDB](https://codex.wordpress.org/HyperDB)类可作为标准[WPDB](https://developer.wordpress.org/reference/classes/wpdb/)类的即插即用替代方案，能同时处理复制结构与分区结构的多数据库服务器。

#### 硬件性能

您的硬件配置将极大影响网站性能。处理器数量、处理器速度、可用内存容量、磁盘空间及存储介质类型都是关键因素。托管服务商通常提供价格越高、性能越优的方案。

#### 地理距离

服务器与网站访问者之间的距离也会影响性能。内容分发网络（CDN）可在不同地理区域镜像静态文件（如图片），从而确保所有访问者都能获得最佳性能。

#### 服务器负载

服务器上的流量大小及其处理负载的配置方式也会产生巨大影响。例如，若不使用缓存方案，随着额外页面请求涌入并堆积，性能将急剧下降直至停滞，通常会导致 Web 或数据库服务器崩溃。

若配置得当，大多数托管方案都能处理极高的流量。将流量分流至其他服务器也可减轻服务器负载。

恶意流量（如登录[暴力破解攻击](https://developer.wordpress.org/advanced-administration/security/brute-force/)、图片盗链（其他网站从高流量页面链接至您的图片文件）或 DoS 攻击）同样会增加服务器负载。识别并拦截此类攻击至关重要。

#### 软件版本与性能

确保使用最新软件同样重要，因为软件升级通常能修复漏洞并提升性能。确保运行最新版本的 Linux（或 Windows）、Apache、MySQL/MariaDB 和 PHP 至关重要。

### WordPress 配置

您选择的主题将极大影响网站性能。一个轻量级快速主题的运行效率远高于那些图形密集、效率低下的主题。

插件数量及其性能同样对网站性能有重大影响。停用并删除不必要的插件是提升性能的重要途径。

及时更新 WordPress 版本也至关重要。

#### 图形尺寸

确保文章中的图片针对网络进行优化，可以节省时间、带宽，并提升搜索引擎排名。

## 性能测试工具

- [在线网页基准测试工具](https://duckduckgo.com/?q=online+web+page+benchmarking+tools) 可以从不同地点、浏览器和连接速度测试实际网站性能。
- 内置的浏览器开发者工具（例如 Firefox 或 Chrome）都包含性能测量工具。

## 如何提升 WordPress 性能

### 优化您的 WordPress 网站

#### 最小化插件数量

提升 WordPress 性能最首要且简单的方法是审视插件。停用并删除所有不必要的插件。尝试选择性禁用插件以测试服务器性能。

是否有某个插件显著影响网站性能？请查阅插件文档、在相应的插件支持论坛寻求帮助，或寻找具有相似功能集的其他插件。

#### 内容优化

*图像文件*

- 是否存在不必要的图像？（例如：能否用文本替代部分图像？）
- 确保所有图像文件都已优化。为每张图像选择正确的格式（JPG/PNG/GIF）和压缩方式。
- 考虑使用更现代的图像格式，如体积更小的 WebP。

*文件总数/大小*

- 能否减少网站平均页面所需的文件数量？
- 当仍在使用 HTTP/1.x 时，建议将多个文件合并为单个优化文件。
- 压缩 CSS 和 JavaScript 文件。

### 升级硬件

为托管服务商的高级服务层级支付更多费用可能非常有效。增加 CPU 和内存（RAM）或切换到使用固态硬盘（SSD）或 NVMe 的主机可以带来显著改善。增加处理器数量和提升处理器速度也会有所帮助。在可能的情况下，尽量将不同功能的服务（如 HTTP 和 MySQL）分离到多台服务器或 VPS 上（理想情况下服务器应位于同一位置以减少延迟）。如果您使用的是共享主机，当您达到资源上限时，升级到具有更高资源限制（如磁盘 I/O、IOPS、NPROC 和总进程数）的方案可能会有所帮助。

### 优化软件

确保您运行的是最新的操作系统版本（例如 Linux 或 Windows Server）、最新的 Web 服务器版本（例如 Apache 或 IIS）、数据库（例如 MySQL 服务器）和 PHP。

如果您无法执行以下任务，您的主机提供商或许能为您执行其中一些任务，或者您可以寻求外部帮助，例如自由职业者。一个好的主机提供商会升级或将您的账户迁移到升级后的服务器以满足推荐配置，但并非所有提供商都会帮助您管理/优化服务器，因为这通常超出了主机套餐服务的范围。如果需要，一个预选软件栈和固定版本的托管 WordPress 主机解决方案可能适合您的需求。

**DNS**：不要在您的 WordPress 服务器上运行 DNS。使用商业 DNS 服务或您的域名注册商提供的免费服务。使用外部服务还可以使在维护或紧急情况下在备份服务器之间切换变得更加容易。它还提供了一定程度的容错能力。如果您在外部服务器上托管 DNS，将减轻您主 Web 服务器的负载。这是一个简单的更改，但会分流一些流量和 CPU 负载。

**Web 服务器**：可以配置您的 Web 服务器以提高性能。有多种技术，从 Web 服务器缓存到设置缓存头以减少每位访客的负载。搜索针对您特定 Web 服务器的优化（例如，搜索“Apache 优化”以获取更多信息）。有些 Web 服务器有付费的高速版本。根据您特定的主机和站点配置，还有许多方法可以调整 Apache 以获得更高的性能（例如 Memcached）。

**PHP**：有多种可用的 PHP 加速器可以显著提高 PHP 文件的性能。这将适用于所有 PHP 文件，而不仅仅是您的 WordPress 安装。搜索 PHP 优化以获取更多信息（例如 [APC](https://www.php.net/manual/book.apcu.php) 或 [OPcache](https://www.php.net/manual/book.opcache.php)）。一些 WordPress 缓存插件提供对 Memcached、APC 和其他操作码缓存的集成支持。较新的 PHP 版本通常也包含更好的性能优化。

**MySQL/MariaDB**：MySQL 或 MariaDB 优化本身就是一门玄学。对查询缓存设置进行一些简单的更改，就能显著影响 WordPress 的性能，因为 WordPress 在每个请求中都会重复执行许多查询。如今，InnoDB 已成为 MySQL 的默认存储引擎，你必须确保使用它。InnoDB 可以进行优化和微调，请在网上搜索 [mysql optimization](https://duckduckgo.com/?q=mysql+optimization)、[mysql innodb performance](https://duckduckgo.com/?q=mysql+innodb+performance) 或 [innodb optimization](https://duckduckgo.com/?q=innodb+optimization) 以获取更多信息和示例。如需了解如何将旧的 MyISAM 表转换为 InnoDB，请在网上搜索 [mysql convert myisam to innodb](https://duckduckgo.com/?q=mysql+convert+myisam+to+innodb)。

**其他服务**：不要在 WordPress 服务器上运行邮件服务器。对于联系表单，请使用联系表单插件配合外部邮件服务。

### 缓存

#### 缓存插件

缓存插件可以轻松安装，并将您的 WordPress 文章和页面缓存为静态文件。这些静态文件随后会提供给用户，从而减轻服务器的处理负载。对于相当静态的页面，这可以将性能提升数百倍。您可以通过在插件目录中搜索 [cache](https://wordpress.org/plugins/search/cache/) 来获取相关插件列表。

当与系统级页面缓存（如 Varnish）结合使用时，效果会非常强大。如果您的文章/页面包含大量动态内容，配置缓存可能会更加复杂。

#### 服务器端缓存

Web 服务器缓存更为复杂，但适用于流量极高的站点。可选的方案众多，超出了本文的讨论范围。最简单的方案从本地服务器缓存开始，而更复杂精细的系统可能会在运行 WordPress 应用程序的 Web 服务器"前端"使用多个缓存服务器（也称为反向代理服务器）。某些 Web 服务器也可同时充当反向代理。在服务器上添加像 [Alternative PHP Cache](https://www.php.net/manual/book.apcu.php) (APC) 这样的操作码缓存，可将 PHP 性能提升数倍。

[Varnish Cache](https://varnish-cache.org/) 可与某些缓存插件协同工作，将预构建的页面存储在内存中并快速提供，无需执行 Apache、PHP 和 WordPress 堆栈。

如前所述，使用外部评论插件而非 WordPress 原生评论功能，可避免读者登录 WordPress，并增加 Varnish Cache 可从缓存中提供的页面浏览量，从而有助于 Varnish 的工作。

#### 浏览器缓存

浏览器缓存通过减少每个页面的请求数量来帮助降低服务器负载。例如，通过对不常更改的文件（如图片、CSS、JavaScript 等静态文件）设置正确的文件头，浏览器会在用户计算机上缓存这些文件。这项技术使浏览器能够检查文件是否已更改，而不是简单地请求它们。结果是您的 Web 服务器可以响应更多的 304 状态码（确认文件未更改），而不是需要发送文件的 200 状态码。

了解更多信息，请查阅 HTTP Cache-Control（特别是 `max-age`）和 Expires 头，以及[实体标签](https://developer.mozilla.org/docs/Web/HTTP/Headers/ETag)。

一些 WordPress 缓存插件集成了对浏览器缓存和 ETag 的支持。

#### 对象缓存

使用 __持久化__ 对象缓存有助于加快页面加载速度，因为它减少了 Web 服务器对数据库的访问次数。例如，您网站的选项数据需要在每次页面浏览时可用。如果没有持久化对象缓存，您的 Web 服务器必须从数据库中读取这些选项来处理每次页面浏览。这些额外的数据库访问会减慢 Web 服务器的响应时间，例如“首字节时间”（TTFB），并且在流量高峰期间可能迅速压垮您的数据库服务器。

要让您的网站使用持久化对象缓存，您的托管服务提供商必须为您提供特定类型的服务器，即[缓存服务器](https://en.wikipedia.org/wiki/Category:Database_caching)。请咨询您的托管服务提供商或外部帮助（如自由职业者），以帮助您安装和配置持久化对象缓存服务器，然后安装一个支持已安装缓存服务器的[对象缓存 WordPress 插件](https://wordpress.org/plugins/search/object+cache/)。

### 延伸阅读

- [WP 对象缓存](https://developer.wordpress.org/reference/classes/wp_object_cache/)

### 内容卸载

#### 使用内容分发网络（CDN）

使用 CDN 可以显著减轻您网站的负载。将图片、JavaScript、CSS 和主题文件的搜索与分发任务交由 CDN 处理，不仅速度更快，还能大幅减轻 WordPress 服务器自身应用栈的压力。若与上文所述的 WordPress 缓存插件配合使用，CDN 的效果将最为显著。一些新型 CDN 还包含全页缓存（FPC）或边缘缓存功能，可缓存网站的完整 HTML 内容。

详情请参阅[知名内容分发服务提供商列表](https://en.wikipedia.org/wiki/Content_delivery_network#Notable_content_delivery_service_providers)。

#### 静态内容

任何静态文件都可以分流到另一台服务器。例如，所有静态图片、JavaScript 或 CSS 文件都可以迁移到不同的服务器。这是极高性能系统（Google、Flickr、YouTube 等）的常用技术，但对于单台服务器不堪重负的小型站点也很有帮助。此外，将这些内容迁移到不同主机名上，可以为未来的多服务器部署奠定基础。

某些 Web 服务器针对静态文件服务进行了优化，其效率远高于 Apache 等复杂 Web 服务器，例如 [lighttpd](https://www.lighttpd.net/)。

[云存储](https://en.wikipedia.org/wiki/Object_storage#Cloud_storage) 是按使用量付费的专用静态文件托管服务。由于没有最低费用，对于流量较低但已达到共享或单台服务器处理极限的站点来说，这可能是一个实用的选择。

#### 多个主机名

通过将静态文件分散到多个主机名上，也能带来用户体验的改进。大多数浏览器对单个主机只会同时发起 2 个请求，因此如果你的页面需要 16 个文件，它们会以每次 2 个的速度请求。如果你将这 16 个文件分散到 4 个主机名上，它们就能以每次 8 个的速度请求。这可以减少用户的页面加载时间，但也会通过创建更多并发请求来增加服务器负载（如果不同的主机名由同一台服务器提供）。此外，如果过度使用，已知的“管道化”技术常常会占满访问者的网络连接。

分流图片是最简单易行的起点。所有图片文件可以均匀地分配到三个主机名之间（例如 `assets1.example.com`、`assets2.example.com`、`assets3.example.com`）。随着流量增长，这些主机名可以迁移到你自己的专用服务器上。注意：避免随机选择主机名，因为这会影响浏览器缓存，导致流量增加，并可能产生过多的 DNS 查询，从而带来性能损失。

同样，任何静态的 JavaScript 和 CSS 文件也可以分流到单独的主机名或服务器上。

在 HTTP/2 和 HTTP/3 下，“HTTP 管道化”已被多路复用技术取代，因此上述技术的使用可能不再必要。

#### 订阅源

您的订阅源可以轻松卸载到外部订阅服务，该服务能够处理所有订阅流量，并仅每隔几分钟从您的站点更新订阅源。这可以显著节省流量。

同样，您也可以将自有订阅源卸载到独立服务器（例如 feeds.example.com），然后自行处理订阅统计和广告投放。

### 压缩

有多种方法可以压缩服务器上的文件和数据，以便更快地将页面传递给读者的浏览器。上文提到的一些[缓存插件](https://wordpress.org/plugins/search/cache/)集成了对大多数常见压缩方法的支持。

部分 WordPress 缓存插件支持使用 Minify 和 Tidy 来压缩和合并样式表与 JavaScript 文件。它们还支持输出压缩，例如 [zlib](https://zlib.net/)，另请参阅[输出压缩](https://codex.wordpress.org/Output_Compression)。

压缩媒体文件（尤其是图像）同样至关重要。

### 数据库优化

#### 清理数据库

某些[优化插件](https://wordpress.org/plugins/search/optimization/)和[数据库插件](https://wordpress.org/plugins/search/database/)可以帮助您减少数据库中的冗余数据。

您还可以设置 WordPress 以[限制文章和页面的修订版本数量](https://wordpress.org/documentation/article/revisions/)。

### 添加服务器

虽然需要额外的专业知识，但添加服务器是提升性能的有效方式。

您可以使用[负载均衡器](https://en.wikipedia.org/wiki/Load_balancing_(computing))将流量分散到多个 Web 服务器，也可以使用 [HyperDB](https://codex.wordpress.org/HyperDB) 或云数据库服务来运行更具扩展性或多个数据库服务器。

网络上有各种指南以及 WordCamp 演讲，涉及在云服务上扩展 WordPress 站点。

### 自动加载选项

自动加载选项是插件和主题的配置设置，在 WordPress 中每次页面加载时都会自动加载。每个插件和主题都定义了自己的选项以及哪些选项是自动加载的。自动加载选项过多可能会拖慢您的网站。通常，您应尽量将网站的自动加载选项保持在 800KB 以下。

默认情况下，自动加载选项保存在 wp_options 表中。可以在此表中逐个选项关闭自动加载功能。有关查看和更改自动加载选项的逐步说明，请咨询您的主机提供商。

如果您使用持久对象缓存，选项（无论是否自动加载）的加载速度会更快、效率更高。

## 额外资源

### 延伸阅读

- [WordPress 高流量处理技巧](https://codex.wordpress.org/High_Traffic_Tips_For_WordPress)

### WordCamp Performance Presentations

- [Democratizing Performance by Pascal Birchler from WordCamp Asia 2024](https://wordpress.tv/2024/04/09/democratizing-performance/)
- [High-Performance WordPress by Iliya Polihronov from WordCamp 2012 (San Francisco)](https://wordpress.tv/2012/09/01/iliya-polihronov-high-performance-wordpress/)
- [WordPress Optimization from WordCamp Israel 2013](https://www.slideshare.net/AlmogBaku/wordpress-optimization-16678718)
- [Copy of the Slides on HyperDB and High Performance from WordCamp 2007 (San Francisco)](https://barry.blog/2007/07/22/high-performance-wordpress/)
   - [Presentation on HyperDB and High Performance from WordCamp 2007 (San Francisco)](https://onemansblog.com/2007/08/16/wordcamp-2007-hyperdb-and-high-performance-wordpress/)
- [50 tips su Web Performance Optimization per siti ad alto traffico WordCamp Bologna (Italy) 2013](https://www.slideshare.net/AndreaCardinali/50-tips-su-web-performance-optimization-per-siti-ad-alto-traffico-wpcamp-bologna-2013)