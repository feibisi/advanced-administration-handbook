---
title: "WordPress 订阅源"
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

# WordPress 订阅源

## WordPress 内置订阅源 {#wordpress-built-in-feeds}

默认情况下，WordPress 提供多种订阅源。它们通过 [bloginfo()](https://developer.wordpress.org/reference/functions/bloginfo/) 模板标签为每种订阅源类型生成，通常列在大多数 WordPress 主题的侧边栏和/或页脚中。它们看起来像这样：

[RDF/RSS 1.0 订阅源](https://web.resource.org/rss/1.0/) 的 URL

```
<?php bloginfo('rdf_url'); ?>
```

[RSS 0.92 订阅源](https://www.rssboard.org/rss-0-9-2) 的 URL

```
<?php bloginfo('rss_url'); ?>
```

[RSS 2.0 订阅源](https://www.rssboard.org/rss-specification) 的 URL

```
<?php bloginfo('rss2_url'); ?>
```

[Atom 订阅源](http://www.atomenabled.org/) 的 URL

```
<?php bloginfo('atom_url'); ?>
```

评论 RSS 2.0 订阅源的 URL

```
<?php bloginfo('comments_rss2_url'); ?>
```

前四个订阅源为不同的订阅阅读器显示您网站内容的最新更新和更改。其中，RSS 订阅源最为知名。最后一个订阅源示例供 RSS 2.0 订阅阅读器使用，不显示您网站的内容，仅显示您网站上的评论。

要跟踪特定文章的评论，可以在单篇文章页面上使用 [post_comments_feed_link()](https://developer.wordpress.org/reference/functions/post_comments_feed_link/) 模板标签，如下所示：

```
<?php post_comments_feed_link('RSS 2.0'); ?>
```

有多种方法可以修改这些订阅源，这些内容在 [自定义订阅源](https://codex.wordpress.org/Customizing_Feeds) 一文中有所介绍。

## 添加订阅源 {#adding-feeds}

并非所有 WordPress 主题都包含 WordPress 提供的所有 RSS 订阅源类型。要向您的网站添加订阅源，请先找到其他订阅源的位置，通常在您主题的 sidebar.php 或 footer.php 模板文件中。然后将上述列出的标签之一添加到列表中，如下例所示：

```
<ul class="feeds">
  <li><a href="<?php bloginfo('rss2_url'); ?>" title="<?php _e('Syndicate this site using RSS'); ?>"><?php _e('<abbr title="Really Simple Syndication">RSS</abbr>'); ?></a></li>
  <li><a href="<?php bloginfo('atom_url'); ?>" title="<?php _e('Syndicate this site using Atom'); ?>"><?php _e('Atom'); ?></a></li>
  <li><a href="<?php bloginfo('comments_rss2_url'); ?>" title="<?php _e('The latest comments to all posts in RSS'); ?>"><?php _e('Comments <abbr title="Really Simple Syndication">RSS</abbr>'); ?></a></li>
</ul>
```

### 为订阅链接添加图形标识 {#adding-graphics-to-feed-links}

许多人喜欢用图形而非文字来表示订阅源。目前已有这类图形或“按钮”的[标准规范](http://www.feedicons.com/)，但您也可以[自行制作](https://kalsey.com/tools/buttonmaker/)以匹配网站的外观和配色。![](https://wordpress.org/documentation/files/2019/03/rssfeed.gif)

要为订阅链接添加图形标识，只需将链接包裹在图形标签外，例如：

```
<a href="<?php bloginfo('rss2_url'); ?>" title="<?php _e('Syndicate this site using RSS'); ?>"><img src="https://example.com/images/feed-icon-14x14.png" alt="RSS Feed" title="RSS Feed"></a>
```

### 更改地址 {#changing-addresses}

如果您当前正在使用其他网志软件并打算切换到 WordPress，或者正在将您的网志迁移到新位置，您可以通过在 `.htaccess` 文件中使用文件重写和重定向来将 RSS 阅读器“转发”到您的新 RSS 源。

编辑您根目录中的 `.htaccess` 文件；如果文件不存在，请创建一个。

以下是一个 b2 源的示例：

```
RewriteRule ^b2rss2.php(.*)? /wordpress/?feed=rss2 [QSA]
```

以下是一个 MovableType 用户的示例：

```
RewriteRule ^index.xml(.*)? /wordpress/?feed=rss2 [QSA]
```