---
title: "备份"
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

# 备份

## WordPress 备份

_注意：想跳过复杂步骤？可直接查看自动化解决方案，例如用于备份的 [WordPress 插件](https://wordpress.org/plugins/search.php?q=backup)。_

您的 WordPress 数据库包含博客中的所有文章、评论和链接。如果数据库被删除或损坏，您将丢失所有已发布的内容。这种情况可能由多种原因导致，并非所有因素都在您的掌控之中。通过妥善备份 WordPress 数据库和文件，您可以快速恢复正常运行。

WordPress 网站备份指南包括：

1. WordPress 网站及其数据库备份
2. 自动备份选项

此外，您可以通过 [WordPress 支持论坛](https://wordpress.org/documentation/)获取在线支持，协助您完成备份流程。

网站备份至关重要，因为问题难免发生，您需要在灾难降临时能够迅速应对。花几分钟时间轻松便捷地备份数据库，将让您有更多时间专注于网站的创意与高效运营。

### 备份常见问题

_请定期备份数据库，并在每次升级前务必进行备份。_

**备份频率应为多久？**

这取决于您的博客更新频率、您希望执行备份的周期，以及如果数据库连同部分文章丢失时您的感受。这完全由您决定。关于备份时机的通用建议如下：

 - 对于文章较少的小型网站，建议每周备份一次。
 - 对于文章发布频繁的高活跃度网站，建议每日备份。

**此方法能否用于备份其他数据？**

可以。备份在任何情况下都是有益的。

**应保留多少份备份？**

建议至少保留 3–5 份最新的 WordPress 备份，以防数据丢失，并将副本存储在不同位置——例如，一份存放在托管服务器，一份存放在云存储（Google Drive、Dropbox 等），一份下载到本地计算机。这样即使某份备份失效或丢失，您始终有可靠的版本来恢复网站。

**备份可以自动化吗？**

可以。有多种自动化备份方法可供选择，我们已在“自动 WordPress 备份”章节中列出部分方案。但强烈建议您偶尔通过手动备份来保存这些自动备份，以确保备份流程正常运行。

**是否有更多关于 WordPress 备份的信息？**

是的。更多信息请参阅“备份资源”章节。

### 备份您的 WordPress 站点

备份 WordPress 站点包含两个部分：**数据库**和**文件**。您需要同时备份两者才能完整恢复一个典型的 WordPress 站点。

有关分步说明，请参阅：

- [备份您的 WordPress 文件](https://developer.wordpress.org/advanced-administration/security/backup-files/)
- [备份您的数据库](https://developer.wordpress.org/advanced-administration/security/backup/database/)

#### 数据库与文件（为何两者都需要）

人们常会疑惑“备份文件”是否也备份了数据库。在典型的 WordPress 设置中，答案是**否**：

- **WordPress 文件**指 Web 服务器上 WordPress 目录中的文件（WordPress 核心、主题、插件、上传内容、`wp-config.php`、`.htaccess` 等）。
- **WordPress 数据库**存储在独立的数据库系统（通常是 MySQL/MariaDB）中。通常**无法**通过下载 WordPress 目录来备份它，因为数据库位于该目录之外。

“备份数据库”时通常需要创建**导出/转储文件**（例如 `.sql`、`.gz` 或 `.bz2` 文件）。该导出文件*确实*是一个文件，可将其与文件备份一同存储——但恢复时仍需将其重新导入 MySQL/MariaDB。

#### 推荐的备份与恢复顺序

为确保备份的一致性，建议将 **文件 + 数据库** 视为一个“备份集”（例如，两者创建时间相近）。

- **备份（典型顺序）**：
  - **先备份数据库**，再备份 WordPress 文件。
  - 可选择将数据库导出文件与文件备份存放在同一备份文件夹/压缩包中，以便统一管理。
- **恢复（典型顺序）**：
  - **先恢复 WordPress 文件**，再恢复/导入数据库。
  - 若迁移过程中更改了数据库凭据，需相应更新 `wp-config.php` 文件。

您的 WordPress 网站包含以下内容：

1. WordPress 核心安装文件
2. WordPress 插件
3. WordPress 主题
4. 图片与文件
5. JavaScript、PHP 及其他代码文件
6. 附加文件与静态网页

这些元素以不同组合形式共同构成您的网站。数据库存储了文章及大量网站生成数据，但并未包含上述共同塑造网站外观与信息的元素。这些内容均需备份保存。

大多数主机会备份整个服务器（包括您的网站），但从其备份中获取网站副本需要时间，而快速恢复至关重要。您需要学会如何自行备份和恢复网站文件。

以下是备份网站文件的几种方法：

**主机提供的备份软件**

多数网站主机会提供备份软件。请咨询您的主机服务商了解具体服务与程序。

**创建网站同步**

[WinSCP](https://winscp.net/eng/index.php) 等程序可让您与网站同步，保持服务器与硬盘内容的镜像更新。这既节省时间，又能确保两地文件均为最新版本。

**将文件复制到桌面**

通过 [FTP 客户端](https://developer.wordpress.org/advanced-administration/upgrade/ftp/) 或 [UNIX Shell 技能](https://codex.wordpress.org/UNIX_Shell_Skills)，您可以将文件复制到计算机文件夹中。随后可将其压缩为 zip 文件以节省空间，便于保存多个版本。

请谨记：至少保留三份备份文件，以防某份损坏或丢失，并应存储在不同位置及不同介质上（如 CD、DVD 或硬盘）。

### 数据库备份说明

请定期备份您的 WordPress 数据库，尤其是在升级或迁移到新位置之前。以下信息将帮助您使用各种流行的服务器软件包来备份 WordPress 数据库。如需详细信息，请联系您的网站托管服务商获取更多信息。

#### 访问 phpMyAdmin

有关 phpMyAdmin 的更多信息，请参阅 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/)。

虽然备份 WordPress 数据库不一定需要熟悉 phpMyAdmin，但这些说明将逐步引导您在服务器上找到 phpMyAdmin。然后，您可以按照以下说明进行简单轻松的备份。更详细的说明，请参阅《备份数据库》。

* Plesk
* cPanel
* Direct Admin
* Ensim
* vDeck
* Ferozo

##### Plesk

在您的网站与域名页面，点击与您在 [WordPress 安装](https://developer.wordpress.org/advanced-administration/before-install/howto-install/)过程中所设置数据库对应的**打开**按钮。这将打开 **phpMyAdmin** 界面：

![image](https://user-images.githubusercontent.com/8250598/189548052-05143263-7980-45b5-b2dc-23fc5a8b19fd.png)

如果您看不到**打开**按钮，请确保关闭**开始创建您的网站**提示：

![image](https://user-images.githubusercontent.com/8250598/189548074-703c1d79-a437-445b-8bf7-ac51272b69f8.png)

点击**选择现有数据库**以查找并选择您的 WordPress 数据库：

![image](https://user-images.githubusercontent.com/8250598/189548312-c455cf50-757e-4bf7-9128-825e3cb0832c.png)

##### cPanel

在 cPanel 主控制面板上，找到 MySQL 徽标并点击 MySQL 数据库链接。在下一页中，找到 **phpMyAdmin** 链接并点击以访问您的 phpMyAdmin。

![image](https://user-images.githubusercontent.com/8250598/189548290-9e815d91-e598-4b31-8bde-3101ac09bd89.png)

![image](https://user-images.githubusercontent.com/8250598/189548157-74dd7be8-ea45-4ee0-90d4-e16f57225d24.png)

##### Direct Admin

在 **您的账户** 页面，找到 **MySQL 管理** 并点击以访问 **phpMyAdmin**。

![image](https://user-images.githubusercontent.com/8250598/189548174-6951023a-c593-4f46-af78-5bf43a390279.png)

![image](https://user-images.githubusercontent.com/8250598/189548195-4b1ca6c1-0a6d-4191-8060-c90e59696ee3.png)

##### Ensim

寻找 MySQL 管理徽标并点击链接。在 **配置** 下选择 **MySQL 管理工具**。

![image](https://user-images.githubusercontent.com/8250598/189548260-d911357c-8681-4c27-a1ad-043d7a678c22.png)

![image](https://user-images.githubusercontent.com/8250598/189548265-2a7b7721-10e9-41d7-a150-ab11422c29cd.png)

##### vDeck

从主控制面板，点击 **Host Manager**，然后点击 **Databases**。在下一个窗口中，点击 **Admin**。另一个窗口将弹出，带您进入 phpMyAdmin 登录界面。

![image](https://user-images.githubusercontent.com/8250598/189548348-9f1135eb-4336-4f45-9fe6-8fa482f758d5.png)

![image](https://user-images.githubusercontent.com/8250598/189548353-75778b1a-686c-44a7-ab15-89b49d94e146.png)

##### Ferozo

使用您的凭据登录 Ferozo 控制面板。登录后，进入 "Base de Datos"（"数据库"）菜单，然后点击 "Acceso phpMyAdmin"（"访问 phpMyAdmin"）。新窗口将打开，显示 phpMyAdmin 登录界面。

![image](https://user-images.githubusercontent.com/8250598/189548372-ebebffc3-9723-4e4f-b478-df0d38499e58.png)

### 使用 phpMyAdmin 进行简单备份

以下是数据库备份的极简版本。找到访问网站 phpMyAdmin 的方法后，请按以下简易步骤操作。

1. 在 phpMyAdmin 面板中点击 **Databases**。（根据 phpMyAdmin 版本不同，此步骤可能非必需）

![image](https://user-images.githubusercontent.com/8250598/189548469-d63090c3-4e43-48d4-8039-507957e4a69c.png)

2. 您可能有多个数据库。点击存放 WordPress 数据的数据库，即[安装 WordPress](https://wordpress.org/installation/how-to-install-wordpress/)时创建的数据库。（旧版本可能通过下拉菜单操作）

3. 下图显示的是结构视图标签页中的默认数据表。您可能有更多数据表——如果安装了统计插件或反垃圾邮件插件就会出现这种情况。

![image](https://user-images.githubusercontent.com/8250598/189548524-84c2f476-7fae-4bc4-b7fb-19b72f72bed8.png)

4. 点击 **Export**。
		- 有两种导出方式：**Quick** 和 **Custom**；若选择 **Custom**，请按以下步骤操作：
				- 选择所有数据表
				- 在 **Output** 部分勾选 **Save output to a file**，并在 **Compression** 处选择 **None**（若数据库非常大，请使用压缩方式）
				- 从 **Format** 下拉菜单中选择 **SQL**
				- 勾选 "Add DROP TABLE"：这在覆盖现有数据库时很有用
				- 勾选 "IF NOT EXISTS"：可防止数据表已存在时恢复过程出错
				- 点击 **Go**。数据将保存至您的计算机

### 自动备份

有多种插件可为您的 WordPress 数据库提供自动定时备份功能。这有助于轻松管理您的备份集合。您可以在 WordPress 管理界面的**插件浏览器**中或通过 [WordPress 插件目录](https://wordpress.org/plugins/)找到自动备份插件。

* [备份插件列表](https://wordpress.org/plugins/tags/backup)

### 备份资源

* [FTP 备份](https://www.guyrutenberg.com/2010/02/28/improved-ftp-backup-for-wordpress/) – 如何自动备份到 FTP 服务器
* [增量备份](https://www.guyrutenberg.com/2013/03/28/incremental-wordpress-backups-using-duply-duplicity/) – 如何使用 duplicity 制作加密的增量备份
* [在 WordPress 中使用 phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/)

#### 备份工具

* [使用 phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/)
* [FTP 客户端](https://developer.wordpress.org/advanced-administration/upgrade/ftp/)
* [使用 FileZilla](https://developer.wordpress.org/advanced-administration/upgrade/ftp/filezilla/)

### 延伸阅读 {#read-further}

* [WordPress 备份](https://developer.wordpress.org/advanced-administration/security/backup/)
* [WordPress 升级详解](https://developer.wordpress.org/advanced-administration/upgrade/upgrading/)