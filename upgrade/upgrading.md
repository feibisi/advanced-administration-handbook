---
title: "升级 WordPress"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Upgrade
    - Repos
    - Data
---

# 升级 WordPress

## 升级 WordPress – 详细说明

本页面包含 [升级说明](https://wordpress.org/documentation/article/updating-wordpress/) 的更详细版本。

### 备份 WordPress {#backup-up-wordpress}

在开始之前，最好先备份您的网站。这样，如果出现任何问题，您可以恢复网站。完整的备份说明可在 WordPress 备份指南中找到。

### 一键升级 {#one-click-upgrade}

WordPress 允许您一键更新。您可以通过点击新版本横幅中的链接（如果有的话）或前往仪表盘 > 更新屏幕来启动更新。进入“更新 WordPress”页面后，点击“立即更新”按钮即可开始该过程。您无需执行其他操作，完成后您的网站将更新至最新版本。

一键更新适用于大多数服务器。如果遇到任何问题，很可能与文件系统的权限问题有关。

#### 托管服务工具 {#hosting-services-tools}

如果您的网站托管在 cPanel（请在左侧菜单栏中查找“WordPress 管理”）或 Plesk（请在左侧菜单栏中查找“WordPress”）中，您可能可以使用 WP-Toolkit。您可以在 WP-Toolkit 内对 WordPress 网站执行一键更新。您还可以在 WP-Toolkit 中配置自动更新。

如果您的网站托管在 WP Squared 服务器上，当有新更新可用时，您会看到通知和一个用于对 WordPress 网站执行一键更新的按钮。自动更新默认启用，您可以在 WP Squared 中配置是否启用或禁用自动更新。

_托管服务提供商：如果您的工具未在此列出，欢迎在 Github 上创建拉取请求以添加它。_

### 手动升级 {#manual-upgrade}

#### 升级流程概述 {#overview-of-the-upgrade-process}

1.  [备份您的数据库](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-1-back-up-your-database)。
2.  备份 WordPress 目录中的[所有 WordPress 文件](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-2-back-up-all-your-wordpress-files)。别忘了您的 [`.htaccess`](https://wordpress.org/documentation/article/wordpress-glossary/#.htaccess) 文件。
3.  [验证您创建的备份](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-3-verify-the-backups)是否存在且可用。这至关重要。
4.  [停用所有插件](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-4-deactivate-all-your-plugins)。
5.  [确保前四个步骤已完成](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-5-ensure-first-four-steps-are-completed)。除非您已完成前四个步骤，否则请勿尝试升级。
6.  从 [https://wordpress.org/download/](https://wordpress.org/download/) [下载并解压 WordPress 安装包](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-6-download-and-extract-the-wordpress-package)。
7.  [删除站点上的旧 WordPress 文件](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-7-delete-the-old-wordpress-files)，但**请勿删除**：
    – `wp-config.php` 文件；
    – `wp-content` 文件夹；特殊例外：应删除 `wp-content/cache` 和 `wp-content/plugins/widgets` 文件夹。
    – `.htaccess` 文件 – 如果您在 `.htaccess` 中添加了自定义规则，请不要删除它；
    – `robots.txt` 文件 – 如果您的博客位于站点根目录（即博客就是站点）并且您创建了此文件，请不要删除它。
8.  将新文件从您的计算机硬盘[上传到站点上相应的 WordPress 文件夹](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-8-upload-the-new-files)。
9.  [运行 WordPress 升级程序](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-9-run-the-wordpress-upgrade-program)并按照屏幕上的说明操作。
10. [更新固定链接和 .htaccess](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-10-update-permalinks-and-htaccess)。
11. [安装更新后的插件和主题](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-11-install-updated-plugins-and-themes)。
12. [重新激活插件](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-12-reactivate-plugins)。
13. [查看 WordPress 中的变更内容](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/#step-14-review-what-has-changed-in-wordpress)。

以上就是升级过程的概述。请继续阅读详细的升级说明。

请记住，如果遇到问题，请重新阅读下面的说明以确保遵循了正确的步骤，并参考[故障排除：常见安装问题](https://developer.wordpress.org/advanced-administration/before-install/howto-install/#common-installation-problems)。

#### 跨多个版本升级 {#upgrading-across-multiple-versions}

_虽然下文给出的方法是“安全”的，但只要你拥有正确的备份，那么确实可以一步到位地从 WordPress 的第一个版本直接升级到最新版本。WordPress 支持此过程，并且在这方面具有极强的向后兼容性。话虽如此，如果你的网站规模较大，升级过程可能比预期更长，此时采用渐进式方法可能会有所帮助。只需记住保留一个正常运行的网站备份，以便始终有回退方案。_

如果你计划跨越**两个**以上主要版本进行升级，应考虑采用渐进式升级，以避免潜在的冲突并最大限度地降低数据库损坏的风险。旧版本的 WordPress 可以从[发布档案库](https://wordpress.org/download/release-archive/)下载。

WordPress 3.7 引入了一个易于使用的一键更新器，可直接将你带到当前版本。此更新步骤是安全的，并且可以从 3.7 版本一键更新到任何后续版本。

##### 从 WordPress 0.7 - 3.6 升级（通过迁移）

目标：
- WordPress：升级到 WordPress 6.2
- PHP：升级到 PHP 7.4
- SQL：升级到 MySQL 8.0 / MariaDB 10.11

损失：
- 内容：无
- 插件：全部
- 主题：全部

这些是最旧版本的 WordPress，并且已经多年不受支持。通常需要假设会有一些损失，虽然不是内容损失，但可能是主题和插件的某些功能损失。

考虑到目标是保留内容并假设其他元素会损失，可以采取以下步骤。

与任何升级一样，首先要做的是备份。从 WP < 3.6 升级的最佳方式是执行内容迁移。

1. 创建一个全新的 WordPress，不包含数据库。
2. 将旧 WordPress 文件从 "/wp-content/uploads/" 目录复制到新站点。
3. 使用旧数据库信息创建一个新数据库。最佳方式是使用 "mysqldump"。
4. 使用所有新数据配置 wp-config.php。
5. 访问 "/wp-admin/" 页面，并按照升级流程操作。

通过这种方式，WordPress 将能够维护和更新数据库中的内容，并能在更新版本的 WordPress 中使用这些内容。

现在应该可以使用默认主题的 WordPress，并且所有内容都可用。

恢复数据库时，字符编码通常会出现技术问题。备份数据可能不是以 UTF-8 编码，而是可能采用 ISO 或 ASCII 等"已弃用"格式。确保在恢复数据库时正确更新字符编码！更多关于[在 WordPress 数据库中转换字符集的信息可以在这里找到](https://codex.wordpress.org/Converting_Database_Character_Sets)。

##### 从 WordPress 3.7 - 4.0 升级

目标
- WordPress：升级到 WordPress 4.1
- PHP：升级到 PHP 5.6.20+
- SQL：升级到 MySQL 5.6 / MariaDB 10.0

可能的损失：
- 内容：无
- 插件：很可能有
- 主题：很可能有

WordPress 版本 <= 4.0 兼容的 PHP 版本如今已很难获得。这些版本可能从 PHP 5.2（甚至更早）到 PHP 5.5 不等。因此，主要目标是升级到一个在许多操作系统上仍然容易获得的版本。

数据库方面也会发生同样的情况。很可能存在 MySQL 5.5 或更早版本。根据您是想继续使用 MySQL 还是迁移到 MariaDB，选择升级路径并将数据库迁移到 MySQL 5.6 或 MariaDB 10.0。

请注意，WP-CLI 不适用于低于 PHP 5.6.20 的 PHP 版本，因此此过程仍需部分手动完成。

与任何升级一样，首先要做的是备份。

删除所有未激活的主题，只保留主主题。如果有子主题处于激活状态，请保留子主题和父主题。

安装并激活 [Twenty Ten](https://wordpress.org/themes/twentyten/) 主题。该主题适用于自 WordPress 3.7 以来的所有站点。

同样地，删除所有已停用（因此未运行）的插件。

停用所有剩余的激活插件。

现在，WordPress 将具有：
-	核心：任意版本（介于 WordPress 3.7 和 WordPress 4.0 之间）
-	主题：Twenty Ten 已激活，主主题已停用。
-	插件：所有应激活的插件都已停用。

此时，使用 [WordPress 4.1](https://wordpress.org/wordpress-4.1.zip)（可在[发布列表](https://wordpress.org/download/releases/)中找到）覆盖 WordPress 核心。安装 WordPress 4.1 主版本，或者如果可用且推荐，安装最新的 4.1.x 次版本。

将系统升级到 PHP 5.6.20+ 和 MySQL 5.6.x 或 MariaDB 10.0.x。请不要安装最新的主版本。

访问 "/wp-admin/" 页面，并按照升级流程操作。

WordPress 将能够维护和更新数据库中的内容，并能够处理这些内容。现在，使用默认主题和所有内容的 WordPress 应该可用且正常工作。

在恢复数据库时，字符编码通常会出现技术问题。备份数据可能不是以 UTF-8 编码，而是可能采用 ISO 或 ASCII 的“已弃用”格式。确保在恢复数据库时正确更新字符编码！更多关于[在 WordPress 数据库中转换字符集的信息可以在这里找到](https://codex.wordpress.org/Converting_Database_Character_Sets)。

继续下一步，即从 WordPress 4.1 升级到 WordPress 4.9。

##### 从 WordPress 4.1 - 4.8 升级

目标
- WordPress：升级到 WordPress 4.9
- PHP：升级到 PHP 7.2
- SQL：保持或升级到 MySQL 5.6 / MariaDB 10.0

可能的损失：
- 内容：无
- 插件：很可能有
- 主题：很可能有

_如果你尚未配置 PHP 5.6.20+，请先配置。很可能一切仍能正常工作。_

从 WordPress 4.1 和 PHP 5.6.20+ 开始，你可以继续手动更新过程，或者开始使用 [WP-CLI](https://wp-cli.org/)，这是一个可以直接通过控制台运行 WordPress 命令的工具，可以简化流程。

与任何升级一样，首先要做的是备份。

删除所有非活动的主题，只保留主主题。如果有活动的子主题，请保留子主题和父主题。

安装并激活 [Twenty Ten](https://wordpress.org/themes/twentyten/) 主题。该主题自 WordPress 3.7 起适用于所有站点。

同样地，删除所有已停用（因此不工作）的插件。

现在，WordPress 将具有：
-	核心：任意版本（在 WordPress 4.1 和 WordPress 4.8 之间）
-	主题：Twenty Ten 处于活动状态，主主题已停用。
-	插件：所有应处于活动状态的插件都已停用。

此时，用 [WordPress 4.9](https://wordpress.org/wordpress-4.9.zip) 覆盖 WordPress 核心，该版本可在[发布列表](https://wordpress.org/download/releases/)中找到。安装 WordPress 4.9 主版本，或者如果可用且推荐，安装最新的 4.9.x 次要版本。

将系统升级到 PHP 7.2，如果尚未升级，则升级到 MySQL 5.6.x 或 MariaDB 10.0.x。请不要安装最新的主版本。

访问 "/wp-admin/" 页面，并按照升级流程操作。

WordPress 将能够维护和更新数据库中的内容，并能够处理这些内容。使用默认主题和所有内容的 WordPress 应该可用且正常工作。

字符编码在恢复数据库时通常会出现技术问题。备份数据可能未以 UTF-8 编码，而是可能采用 ISO 或 ASCII 的“已弃用”格式。确保在恢复数据库时正确更新字符编码！更多关于[在 WordPress 数据库中转换字符集的信息可以在这里找到](https://codex.wordpress.org/Converting_Database_Character_Sets)。

继续下一步，即从 WordPress 4.9 升级到 WordPress 5.3。

##### WordPress 4.9 - 5.2

目标
- WordPress：升级至 WordPress 5.3
- PHP：升级至 PHP 7.4
- SQL：保持或升级至 MySQL 8.0 / MariaDB 10.3

损失：
- 内容：无
- 插件：可能无
- 主题：可能无

_如果尚未配置 PHP 7.4，请立即配置。很可能一切仍会正常运行。_

WordPress 4.9 是最后一个包含经典编辑器的版本，因此许多因害怕新编辑器而停止更新 WordPress。WordPress 5.0+ 完全兼容经典编辑器内容，因此升级不会丢失任何内容。

此外，WordPress 4.9 发布时，PHP 7.0+ 已非常稳定，且 WordPress 5.0 版本已提供支持。从 PHP 5.6.20+ 升级到 PHP 7.0+ 应该非常稳定。

从 WordPress 4.9 开始，您可以继续手动更新过程，或开始使用 [WP-CLI](https://wp-cli.org/)，该工具可直接通过控制台运行 WordPress 命令，从而简化流程。

与任何升级一样，首先要做的是备份。

删除所有未激活的主题，仅保留主主题。如果有子主题处于活动状态，请保留子主题和父主题。

安装并激活 [Twenty Ten](https://wordpress.org/themes/twentyten/) 主题。该主题自 WordPress 3.7 起适用于所有站点。

同样，删除所有已停用的插件。

现在，WordPress 将具有：
-	核心：任意版本（介于 WordPress 4.9 和 WordPress 5.2 之间）
-	主题：Twenty Ten 处于活动状态，主主题已停用。
-	插件：所有应处于活动状态的插件均已停用。

此时，使用 [WordPress 5.3](https://wordpress.org/wordpress-5.3.zip) 覆盖 WordPress 核心，该版本可在[发布列表](https://wordpress.org/download/releases/)中找到。安装 WordPress 5.3 主版本，或如果可用且推荐，安装最新的 5.3.x 次版本。

将系统升级至 PHP 7.4，如果尚未升级，则升级至 MySQL 8.0.x 或 MariaDB 10.3.x。请不要安装最新的主版本。

访问 "/wp-admin/" 页面，并按照升级流程操作。

WordPress 将能够维护和更新数据库中的内容，并能够处理这些内容。

到达此阶段后，请进行新的备份，因为将进行更多更新，此时 WordPress 处于良好状态。

WordPress 4.9+ 中可用的大多数插件应与 WordPress 5.3 兼容，因此请尝试更新插件列表中的所有可用插件。请逐一更新并检查所有警告和错误。如果出现重大错误，请尝试使用该插件的旧版本。通常，这些旧版本位于 wordpress.org 上每个插件页面的“开发者”选项卡末尾。

对主题进行相同操作。WordPress 4.9+ 中可用的大多数主题应与 WordPress 5.3 兼容。

继续下一步，即从 WordPress 5.3 升级到 WordPress 6.2。

##### WordPress 5.3 - 6.2

目标
- WordPress：升级至 WordPress 6.2
- PHP：升级至 PHP 7.4
- SQL：保持或升级至 MySQL 8.0 LTS / MariaDB 10.11 LTS

损失：
- 内容：无
- 插件：很可能无
- 主题：很可能无

_如果你尚未配置 PHP 7.4，请立即配置。很可能一切仍会正常运行。_

正常升级所有内容。一切应该都能正常工作。

##### WordPress 6.3 - 6.8

目标
- WordPress：升级到 WordPress 6.8
- PHP：至少升级到 PHP 8.1（WordPress 6.6+ 支持 PHP 8.2）
- SQL：维持或升级到 MySQL 8.0 LTS / MariaDB 10.11 LTS

可能的损失：
- 内容：无
- 插件：很可能无
- 主题：很可能无

_如果你尚未配置 PHP 8.1，请立即配置。很可能一切仍会正常运行。_

当 WordPress 6.3 发布时，停止了对 PHP 5.6 的支持，并将 PHP 7.0 确立为最低 PHP 版本。从 PHP 5.6.20+ 升级到 PHP 7.0+ 应该非常稳定。

当 WordPress 6.6 发布时，停止了对 PHP 7.0 和 7.1 的支持，并将 PHP 7.2.24 确立为最低 PHP 版本。从 PHP 7.0+ 或 PHP 7.1+ 升级到 PHP 7.2+ 应该非常稳定。WordPress 6.6 也支持 PHP 8.2，因此你可以在升级 WordPress 后尝试切换到 PHP 8.2。

正常升级所有内容。一切应该都能正常运行。

### 故障排除 {#troubleshooting}

**布局混乱或出现错误**

如果你的博客现在看起来布局混乱或出现行错误，罪魁祸首可能是一个与新代码不兼容的旧插件。在你的 WordPress [管理屏幕](https://wordpress.org/documentation/article/administration-screens)中，停用所有非 WordPress 默认自带的插件。然后逐个重新激活它们。

**进行过自定义修改/“黑客”操作？**

如果你修改过其他 WordPress 文件（即“黑入”过 WordPress），你应该记录下你的修改。你需要将你的编辑内容转移到新代码中。[WordPress 版本](https://codex.wordpress.org/WordPress_Versions)列出了每个版本中发生变化的文件。

**避免使用旧代码**

升级能让你获得最新、最好的代码。使用你的旧代码，无论你做了多少自定义，几乎肯定会引发问题。直接使用你修改过的旧代码的诱惑可能很大，但出现错误的可能性要大得多。

**可以回退到旧版本吗**

可以，但通常不建议将当前版本回滚（降级）到旧版本。这是因为新版本通常包含安全更新，回滚可能会让你的网站面临风险。其次，版本间数据库结构的差异可能导致在维护网站内容、文章、评论以及依赖数据库中存储信息的插件时出现复杂问题。如果你仍然执意要这样做，请自行承担风险。**请注意，如果没有在尝试升级前备份整个网站和数据库，成功回滚几乎是不可能的。** 删除除 `wp-config.php` 外的所有 WordPress 文件。[上传](https://developer.wordpress.org/advanced-administration/upgrade/ftp/filezilla/)备份文件到你的服务器，并[恢复数据库备份](https://wordpress.org/article/restoring-your-database-from-backup/)。记住，你必须拥有良好的备份才能使回滚生效。对于较旧的 WordPress 版本，回滚可能无法成功。

**获取更多帮助**

如果在升级后遇到任何错误，请查阅[故障排除：常见安装问题](https://developer.wordpress.org/advanced-administration/before-install/howto-install/#common-installation-problems)、[故障排除](https://codex.wordpress.org/Troubleshooting)以及[安装类文章](https://wordpress.org/documentation/category/installation/)。如果找不到答案，请在 [WordPress 支持论坛](https://wordpress.org/support/forums/)上清晰地描述你的问题。届时会有人询问你是否使用了任何旧代码。你将被要求修改它，所以不如现在就改掉 🙂。

## 配置自动后台更新

### 更新类型 {#update-types}

自动后台更新功能于 [WordPress 3.7](https://wordpress.org/documentation/wordpress-version/version-3-7/) 版本引入，旨在提升安全性并优化整体更新体验。在大多数站点上，WordPress 的自动更新功能默认处于启用状态。特殊情况下，插件和主题也可能自动更新。翻译文件默认会自动更新。

WordPress 包含四种自动后台更新类型：

1. 核心更新
2. 插件更新
3. 主题更新
4. 翻译文件更新

#### 核心更新 {#core-updates}

核心更新细分为三种类型：

1.  核心开发更新，称为“前沿版本”
2.  次要核心更新，例如维护和安全版本
3.  主要核心版本更新

在 WordPress 5.6 之前，默认情况下，每个站点仅对次要核心版本和翻译文件启用了自动更新。从 WordPress 5.6 及更高版本开始，每个新站点都对次要版本和主要版本启用了自动更新。

已运行开发版本的站点默认也启用了对后续开发版本的自动更新。

### 更新配置 {#update-configuration}

自动更新可通过以下两种方法之一进行配置：在 `wp-config.php` 中定义常量，或使用插件添加过滤器。

#### 通过 wp-config.php 配置 {#configuration-via-wp-config-php}

使用 `wp-config.php` 可以完全禁用自动更新，并可根据更新类型禁用或配置核心更新。

##### 用于禁用所有更新的常量 {#constant-to-disable-all-updates}

核心开发者经过慎重考虑，决定默认启用次要版本和翻译文件的自动更新。未来，这将是确保您的网站保持最新和安全的最佳方式之一，因此强烈不建议禁用这些更新。

要完全禁用所有类型的自动更新（无论是核心更新还是其他更新），请将以下代码添加到您的 `wp-config.php` 文件中：

```
define( 'AUTOMATIC_UPDATER_DISABLED', true );
```

##### 配置核心更新的常量 {#constant-to-configure-core-updates}

要为主版本或开发目的启用自动更新，可以从 `WP_AUTO_UPDATE_CORE` 常量开始。通过三种方式之一定义此常量，可以一次性批量启用或禁用多种类型的核心更新。

```
define( 'WP_AUTO_UPDATE_CORE', true );
```

`WP_AUTO_UPDATE_CORE` 可以用以下三个值之一定义，每个值会产生不同的行为：

* 值为 `true` – 开发版、次要版本和主要版本更新**全部启用**
* 值为 `false` – 开发版、次要版本和主要版本更新**全部禁用**
* 值为 `'minor'` – 次要版本更新**启用**，开发版和主要版本更新**禁用**

请注意，只有已运行开发版本的站点才会收到开发更新。

对于开发站点，`WP_AUTO_UPDATE_CORE` 的默认值为 `true`。对于其他站点，`WP_AUTO_UPDATE_CORE` 的默认值为 `minor`。

从 WordPress 5.6 开始，新 WordPress 安装的 `WP_AUTO_UPDATE_CORE` 默认值为 `true`。对于新网站，`WP_AUTO_UPDATE_CORE` 的默认值为 `minor`。

#### 通过过滤器配置 {#configuration-via-filters}

使用过滤器可以实现对自动更新的精细控制。

放置这些过滤器的最佳位置是在 [必须使用插件](https://developer.wordpress.org/advanced-administration/plugins/mu-plugins/) 中。

请*勿*在 `wp-config.php` 中直接添加 `add_filter()` 调用。此时 WordPress 尚未完全加载，可能导致与 WP-CLI 等其他应用程序冲突。

##### 通过过滤器禁用所有更新 {#disabling-all-updates-via-filter}

你也可以使用以下过滤器来禁用所有自动更新：

```
add_filter( 'automatic_updater_disabled', '__return_true' );
```

##### 通过过滤器进行核心更新 {#core-updates-via-filter}

要仅启用所有核心类型的更新，请使用以下过滤器：

```
add_filter( 'auto_update_core', '__return_true' );
```

但是，假设您不想简单地启用或禁用所有三种类型的核心更新，而是希望有选择地启用或禁用它们。这时就需要用到 `allow_dev_auto_core_updates`、`allow_minor_auto_core_updates` 和 `allow_major_auto_core_updates` 过滤器。

WordPress 内置了两个简写函数，允许您用单行代码启用或禁用特定类型的核心更新。它们是 [__return_true](https://developer.wordpress.org/reference/functions/__return_true) 和 [__return_false](https://developer.wordpress.org/reference/functions/__return_false)。以下是一些过滤器示例：

要单独*启用*它们（如需禁用，请使用 **false** 代替 **true**）：

```
add_filter( 'allow_dev_auto_core_updates', '__return_true' ); // 启用开发版本更新
add_filter( 'allow_minor_auto_core_updates', '__return_true' ); // 启用次要更新
add_filter( 'allow_major_auto_core_updates', '__return_true' ); // 启用主要更新
```

面向开发者：即使 WordPress 目录或其任何父目录中存在 VCS 文件夹（.git、.hg、.svn 等），也要*启用*自动更新：

```
add_filter( 'automatic_updates_is_vcs_checkout', '__return_false', 1 );
```

##### Plugin & Theme Updates via Filter {#plugin-theme-updates-via-filter}

By default, automatic background updates only happen for plugins and themes in special cases, as determined by the WordPress.org API response, which is controlled by the WordPress security team for patching critical vulnerabilities. To enable or disable updates in all cases, you can leverage the `auto_update_$type` filter, where `$type` would be replaced with "plugin" or "theme".

Automatic updates for All plugins

```
add_filter( 'auto_update_plugin', '__return_true' );
```

Automatic updates for All themes:

```
add_filter( 'auto_update_theme', '__return_true' );
```

You can use `__return_false` instead of `__return_true` to specifically disable all plugin & theme updates, even forced security pushes from the WordPress security team.

The `auto_update_$type` filters also allow for more fine-grained control, as the specific item to updated is also passed into the filter. If you wanted to enable auto-updates for specific plugins only, then you could use code like this:

```
function auto_update_specific_plugins ( $update, $item ) {
	// Array of plugin slugs to always auto-update
	$plugins = array (
		'akismet',
		'buddypress',
	);
	if ( in_array( $item->slug, $plugins ) ) {
		 // Always update plugins in this array
		return true;
	} else {
	 	// Else, use the normal API response to decide whether to update or not
		return $update;
	}
}
add_filter( 'auto_update_plugin', 'auto_update_specific_plugins', 10, 2 );
```

##### 通过过滤器更新翻译文件 {#translation-updates-via-filter}

与次要核心更新相同，默认已启用自动翻译文件更新。

如需禁用翻译文件更新，请使用以下代码：

```
add_filter( 'auto_update_translation', '__return_false' );
```

##### 通过过滤器禁用邮件 {#disable-emails-via-filter}

```
// 禁用更新邮件
add_filter( 'auto_core_update_send_email', '__return_false' );
```

此过滤器也可用于根据邮件 $type（成功、失败、严重）、更新类型对象 $core_update 或 $result 来操控更新邮件：

```
/* @param bool   $send        是否发送邮件。默认为 true。
@param string $type        要发送的邮件类型。可以是 'success'、'fail'、'critical' 之一。
@param object $core_update 尝试的更新提议。
@param mixed  $result      核心更新的结果。可以是 WP_Error。
*/
apply_filters( 'auto_core_update_send_email', true, $type, $core_update, $result );
```

### 资源 {#resources}

* 更多示例请访问：[https://make.wordpress.org/core/2013/10/25/the-definitive-guide-to-disabling-auto-updates-in-wordpress-3-7/](https://make.wordpress.org/core/2013/10/25/the-definitive-guide-to-disabling-auto-updates-in-wordpress-3-7/)
* 更多信息请见：[如何在 WordPress 3.7 中配置自动更新？](https://wordpress.stackexchange.com/questions/120081/how-do-i-configure-automatic-updates-in-wordpress-3-7)
* 关于 wp-cli 冲突的信息：[https://github.com/wp-cli/wp-cli/issues/1310](https://github.com/wp-cli/wp-cli/issues/1310)