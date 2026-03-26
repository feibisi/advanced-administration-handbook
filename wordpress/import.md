---
title: "导入内容"
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

# 导入内容

使用 WordPress 导入工具，您可以将内容从另一个 WordPress 站点或其他发布系统导入到您的站点。

您可以在 WordPress [管理屏幕](https://wordpress.org/documentation/article/administration-screens/)左侧导航栏的 [工具](https://wordpress.org/documentation/article/administration-screens/#tools-managing-your-blog) -> [导入](https://wordpress.org/documentation/article/tools-import-screen/) 下找到此处描述的许多导入器。

您还可以从管理屏幕所列系统之外的发布系统导入内容。每个系统的操作步骤不同，因此请根据需要参考以下列出的步骤和插件。如果您是 WordPress 新手，请查看 [WordPress 功能](https://wordpress.org/about/features/) 和 [使用 WordPress](https://codex.wordpress.org/Working%20with%20WordPress) 页面以开始使用。

如果遇到问题，请在 [WordPress 支持论坛](https://wordpress.org/support/forums/) 中搜索解决方案，或尝试查看 [常见问题解答](https://wordpress.org/documentation/article/faq-work-with-wordpress/)。

## 导入前注意事项

如果导入的文件过大，服务器可能在导入时内存耗尽。若发生这种情况，您将看到类似“致命错误：允许的 8388608 字节内存大小已耗尽”的错误提示。

若您拥有服务器足够权限，可通过编辑 `php.ini` 文件来增加可用内存。或者，也可联系托管服务提供商协助处理。否则，您可以将导入文件拆分为多个较小文件分别导入。

若导入过程失败，仍可能已创建部分内容。当您解决问题后重新尝试时，可能会产生重复数据。请在导入失败后检查网站，必要时删除相关记录以避免此问题。

## b2evolution

有两种方法可以将 b2evolution 内容导入 WordPress。

* **Movable Type 导出格式**：您可以重新设计 b2evolution 博客的皮肤，使其在查看源代码时显示为 [Movable Type 导出格式](https://movabletype.org/documentation/appendices/import-export-format.html)。您可以保存导出文件并将其作为 Movable Type 数据导入。请参阅 Movable Type 和 TypePad。
* **BIMP 导入脚本**：您可以使用 [BIMP 导入脚本](https://wittyfinch.com/bimp-importer-migrate-b2evolution-to-wordpress/) 将 b2evolution 的博客、分类、文章、评论、文件和用户导入您的 WordPress 安装（v3 及以上版本）。请注意，此服务需要付费。

## Blogger

您可以从 Blogger 导入文章、评论、分类和作者。WordPress 包含一个专门用于从 Blogger 导入内容的导入工具。

1. 将您的 Blogger 内容导出为 XML 文件。
2. 在您的 WordPress 站点中，于管理屏幕左侧导航栏选择 工具 -> 导入。
3. 在 "Blogger" 下，如果您尚未安装 Blogger 导入器，请点击 "立即安装"。
4. 点击 "运行导入器" 链接。
5. 点击 "选择文件" 并找到您的 Blogger XML 文件。
6. 点击 "上传文件并导入"。

## Drupal

有多种资源可帮助您将内容从 Drupal 迁移到 WordPress。这里重点介绍几种，您可能还会在网上搜索到许多其他工具。

* [FG Drupal to WordPress](https://wordpress.org/plugins/fg-drupal-to-wp/)。兼容 Drupal 4、5、6、7、8 和 9。
* [Drupal2WordPress Plugin](https://github.com/jpSimkins/Drupal2WordPress-Plugin)。使用此插件可导入分类、内容、媒体、评论和用户。您 Drupal 站点中包含的任何外部图像都可以获取并添加到媒体库，并添加到您的页面和文章中。
* [Drupal to WordPress migration SQL queries explained](https://anothercoffee.net/drupal-to-wordpress-migration-sql-queries-explained/) 包含针对某些迁移问题（如重复分类、分类超过最大字符长度和重复 URL 别名）的解决方案。

## XML 与 CSV

以下资源可帮助您将 XML 或 CSV 内容导入 WordPress。

* [WP All Import](https://wordpress.org/plugins/wp-all-import/) 插件可导入任何 XML 或 CSV 文件，并与 [WP All Export](https://wordpress.org/plugins/wp-all-export/) 插件集成。

## Joomla

对于 Joomla，您可以使用 [FG Joomla to WordPress](https://wordpress.org/plugins/fg-joomla-to-wordpress/)。该插件已在大型数据库上通过 Joomla 1.5 至 4.0 版本测试，并兼容多站点安装。

## LiveJournal

WordPress 包含一个专门用于从 LiveJournal 导入内容的导入工具。

1. 在您的 WordPress 站点中，于管理屏幕左侧导航栏选择 工具 -> 导入。
2. 在 "LiveJournal" 下，如果您尚未安装 LiveJournal 导入器，请点击 "立即安装"。
3. 点击 "运行导入器" 链接。
4. 输入您的 LiveJournal 用户名和密码，然后点击 "连接到 LiveJournal 并导入"。

## Live Space

请参阅 [Live Space Mover](https://b2.broom9.com/?page_id=519) 了解如何使用 Python 脚本将博客文章从 Live Space 导入到 WordPress。

## Magento

[FG Magento to WooCommerce](https://wordpress.org/plugins/fg-magento-to-woocommerce/) 插件可将您的 Magento 产品和 CMS 页面迁移到 WooCommerce。

## Mambo

您可以使用插件 [FG Joomla to WordPress](https://wordpress.org/plugins/fg-joomla-to-wordpress/)。此 WordPress 插件适用于 Mambo 4.5 和 4.6。

## Movable Type 和 TypePad

WordPress 包含一个专门用于从 Movable Type 和 TypePad 导入内容的导入工具。

1. 在您的 WordPress 站点中，于管理屏幕左侧导航栏选择工具 -> 导入。
2. 在“Movable Type 和 TypePad”下，如果您尚未安装导入器，请点击“立即安装”。
3. 点击“运行导入器”链接。
4. 点击“选择文件”并导航到您的导出文件。
5. 点击“上传文件并导入”。

以下文章提供了有关此过程的更多信息：

* [从 Movable Type 导入到 WordPress](https://codex.wordpress.org/Importing%20from%20Movable%20Type%20to%20WordPress)
* [导入和导出内容](https://www.movabletype.org/documentation/administrator/maintenance/import-export.html)
* [关于大规模 WordPress 迁移的注意事项](https://blog.birdhouse.org/2008/02/07/notes-on-a-massive-wordpress-migration/)

## Nucleus CMS

以下资源可帮助您将内容从 Nucleus CMS 迁移到 WordPress。

* [从 Nucleus CMS 导入指南](https://mamchenkov.net/wordpress/2005/04/26/nucleus2wordpress/)
* [从 Nucleus CMS 导入的脚本](http://james.onegoodcookie.com/pub/import-nucleus.phps)
* [Nucleus 转 WordPress](https://github.com/AbdussamadA/nucleus-importer)

## PrestaShop

[FG PrestaShop to WooCommerce](https://wordpress.org/plugins/fg-prestashop-to-woocommerce/)。此 WordPress 插件兼容 PrestaShop 1.0 至 1.7 版本。

## Roller

参见 [从 Roller 导入](https://codex.wordpress.org/Importing%20From%20Roller)。

另请参阅 [将 Roller 博客迁移到 WordPress](https://nullpointer.debashish.com/migrating-a-roller-blog-to-wordpress)。

## RSS

WordPress 内置了一个专门用于从 RSS 导入内容的工具。

1.  在您的 WordPress 站点后台，从左侧导航栏选择 工具 -> 导入。
2.   在“RSS”下方，如果您尚未安装导入器，请点击“立即安装”。
3.   点击“运行导入器”链接。
4.   点击“选择文件”并找到您的 XML 文件。
5.   点击“上传文件并导入”。

## 机缘巧合

* [从 Serendipity 迁移到 WordPress – 文章导入](https://obviate.io/2010/06/11/serendipity-to-wordpress-post-import/).

## SPIP

插件 [FG SPIP to WordPress](https://wordpress.org/plugins/fg-spip-to-wp/) 可将分类、文章、新闻和图片从 SPIP 迁移到 WordPress。它已在 SPIP 1.8、1.9、2.0、3.0、3.1 和 3.2 版本上测试通过，并兼容多站点安装。

## Sunlog

1. 打开 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/) 查看您 Sunlog 安装的数据库。您只需要两个表："blogname_entries" 和 "blogname_comments"。
2. 使用 phpMyAdmin 将这两个表导出为 XML 文件。
3. 在您的 WordPress 站点上安装 [WP All Import](https://wordpress.org/plugins/wp-all-import/) 插件。
4. 创建以下字段映射：
  * `headline=title`
  * `content=entry+more`
  * `date=timestamp`（使用 Unix 时间戳格式）
  * `categories="cat,"`（每个值用分号分隔）。

## Textpattern

* [修复 Textpattern 导入](https://wordpress.org/support/topic/fix-textpattern-import/)
* [Textpattern 到 WordPress 导出工具](https://github.com/drewm/textpattern-to-wordpress)

## Tumblr

WordPress 包含一个专门为从 Tumblr 导入内容而设计的导入工具。

1.  在您的 WordPress 站点中，在管理屏幕的左侧导航栏选择 工具 -> 导入。
2.  在 "Tumblr" 下，如果您尚未安装导入器，请点击 "立即安装"。
3.  点击 "运行导入器" 链接。
4.  点击 "选择文件" 并导航到您的导出文件。
5.  点击 "上传文件并导入"。
6.  在 Tumblr 上创建一个应用程序，为您的博客和 Tumblr 服务器之间提供连接点。
7.  复制并粘贴 "OAuth Consumer Key" 和 "Secret Key"。
8.  点击 "连接到 Tumblr"。

## Twitter

有多种插件可将您的推文导入 WordPress，例如 [将您的 Twitter 时间线导入 WordPress](https://blog.birdhouse.org/2008/08/17/get-your-twitter-timeline-into-wordpress/)。

## TypePad

请参阅 [Movable Type 和 TypePad](#movable-type-and-typepad)。

## WooCommerce 产品 (CSV)

如果您已安装 WooCommerce 插件，此导入器将已安装。点击“运行导入器”以上传 CSV 文件。

## WooCommerce 税率（CSV）

如果您已安装 WooCommerce 插件，此导入工具将自动安装。点击“运行导入工具”上传 CSV 文件。

## WordPress

WordPress 包含一个专门用于从其他 WordPress 博客导入内容的工具。

1.  在您的 WordPress 站点中，于管理界面左侧导航栏选择 工具 -> 导入。
2.  在“WordPress”下方，如果您尚未安装导入工具，请点击“立即安装”。
3.  点击“运行导入工具”链接。
4.  点击“选择文件”，然后导航至从源站点导出的 WXR 文件。
5.  点击“上传文件并导入”。

系统将首先要求您将此导出文件中的作者映射到博客中的用户。对于每位作者，您可以选择映射到博客中的现有用户或创建新用户。然后，WordPress 会将上传文件中包含的每篇文章、评论和分类导入您的博客。此外，您可以通过勾选“下载并导入文件附件”选项来导入附件。

## Xanga

[xanga.r](https://www.timwylie.com/xword.html) 是一个解析 Xanga 页面以获取帖子和评论的程序。然后，它可以将其输出为 WordPress RSS 2.0 XML 格式，供 WordPress 导入。