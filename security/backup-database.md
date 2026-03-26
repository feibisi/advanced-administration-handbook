---
title: "Backup Database"
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

## 备份数据库

> 强烈建议您定期并在升级前备份数据库。

如果出现问题，您可以从备份中[恢复数据库](https://developer.wordpress.org/advanced-administration/security/backup/)。

**注意：** 以下步骤备份的是 WordPress 数据库（文章、页面、评论、设置等），但**不**备份您的 WordPress 文件（主题、插件、上传内容、`wp-config.php` 等）。要进行全站备份，请参阅 [WordPress 备份](https://developer.wordpress.org/advanced-administration/security/backup/)和[备份 WordPress 文件](https://developer.wordpress.org/advanced-administration/security/backup-files/)。

备份数据库时，通常会创建一个导出文件（例如 `.sql`、`.gz` 或 `.bz2` 格式）。该导出文件可以与您的文件备份一起存储，这样数据库和文件就作为一个“备份集”保存在一起，但恢复时仍需将其导入到 MySQL/MariaDB 中。

### 使用 cPanel X 进行备份 {#backup-using-cpanel-x}

cPanel 是许多网络主机使用的流行控制面板。其备份功能可用于备份您的 MySQL 数据库。请勿生成完整备份，因为这些备份严格用于存档目的，无法通过 cPanel 恢复。请找到“下载 MySQL 数据库备份”并点击数据库名称。一个 `*.gz` 文件将被下载到您的本地驱动器。

恢复时无需解压此文件。使用相同的 cPanel 程序，浏览到该 gz 文件并上传。上传完成后，浏览器底部将显示转储完成。如果您要上传到新主机，则需要重新创建数据库用户及匹配的密码。如果更改了密码，请在 wp-config.php 文件中进行相应更改。

### 使用 phpMyAdmin {#using-phpmyadmin}

[phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/) 是用于操作数据库的程序名称。

以下信息已通过使用 phpMyAdmin 版本 4.4.13 连接运行在 Linux 上的 MySQL 版本 5.6.28 进行测试验证。

[![phpmyadmin_top](https://wordpress.org/documentation/files/2018/11/phpmyadmin_top.jpg)](https://wordpress.org/documentation/files/2018/11/phpmyadmin_top.jpg)

#### 快速备份流程 {#quick-backup-process}

当您需要在不压缩的情况下备份 WordPress 数据库中的所有表时，可以使用简单方法。要恢复此备份，您的新数据库不应包含任何表。

1. 登录服务器上的 phpMyAdmin
2. 在左侧窗口中选择您的 WordPress 数据库。本例中数据库名为 "wp"
3. 右侧窗口将显示 WordPress 数据库中的所有表。点击顶部标签页中的“导出”标签

[![](https://wordpress.org/documentation/files/2018/11/phpmyadmin_dbtop.jpg)](https://wordpress.org/documentation/files/2018/11/phpmyadmin_dbtop.jpg)

4. 确保选择“快速”选项，点击“执行”后将提示下载文件。将文件保存到计算机。根据数据库大小，此过程可能需要一些时间

[![phpmyadmin_quick_export](https://wordpress.org/documentation/files/2018/11/phpmyadmin_quick_export.jpg)](https://wordpress.org/documentation/files/2018/11/phpmyadmin_quick_export.jpg)

#### 自定义备份流程 {#custom-backup-process}

如需更改默认行为，请选择自定义备份。在上述步骤 4 中，选择自定义选项。详细选项将显示。

[![phpmyadmin_custom_export](https://wordpress.org/documentation/files/2018/11/phpmyadmin_custom_export.jpg)](https://wordpress.org/documentation/files/2018/11/phpmyadmin_custom_export.jpg)

##### 表格部分 {#the-table-section}

数据库中的所有表格均被选中。如果您有其他程序使用该数据库，请仅选择与您的 WordPress 安装对应的表格。这些表格将以 "wp_" 开头，或您在 'wp-config.php' 文件中指定的 'table_prefix' 前缀开头。

如果仅安装了 WordPress 博客，请保持原样（或点击“全选”以恢复默认选择）。

##### 输出部分 {#the-output-section}

从压缩框中选择 'zipped' 或 'gzipped' 来压缩数据。

[![phpmyadmin_export_output](https://wordpress.org/documentation/files/2018/11/phpmyadmin_export_output.jpg)](https://wordpress.org/documentation/files/2018/11/phpmyadmin_export_output.jpg)

##### 格式部分 {#the-format-section}

确保已选择 SQL。与 CSV 或其他数据格式不同，此选项导出一系列 SQL 命令。

在格式特定选项部分，保持选项不变。

[![phpmyadmin_export_formatspecific](https://wordpress.org/documentation/files/2018/11/phpmyadmin_export_formatspecific.jpg)](https://wordpress.org/documentation/files/2018/11/phpmyadmin_export_formatspecific.jpg)

##### 对象创建选项部分 {#the-object-creation-options-section}

选择添加 DROP TABLE / VIEW / PROCEDURE / FUNCTION / EVENT / TRIGGER 语句。在目标数据库创建表之前，如果旧表存在，它将调用 DROP 语句删除旧表。

[![phpmyadmin_export_object](https://wordpress.org/documentation/files/2018/11/phpmyadmin_export_object.jpg)](https://wordpress.org/documentation/files/2018/11/phpmyadmin_export_object.jpg)

### 数据创建选项部分 {#the-data-creation-options-section}

保持选项不变。

[![phpmyadmin_export_data](https://wordpress.org/documentation/files/2018/11/phpmyadmin_export_data.jpg)](https://wordpress.org/documentation/files/2018/11/phpmyadmin_export_data.jpg)

现在点击窗口底部的“Go”，系统应会提示您下载文件。将此文件保存到您的计算机。根据数据库大小，这可能需要一些时间。

**请注意** – 您尚未备份文件和文件夹（例如图像），但您的所有文章和评论现在都已安全保存。

### 使用原生 MySQL/MariaDB 命令 {#using-straight-mysql-mariadb-commands}

phpMyAdmin 无法处理大型数据库，因此使用原生 MySQL/MariaDB 代码会有所帮助。

将目录切换到您想要导出备份的目标目录：

```
user@linux:~> cd files/blog
user@linux:~/files/blog>
```

使用 `mysqldump` 命令，并指定您的 MySQL 服务器名、用户名和数据库名。它会提示您输入密码（如需帮助，请尝试：`man mysqldump`）。

**备份所有数据库表**

```
mysqldump --add-drop-table -h mysql_hostserver -u mysql_username -p mysql_databasename﻿
```

**仅备份数据库中的特定表**

```
mysqldump --add-drop-table -h mysql_hostserver -u mysql_username -p mysql_databasename﻿
```

示例：

```
user@linux:~/files/blog> mysqldump --add-drop-table -h db01.example.net -u dbocodex -p wp > blog.bak.sql
Enter password: (输入密码)
```

**使用 bzip2 压缩备份文件**

```
user@linux:~/files/blog> bzip2 blog.bak.sql
```

您可以将上述两个命令的操作合并为一行完成：

```
user@linux:~/files/blog> mysqldump --add-drop-table -h db01.example.net -u dbocodex -p wp | bzip2 -c > blog.bak.sql.bz2
Enter password: (输入密码)
```

`|`（管道）后面的 `bzip2 -c` 表示备份在生成时即被压缩，而 `> blog.bak.sql.bz2` 则将 bzip 的输出发送到名为 `blog.bak.sql.bz2` 的文件中。

尽管 bzip2 比旧的压缩算法（.Z、.zip、.gz）能更有效地压缩大多数文件，但其[压缩和解压缩速度要慢得多](https://en.wikipedia.org/wiki/Bzip2)。如果您需要备份大型数据库，gzip 是一个更快的选择。

```
user@linux:~/files/blog> mysqldump --add-drop-table -h db01.example.net -u dbocodex -p wp | gzip > blog.bak.sql.gz
```

### 使用 MySQL Workbench {#using-mysql-workbench}

[MySQL Workbench](https://dev.mysql.com/downloads/workbench/)（曾被称为 My SQL Administrator）是一款用于执行管理操作的程序，例如配置 MySQL 服务器、监控其状态和性能、启动和停止服务、管理用户和连接、执行备份、恢复备份以及许多其他管理任务。

虽然您可以使用命令行界面（例如 [mysqladmin](https://dev.mysql.com/doc/refman/8.0/en/mysqladmin.html) 或 [mysql](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) 提供的界面）执行大多数此类任务，但 MySQL Workbench 在以下方面具有优势：

* 其图形用户界面使其使用起来更加直观。
* 它更好地展示了对于 MySQL 服务器性能、可靠性和安全性至关重要的设置。
* 它以图形方式显示性能指标，从而更容易确定和调整服务器设置。
* 它适用于 Linux、Windows 和 MacOS X，并允许远程客户端跨平台备份数据库。只要您能访问远程服务器上的 MySQL 数据库，就可以将数据备份到任何您有写入权限的地方。
* 与 phpMyAdmin 不同，它对要备份的数据库大小没有限制。

以下信息已通过 MySQL Workbench 版本 6.3.6 连接运行在 Linux 上的 MySQL 版本 5.6.28 进行过测试验证。

[![mysql_workbench_top](https://wordpress.org/documentation/files/2018/11/mysql_workbench_top.jpg)](https://wordpress.org/documentation/files/2018/11/mysql_workbench_top.jpg)

#### 备份数据库 {#backing-up-the-database}

本文假设您已安装 MySQL Workbench 并完成配置，能够本地或远程登录 MySQL 数据库服务器。如需安装说明，请参考适用于您平台的 MySQL Workbench 安装包随附文档，或查阅[在线文档](https://dev.mysql.com/doc/workbench/en/)。

1. 启动 MySQL Workbench
2. 若数据库实例显示在首页，请直接点击。或通过顶部菜单选择 Database -> Connect Database，输入必要信息后点击 OK
3. 在左侧窗口点击 Data Export

[![mysql_workbench_export](https://wordpress.org/documentation/files/2018/11/mysql_workbench_export.jpg)](https://wordpress.org/documentation/files/2018/11/mysql_workbench_export.jpg)

1. 选择需要备份的 WordPress 数据库
2. 在 Export Options 中指定目标目录，需确保对备份目录具有写入权限
3. 点击窗口右下角的 Start Export

[![mysql_workbench_export2](https://wordpress.org/documentation/files/2018/11/mysql_workbench_export2.jpg)](https://wordpress.org/documentation/files/2018/11/mysql_workbench_export2.jpg)

#### 从备份恢复 {#restoring-from-a-backup}

1.  启动 MySQL Workbench。
2.  如果您的数据库实例显示在首页，请点击它。或者，点击 Database -> Connect Database，然后点击 OK。
3.  在左侧窗口中点击 Data Import/Restore。
4.  指定您存放备份文件的文件夹。点击 Import from Dump Project Folder 右侧的 "…"，选择备份文件夹，然后点击 Open。
5.  点击窗口右下角的 Start Import。数据库恢复将开始。

[![mysql_workbench_import](https://wordpress.org/documentation/files/2018/11/mysql_workbench_import.jpg)](https://wordpress.org/documentation/files/2018/11/mysql_workbench_import.jpg)

### MySQL GUI Tools {#mysql-gui-tools}

In addition to MySQL Workbench, there are many GUI tools that let you backup (export) your database.

| Name | OS (Paid edition) | OS (Free edition) | |
|---|---|---|
| [MySQL Workbench](https://www.mysql.com/products/workbench/) | Windows/Mac/Linux | Windows/Mac/Linux | See [above](https://developer.wordpress.org/advanced-administration/security/backup/database/#Using_MySQL_Workbench) |
| [EMS SQL Management Studio for MySQL](https://www.sqlmanager.net/products/mysql/studio) | Windows | | |
| [Aqua Data Studio](https://www.aquafold.com/) | Windows/Mac/Linux | Windows/Mac/Linux (14 days trial) | Available in 9 languages |
| [Navicat for MySQL](https://www.navicat.com/en/products/navicat-for-mysql) | Windows/Mac/Linux | Windows/Mac/Linux (14 days trial) | Available in 8 languages |
| [SQLyog](https://webyog.com/en/) | Windows | | |
| [Toad for MySQL](https://www.toadworld.com/) | | Windows | |
| [HeidiSQL](https://www.heidisql.com/) | | Windows | |
| [Sequel Pro](https://sequelpro.com/) | Mac | CocoaMySQL successor | |
| [Querious](https://www.araelium.com/querious/) | | Mac | |

### 使用 WordPress 数据库备份插件 {#using-wordpress-database-backup-plugin}

您可以在 [WordPress 插件目录](https://wordpress.org/plugins/search/database+backup/)中找到能帮助您备份数据库的插件。

以下说明适用于名为 [WP-DB-Backup:](https://wordpress.org/plugins/wp-db-backup/) 的插件。

#### 安装 {#installation}

1. 在[管理](https://wordpress.org/documentation/article/administration-screens/) > [插件](https://wordpress.org/documentation/article/administration-screens/#plugins-add-functionality-to-your-blog) > [添加新插件](https://wordpress.org/documentation/article/administration-screens/#add-new-plugins)中搜索“WP-DB-Backup”。
2. 点击“立即安装”。
3. 激活插件。

#### 备份 {#backing-up}

1.  导航至 [管理](https://wordpress.org/documentation/article/administration-screens/) > [工具](https://wordpress.org/documentation/article/administration-screens/#tools-managing-your-blog) > 备份
2.  WordPress 核心表将始终被备份。从“表”部分选择一些选项。

[![wp-db-backup_table](https://wordpress.org/documentation/files/2018/11/wp-db-backup_table.jpg)](https://wordpress.org/documentation/files/2018/11/wp-db-backup_table.jpg)

3.  选择备份选项；备份可以下载或通过电子邮件发送。

4.  最后，点击“立即备份！”按钮以实际执行备份。您也可以安排定期备份。

[![wp-db-backup_settings](https://wordpress.org/documentation/files/2018/11/wp-db-backup_settings.jpg)](https://wordpress.org/documentation/files/2018/11/wp-db-backup_settings.jpg)

#### 恢复数据 {#restoring-the-data}

生成的文件是一个标准的 SQL 文件。如需了解如何上传该文件，请参阅[从备份恢复数据库](https://developer.wordpress.org/advanced-administration/security/backup/)。

### 更多资源 {#more-resources}

* [WordPress.org 官方仓库中的备份插件](https://wordpress.org/plugins/search.php?q=backup)
* [WordPress 备份](https://developer.wordpress.org/advanced-administration/security/backup/)

### 外部资源 {#external-resources}

* [如何安排 WordPress 数据库的每日备份](https://www.narga.net/schedule-backup-wordpress-database/)

## 从备份恢复数据库

### 使用 phpMyAdmin {#using-phpmyadmin}

[phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/) 是一款通过网页界面远程操作数据库的程序。优质的主机套餐通常会包含此工具。有关备份 WordPress 数据库的信息，请参阅[备份数据库](https://developer.wordpress.org/advanced-administration/security/backup/database/)。

本文信息已通过运行在 Unix 系统上的 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/) 4.0.5 版本测试。

以下操作将**替换**您当前的数据库为备份版本，**使**您的数据库**回退**到备份时的状态。

#### 恢复流程 {#restore-process}

使用 phpMyAdmin，请按照以下步骤恢复 MySQL/MariaDB 数据库。

1.  登录 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/)。
2.  点击 "Databases"，然后选择您要将数据导入的数据库。
3.  随后您将看到该数据库中已有的表列表，或者一个显示没有表存在的界面。这取决于您的设置。
4.  屏幕顶部会有一排选项卡。点击 **Import** 选项卡。
5.  在下一个屏幕上，会有一个文本文件位置框，旁边有一个名为 **Browse** 的按钮。
6.  点击 **Browse**。找到存储在您计算机上的备份文件。
7.  确保在 **Format** 下拉菜单中选择了 **SQL**。
8.  点击 **Go** 按钮。

现在去喝杯咖啡。这一步需要一些时间。最终您会看到一个成功界面。

如果出现错误信息，最好的办法是到 [WordPress 支持论坛](https://wordpress.org/documentation/)发帖寻求帮助。

### Using MySQL/MariaDB Commands {#using-mysql-mariadb-commands}

The restore process consists of unarchiving your archived database dump, and importing it into your MySQL/MariaDB database.

Assuming your backup is a `.bz2` file, created using instructions similar to those given for [Backing up your database using MySQL/MariaDB commands](https://developer.wordpress.org/advanced-administration/security/backup/database/#using-straight-mysqlmariadb-commands), the following steps will guide you through restoring your database:

1. Unzip your `.bz2` file:

```
user@linux:~/files/blog> bzip2 -d blog.bak.sql.bz2
```

**Note:** If your database backup was a `.tar.gz` file called `blog.bak.sql.tar.gz`, then

```
tar -zxvf blog.bak.sql.tar.gz
```

is the command that should be used instead of the above.

2. Put the backed-up SQL back into MySQL/MariaDB:

```
user@linux:~/files/blog> mysql -h mysqlhostserver -u mysqlusername -p databasename < blog.bak.sql  
Enter password: (enter your mysql password)   
user@linux:~/files/blog>
```