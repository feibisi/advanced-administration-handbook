---
title: "回环"
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

# 回环

回环是指您自己的服务器或网站尝试连接到自身。

WordPress 使用此功能来触发定时发布文章，以及插件或主题可能引入的其他定时事件。

在插件或主题编辑器中修改时也会用到回环，通过重新连接到网站来确保所做的更改不会破坏您的网站。

## 排查回环问题 {#troubleshooting-loopback-issues}

如果您遇到定时发布文章或其他定时事件无法运行的问题，或者看到关于回环失败的站点健康警告，您可能需要排查这些问题。

回环失败最常见的原因是插件或主题冲突，您应该首先按照常规的故障排除步骤操作：

## 常见故障排除步骤 {#common-troubleshooting-steps}

* **停用所有插件**（是的，所有插件）以查看是否解决了问题。如果有效，请逐个重新激活插件，直到找到有问题的插件。如果无法进入管理后台，请尝试通过 [SFTP/FTP](https://developer.wordpress.org/advanced-administration/upgrade/ftp/) 或 phpMyAdmin 重置插件文件夹（如果需要帮助，请阅读[无法登录 wp-admin 时如何停用所有插件](https://wordpress.org/documentation/article/faq-troubleshooting/)）。有时，看似未激活的插件仍可能导致问题。同时请记得停用 `mu-plugins` 文件夹中的任何插件。最简单的方法是将该文件夹重命名为 `mu-plugins-old`。
* 切换到 Twenty-Something 主题以排除特定主题的问题。如果无法登录更改主题，可以通过 [SFTP/FTP](https://developer.wordpress.org/advanced-administration/upgrade/ftp/) 删除主题文件夹，只保留 `twentytwentythree`。这将强制您的网站使用该主题。
* 如果可以安装插件，请安装 [Health Check](https://wordpress.org/plugins/health-check/) 插件。在故障排除选项卡中，您可以点击按钮来禁用所有插件并为您更改主题，同时您仍保持登录状态，**且不会影响网站的正常访客**。