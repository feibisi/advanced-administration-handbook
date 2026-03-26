---
title: "创建网络前的准备工作"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Multisite
    - Repos
    - Data
---

# 创建网络前的准备工作

本节概述了在开始[创建多站点网络](https://developer.wordpress.org/advanced-administration/multisite/create-network/)之前需要考虑的一些要求。

## 你真的需要多站点网络吗？{#do-you-really-need-a-network}

多站点网络中的站点是独立的，非常像 WordPress.com 上独立的博客。它们不像其他类型网络中的事物那样*相互连接*（尽管插件可以在站点之间创建各种类型的互连）。如果你计划创建紧密互连、共享数据或共享用户的站点，那么多站点网络可能不是最佳解决方案。

例如，如果你只是希望不同的网页集合看起来完全不同，那么你可能可以通过使用插件来切换主题、模板或样式表，在单个站点中实现你的需求。

再例如，如果你只是希望不同的用户组能够访问不同的信息，那么你可能可以通过使用插件来切换功能、菜单和链接 URL，在单个站点中实现你的需求。

## 多站点网络类型 {#types-of-multisite-network}

根据您希望网络如何处理 URL 以及是否允许最终用户按需创建新站点，您可以在几种不同类型的多站点网络中进行选择。

不同类型的网络有不同的服务器要求，这些要求将在下文的一个章节中描述。如果您无法完全控制服务器，那么某些类型的多站点网络可能无法使用。例如，如果您使用共享主机环境，可能就无法完全控制服务器。在这种情况下，您需要与主机环境的运营方协商相关要求。

网络中的站点拥有不同的 URL。您可以选择以下两种方式之一来指定站点的 URL：

* 每个站点使用不同的**子域名**。例如：`site1.example.com`、`site2.example.com`。
* 每个站点使用不同的**路径**。例如：`example.com/site1`、`example.com/site2`

此外，您也可以映射像 `example1.com`、`example2.com` 等这样的域名，但建议使用插件来实现。您可以直接在网络设置中进行更改，但这被视为高级管理操作。

[![管理站点界面](https://i0.wp.com/wordpress.org/support/files/2018/11/sites-edit-site_4.7.png?fit=612%2C235&ssl=1)](https://i0.wp.com/wordpress.org/support/files/2018/11/sites-edit-site_4.7.png?fit=612%2C235&ssl=1)

管理站点

您还可以选择是否允许最终用户按需创建新站点。基于域名的按需站点通常只能使用像 `site1.example.com` 和 `site2.example.com` 这样的子域名。基于路径的按需站点也是可行的。

多站点安装过程使用不同的术语。**子域名安装**会创建一个基于域名的网络，即使您可能为站点使用单独的映射域名而非子域名。**子目录安装**会创建一个基于路径的网络，即使它并不使用文件系统目录。如果您想使用**子域名**安装，必须将 WordPress 安装在 Web 路径的根目录（即 domain.com），但如果您选择从 WordPress 自己的目录运行，则**无需**将其安装在根目录（即 /public_html/）。

多站点网络安装完成后，WordPress 在“网络管理”用户界面中使用**域名**和**路径**这两个术语来指代每个站点的域名和路径。超级管理员（即多站点网络管理员）可以编辑站点的域名和路径设置，尽管对已建立的站点进行此类更改并不常见，因为这会改变它们的 URL。

插件可以扩展可用选项并协助管理。请在[插件目录](https://wordpress.org/plugins/)中搜索“multisite”或点击[此链接](https://wordpress.org/plugins/search/multisite/)。

## 管理员要求 {#admin-requirements}

要创建一个多站点网络，您必须是 WordPress 安装的管理员，并且通常需要访问服务器的文件系统，以便编辑文件和创建目录。例如，您可以使用 [FTP](https://wordpress.org/documentation/article/glossary#ftp)、[cPanel](https://wordpress.org/documentation/article/glossary#cpanel) 中的文件管理器或其他方式来访问服务器的文件系统。

您不一定需要了解 WordPress [开发](https://wordpress.org/documentation/article/glossary#developer)、[PHP](https://wordpress.org/documentation/article/glossary#php)、[HTML](https://wordpress.org/documentation/article/glossary#html)、[CSS](https://wordpress.org/documentation/article/glossary#css)、服务器管理或系统管理，尽管了解这些知识可能有助于故障排除或在安装后自定义您的多站点网络。

## Server Requirements {#server-requirements}

When you are planning a network, it can sometimes be helpful to use a development server for initial testing. However, setting up a development server that exactly matches your production server is not always possible, and transferring an entire network to a production server may not be easy. A test site on your production server is sometimes a more useful way to test your planned network.

In all cases, you will need to make sure your server can use the more complex .htaccess (or nginx.conf or web.config) rules that Multisite requires.

Multisite requires [mod_rewrite](https://wordpress.org/documentation/article/glossary#mod-rewrite) to be loaded on the Apache server, support for it in [.htaccess](https://wordpress.org/documentation/article/glossary#htaccess) files, and Options FollowSymLinks either already enabled or at least not permanently disabled. If you have access to the server configuration, then you could use a Directory section instead of a .htaccess file. Also make sure that your httpd.conf file is set for "AllowOverride" to be "All" or "Options All" for the vhost of the domain. You can ask your webhost for more information on any of this.

Some server requirements depend on the type of multisite network you want to create, as follows.

### 基于域名的网络 {#domain-based}

也称为“子域名”安装，基于域名的网络使用类似 https://subsite.example.com 的 URL。

基于域名的网络将不同的域名映射到服务器文件系统中安装 WordPress 的同一目录。您可以通过多种方式实现这一点，例如：

* 配置通配符子域名
* 配置虚拟主机，为每个主机指定相同的文档根目录
* 在 [cPanel](https://wordpress.org/documentation/article/glossary#cpanel) 或类似的网络托管控制面板中创建附加域或子域名

按需创建的基于域名的站点需要使用通配符子域名方法。您可以使用其他方法在同一网络中手动创建额外的站点。

无论使用哪种方法，您都需要配置 DNS（将域名映射到服务器的 IP 地址）和服务器（将域名映射到 WordPress 安装目录）。然后 WordPress 会将域名映射到站点。

为了使子域名正常工作，WordPress _应该_ 从您的网站文件夹根目录（例如 `public_html`）运行。要从非根目录使子域名工作，需要具备虚拟主机和重定向的经验。

外部链接：

* [通配符 DNS 记录](https://en.wikipedia.org/wiki/Wildcard_DNS_record)（维基百科）
* [Apache 虚拟主机](https://httpd.apache.org/docs/2.0/en/vhosts/)（Apache HTTP 服务器文档）
* [cPanel 域名](https://documentation.cpanel.net/display/74Docs/cPanel+Features+List#DomainsTab)（cPanel 文档）

有关如何在各种系统上配置通配符子域名的一些示例，请参阅：[配置通配符子域名](https://wordpress.org/documentation/article/configuring-wildcard-subdomains/)

### 基于路径的 {#path-based}

也称为“子文件夹”或“子目录”安装，基于路径的网络使用类似 https://example.com/subsite 的 URL。

如果您已经在站点中使用了固定链接，那么基于路径的网络同样可以工作，您不需要本节中的其他信息。但请注意，您的主站点将使用以下 URL 模式来显示文章：https://example.com/blog/[postformat]/

目前，**无法**在不进行手动配置的情况下移除博客别名，且配置位置不直观。不建议这样做。

## WordPress 设置要求 {#wordpress-settings-requirements}

安装多站点网络时，您需要从一个现有的 WordPress 安装开始。如果这是一个拥有自己域名的新安装，则无需阅读本节。如果这是一个已建立的站点，或者无法仅通过域名访问，则需满足以下要求才能将其转换为多站点网络。

### 注意事项 {#be-aware}

自 3.5 版本起，[为 WordPress 设置独立目录](https://developer.wordpress.org/advanced-administration/server/wordpress-in-directory/) 功能已支持多站点网络，但您必须在启用多站点网络**之前**完成“独立目录”的修改。

虽然不建议在域名 URL 中使用 www，但如果您选择这样做并计划为多站点网络使用**子域名**，请确保**同时**将站点地址和 WordPress 地址设为相同。还需注意，部分主机会默认显示此类 URL：

[![](https://i0.wp.com/wordpress.org/support/files/2018/11/no-www.png?fit=474%2C215&ssl=1)](https://wordpress.org/documentation/files/2018/11/no-www.png)

基于此以及诸多其他原因，我们建议您尽可能避免在域名中使用 www。如果您计划将其更改为 `domain.com` 或 `www.domain.com`，请在开始多站点网络的其他设置**之前**完成更改，因为事后修改域名会更加复杂。

### 限制 {#restrictions}

在以下情况下，您**无法创建网络**：

* "WordPress 地址 (URL)" 使用了除 ':80'、':443' 之外的端口号。

在以下情况下，您_无法选择**子域名**安装_（用于基于域名的网络）：

* WordPress URL 包含路径，而不仅仅是域名。（即，WordPress 未安装在文档根目录中，或者您未使用该文档根目录的 URL。）
* "WordPress 地址 (URL)" 是 `localhost`。
* "WordPress 地址 (URL)" 是 IP 地址，例如 127.0.0.1。

（请注意，您可以在本地机器上通过使用 hosts 文件将其他主机名映射到 IP 地址 127.0.0.1 来创建基于域名的网络用于测试目的，这样您就永远不需要使用主机名 `localhost`。）

如果您的现有 WordPress 安装已设置超过一个月，由于现有固定链接的问题，您_无法选择**子目录**安装_（用于基于路径的网络）。（此问题将在未来版本中修复。更多信息请参阅[切换网络类型](https://developer.wordpress.org/advanced-administration/multisite/administration/#switching-network-types)。）

_更多详情请参阅 `wp-admin/network.php`）_