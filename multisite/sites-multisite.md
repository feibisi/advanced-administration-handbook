---
title: "将 WordPress 站点迁移至 WordPress 多站点"
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

# 将 WordPress 站点迁移至 WordPress 多站点

本教程介绍如何将多个 WordPress 独立安装迁移至 WordPress 多站点安装。您可以迁移使用自有域名的站点，以及使用主域名下子域名的站点。

本教程假设您在使用 cPanel 的服务器上托管 WordPress。如果您使用其他解决方案管理服务器，则需要调整这些说明。

## 操作步骤 {#steps-to-follow}

### 备份您的网站 {#backup-your-site}

在 cPanel 中生成完整的网站备份。通过 FTP 复制服务器上的所有文件也可能有所帮助，这样您可以方便地访问插件和主题文件，这在后续步骤中会用到。

### 从现有 WordPress 安装中导出 {#export-from-your-existing-wordpress-installs}

在您现有的每个 WordPress 安装中，前往 WordPress 的 工具 > 导出。下载包含每个站点所有文章和页面的 WXR 文件。请参阅 [工具导出屏幕](https://wordpress.org/documentation/article/tools-export-screen/) 上的说明。

请确保您的导出文件确实包含所有文章和页面。您可以使用文本编辑器查看导出文件的最后一条记录来验证这一点。最后一条记录应是最新的文章。

某些插件可能会与导出过程冲突，导致生成空文件或不完整的文件。为稳妥起见，您可能应在执行导出前禁用所有插件。

首先删除所有隔离的垃圾评论也是一个好主意，因为这些评论也会被导出，导致文件不必要地变大。

注意：此方法不会导出小工具配置和博客/插件设置。如果您在单个托管账户内迁移，请在此阶段记下这些设置，因为当您删除旧域名时，它们将会消失。

### 安装 WordPress {#install-wordpress}

安装 WordPress。请按照 [安装 WordPress](https://developer.wordpress.org/advanced-administration/before-install/howto-install/) 的说明进行操作。

### 激活多站点 {#activate-multisite}

在您的 WordPress 安装中激活多站点功能。这需要多次编辑 wp-config.php 文件。您必须选择子域名选项，而非子目录选项。请参阅关于如何[创建网络](https://developer.wordpress.org/advanced-administration/multisite/create-network/)的说明。

### Create blogs for each site you want to import {#create-blogs-for-each-site-you-want-to-import}

Create blogs for each of the sites you want to host at separate domains. For example, `importedblogdotorg.mydomain.com`.

Note: choose the name carefully, because changing it causes admin redirection issues. This is particularly important if you are migrating a site within the same hosting account.

### 为每个博客导入 WXR 文件 {#import-wxr-files-for-each-blog}

进入每个博客的后台，导入对应博客导出的 WXR 文件。将作者映射到正确的用户，或创建新用户。务必勾选导入照片和其他附件的选项。请参阅“工具 > 导入”子面板上的说明。

注意：如果选择将图片从源站点导入到目标站点，请确保它们已上传到正确位置，并在相应的文章或页面中正确显示。

### 复制主题和插件文件 {#copy-theme-and-plugin-files}

开始前，请确认你的插件能在网络安装中运行。如果插件不支持，请勿安装。可通过搜索插件功能并加上"multisite"或"mu"来寻找合适的替代品，例如"social bookmarking plugin wordpress multisite"。

将主题和插件文件从旧的 WP 安装复制到新 wp-content 的相应目录中。你可以为整个网络激活主题，也可以前往超级管理员 > 站点，点击要编辑的站点，仅为该站点启用特定主题。

注意：如果使用子主题，请将父主题和子主题都复制到新站点。

### 为每个站点编辑 WordPress 配置设置 {#edit-wordpress-configuration-settings-for-each-site}

编辑每个站点的配置设置、小部件等。在此步骤结束时，每个站点应看起来与之前完全一样，只是 URL 使用子域名.example.com 或 example.com/子站点，而非其正确、最终的 URL。

## 潜在问题 {#potential-problems}

### PHP 配置限制 {#limitations-of-php-configuration}

您可能会遇到主机上的 PHP 配置问题。主要有两个潜在问题：一是 PHP 的 `max_upload_size` 对于 WXR 文件来说可能太小；二是 PHP 内存限制可能不足以导入所有文章。

有两种解决方法：一是请求您的主机提供商提高限制（即使是临时的）；二是在 `/wp-admin/` 和 `/wp-includes` 目录中放置 php.ini 文件来提高限制（php.ini 文件不递归生效，因此必须放在这些目录中）。通常设置 10 MB 上传限制和 128 MB 内存限制即可，但请先咨询您的主机提供商，以免违反服务条款。

如需 PHP 配置问题帮助，请在 [WordPress 论坛支持](https://wordpress.org/documentation/forums/) 中搜索。

### 将附加域转换为停靠域 {#converting-add-on-domains-to-parked-domains}

在 cPanel 中删除附加域并用停靠域替换，也会删除与这些域关联的所有域名转发器和电子邮件转发器。请注意这一点，以便在切换后可以恢复这些转发器。

### 导入用户的限制 {#limitations-of-importing-users}

虽然存在上述方法可将内容导入到多站点博客实例中，但在导入多个用户时，您会遇到巨大的麻烦。用户会在导入过程中生成，但您无法为新博客获取任何角色或附加信息。

### 丢失设置 {#losing-settings}

如果旧站点已无法访问，而您发现自己忘记复制某些设置，或想确保所有配置都正确无误，请通过谷歌搜索您的站点，然后点击查看缓存版本。此选项仅在您的新站点被爬取之前可用，因此最好尽快操作。

另一个选择可能是使用 [互联网档案馆 Wayback Machine](https://archive.org/web/)。他们可能已存档了该站点（或部分内容）的副本。