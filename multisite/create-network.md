---
title: "创建网络"
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

# 创建网络

您可以使用[多站点](https://wordpress.org/documentation/article/glossary/#multisite)功能创建一个[站点](https://wordpress.org/documentation/article/glossary/#site)[网络](https://wordpress.org/documentation/article/glossary/#network)。本文包含创建多站点网络的说明。建议先阅读文章“[创建网络前的准备](https://developer.wordpress.org/advanced-administration/multisite/prepare-network/)”，其中包含关于规划网络的重要信息。

多站点网络可以非常类似于您个人版的 WordPress.com。您网络的最终用户可以按需创建自己的站点，就像 WordPress.com 的最终用户可以按需创建博客一样。如果您不需要允许最终用户按需创建自己的站点，您可以创建一个多站点网络，其中只有您（管理员）可以添加新站点。

多站点网络是一组共享相同 WordPress 安装核心文件的站点集合。它们还可以共享插件和主题。网络中的各个站点是*虚拟*站点，因为它们没有在服务器上拥有自己的目录，尽管它们在共享安装中有独立的媒体上传目录，并且在数据库中有独立的表。**注意：**[升级后找不到网络管理菜单？](https://developer.wordpress.org/advanced-administration/multisite/administration/#network-admin-link-location)

## 步骤 0：准备工作 {#step-0-before-you-begin}

与典型的单站点 WordPress 安装相比，网络安装需要考虑更多事项。您必须决定使用子域名还是子目录，以及如何管理它们。安装主题和插件的方式也不同：例如，网络中的每个独立站点都可以激活两者，但都不能安装。

BitNami 也提供[即用型安装包](https://codex.wordpress.org/User:Beltranrubo/BitNami_Multisite)。

**请务必完整阅读[创建网络前的准备工作](https://developer.wordpress.org/advanced-administration/multisite/prepare-network/)，然后再继续。**

## 步骤 1：准备您的 WordPress {#step-1-prepare-your-wordpress}

创建网络时，您现有的 WordPress 站点将被更新。除非这是一个全新安装且您不会丢失任何内容，否则请[备份您的数据库和文件](https://developer.wordpress.org/advanced-administration/security/backup/)。

请确认[固定链接](https://wordpress.org/documentation/article/using-permalinks/)在您的单个 WP 实例上正常工作。

同时，请停用所有已启用的插件。您可以在网络创建完成后重新激活它们。

如果您计划[将 WordPress 从其自身目录中移出运行](https://developer.wordpress.org/advanced-administration/server/wordpress-in-directory/)，请在启用多站点功能*之前*完成此操作。

## 步骤 2：启用多站点功能 {#step-2-allow-multisite}

要启用“网络设置”菜单项，您必须先在 [wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件中定义多站点。

打开 `wp-config.php` 文件，在出现 `/* That's all, stop editing! Happy publishing. */` 的**上方**添加以下代码行。如果文件中没有该行，则在第一个以 `require` 或 `include` 开头的代码行之前添加：

```
/* Multisite */
define( 'WP_ALLOW_MULTISITE', true );
```

您需要刷新浏览器才能继续。

## 步骤 3：安装网络 {#step-3-installing-a-network}

上一步操作使您的**工具菜单**中启用了**网络设置**项。使用该菜单项进入**创建 WordPress 站点网络**屏幕。

要查看创建 WordPress 站点网络屏幕的示例，请参阅[管理](https://wordpress.org/documentation/article/administration-screens/) > [工具](https://wordpress.org/documentation/article/administration-screens/#tools-managing-your-blog) > [网络设置](https://wordpress.org/documentation/article/tools-network-screen/)。该屏幕在所有情况下看起来并不完全相同。显示的示例是针对 `localhost` 上的安装，这限制了可用的选项。

[![创建 WordPress 站点网络页面](https://i0.wp.com/wordpress.org/support/files/2018/11/network-create.png?fit=1024%2C743&ssl=1)](https://i0.wp.com/wordpress.org/support/files/2018/11/network-create.png?fit=1024%2C743&ssl=1)
_创建 WordPress 站点网络页面_

**网络中站点的地址**

您可以在子域名和子目录之间进行选择，除非[现有设置](https://developer.wordpress.org/advanced-administration/multisite/prepare-network/#wordpress-settings-requirements)限制了您的选择。

您必须选择其中一种。尽管屏幕上有建议，但您可以在安装后将网络重新配置为使用另一种选择，但重新配置可能并不容易。

尽管屏幕上可能有建议，但您只需要为按需的基于域名的站点配置通配符 DNS。

再次提醒：请参阅[创建网络之前](https://developer.wordpress.org/advanced-administration/multisite/prepare-network/)。

*   **子域名** — 一种基于域名的网络，其中按需站点使用子域名
*   **子目录** — 一种基于路径的网络，其中按需站点使用路径

**网络详细信息**

这些信息会自动填充，但您可以进行更改。`服务器地址`：您用于访问 WordPress 安装的 URL 的域名。`网络标题`：您整个网络的标题。`网络管理员电子邮件`：您作为整个网络超级管理员的电子邮件地址。

仔细核对详细信息，然后按**安装**按钮。

**注意：** 当您尚未配置通配符子域名，或者根本不需要它们时，安装程序可能会检查通配符子域名。如果警告不适用于您的网络，请忽略它。有关通配符子域名的信息，请参阅[创建网络之前](https://developer.wordpress.org/advanced-administration/multisite/prepare-network/)中的[服务器要求](https://developer.wordpress.org/advanced-administration/multisite/prepare-network/#server-requirements)部分。

## 步骤 4：启用网络 {#step-4-enabling-the-network}

要启用您的网络，请按照"创建 WordPress 站点网络"屏幕上的说明操作。您看到的说明是针对您的安装定制的，可能与您在此处看到的示例不同。

[![创建站点网络时已填充的设置](https://i0.wp.com/wordpress.org/support/files/2018/11/tools-network-created.png?fit=1024%2C742&ssl=1)](https://i0.wp.com/wordpress.org/support/files/2018/11/tools-network-created.png?fit=1024%2C742&ssl=1)
_创建站点网络时已填充的设置_

备份您现有的 `wp-config.php` 和 `.htaccess` 文件，除非这是全新安装且您没有需要保留的内容。

有两个步骤：

1.  将指定的行添加到您的 [wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件中。这些额外的行应添加在您于[步骤 1：准备您的 WordPress](https://developer.wordpress.org/advanced-administration/multisite/create-network/#step-1-prepare-your-wordpress) 中添加的那一行之后。
2.  将指定的行添加到您的 `.htaccess` 文件中。如果您没有 `.htaccess` 文件，请在 `wp-config.php` 文件所在的目录中创建它。如果您*已经*有一个 `.htaccess` 文件，请用这些新行替换任何现有的 WordPress 相关行。在某些情况下，您可能还需要在文件开头添加 `Options FollowSymlinks`。

完成这些步骤后，使用提供的链接重新登录。您可能需要清除浏览器的缓存和 Cookie 才能登录。

## 步骤 5：网络管理员设置 {#step-5-network-admin-settings}

[![](https://i0.wp.com/wordpress.org/support/files/2018/11/network-admin-link.png?fit=383%2C184&ssl=1)](https://i0.wp.com/wordpress.org/support/files/2018/11/network-admin-link.png?fit=383%2C184&ssl=1)

在 WordPress 工具栏左侧，**我的站点**现在是第二项。在那里，列出了您的所有站点，带有便捷的弹出菜单，以及一个**网络管理员**菜单项。在**网络管理员**下，您可以使用**仪表盘**项转到网络仪表盘屏幕。

前往[设置屏幕](https://developer.wordpress.org/advanced-administration/multisite/admin/)配置网络选项，并前往[站点屏幕](https://developer.wordpress.org/advanced-administration/multisite/admin/#Sites)管理您的站点。

更多信息，请参阅：[网络管理员](https://developer.wordpress.org/advanced-administration/multisite/admin/)

[已升级但找不到网络管理员菜单？](https://developer.wordpress.org/advanced-administration/multisite/administration/#network-admin-link-location)

## 步骤 6：管理 {#step-6-administration}

由于多站点的额外复杂性，你可能需要了解一些关于网络高级管理的额外知识。即使你熟悉 WordPress，多站点网络管理的位置和行为也可能令人困惑。

阅读 [多站点网络管理](https://developer.wordpress.org/advanced-administration/multisite/administration/) 以获取更多信息。

如需故障排除帮助：

* [调试 WordPress 网络](https://developer.wordpress.org/advanced-administration/debug/debug-network/)

## 相关文章 {#related-articles}

* [托管 WordPress](https://wordpress.org/documentation/article/hosting-wordpress/)
* [安装多个博客](https://developer.wordpress.org/advanced-administration/before-install/multiple-instances/)
* [如何让我的插件适配多站点？](https://stackoverflow.com/questions/13960514/how-to-adapt-my-plugin-to-multisite/)