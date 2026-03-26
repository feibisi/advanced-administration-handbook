---
title: "oEmbed"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Wordpress
    - Repos
    - Data
---

# oEmbed

简易嵌入功能主要由 oEmbed 协议驱动，该协议允许消费者（如您的博客）向提供者（如 YouTube）请求嵌入内容所需的 HTML。

oEmbed 旨在避免从托管您希望嵌入的媒体网站复制粘贴 HTML。它支持不同类型的内容，如视频、图像、文本等。

## 这适用于任何 URL 吗？

不，默认情况下不行。出于安全原因，WordPress 核心有一个[内部白名单](https://developer.wordpress.org/reference/hooks/oembed_providers/)，仅允许特定 URL 可嵌入。好消息是白名单可以修改，可以通过注册其句柄来添加新的网站和 URL。

## 如何添加或更改对网站的支持？

添加对额外网站的支持取决于该网站是否支持 oEmbed。[oEmbed.com](https://oembed.com/) 提供了数百个[支持的提供者](https://oembed.com/providers.json)的列表。

### 添加对支持 oEmbed 的网站的支持

如果网站支持 oEmbed，您需要调用 [`wp_oembed_add_provider()`](https://developer.wordpress.org/reference/functions/wp_oembed_add_provider/) 将网站和 URL 格式添加到内部白名单。

### 添加对不支持 oEmbed 的网站的支持

您需要使用 [`wp_embed_register_handler()`](https://developer.wordpress.org/reference/functions/wp_embed_register_handler/) 注册一个处理程序，并提供一个生成 HTML 的回调函数。

### 移除对支持 oEmbed 的网站的支持

如果您希望移除一个支持 oEmbed 的提供者，您需要调用 [`wp_oembed_remove_provider`](https://developer.wordpress.org/reference/functions/wp_oembed_remove_provider/)。

## oEmbed 发现功能呢？

自 4.4 版本起，WordPress 支持 oEmbed 发现，但对可通过非白名单网站嵌入的内容类型有严格限制。

具体来说，HTML 和视频内容被过滤为仅允许链接、块引用和 iframe，并且这些内容还会被额外过滤以防止恶意内容的插入。然后，HTML 会被修改以进行沙盒化，并对其施加额外的安全限制。

但是，如果您认为自己足够了解，不需要这种级别的安全性，您可以授予 `unfiltered_html` 用户（管理员和编辑者）从在其 `<head>` 中包含 oEmbed 发现标签的网站嵌入内容的能力。

对于“链接”和“照片”类型的 oEmbed 发现内容，过滤方式并不那么严格；然而，出于安全考虑并防止任何恶意内容在网站上显示，它会进行适当的转义。