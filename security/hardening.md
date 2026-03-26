---
title: "强化 WordPress 安全性"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Security
    - Repos
    - Data
---

# 强化 WordPress 安全性

WordPress 非常[重视安全性](https://wordpress.org/about/security/)，但与其他系统一样，如果不采取一些基本的安全预防措施，可能会出现潜在的安全问题。本文将介绍一些常见的漏洞形式，以及您可以采取的措施来保护 WordPress 安装的安全。

本文并非解决安全问题的终极快速方案。如果您有特定的安全疑虑或疑问，应与您信任的、具备足够计算机安全和 WordPress 知识的人士讨论。

## 什么是安全？ {#what-is-security}

从根本上说，安全**并非**指绝对安全的系统。这样的系统可能不切实际，或者难以找到和/或维护。安全实际上是降低风险，而非消除风险。它意味着在合理范围内，运用所有可用的适当控制措施，以改善整体安全状况，降低成为攻击目标并随后被入侵的可能性。

**网站托管服务**

在网站安全方面，一个良好的起点通常是您的托管环境。如今，您有多种选择，虽然托管服务提供商会提供一定程度的安全保障，但重要的是要了解他们的责任在哪里结束，而您的责任从哪里开始。这里有一篇很好的文章，解释了[网络托管服务与您网站安全之间复杂的动态关系](https://perezbox.com/2014/11/how-hosts-manage-your-website-security/)。一个安全的服务器能保护服务器管理员控制下的资源的隐私性、完整性和可用性。

可信赖的网站托管服务商应具备以下特质：

* 乐于讨论您的安全顾虑，并说明其托管服务提供哪些安全功能和流程。
* 提供所有服务器软件的最新稳定版本。
* 提供可靠的备份和恢复方法。

通过确定需要保护的软件和数据，来决定您的服务器需要哪些安全措施。本指南的其余部分将帮助您完成这项工作。

**网站应用程序**

人们很容易将目光投向网站托管服务商，并将安全责任推给他们，但网站所有者自身也承担着巨大的安全责任。托管服务商通常负责您网站所依赖的基础设施，但并不对您选择安装的应用程序负责。

要理解这一点为何重要以及其重要性体现在哪里，您必须[了解网站是如何被入侵的](https://blog.sucuri.net/2015/05/website-security-how-do-websites-get-hacked.html)。入侵很少归因于基础设施，而最常归因于应用程序本身（即您负责的环境）。

### 安全主题 {#security-themes}

在为系统的每个方面考虑安全时，请牢记以下一些总体思路：

**限制访问**

做出明智的选择，以减少恶意人员可能利用的入口点。

**遏制**

应配置您的系统，以在系统被入侵时最大限度地减少可能造成的损害。

**准备与认知**

定期备份并了解 WordPress 安装的状态。制定灾难情况下的备份和恢复计划，可以帮助您在出现问题时更快地恢复在线。

**可信来源**

不要从未知来源获取插件/主题。请仅使用 WordPress.org 官方仓库或知名公司的产品。尝试从外部获取插件/主题[可能导致问题](https://blog.sucuri.net/2014/03/unmasking-free-premium-wordpress-plugins.html)。

### 您计算机上的漏洞 {#vulnerabilities-on-your-computer}

请确保您使用的计算机没有间谍软件、恶意软件和病毒感染。如果您的计算机上存在键盘记录器，WordPress 或您的 Web 服务器上的任何安全措施都将无济于事。

请始终将您的操作系统及其上的软件（尤其是您的 Web 浏览器）保持最新，以防范安全漏洞。如果您正在浏览不受信任的站点，我们还建议您在浏览器中使用 no-script 等工具（或禁用 JavaScript/Flash/Java）。

### WordPress 中的漏洞 {#vulnerabilities-in-wordpress}

与许多现代软件包一样，WordPress 会定期更新以解决可能出现的新安全问题。提升软件安全性始终是一项持续的工作，为此**您应始终使用最新版本的 WordPress**。旧版本的 WordPress 不会获得安全更新维护。

#### Updating WordPress {#updating-wordpress}

Main article: [Updating WordPress](https://wordpress.org/documentation/article/updating-wordpress/).

The latest version of WordPress is always available from the main WordPress website at https://wordpress.org. Official releases are not available from other sites — **never** download or install WordPress from any website other than https://wordpress.org.

Since version 3.7, WordPress has featured automatic updates. Use this functionality to ease the process of keeping up to date. You can also use the WordPress Dashboard to keep informed about updates. Read the entry in the Dashboard or the WordPress Developer Blog to determine what steps you must take to update and remain secure.

If a vulnerability is discovered in WordPress and a new version is released to address the issue, the information required to exploit the vulnerability is almost certainly in the public domain. This makes old versions more open to attack, and is one of the primary reasons you should always keep WordPress up to date.

If you are an administrator in charge of more than one WordPress installation, consider using [Subversion](https://codex.wordpress.org/Installing/Updating_WordPress_with_Subversion) to make management easier.

#### 报告安全问题 {#reporting-security-issues}

如果您认为在 WordPress 中发现了安全漏洞，可以通过报告问题来提供帮助。请参阅[安全常见问题解答](https://make.wordpress.org/core/handbook/testing/reporting-security-vulnerabilities/)了解如何报告安全问题。

如果您认为发现了错误，请报告它。请参阅[提交错误](https://make.wordpress.org/core/handbook/testing/reporting-bugs/)了解如何操作。您可能发现了一个漏洞，或是一个可能导致漏洞的错误。

### Web 服务器漏洞 {#web-server-vulnerabilities}

运行 WordPress 的 Web 服务器及其上的软件可能存在漏洞。因此，请确保您运行的是安全、稳定的 Web 服务器版本及其上的软件，或者确保您使用的是值得信赖的主机，由他们为您处理这些事务。

如果您使用的是共享服务器（除了您自己的网站外还托管其他网站），并且同一服务器上的某个网站遭到入侵，那么即使您遵循了本指南中的所有建议，您的网站也可能受到威胁。请务必向您的[网络主机](https://wordpress.org/documentation/article/glossary/#Hosting_provider)咨询他们采取了哪些安全预防措施。

### 网络漏洞 {#network-vulnerabilities}

两端网络——WordPress 服务器端和客户端网络端——都应是可信的。这意味着需要更新家庭路由器的防火墙规则，并谨慎选择工作网络。在网吧通过未加密连接（无论是有线还是无线）发送密码，**不是**可信网络。

您的网络托管服务商应确保其网络不受攻击者入侵，您也应如此。网络漏洞可能导致密码和其他敏感信息被截获。

### 密码 {#passwords}

良好的安全习惯可以避免许多潜在漏洞。设置强密码是其中的重要一环。

密码的目标是让他人难以猜测，并使[暴力破解攻击](https://developer.wordpress.org/advanced-administration/security/brute-force/)难以成功。可以使用许多[自动密码生成器](https://www.google.com/?q=password+generator)来创建安全密码。

WordPress 还提供了密码强度检测功能，在 WordPress 中更改密码时会显示。更改密码时请利用此功能确保密码强度足够。

选择密码时应避免以下情况：

* 任何与本人真实姓名、用户名、公司名称或网站名称相关的排列组合。
* 任何语言词典中的单词。
* 过短的密码。
* 纯数字或纯字母密码（混合使用最佳）。

强密码不仅对保护博客内容至关重要。黑客一旦获取管理员账户访问权限，就能安装恶意脚本，可能危及整个服务器。

除了使用强密码外，建议启用[两步验证](https://developer.wordpress.org/advanced-administration/security/mfa/)作为额外的安全措施。

### FTP {#ftp}

连接到您的服务器时，如果您的网络主机提供 SFTP 加密，您应该使用它。如果您不确定您的网络主机是否提供 SFTP，直接询问他们即可。

使用 SFTP 与 FTP 相同，区别在于您的密码和其他数据在计算机和网站之间传输时会被加密。这意味着您的密码永远不会以明文形式发送，攻击者无法截获。

### 文件权限 {#file-permissions}

WordPress 的一些巧妙功能源于允许 Web 服务器对各类文件进行写入操作。然而，允许文件写入访问存在潜在风险，尤其是在共享托管环境中。

最佳做法是尽可能严格锁定文件权限，仅在需要允许写入访问时放宽限制，或为特定目的（如上传文件）创建限制较少的专用文件夹。

以下是一种可行的权限设置方案。

所有文件应归您的用户账户所有，且仅对您可写。任何需要 WordPress 写入访问的文件应对 Web 服务器可写——如果您的托管环境要求如此，这可能意味着这些文件需要由 Web 服务器进程使用的用户账户进行组所有。

**`/`**

WordPress 根目录：所有文件应仅对您的用户账户可写，但若希望 WordPress 自动生成重写规则，则 `.htaccess` 文件除外。

**`/wp-admin/`**

WordPress 管理区域：所有文件应仅对您的用户账户可写。

**`/wp-includes/`**

WordPress 应用程序逻辑主体：所有文件应仅对您的用户账户可写。

**`/wp-content/`**

用户提供的内容：设计为对您的用户账户和 Web 服务器进程可写。

在 `/wp-content/` 目录内，您会发现：

**`/wp-content/themes/`**

主题文件。若需使用内置主题编辑器，所有文件需对 Web 服务器进程可写。若不使用内置主题编辑器，所有文件可仅对您的用户账户可写。

**`/wp-content/plugins/`**

插件文件：所有文件应仅对您的用户账户可写。

`/wp-content/` 中可能存在的其他目录应由请求它们的插件或主题进行说明。其权限设置可能有所不同。

#### 更改文件权限 {#changing-file-permissions}

如果您拥有服务器的 shell 访问权限，可以使用以下命令递归更改文件权限：

对于目录：

```
find /path/to/your/wordpress/install/ -type d -exec chmod 755 {} \;
```

对于文件：

```
find /path/to/your/wordpress/install/ -type f -exec chmod 644 {} \;
```

#### 关于自动更新 {#regarding-automatic-updates}

当您指示 WordPress 执行自动更新时，所有文件操作将以文件所有者的身份执行，而非 Web 服务器用户身份。所有文件权限将设置为 0644，目录权限设置为 0755，仅允许所有者写入，其他用户（包括 Web 服务器）仅可读取。

### 数据库安全 {#database-security}

若在同一服务器上运行多个博客站点，建议考虑将它们分别置于由不同用户管理的独立数据库中。这一操作最好在初次进行 [WordPress 安装](https://developer.wordpress.org/advanced-administration/before-install/howto-install/)时完成。这是一种隔离策略：即使入侵者成功攻破某个 WordPress 实例，也能极大增加其篡改其他博客的难度。

若您自行管理 MySQL，请确保理解 MySQL 配置并禁用非必要功能（例如接受远程 TCP 连接）。更多信息请参阅 [MySQL 安全指南](https://dev.mysql.com/doc/refman/8.0/en/security.html)。

#### 限制数据库用户权限 {#restricting-database-user-privileges}

对于正常的 WordPress 操作，例如发布博客文章、上传媒体文件、发表评论、创建新 WordPress 用户和安装 WordPress 插件，MySQL 数据库用户仅需要对 MySQL 数据库的数据读取和写入权限：SELECT、INSERT、UPDATE 和 DELETE。

因此，任何其他数据库结构和管理权限，例如 DROP、ALTER 和 GRANT，都可以被撤销。通过撤销这些权限，您也在加强遏制策略。

**注意：** 某些插件、主题和主要的 WordPress 更新可能需要更改数据库结构，例如添加新表或更改架构。在这种情况下，安装插件或更新软件之前，您需要临时授予数据库用户所需的权限。

**警告：** 尝试在没有这些权限的情况下进行更新，当数据库架构发生更改时可能会导致问题。因此，**不**建议撤销这些权限。如果您确实出于安全原因需要这样做，请确保您首先制定了可靠的备份计划，定期进行完整的数据库备份，并已验证备份有效且易于恢复。失败的数据库升级通常可以通过将数据库恢复到旧版本、授予适当权限，然后让 WordPress 再次尝试数据库更新来解决。恢复数据库将使其回到旧版本，WordPress 管理界面随后会检测到旧版本，并允许您对其运行必要的 SQL 命令。大多数 WordPress 升级不会更改架构，但有些会。只有主要版本升级（例如从 3.7 到 3.8）才会更改架构。次要升级（例如从 3.8 到 3.8.1）通常不会。尽管如此，**请定期备份**。

### 保护 wp-admin {#securing-wp-admin}

为 `/wp-admin/` 添加服务器端密码保护（例如 [BasicAuth](https://en.wikipedia.org/wiki/Basic_access_authentication)）能为博客管理后台、登录界面及文件增加第二层防护。这迫使攻击者或机器人必须突破这层额外保护，而非直接攻击实际的管理文件。许多 WordPress 攻击是由恶意软件机器人自主执行的。

但仅保护 `wp-admin/` 目录可能会破坏部分 WordPress 功能，例如位于 `wp-admin/admin-ajax.php` 的 AJAX 处理器。请查阅 [资源](https://developer.wordpress.org/advanced-administration/resources/) 章节获取关于如何正确设置 `wp-admin/` 目录密码保护的详细文档。

针对 WordPress 博客的最常见攻击通常分为两类：

1. 向服务器发送精心构造的 HTTP 请求，其中包含针对特定漏洞的利用载荷。这包括利用老旧/过期的插件和软件。
2. 通过“暴力破解”密码猜测来尝试获取博客访问权限。

这种“第二层”密码保护的终极实施方案是要求通过 HTTPS SSL 加密连接进行管理，以确保所有通信和敏感数据均被加密。_参见 [SSL 加密管理](https://developer.wordpress.org/advanced-administration/security/https/)。_

### 保护 wp-includes 目录 {#securing-wp-includes}

可以添加第二层保护来防止用户访问通常不应被直接访问的脚本文件。一种方法是在 .htaccess 文件中使用 mod_rewrite 模块来阻止这些脚本。**注意：** 为确保以下代码不被 WordPress 覆盖，请将其放置在 .htaccess 文件的 `# BEGIN WordPress` 和 `# END WordPress` 标签之外。WordPress 可能会覆盖这些标签之间的任何内容。

```
# 阻止仅包含文件。
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^wp-admin/includes/ - [F,L]
RewriteRule !^wp-includes/ - [S=3]
RewriteRule ^wp-includes/[^/]+\.php$ - [F,L]
RewriteRule ^wp-includes/js/tinymce/langs/.+\.php - [F,L]
RewriteRule ^wp-includes/theme-compat/ - [F,L]
</IfModule>
# BEGIN WordPress
```

请注意，这在多站点（Multisite）环境下可能无法良好工作，因为 `RewriteRule ^wp-includes/[^/]+\.php$ - [F,L]` 会阻止 ms-files.php 文件生成图像。省略该行可以使代码正常工作，但会降低安全性。

### 保护 wp-config.php 文件 {#securing-wp-config-php}

您可以将 `wp-config.php` 文件移动到 WordPress 安装目录的上层目录。这意味着，对于安装在网站根目录下的站点，您可以将 `wp-config.php` 存储在网站根目录文件夹之外。

**注意：** 有人认为 [移动 wp-config.php 文件的安全益处有限](https://wordpress.stackexchange.com/questions/58391/is-moving-wp-config-outside-the-web-root-really-beneficial)，并且如果操作不当，实际上可能引入严重的安全漏洞。[其他人则持不同意见](https://wordpress.stackexchange.com/questions/58391/is-moving-wp-config-outside-the-web-root-really-beneficial/74972#74972)。

请注意，`wp-config.php` 可以存储在 WordPress 安装目录（即包含 wp-includes 的目录）的上层目录中。同时，请确保只有您（和 Web 服务器）可以读取此文件（通常意味着权限设置为 400 或 440）。

如果您使用的服务器支持 .htaccess 文件，可以在该文件顶部添加以下内容，以拒绝任何试图访问该文件的请求：

```
<Files "wp-config.php">
Require all denied
</Files>
```

### 禁用文件编辑 {#disable-file-editing}

默认情况下，WordPress 仪表盘允许管理员编辑 PHP 文件，例如插件和主题文件。如果攻击者能够登录，这通常是他们首先利用的工具，因为它允许执行代码。WordPress 提供了一个常量来禁用仪表盘中的编辑功能。将以下代码行放入 wp-config.php 文件，等同于移除所有用户的 'edit_themes'、'edit_plugins' 和 'edit_files' 权限：

```
define( 'DISALLOW_FILE_EDIT', true );
```

这并不能阻止攻击者向您的网站上传恶意文件，但可能会阻止某些攻击。

### 插件 {#plugins}

首先，请确保您的插件始终保持最新。此外，如果您不使用某个特定插件，请将其从系统中删除。

#### 防火墙 {#firewall}

有许多插件和服务可以充当您网站的防火墙。其中一些通过修改您的 .htaccess 文件并在 WordPress 处理之前，在 Apache 级别限制某些访问来工作。一个很好的例子是 [iThemes Security](https://wordpress.org/plugins/better-wp-security/) 或 [All in One WP Security](https://wordpress.org/plugins/all-in-one-wp-security-and-firewall/)。一些防火墙插件在 WordPress 级别工作，例如 [WordFence](https://wordpress.org/plugins/wordfence/) 和 [Shield](https://wordpress.org/plugins/wp-simple-firewall/)，它们会在 WordPress 加载时但尚未完全处理之前尝试过滤攻击。

除了插件，您还可以在 Web 服务器上安装 WAF（Web 应用程序防火墙），以便在 WordPress 处理之前过滤内容。最受欢迎的开源 WAF 是 ModSecurity。

网站防火墙也可以作为互联网流量和您托管服务器之间的中间层添加。这些服务都充当反向代理，它们接受初始请求并将其重新路由到您的服务器，同时剥离所有恶意请求。它们通过修改您的 DNS 记录（通过 A 记录或完整的 DNS 交换）来实现这一点，允许所有流量首先通过新网络。这导致所有流量在到达您的网站之前都经过防火墙过滤。一些公司提供此类服务，例如 [CloudFlare](https://www.cloudflare.com/)、[Sucuri](https://sucuri.net/wordpress-security/) 和 [Incapsula](https://www.imperva.com/)。

此外，这些第三方服务提供商默认情况下也充当内容分发网络（CDN），提供性能优化和全球覆盖。

#### 需要写入权限的插件 {#plugins-that-need-write-access}

如果某个插件需要写入您的 WordPress 文件和目录，请阅读其代码以确保其合法性，或咨询您信任的人。可查阅的地方包括[支持论坛](https://wordpress.org/support/welcome/)和[IRC 频道](https://make.wordpress.org/support/handbook/appendix/other-support-locations/introduction-to-irc/)。

#### 代码执行插件 {#code-execution-plugins}

正如我们所说，强化 WordPress 的部分目标是在发生成功攻击时控制损害。允许从数据库条目中执行任意 PHP 或其他代码的插件，会显著放大成功攻击时造成损害的可能性。

避免使用此类插件的一种方法是使用调用函数的[自定义页面模板](https://wordpress.org/documentation/article/pages/#Creating_your_own_Page_Templates)。这种安全措施的部分有效性，仅在您[禁止在 WordPress 内编辑文件](#File_Permissions)时才得以体现。

### Security through obscurity {#security-through-obscurity}

[Security through obscurity](https://en.wikipedia.org/wiki/Security_through_obscurity) is generally an unsound primary strategy. However, there are areas in WordPress where obscuring information _might_ help with security:

1. **Rename the administrative account:** When creating an administrative account, avoid easily guessed terms such as `admin` or `webmaster` as usernames because they are typically subject to attacks first. On an existing WordPress install you may rename the existing account in the MySQL command-line client with a command like:
```
UPDATE wp_users SET user_login = 'newuser' WHERE user_login = 'admin';
```
or by using a MySQL frontend like [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/).
2. **Change the table_prefix:** Many published WordPress-specific SQL-injection attacks make the assumption that the table_prefix is `wp_`, the default. Changing this can block at least some SQL injection attacks.

### 数据备份 {#data-backups}

请定期备份您的数据，包括 MySQL 数据库。详见主文：[备份您的数据库](https://developer.wordpress.org/advanced-administration/security/backup/database/)。

数据完整性对于可信备份至关重要。对备份进行加密、为每个备份文件保留独立的 MD5 哈希记录，以及/或将备份存储在只读介质上，可以增强您对数据未被篡改的信心。

一个完善的备份策略可以包括：在可信位置保存一组定期对整个 WordPress 安装（包括 WordPress 核心文件和数据库）的快照。设想一个每周制作快照的网站。这样的策略意味着，如果网站在 5 月 1 日遭到入侵，但直到 5 月 12 日才检测到，网站所有者将拥有入侵前的备份来帮助重建网站，甚至可能还有入侵后的备份，这将有助于确定网站是如何被入侵的。

### 日志记录 {#logging}

日志是理解网站动态的最佳伙伴，尤其是在进行取证分析时。与普遍看法相反，日志能让你看到发生了什么、由谁操作以及何时发生。遗憾的是，日志不会显示登录的用户名，但能帮你识别IP地址、时间，更重要的是攻击者可能采取的行动。通过日志，你可以发现以下攻击行为——跨站脚本攻击（XSS）、远程文件包含（RFI）、本地文件包含（LFI）以及目录遍历尝试。你还能看到暴力破解企图。网上有各种[示例和教程](https://blog.sucuri.net/2015/08/ask-sucuri-how-did-my-wordpress-website-get-hacked-a-tutorial.html)可帮助你解析和分析原始日志。

当你更熟悉日志后，就能观察到诸如主题和插件编辑器的使用情况、小工具更新记录以及文章页面的新增时间。这些都是对Web服务器进行取证工作的关键要素。部分WordPress安全插件也能提供协助，例如[Sucuri审计工具](https://wordpress.org/plugins/sucuri-scanner/)或[审计追踪](https://wordpress.org/plugins/audit-trail/)插件。

从安全层面考虑，你的Web服务器需要部署两个关键的开源解决方案，这是分层安全策略的重要组成。

OSSEC可运行于任何NIX发行版及Windows系统。正确配置后功能非常强大，其核心理念是关联并聚合所有日志。必须确保配置涵盖所有access_logs和error_logs，若服务器托管多个网站也需纳入考量。同时要注意过滤干扰信息——默认设置会产生大量杂讯，需通过配置提升实效性。

### 监控 {#monitoring}

有时预防措施并不足够，您仍可能遭受攻击。因此入侵检测/监控至关重要，它能帮助您更快响应、查明事件并恢复网站。

#### 监控日志 {#monitoring-your-logs}

如果您使用的是专用服务器或虚拟私有服务器，并拥有 root 访问权限，则可以轻松配置以查看系统运行状况。[OSSEC](https://www.ossec.net/) 能便捷地实现此功能，这篇简短的文章或许能为您提供帮助：[OSSEC 用于网站安全 – 第一部分](https://perezbox.com/2013/03/ossec-for-website-security-part-i/)。

#### 监控文件变更 {#monitoring-your-files-for-changes}

攻击发生时总会留下痕迹，无论是在日志中还是在文件系统中（新增文件、修改文件等）。例如，若使用 [OSSEC](https://www.ossec.net/)，它将监控文件并在变更时发出警报。

##### 目标 {#goals}

文件系统追踪的目标包括：

* 监控已更改和新增的文件
* 记录变更和新增内容
* 能够回滚细粒度的更改
* 自动告警

##### 通用方法 {#general-approaches}

管理员可以通过以下通用技术监控文件系统：

* 系统工具
* 版本控制
* 操作系统/内核级监控

##### 特定工具 {#specific-tools}

文件系统监控的选项包括：

* [diff](https://en.wikipedia.org/wiki/Diff_utility) – 构建站点的干净测试副本并与生产环境进行比较
* [Git](https://git-scm.com/) – 源代码管理
* [inotify](https://en.wikipedia.org/wiki/Inotify) – 操作系统内核级文件监控服务，可在文件系统事件上运行命令
* [Watcher](https://github.com/gregghz/Watcher/blob/master/jobs.yml) – Python inotify 库
* [OSSEC](https://www.ossec.net/) – 开源基于主机的入侵检测系统，执行日志分析、文件完整性检查、策略监控、rootkit 检测、实时警报和主动响应。

##### 注意事项 {#considerations}

配置基于文件的监控策略时，需要考虑许多因素，包括以下几点。

###### 以 root 身份运行监控脚本/服务

这将使攻击者难以禁用或修改您的文件系统监控解决方案。

###### 在计划维护/升级期间禁用监控

这可以防止您在网站进行常规维护时收到不必要的通知。

###### 仅监控可执行文件类型

仅监控可执行文件类型（例如 .php 文件等）可能是相对安全的。过滤掉非可执行文件可以减少不必要的日志条目和警报。

###### 使用严格的文件系统权限

了解如何保护文件权限和所有权。通常，应尽可能避免授予 _执行_ 和 _写入_ 权限。

#### 外部监控您的 Web 服务器 {#monitoring-your-web-server-externally}

如果攻击者试图篡改您的网站或添加恶意软件，您也可以使用基于 Web 的完整性监控解决方案来检测这些变化。如今这类方案形式多样，使用您常用的搜索引擎搜索“Web 恶意软件检测与修复”，很可能会得到一长串服务提供商列表。

### 官方 WordPress 资源 {#resources}

* [WordPress 安全白皮书](https://wordpress.org/about/security/)
* [暴力破解攻击](https://developer.wordpress.org/advanced-administration/security/brute-force/)
* [安全常见问题解答](https://make.wordpress.org/core/handbook/testing/reporting-security-vulnerabilities/)
* [常见问题解答 – 我的网站被黑了](https://wordpress.org/documentation/article/faq-my-site-was-hacked/)

### 另请参阅 {#see-also}

* [开源安全详解](https://snyk.io/series/open-source-security/) (Snyk)
* [WordPress 安全吗？](https://patchstack.com/articles/is-wordpress-safe/) (Patchstack)
* [身份验证与授权 — Apache HTTP 服务器 2.2 官方文档](https://httpd.apache.org/docs/current/howto/auth.html)
* [安全控制](https://docs.nginx.com/nginx/admin-guide/security-controls/) 和 [NGINX 高级安全文档](https://docs.nginx.com/nginx-management-suite/acm/how-to/policies/advanced-security/)
* Gridpane 的 [WordPress 安全知识库](https://gridpane.com/knowledgebase/security/) 和 [WordPress 安全分步指南](https://gridpane.com/knowledgebase/security-strategies-and-tools/)
* [WordPress 如何使用身份验证 Cookie 和会话：技术深度解析](https://snicco.io/blog/how-wordpress-uses-authentication-cookies-and-sessions) (Snicco)
* [WordPress 如何使用盐值以及为何不应轮换它们：技术深度解析](https://snicco.io/blog/wordpress-salts) (Snicco)
* [会话管理与安全](https://github.com/snicco/fortress/blob/beta/docs/modules/session/session-managment-and-security.md#session-management-and-security) (Snicco)
* [WordPress 信息安全加固指南](https://cio.ubc.ca/information-security/policy-standards-and-resources/securing-wordpress) (不列颠哥伦比亚大学 OCIO)
* [安全：从基础到企业级，与 Calvin Alkan、Kathy Zant 和 Carl Alexander 探讨](https://openchannels.fm/security-from-the-basics-to-enterprise-with-calvin-alkan/) (视频)
* [WordPress 安全：拨开迷雾](https://blog.sucuri.net/2012/08/wordpress-security-cutting-through-the-bs.html)
* [电子书：锁定 WordPress](https://newcodepoet.files.wordpress.com/2012/07/lockingdownwordpress1-1.pdf)
* [Brad Williams：锁定它 (视频)](https://wordpress.tv/2010/01/23/brad-williams-security-boston10/)
* [安全插件](https://wordpress.org/plugins/tags/security)