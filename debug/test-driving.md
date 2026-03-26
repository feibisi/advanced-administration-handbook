---
title: "测试驱动 WordPress"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Debug
    - Repos
    - Data
---

# 测试驱动 WordPress

有时您需要在公众视线之外测试 WordPress 网站的更改。对实时网站进行修改可能会对读者产生不利影响。

您有几种选择。

### 创建沙盒环境

此操作适用于测试驱动您的 WordPress 主题和样式表，让您能在计算机上开发 [WordPress 主题](https://wordpress.org/documentation/article/using-themes/)。该环境限制您仅能处理 CSS，无法使用插件和 WordPress 的其他高级功能，最适合仅进行页面样式设计。

### 隐藏您的 WordPress 测试区域

您还可以在网站服务器上关闭对 WordPress 测试站点的访问。这需要您对 .htaccess 和 Apache 有一定了解，但它能让您在互联网上继续工作的同时，不向公众暴露您的测试站点。

### 在您的计算机上安装 WordPress

如果您决心全面测试 WordPress，可以通过一些修改在个人计算机上安装 WordPress。这使您能够完全控制 WordPress 的操作和功能。您仍可使用插件、模板文件、主题，并像在互联网上一样重新设计所有内容，而无需消耗带宽或忍受缓慢的网络访问速度。我们为此提供两套说明：在计算机上全新安装 WordPress 和安装现有 WordPress 站点。我们还介绍了在计算机上完成站点开发后，如何将 WordPress 迁移至您的网站。

## 创建沙盒

**沙盒**这个术语源于儿时玩耍和堆砌沙堡的沙坑。它是一个用于实践概念和探索想象力的游乐场。WordPress 沙盒本质上是 WordPress 站点上生成页面的副本，保存到您的硬盘驱动器，供您在开发最终主题和设计站点外观时使用。

WordPress 使用不同的[模板文件](https://codex.wordpress.org/Templates)在您的站点上生成不同的视图。通常，有**首页视图**、**单篇文章视图**和用于分类、归档和搜索的**多篇文章视图**。有关 WordPress 主题结构的更多信息，请参阅[站点架构 1.5](https://developer.wordpress.org/advanced-administration/wordpress/site-architecture/)。由于不同的页面视图使用不同的 CSS 样式，您至少需要按照以下说明将三种页面视图放入沙盒中。

1.  在管理后台的“外观”屏幕中选择您要基于其进行工作的 WordPress 主题。
2.  从您的初始或测试 WordPress 站点，查看以下页面视图之一：
    *   首页
    *   单篇文章
    *   多篇文章页面
3.  在您的浏览器中：
    *   选择“文件”>“另存为”。
    *   使用上述“标题”之一命名保存的页面。
    *   将每个页面的文件保存到您的沙盒文件夹。
4.  从您的主题文件夹中，将 style.css 样式表文件复制到您的沙盒文件夹。
5.  在文本编辑器中打开您保存的三个文件，并更改以下内容：

```
<style type="text/css" media="screen">
@import url('/wp-content/themes/yourtheme/style.css');
</style>
```

改为：

```
<link rel="stylesheet" type="text/css" media="screen" href="style.css" />
```

最后，找到与样式表关联的所有图像文件和图形，例如背景图像、图标、项目符号或其他，并将它们复制到您的沙盒文件夹。样式表中指向这些项目的链接不应包含文件夹路径，或者应链接到沙盒文件夹内的子文件夹，例如：

```
header {
  margin:5px;
  padding:10px;
  background:url(images/background.jpg)....
}
```

要测试此操作，请双击该文件夹中的一个页面以在浏览器中查看。如果样式和图形可见，则表示成功。如果不可见，请检查样式表的链接。

现在，您就有了一个可以使用的沙盒。

要使用您的沙盒，请打开两个程序。一个是打开 style.css 文件的文本编辑器，另一个是浏览器，其中显示您正在处理的页面。在样式表中进行更改，保存它，然后[完全刷新](https://wordpress.org/support/topic/i-make-changes-and-nothing-happens/)浏览器屏幕并查看更改。然后重复此过程。完成一个页面后，转到下一个页面，检查这些更改，并在必要时添加更多更改。

### 沙盒使用提示

以下是在沙盒环境中操作的一些建议。

### 频繁备份

在修改过程中，进行重大调整前请将文件夹中的文件复制到备份文件夹。您也可以使用压缩工具（如 zip 程序）保存整个文件夹。若修改出现问题，您仍可使用最近的备份文件继续工作。

### 追踪 DIV 和 CLASS

开始前，请用文本编辑器逐一查看三个页面视图的源代码文件，并为每个分区的起始和结束位置添加注释。这些分区常跨越模板文件的行，难以追踪。通过记录它们的起止位置，让工作更轻松。

### 记录笔记

在进行重大变更前，请先写下你要修改的内容。这样当后续出现问题、需要追溯变更历史时，你就有笔记可以参考。

### 在代码中添加注释

当对样式表或网页源代码进行重大修改时，请在代码中添加注释。这也有助于您跟踪所做的更改。

### 小步前进

一次性进行大量更改会使你难以找出导致问题的小改动。更好的方法是采取小步骤并检查，然后进行额外的小修改并再次检查。

### 避免修改模板文件

首先应遵循现有的网站架构和样式规范。若计划公开发布主题，必须审慎考虑并谨慎处理模板文件的修改。若仅为个人使用重建网站，可先在保存的 HTML 页面源代码中调整，后续再将变更迁移至模板文件。

### 将沙盒环境迁移至主题
当您完成所有修改并对结果满意后，即可将沙盒环境移回 WordPress 主题。

1. 将 style.css 样式表文件上传至网站上的 WordPress 主题文件夹，替换旧文件。
2. 将所有图形和图像上传至主题文件夹或子文件夹。
3. 在浏览器中查看您的网站，更改应立即生效。
4. 若您修改了三个页面中任意页面的源代码，请定位到对应的具体模板文件，并在网站上的这些模板文件中进行相应修改。

### 安装另一个博客

1. 再次安装 WordPress，但在 wp-config.php 文件中使用不同的数据表前缀。
2. 在“设置”>“撰写”>“更新服务”中，清空该文本框。
3. 不要告诉任何人你的博客地址。

如果你从你的博客访问其他网站，你的网站可能会因为浏览器中的引荐来源而被发现。为防止这种情况，请先访问你的真实博客，然后再访问其他网站。

此方法在测试后期很有用，因为你可以邀请他人使用不同的浏览器或屏幕分辨率进行测试。

## 隐藏您的 WordPress 测试区域

要隐藏您的 WordPress 测试文件夹，您可以在 Apache 网络服务器上使用 `.htaccess` 文件。`.htaccess` 文件是一个存储服务器指令的文件，这些指令告诉服务器在特定情况下该做什么。您也可以使用 Apache 配置文件 (httpd.conf) 或其他方法，但 `.htaccess` 文件仅适用于其所在的文件夹及其所有子文件夹，从而允许您限制对特定文件夹的访问。

请记住，这仅在支持 `.htaccess` 的服务器上有效。如果您不确定您的服务器是否支持 `.htaccess`，请联系您的主机提供商。根据您对主机服务器的访问权限，您可能能够也可能无法执行此操作。您可能需要他们的帮助。如果您运行自己的服务器，或者您的主机提供商对此不了解，请查阅 [AllowOverride 文档](https://httpd.apache.org/docs/2.0/mod/core.html#allowoverride)。

使用 `.htaccess` 文件，您需要提供指令来告诉服务器限制或拒绝对您的 WordPress 测试站点的访问。在安装 WordPress 的文件夹或目录中，请执行以下操作：

1.  使用文本编辑器创建一个名为 `.htaccess` 的空白文本文件。
2.  您需要以下信息：
3.  - 您站点服务器上一个公众无法访问的目录的完整路径（例如 https://example.com/public_html/ 可访问，但 https://example.com/private/ 不可访问。请使用后者）。
4.  - 受保护区域的名称，例如“输入密码”或“安全区域”（这不重要，简单即可）。
5.  在文件中输入以下内容，将 `/full/path/of/directory/` 和 `Security Area` 替换为上述信息：`AuthUserFile /full/path/of/directory/.htpasswd AuthName "Security Area" AuthType Basic require valid-user`
6.  保存此 .htaccess 文件并将其上传到您想要隐藏和保护的服务器目录。这将是 WordPress 的安装目录，例如 `/wordpress/` 或 `blog`。
7.  使用 Telnet、cPanel 或其他方式访问服务器的命令面板，转到指定为 `AuthUserFile` 的目录。
8.  输入以下命令，其中 `user_name` 是访问的用户名：`htpasswd -c .htpasswd user_name`
9.  出现提示时，输入密码并确认。
10. 记下您的密码和用户名，并将其保存在安全的地方。

当您准备好向公众开放站点并移除保护时，请从相应位置删除密码文件和 `.htaccess` 文件。

强烈建议您移除默认的 ping URL 指向 [Ping-o-Matic!](https://pingomatic.com/)，否则您的测试文章将发出 ping，并且您的测试博客将被公开，尽管无法访问。

### Htaccess 资源

- [.htaccess 文件使用指南](https://httpd.apache.org/docs/2.0/howto/htaccess.html)
- [认证、授权与访问控制](https://httpd.apache.org/docs/2.0/howto/auth.html)

## 在 Mac 上安装 WordPress

使用以下说明在 Mac 上设置本地服务器环境，用于测试和开发。

- [在 Mac 上使用 MAMP 本地安装 WordPress](https://codex.wordpress.org/Installing_WordPress_Locally_on_Your_Mac_With_MAMP)

## Installing WordPress on Your Windows Desktop

In order for WordPress to work, it must have access to an Apache server, MySQL/MariaDB, and phpMyAdmin. Installing these separately can be painful. Luckily for us, [XAMPP](https://www.apachefriends.org/download.html) installs all of these with one program, allowing you to run WordPress on your computer. There are two versions of the program, Basic and Lite. The Lite version is usually adequate.

1. Download and install [XAMPP](https://www.apachefriends.org/download.html).
2. This installs by default into `C:/xampplite` or `C:\xampp`.
3. Start XAMPP from `c:\xampplite` or `c:\xampp`.
4. You may need to restart your computer to allow apache services to start.
5. In your browser, go to http://localhost/xampp.
6. In the left column under Tools, click **phpMyAdmin**.
7. Login is admin.
8. In **Create new database** enter **wordpress**.
9. In the next box, select **utf8 unicode ci**.
10. Click **Create** button.
11. Unzip your WordPress download into the `htdocs` directory `– c:\xampp\htdocs\`.
12. From the folder, open `wp-config-sample.php` in a text editor.
13. The connection details you need are as follows:
```
// ** MySQL settings ** //
define('DB_NAME', 'wordpress'); // The name of the database
define('DB_USER', 'root'); // Your MySQL username
define('DB_PASSWORD', ''); // ...and password
define('DB_HOST', 'localhost'); // 99% chance you won't need to change this
```
14. Save as `wp-config.php`.
15. Install by going to http://localhost/wordpress/wp-admin/install.php

**IMPORTANT:** It is possible to use this to actually host your blog if you have a good enough connection. If you want to do this, you MUST increase the security level. This description is NOT SECURE if you allow web access to your blog.

## 安装现有 WordPress 站点

借助 XAMPP，您可以直接在计算机上安装 WordPress 并尽情使用。这种方式完全隔离于公开环境，您的所有操作失误都不会被外界看到。当您准备就绪后，即可将其迁移至网站，供所有人访问。

### 要求

1. 能够访问您的服务器数据库。
2. 能够将整个 WordPress 安装文件下载到您的计算机。
3. [适用于 Windows 的基础版 XAMPP](https://www.apachefriends.org/download.html)
4. 硬盘上有足够的空间来容纳您的数据库、WordPress 安装文件和 XAMPP。

### 备份 WordPress

![phpMyAdmin MySQL 数据库](https://user-images.githubusercontent.com/6118303/189545243-6ac55696-9f98-41fe-ab06-061c505f5ab2.png)

首先，完整备份您的 WordPress 站点，包括文件和数据库。这将确保您有一个完好的副本可以回退，以防万一。

然后需要对您的数据库进行第二次备份，但这需要您先做一些清理工作。

通常情况下，WordPress 数据库中存在统计表，这些表包含大量数据，会增加数据库的整体大小，并减慢备份和下载数据库备份副本的过程。您不必清除这些数据，但通常认为这样做是个好主意。

1.  登录您网站服务器上的 PHPMyAdmin。
2.  在主登录界面，选择 **数据库**。
3.  选择您的 WordPress 数据库名称。
4.  从屏幕顶部的标签中，选择 **导出**。
5.  在[导出部分](https://wordpress.org/documentation/files/2018/11/phpmyadmin-export-tab.jpg)顶部的框架中，您将看到数据库中的表列表。
6.  您需要仅选择与您的 WordPress 安装对应的那些表。它们将是您在 `wp-config.php` 文件中找到的带有 `table_prefix` 的表。如果只安装了 WordPress，则从左侧列中选择 **全选**。
7.  确保选中 SQL 按钮。
8.  在面板右侧，确保勾选以下复选框。

![phpMyAdmin 导出界面](https://user-images.githubusercontent.com/6118303/189546285-35103e54-cba6-42ee-86ea-8ea480bf1630.png)

-   结构
-   添加 AUTO_INCREMENT 值
-   用反引号括住表和字段名
-   数据 勾选"另存为文件"选项，并保持模板名称不变。
-   对于压缩，选择 **无**。点击 **执行**。
-   系统应提示您下载一个文件。将该文件保存到您的计算机。根据数据库大小，这可能需要一些时间。

### 下载 WordPress
现在，将整个 WordPress 站点下载到您的计算机。这通常通过 [FTP 客户端程序](https://developer.wordpress.org/advanced-administration/upgrade/ftp/) 完成。请确保包含根目录或 WordPress 目录中的所有核心 WordPress 文件，包括 `index.php`。

此时您的计算机上应包含以下两项：

1. 一个或多个数据库备份。
2. 所有 WordPress 文件、文件夹和图片目录。

请再次将备份文件复制到计算机上的安全位置，以便在下一阶段处理备份副本。

### 安装基础版 XAMPP

1. 安装 XAMPP。默认情况下，它将安装到 `C:\xampp`。
2. 进入 `C:\xampp\apache\conf`，用[文本编辑器](https://wordpress.org/documentation/article/wordpress-glossary/#Text%20editor)打开名为 `httpd.conf` 的文件。
3. 大约在第 166 行，你会找到：`#LoadModule rewrite_module modules/mod_rewrite.so`。
4. 删除 # 并保存文件（这将启用 `mod_rewrite`）。
5. 在 `C:\xampp\htdocs` 内创建一个文件夹。这将是用于 WordPress 的。
6. 将所有下载的 WordPress 文件（不包括 sql 备份）复制到该目录中。
7. 使用文本编辑器，打开你 WordPress 安装中的 `wp-config.php` 文件。
8. 更改新 MySQL 连接的详细信息：
```
// ** MySQL 设置 ** //
define('DB_NAME', 'wordpress'); // 你创建的新数据库的名称
define('DB_USER', 'root'); // 保持原样
define('DB_PASSWORD', ''); // 保持为空
define('DB_HOST', 'localhost'); // 99% 的情况下你不需要更改此项
```
9. 你的主 `index.php` 文件应位于 `/htdocs` 文件夹或子目录中，例如 `/htdocs/wordpress/`。请记下这个路径。
10. 通过点击橙色的 `xampp_start.exe` 或使用 `Program Files \ApacheFriends\XAMPP\XAMPP Control Panel` 中的控制台程序来启动 XAMPP。
11. 一旦它开始运行，点击左侧边栏中的 phpMyAdmin 链接。
12. 创建一个数据库。名称应与你在 `wp-config.php` 文件中使用的名称匹配。

### Importing Your SQL Backup File

Before you begin to import your SQL backup file, you need to change some information inside your `.SQL` file.

1. Using your text editor, open the `.sql` backup database file you downloaded.
2. Find and replace all the instances of your old URL with your new URL. For instance if your blog address is at https://example.com/wordpress/, and your files on your computer are at `/htdocs/wordpress/`, replace it with http://127.0.0.1/wordpress/.
3. Click **Save – Do not use Save as**.

![phpMyAdmin SQL tab](https://user-images.githubusercontent.com/6118303/189546617-26a843c4-e793-4c44-b2a6-13a32b366a8e.png)

Now it is time to import your sql file. From within the phpMyAdmin on your computer, click on **your database name**, then choose the **SQL** tab. From this screen, click **Browse** and find your backup files on your computer. Click **Go**. This can take a few minutes to import.

![Success message](https://user-images.githubusercontent.com/6118303/189546659-7e3cefe1-1744-4458-ac49-98fbd04626c5.png)

Once the procedure is complete, your database will be restored and will work just as it did before. If something goes wrong with this last part of the process, it could be that either your backup was corrupted in some way, or something went wrong with the database itself. **Keep your backup files safe!**

If everything so far has gone well, it is now time to visit your blog! In your browser, type in http://127.0.0.1/wordpress/index.php or the actual name of the folder you created for your WordPress files.

If you get a 404, check you have the right place. You do not need to put htdocs or xampp or anything else after the http://127.0.0.1/ except use your directory name.

WordPress should now function just as it did on the web. You do not need to use the built-in editor in WordPress to alter your files. Just open the files directly in a text editor and edit them as you would any other file. Refreshing your browser’s web page will then show the effects.

### 将测试版 WordPress 站点迁移至您的网站

即将推出 – 如何将您电脑上的测试站点重新部署到主机服务器上。

### 资源
- [qSandbox.com – 创建免费的 WordPress 测试站点来试用（新）插件和主题](https://qsandbox.com/app/)