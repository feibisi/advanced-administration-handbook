---
title: "配置通配符子域名"
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

# 配置通配符子域名

通配符子域名对于基于域名的 WordPress [多站点](https://wordpress.org/documentation/article/wordpress-glossary/#multisite)网络非常有用，它允许终端用户按需创建新站点。在此类网络中，每个新站点都有自己的子域名，而通配符配置意味着这些子域名无需单独配置。有关如何创建多站点网络的信息，请参阅：[创建网络](https://developer.wordpress.org/advanced-administration/multisite/create-network/)。

本页包含了一些示例，说明如何在不同情况下配置通配符子域名。如果您无法确定如何在特定的 Web 服务器上设置通配符子域名，*请联系您的网络托管服务商*以获取指导。

## Apache {#apache}

在 `httpd.conf` 文件中，或在包含您网站账户 `VirtualHost` 部分的包含文件中，添加如下一行（如果尚未存在）：

```
ServerAlias *.example.com
```

同时创建一个通配符 DNS 记录，例如：

```
*.example.com A 192.0.2.1
```

## CPanel {#cpanel}

在您的 CPanel 中创建一个名为 "*"（通配符）的子域 (`*.example.com`)。请确保将其指向与您的 `wp-config.php` 文件所在的文件夹位置相同。

## Plesk {#plesk}

在使用 Plesk 面板的服务器上为通配符子域设置服务器时，有几个步骤与使用 cPanel（或无控制面板）的服务器不同。这篇文章 [在 Plesk 控制面板下为多站点配置通配符子域](https://codex.wordpress.org/Configuring_Wildcard_Subdomains_for_multi_site_under_Plesk_Control_Panel) 详细说明了所有涉及的步骤。

## DirectAdmin {#directadmin-panel}

点击 "用户面板" -> DNS 管理 -> 使用三列添加以下三条记录：

```
* A 192.0.2.1
```

点击 "管理员面板"（如果您没有"管理员面板"，请让您的主机提供商操作） -> 自定义 Httpd -> yourdomain.com -> 在文本输入区域，精确地粘贴以下内容并"保存"：

```
ServerAlias *.|DOMAIN|
```

_如果您需要撤销自定义 Httpd 设置：返回此处，删除输入区域的文本，保存。_

- DirectAdmin.com: [Apache 通配符文档](https://help.directadmin.com/item.php?id=127)。DirectAdmin.com 论坛: [WordPress 通配符子域名](https://forum.directadmin.com/threads/wildcard-subdomains-yea-i-know-its-a-common-one.29074/#post-195033)。

## Amazon Web Services {#amazon-web-services}

默认情况下，AWS 实例不会分配永久 IP 地址。这意味着“服务器”的 IP 地址在重启时可能会改变。要解决此问题，请为您的服务器实例分配一个弹性 IP 地址，并在向域名注册商配置 A 记录时使用该 IP 地址。

AWS 弹性负载均衡器无法分配弹性 IP，因此必须使用 CName 为其提供一个友好的 URL。您不能为根 URL 设置 CName。因此，您必须将域名根目录（example.com）指向具有弹性 IP 地址的特定服务器实例，并创建一个通配符 CName（*.example.com），将其指向您的弹性负载均衡器。然后，在您的 .htaccess 文件中，将所有域名根目录流量（example.com）重定向到特定的子域名（www.example.com）。

**注意：**

- 目前一些域名注册商不支持通配符 CName。
- 亚马逊的 Route53 域名服务可以解决 CName 问题，但需要额外付费。