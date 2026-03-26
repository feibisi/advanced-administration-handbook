---
title: "Backup Files"
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

## 备份 WordPress 文件

备份 WordPress 网站包含两个部分：**数据库**和**文件**。

本页仅讨论**文件**备份；如需备份 WordPress 数据库，请参阅[备份数据库](https://developer.wordpress.org/advanced-administration/security/backup/database/)。

### 备份文件时是否同时备份数据库？

通常**不会**。您的 WordPress 文件存放在网络服务器的 WordPress 目录中，但数据库存储在 WordPress 目录之外的独立数据库系统（通常是 MySQL/MariaDB）中。

如果您将数据库导出为 `.sql`/`.gz`/`.bz2` 文件，该导出文件*确实*是一个文件——您可以将其与文件备份一起存储（例如放在同一文件夹或压缩包中），以便与对应的文件备份保持一致。恢复时仍需将该文件重新导入数据库。

您的 WordPress 网站包含以下文件：

* WordPress 核心安装文件
* WordPress 插件
* WordPress 主题
* 图片和文件
* JavaScript、PHP 脚本及其他代码文件
* 附加文件和静态网页

所有与网站外观相关的内容都存在于某个文件中，需要进行备份。此外，您必须备份 WordPress 目录（包括子目录）中的所有文件以及您的 [`.htaccess`](https://wordpress.org/documentation/article/wordpress-glossary/#.htaccess) 文件。

虽然大多数主机会备份整个服务器（包括您的网站），但最好还是自行备份文件。最简单的方法是使用 [FTP 程序](https://developer.wordpress.org/advanced-administration/upgrade/ftp/) 将 WordPress 文件从主机下载到本地计算机。

默认情况下，名为 wp-content 的目录中的文件是您自己生成的内容，例如编辑过的主题、新安装的插件和上传的文件。请特别注意备份此区域以及包含连接详情的 `wp-config.php` 文件。

其余文件主要是 WordPress 核心文件，由 [WordPress 下载压缩包](https://wordpress.org/download/) 提供。

更多信息请阅读 [备份您的 WordPress 网站](https://developer.wordpress.org/advanced-administration/security/backup/#backing-up-your-wordpress-site)。

其他备份文件的方法包括：

**网站主机提供的备份软件**

大多数网站主机提供备份网站的软件。请咨询您的主机了解他们提供的服务和程序。

**与网站创建同步**

[WinSCP](https://winscp.net/eng/index.php) 和其他程序允许您与网站同步，以保持服务器和硬盘驱动器上内容的镜像副本更新。这可以节省时间，并确保两地都有最新的文件。

#### 在 WinScp 中同步文件 {#synchronize-your-files-in-winscp}

1. 使用 WinScp 正常登录您的 FTP 服务器。
2. 点击“同步”按钮。远程目录将自动设置为当前 FTP 目录（通常是您的根目录）。本地目录将设置为点击同步时的本地目录。您可能需要将其更改为计算机上的其他目录。方向应设置为“本地”，以便将文件从您的网络主机复制到您的机器。同步模式应设置为“同步文件”。
3. 点击“确定”以显示操作摘要。
4. 再次点击“确定”以完成同步。

**将文件复制到桌面**

使用 [FTP 客户端](https://developer.wordpress.org/advanced-administration/upgrade/ftp/) 或 [UNIX Shell 技能](https://codex.wordpress.org/UNIX_Shell_Skills)，您可以将文件复制到计算机上的文件夹。之后，您可以将其压缩为 zip 文件以节省空间，从而保留多个版本。

通常，无需复制 WordPress 核心文件，因为您可以从新下载的 WordPress zip 文件替换它们。需要备份的重要文件是您的 wp-config.php 文件（包含您的设置）和您的 wp-content 目录（及其内容），其中包含所有主题和插件文件。