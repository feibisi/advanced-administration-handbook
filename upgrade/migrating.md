---
title: "迁移 WordPress"
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

# 迁移 WordPress

## 更改站点 URL

在单站点 WordPress 安装的 `设置 -> 常规` 页面中，有两个分别名为“WordPress 地址（URL）”和“站点地址（URL）”的字段。这些设置非常重要，因为它们控制着 WordPress 的安装位置。这些设置控制着页面管理区域及前端的 URL 显示，并在整个 WordPress 代码中使用。

- “站点地址（URL）”设置是您希望用户在浏览器中输入以访问您的 WordPress 博客的地址。
- “WordPress 地址（URL）”设置是您的 WordPress 核心文件所在的地址。

**注意：** 两个设置都应包含 https:// 部分，且末尾不应有斜杠 `/`。

有时，用户可能需要手动更改（或修复）这些设置。通常，当他们更改其中一个或两个设置后，发现网站无法正常工作。这可能导致用户无法轻易找到纠正问题的方法。本文告诉您如何直接更改这些设置。

如果您要将 WordPress 从一个站点迁移到另一个站点，这里还提供了额外信息，因为这同样需要更改站点 URL。如果您只是尝试修复一个“损坏”的站点，则不应使用这些额外信息。

**警告！** 这些说明仅适用于单站点 WordPress 安装。如果您使用 WordPress 多站点，则需要手动编辑数据库。

有四种简单的方法可以手动更改站点 URL。其中任何一种方法都有效，并且功能大致相同。

#### 编辑 wp-config.php

可以在 `wp-config.php` 文件中手动设置站点 URL。

将以下两行添加到您的 `wp-config.php` 文件中，其中 "example.com" 是您网站的正确地址。

```
define( 'WP_HOME', 'https://example.com' );
define( 'WP_SITEURL', 'https://example.com' );
```

这不一定是最佳解决方案，它只是将值硬编码到网站本身。使用此方法后，您将无法再在常规设置页面上编辑这些值。

#### 编辑 functions.php 文件

如果您可以通过 FTP 访问网站，那么当您错误地修改了相关设置值时，此方法将帮助您快速恢复网站运行。

1. 通过 FTP 连接到网站，获取当前使用主题的 `functions.php` 文件副本。您将使用简单的文本编辑器编辑此文件，然后将其上传回网站。
2. 在文件开头的 `<?php` 行之后，立即添加以下两行代码：

```
update_option( 'siteurl', 'https://example.com' );
update_option( 'home', 'https://example.com' );
```
请务必将 `example.com` 替换为您自己的网址。

3. 将文件上传回网站的相同位置。FileZilla 提供了一个便捷的“编辑文件”功能，可以快速完成上述所有操作；如果您可以使用该功能，请加以利用。
4. 多次加载登录页面或管理页面。网站应该会恢复正常。

**重要提示！** 请勿将此代码保留在 `functions.php` 文件中。网站恢复运行后，请立即移除这些代码。

注意：如果您的主题没有 `functions.php` 文件，请使用文本编辑器创建一个新文件。添加 `<?php` 标签以及以下两行代码，并将 `example.com` 替换为您自己的网址：

```	
<?php
update_option( 'siteurl', 'https://example.com' );
update_option( 'home', 'https://example.com' );
```

将此文件上传到您的主题目录。网站恢复运行后，请移除这些代码行或删除该文件。

#### LAN-based site to externally accessible site

Here are some additional details that step you through transferring a LAN-based WordPress site into an externally accessible site, as well as enabling editing the wordpress site from inside the LAN.

Two important keys are router/firewall modifications and the "wait 10+ minutes" after making the changes at the end.

1. Using SSH to log into your server (nano is a server preinstalled text editor):

`$ nano /var/www/books/wp-content/themes/twentyeleven/functions.php`

2. Add lines just after `<?php`

```
update_option( 'siteurl', 'https://example.com:port/yourblog');
update_option( 'home', 'https://example.com:port/yourblog');
```

3. Refresh your web browser using your external site URL:

https://example.com:port/yourblog
`$ nano /var/www/books/wp-content/themes/twentyeleven/functions.php`

4. Remove those lines you just added (or comment them out)

5. Access your router, these steps are for pfSense, other routers should have similar settings to look for/watch out for)

6. Add to firewall/nat table a line like this

`wan/tcp/port/LAN.server.IP/80`

7. Add to firewall/rules table a line like this:

`tcp/*/port/LAN.server.IP/port/*`

8. Uncheck the box at System/advanced/network address translation/Disable NAT Reflection

`"Disables the automatic creation of NAT redirect rules for access to your public IP addresses from within your internal networks. Note: Reflection only works on port forward type items and does not work for large ranges > 500 ports."`

9. Then go do something for ten minutes and when you get back see if the external url https://example.com:port/yourblog from a LAN browser brings the page up correctly.

#### Relocate method

WordPress supports an automatic relocation method intended to be a quick assist to getting a site working when relocating a site from one server to another.

#### Code function

When RELOCATE has been defined as true in `wp-config.php` (see next chapter), the following code in `wp-login.php` will take action:

```
if ( defined( 'RELOCATE' ) AND RELOCATE ) {    
	// Move flag is set
	if ( isset( $_SERVER['PATH_INFO'] ) AND ($_SERVER['PATH_INFO'] != $_SERVER['PHP_SELF']) ) 
		$_SERVER['PHP_SELF'] = str_replace( $_SERVER['PATH_INFO'], "", $_SERVER['PHP_SELF'] );
	$url = dirname( set_url_scheme( 'https://'. $_SERVER['HTTP_HOST'] . $_SERVER['PHP_SELF'] ) );
	if ( $url != get_option( 'siteurl' ) )
		update_option( 'siteurl', $url );
}
```

**Steps**

1. Edit the `wp-config.php` file.
2. After the "define" statements (just before the comment line that says "That's all, stop editing!"), insert a new line, and type: `define('RELOCATE',true);`
3. Save your `wp-config.php` file.
4. Open a web browser and manually point it to `wp-login.php` on the new server. For example, if your new site is at https://www.example.com, then type https://www.example.com/wp-login.php into your browser's address bar.
5. Login as per normal.
6. Look in your web browser's address bar to verify that you have, indeed, logged in to the correct server. If this is the case, then in the Admin back-end, navigate to `Settings > General` and verify that both the address settings are correct. Remember to Save Changes.
7. Once this has been fixed, edit `wp-config.php` and either completely remove the line that you added (delete the whole line), comment it out (with `//`) or change the true value to false if you think it's likely you will be relocating again.

**Note:** When the `RELOCATE` flag is set to true, the Site URL will be automatically updated to whatever path you are using to access the login screen. This will get the admin section up and running on the new URL, but it will not correct any other part of the setup. You'll still need to alter those manually. 

**Important!** Leaving the RELOCATE constant in your `wp-config.php` file is insecure, as it allows an attacker to change your site URL to anything they want in some configurations. Always remove the RELOCATE line from `wp-config.php` after you're done.

#### 直接在数据库中修改 URL

如果您知道如何访问主机上的 phpMyAdmin，可以直接编辑这些值来让网站重新运行。

1. [备份数据库](https://developer.wordpress.org/advanced-administration/security/backup/database/)并将副本保存在站外。
2. 登录 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/)。
3. 点击您的**数据库**链接。
4. 将显示数据库列表。选择您的 WordPress 数据库。
5. 数据库中的所有表将显示在屏幕上。
6. 在列表中查找 `wp_options`。**注意：** 如果您在安装时更改过，表前缀 `wp_` 可能不同。
7. 点击标有**浏览**的小图标。
8. 将打开显示 wp_options 表内字段列表的界面。
9. 在 option_name 字段下，向下滚动查找 `siteurl`。
10. 点击**编辑字段**图标（通常位于该行最左侧）。
11. 将出现**编辑字段**窗口。
12. 在 option_value 输入框中，仔细将 URL 信息更改为新地址。
13. 确认无误后点击**执行**保存信息。
14. 您将返回 `wp_options` 表。
15. 在表中查找 home 字段并点击**编辑字段**。**注意：** `wp_options` 内有多个分页，使用 > 符号翻页查找。
16. 在 option_value 输入框中，仔细将 URL 信息更改为新地址。
17. 确认无误后点击**执行**保存信息。

### 迁移站点

将站点从一个位置迁移到另一个位置时，有时需要手动修改数据库中的数据，以使新的站点 URL 信息能被正确识别。虽然存在许多辅助工具，通常应优先使用这些工具而非手动修改。

此处提供的信息仅供参考。这些数据可能不完整或不准确。

若要将 WordPress 从一个系统迁移到另一个系统，您应首先阅读 [迁移 WordPress](https://developer.wordpress.org/advanced-administration/upgrade/migrating/) 文章。

#### 修改数据表前缀

与许多 WordPress 管理员一样，您可能通过修改 `wp-config.php` 文件，在单一数据库中运行多个 WordPress 安装。这些修改通常涉及动态设置数据表前缀。如果您确实需要更改数据表前缀，则必须同时更新 `prefix_usermeta` 表中的若干条目。

与上一节相同，请记住 SQL 更改是永久性的，因此您应首先备份数据库：

如果您正在为某个站点更改数据表前缀，请记得同时修改 usermeta 表中的表前缀。这将使新站点能够正确识别旧站点的用户权限。
```
UPDATE `newprefix_usermeta` SET `meta_key` = REPLACE( `meta_key` , 'oldprefix_', 'newprefix_' );
```

#### 修改模板文件

在您的 WordPress 主题中，打开每个模板文件，搜索所有手动输入的旧域名引用并将其替换为新域名。请仔细检查各个模板文件（如 `sidebar.php` 和 `footer.php`）中可能存在的特定手写代码链接。WordPress 使用名为 `bloginfo()` 的模板标签，根据管理后台 > 设置 > 常规面板中填写的信息自动生成网站地址，因此模板文件中的此标签无需修改。

#### 修改配置文件

如果您的数据库发生迁移或某些变更，您需要更新 WordPress 配置文件。

1. 仅在以下情况下需要修改配置文件：
  - 数据库已迁移至其他服务器，不再运行于本地主机
  - 您已重命名数据库
  - 您已更改数据库用户名
2. 备份您的 `wp-config.php` 文件副本。
3. 在[文本编辑器](https://wordpress.org/documentation/article/wordpress-glossary/#Text_editor)中打开 `wp-config.php` 文件。
4. 检查文件内容。特别需要关注[数据库主机配置项](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/#set-database-host)。
5. 保存文件。
6. 此时您的 WordPress 博客应能正常运行。

#### 验证配置文件

1. 在您的[管理](https://wordpress.org/documentation/article/administration-screens/) > [设置](https://wordpress.org/documentation/article/administration-screens/#general) > [常规](https://wordpress.org/documentation/article/settings-general-screen/)面板中，验证您在上述“更改 URL”中所做的修改是否正确。
2. 验证您的 **WordPress 地址 (URL)** 中的引用是否包含新地址。
3. 验证您的 **站点地址 (URL)** 中的引用是否包含新地址。
4. 如果您已进行更改，请点击 **保存更改**。

#### 修改 .htaccess 文件

在管理后台 > 设置 > 常规面板中更改信息后，如果您正在使用固定链接或任何重写或重定向规则，则需要更新您的 .htaccess 文件。

1. **备份**您的 `.htaccess` 文件。这不是建议，而是必需步骤。
2. 在[文本编辑器](https://wordpress.org/documentation/article/glossary/#text-editor)中打开 `.htaccess` 文件。
3. 检查其内容，找到您之前添加的任何自定义重写或重定向规则。**复制**这些内容到另一个文本文件中妥善保存。
4. 关闭文件。
5. 按照固定链接子面板上的说明，将固定链接更新到 `.htaccess` 文件中。
6. 打开新的 `.htaccess` 文件，检查您的自定义重写和重定向规则是否仍然存在。如果不存在，请从保存的文件中复制它们并粘贴到新的 `.htaccess` 文件中。
7. 根据需要修改这些自定义重写和重定向规则，以反映新的站点地址。
8. 保存文件。
9. 测试这些重定向，确保它们正常工作。

如果操作失误，您可以从备份中[恢复数据库](https://developer.wordpress.org/advanced-administration/security/backup/)并重试。因此请确保第一次就操作正确。

#### 其他注意事项

迁移网站时，您可能还需要修改以下内容以修正 URL：

1. 图片链接：图片链接存储在 `wp_posts` 表的 "post_content" 字段中。您可以使用类似的代码更新图片链接。
2. wp_options：除了上述 "siteurl" 和 "home" 项，还有其他 option_value 也需要修改，例如 "upload path" 以及某些插件项（取决于已安装的插件，如 widgets、stats、DMSGuestbook、sitemap 等）。
3. 要修复包含过时 URL 的小工具，可在仪表盘 / 外观 / 小工具中进行编辑。
4. 对剩余项执行完整的数据库搜索。**务必**了解您要修改的内容，并逐一检查每个项是否存在不当替换。
5. 如果您运行的是网络/多站点，则需替换数据库中的 URL 实例。它们存储在多个表中，包括每个站点（博客）。替换时请谨慎操作，确保在修改前了解字段的含义。请参阅下方重要的 GUID 说明，了解不应修改的示例。
6. **注意：** 如果在数据库选项表的 `dashboard_incoming_links` 下找到旧 URL，可以忽略或删除该选项。自 WP 3.8 起已不再使用。

#### 重要 GUID 说明

当执行上述操作并直接在数据库中更改 URL 时，您会遇到 URL 位于 `wp_posts` 表的 "guid" 列中的情况。**切勿**更改此字段的内容，这一点至关重要。

术语 "GUID" 代表 "全局唯一标识符"。该字段旨在存储文章的标识符，该标识符 a) 在整个时空范围内是唯一的，并且 b) **永不**更改。GUID 字段主要用于生成 WordPress 订阅源。

当订阅阅读器读取订阅源时，它会使用 GUID 字段的内容来判断之前是否已显示过特定项目。它通过多种方式之一来实现这一点，但最常见的方法只是存储一个已显示并"标记为已读"或类似状态的 GUID 列表。

因此，更改 GUID 将意味着许多订阅阅读器会突然在用户的阅读器中再次显示您的内容，就好像它是新内容一样，这可能会惹恼您的用户。

为了使 GUID 字段具有"全局"唯一性，一个公认的惯例是使用 URL 或 URL 的某种表示形式。因此，如果您拥有 `example.com`，那么您是唯一使用 `example.com` 的人，因此它对您和您的网站是唯一的。这就是 WordPress 使用固定链接或其某种形式作为 GUID 的原因。

然而，第二部分是 GUID **绝不能**更改。即使您更换了域名，文章仍然是同一篇文章，即使位于新位置。当您更改 URL 时，订阅阅读器切换到您的新订阅源时，仍应知道它们之前已阅读过您的某些文章，因此 GUID **必须**保持不变。

**在任何情况下，都切勿更改 GUID 列的内容。**

如果需要将默认的上传文件夹更改为其他位置，则需要在文章表的 `post_content` 列中更改任何媒体 URL。例如，如果默认上传文件夹从 `wp-content/uploads` 更改为 `images`：

`UPDATE wp_posts SET post_content = REPLACE(post_content,'www.domain.com/wp-content/uploads','www.domain.com/images');`

#### 多站点注意事项

请参阅 [迁移 WordPress 多站点](https://developer.wordpress.org/advanced-administration/upgrade/migrating/#moving-wordpress-multisite)

#### wp-cli

[wp-cli](https://wp-cli.org/) 是一个非常实用的命令行工具。

`wp search-replace 'example.dev' 'example.com' --skip-columns=guid`

或者，如果只想更改选项，可以执行：
```
wp option update home 'https://example.com'
wp option update siteurl 'https://example.com'
```

# 迁移 WordPress

无论您是将 WordPress 迁移到新服务器还是同一服务器的不同位置，都无需重新安装。WordPress 足够灵活，能够处理所有这些情况。

## 迁移至新服务器

若要将 WordPress 从一台服务器迁移至另一台，请先备份网站中的 WordPress 目录、图片、插件及其他文件，同时备份数据库。参阅 [WordPress 备份](https://developer.wordpress.org/advanced-administration/security/backup/) 与 [数据库备份](https://developer.wordpress.org/advanced-administration/security/backup/database/)。

### 保持域名和 URL 不变

在不更改 WordPress 站点主页和站点 URL 的情况下迁移域名非常简单，大多数情况下只需移动文件即可完成。

- 如果数据库和 URL 保持不变，只需复制文件和数据库即可迁移。
- 如果数据库名称或用户发生更改，请[编辑 wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 以填入正确的值。
- 若想在切换前进行测试，必须临时更改数据库表 "wp_options" 中的 "siteurl" 和 "home"（通过 phpMyAdmin 或类似工具）。
- 如果设置了任何重写规则（固定链接），必须在站点上线时禁用 .htaccess 并重新配置固定链接。

### Changing Your Domain Name and URLs

Moving a website and changing your domain name or URLs (i.e. from https://example.com/site to https://example.com, or https://example.com to https://example.net) requires the following steps – in sequence.

1. Download your existing site files.
2. Export your database – go in to MySQL and export the database.
3. Move the backed up files and database into a new folder – somewhere safe – this is your site backup.
4. Log in to the site you want to move and go to Settings > General, then change the URLs. (ie from https://example.com/ to https://example.net) – save the settings and expect to see a 404 page.
5. Download your site files again.
6. Export the database again.
7. Edit `wp-config.php` with the new server's MySQL database name, user and password.
8. Upload the files.
9. Import the database on the new server.

When your domain name or URLs change there are additional concerns. The files and database can be moved, however references to the old domain name or location will remain in the database, and that can cause issues with links or theme display.

If you do a search and replace on your entire database to change the URLs, you can cause issues with data serialization, due to the fact that some themes and widgets store values with the length of your URL marked. When this changes, things break. To avoid that serialization issue, you have three options:

1. Use the [Velvet Blues Update URLs](https://wordpress.org/plugins/velvet-blues-update-urls/) or [Better Search Replace](https://wordpress.org/plugins/better-search-replace/) plugins if you can access your Dashboard.
2. Use [WP-CLI's search-replace](https://developer.wordpress.org/cli/commands/search-replace/) if your hosting provider (or you) have installed WP-CLI.
3. Use the [Search and Replace for WordPress Databases Script](https://interconnectit.com/products/search-and-replace-for-wordpress-databases/) to safely change all instances on your old domain or path to your new one. (**only use this option if you are comfortable with database administration** )

Note: Only perform a search and replace on the wp_posts table.
Note: Search and Replace from Interconnectit is a 3rd party script

## 在现有服务器上移动目录

将 WordPress 文件从服务器上的一个位置移动到另一个位置（即更改其 URL）需要特别注意。如果您想将 WordPress 移动到其自己的文件夹，但让它从您的域名根目录运行，请阅读《为 WordPress 设置独立目录》以获取详细说明。

以下是将您的 WordPress 站点移动到同一服务器上新位置的分步说明：

1. Create the new location using one of these two options:
   - If you will be moving your WordPress core files to a new directory, create the new directory.
   - If you want to move WordPress to your root directory, make sure all `index.php`, [.htaccess](https://wordpress.org/documentation/article/glossary/#htaccess), and other files that might be copied over are backed up and/or moved, and that the root directory is ready for the new WordPress files.
2. Log in to your site.
3. Go to the [Administration](https://wordpress.org/documentation/article/administration-screens/) > [Settings](https://wordpress.org/documentation/article/administration-screens/#settings-configuration-settings) > [General](https://wordpress.org/documentation/article/settings-general-screen/) screen.
4. In the box for **WordPress Address (URL)**: change the address to the new location of your main WordPress core files.
5. In the box for **Site Address (URL)**: change the address to the new location, which should match the WordPress (your public site) address.
6. Click **Save Changes**.
7. (Do not try to open/view your site now!)
8. Move your WordPress core files to the new location. This includes the files found within the original directory, such as https://example.com/wordpress, and all the sub-directories, to the new location.
9. Now, try to open your site by going to yourdomain.com/wp-admin. Note, you may need to go to yourdomain.com/wp-login.php
10. If you are using [Permalinks](https://wordpress.org/documentation/article/using-permalinks/), go to the Administration > Settings > [Permalinks](https://wordpress.org/documentation/article/settings-permalinks-screen/) panel and update your Permalink structure to your [.htaccess](https://wordpress.org/documentation/article/glossary/#htaccess), file, which should be in the same directory as the main `index.php` file.
11. Existing image/media links uploaded media will refer to the old folder and must be updated with the new location. You can do this with the [Better Search Replace](https://wordpress.org/plugins/better-search-replace/) or [Velvet Blues Update URLs](https://wordpress.org/plugins/velvet-blues-update-urls/) plugins, [WP-CLI's search-replace](https://developer.wordpress.org/cli/commands/search-replace/) if your hosting provider (or you) have installed WP-CLI, manually in your SQL database, or by using the 3rd party database updating tool [Search and Replace Databases Script](https://interconnectit.com/products/search-and-replace-for-wordpress-databases/) * **Note:** this script is best used by experienced developers.
12. In some cases your permissions may have changed, depending on your ISP. Watch for any files with "0000" permissions and change them back to "0644".
13. If your theme supports menus, links to your home page may still have the old subdirectory embedded in them. Go to Appearance > Menus and update them.
14. Sometimes you would need to restart your server, otherwise your server may give out an error. (happens in MAMP software (Mac)).
15. It is important that you set the URI locations BEFORE you move the files.

#### 如果您忘记更改位置

如果您在更改 URI 之前不小心移动了文件：您有两个选择。

1. 假设文件原本位于 /path/to/old/，您在更改 URI 之前将它们移动到了 /path/to/new。修复方法是使 
```
/path/to/old/ 成为指向 /path/to/new/ 的符号链接（对于 Windows 用户，“符号链接”等同于“快捷方式”），即
ln -s /path/to/new /path/to/old
```
然后正常按照上述步骤操作。之后，如果您愿意，可以删除该符号链接。

2. 如果您忘记更改 WordPress 地址和博客地址，您将无法通过 WordPress 界面进行更改。但是，如果您可以访问数据库，则可以修复此问题。进入您网站的数据并找到 wp_options 表。此表存储了您可以在界面中设置的所有选项。WordPress 地址和博客地址分别存储为 `siteurl` 和 `home`（option_name 字段）。您只需将 `option_name='siteurl‘` 或 `option_name='home‘` 记录的 option_value 字段更改为正确的 URL 即可。

注意：有时，WordPress 地址和博客地址存储在 [WordPress 瞬态](https://developer.wordpress.org/apis/handbook/transients/)中。搜索和替换脚本可能难以将它们修改为新地址，因此某些插件可能因此引用旧地址。瞬态是存储在 wp_options 数据库表中的临时（缓存）值，删除后可以按需重新创建。因此，从迁移的数据库副本中删除它们是安全的，可以让它们重新创建。此数据库查询（再次提醒，请先备份！）将清除所有瞬态：

```
DELETE FROM `wp_options` WHERE option_name LIKE '%\_transient\_%' 
```

#### 如果您意外更改了 WordPress 站点 URL

假设您意外更改了 URI，导致无法移动文件（但仍可通过重定向等方式访问登录页面）。

可以使用 `wp-login.php` 来（重新）设置 URI。找到这行代码：

```
require( dirname(__FILE__) . '/wp-load.php' );
```

并在其下方插入以下代码：

```
//FIXME：请务必注释/移除这些临时修复行。（数据库更新后立即操作）
update_option('siteurl', 'https://example.com/the/path' );
update_option('home', 'https://example.com/the/path' );
```

操作完成。请测试您的站点以确保其正常工作。如果更改涉及站点新地址，请务必告知用户新地址，并考虑在 `.htaccess` 文件中添加重定向指令，以引导访客访问新位置。

[更改站点 URL](https://developer.wordpress.org/advanced-administration/upgrade/migrating/) 也提供了此过程的详细说明。

## 管理您的旧网站

### 关闭旧网站
1. 从您的**旧**网站下载一份 WordPress 核心文件到硬盘，并根据新服务器情况[编辑 wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/)。
2. 返回您的**旧**网站，进入[管理](https://wordpress.org/documentation/article/administration-screens/) > [设置](https://wordpress.org/documentation/article/administration-screens/#settings-configuration-settings) > [常规](https://wordpress.org/documentation/article/settings-general-screen/)页面，将两个 URL 都更改为新网站的地址。
3. 登录服务器，进入 phpMyAdmin，将数据库导出为文件并保存（但请保留旧数据库以防万一）。现在，将这个新数据库以及编辑过的 wp-config.php 和 WordPress 核心文件副本上传到新服务器即可！

#### 保持运行
警告：请确保在继续操作前已备份旧网站的 WordPress 数据库！

_第一部分 – 激活新网站_

1. 将整个 WordPress 安装文件下载到硬盘。为文件夹命名以明确表示这是旧网站的安装文件。
2. 下载数据库。
3. 返回旧网站，进入设置选项，将网址（两个都）更改为新网站的网址。
4. 再次将整个 WordPress 安装文件下载到硬盘。为文件夹命名以明确表示这是新网站的安装文件。
5. 再次下载数据库（但保留旧数据库）。将此数据库上传到新服务器。如果在新服务器上使用相同的数据库名称并创建与旧服务器相同登录凭据的用户，操作将最为简便。
6. 如果使用了不同的数据库名称和/或用户（参见上一步），请在新网站的安装文件夹中[适当编辑 wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/)。
7. 将新网站的安装文件夹上传到新网站。完成！您的新网站应该可以正常工作了！

_第二部分 – 恢复旧网站_

1. 在原始服务器上，删除旧网站的数据库（请记住，您应该在最初已在本地计算机上保存了副本）。
2. 将旧网站的安装文件夹上传到原始服务器，覆盖当前存在的文件（您也可以删除服务器上的安装文件夹，然后直接重新上传旧网站的文件）。
3. 将本地计算机上的旧网站数据库上传到服务器。这样就完成了！

另一种便于复制文章、评论、页面、分类和自定义字段（文章状态、数据、固定链接、ping 状态等）的操作流程：

1. 安装一个新的 WordPress 网站
2. 进入旧网站的管理面板。在管理 > 导出中，在“限制作者”菜单中选择“全部”。
3. 点击“下载导出文件”
4. 在新网站中进入管理 > 导入，选择 WordPress 项目。
5. 在显示的页面上，选择刚刚导出的文件。点击“上传文件并导入”
6. 将出现一个新页面。在“分配作者”中，将作者分配给现有用户或创建新用户。
7. 点击“提交”
8. 最后，点击“开始使用”

_注意：使用此方法时，如果新网站中已有一些文章（如 Hello World、信息页面等），这些文章不会被删除。只会添加文章。而使用前一种方法时，新网站中的文章将被删除。_

## 迁移 WordPress 多站点

[多站点](https://developer.wordpress.org/advanced-administration/multisite/create-network/)的迁移稍显复杂，因为数据库本身包含多个对服务器名称和文件夹位置的引用。如果你只是将站点迁移到使用相同域名的新服务器，可以像处理传统安装一样，直接复制文件和数据库。

反之，如果你要更改域名，那么迁移多站点的最佳方式是：先移动文件，编辑 `.htaccess` 和 `wp-config.php`（如果包含多站点的文件夹名称发生了变化），然后手动编辑数据库。搜索所有包含你域名的实例，并根据需要进行更改。这一步目前还无法轻松实现自动化。安全起见，你可以搜索/替换任何 `wp_x_posts` 表，但切勿在没有使用 [WordPress 数据库搜索替换](https://github.com/interconnectit/Search-Replace-DB)脚本（又称 interconnectit 脚本）的情况下进行全局搜索/替换。

如果你要将多站点从一个文件夹迁移到另一个文件夹，需要确保编辑 `wp_blogs` 条目以正确更改文件夹名称。无论如何，你都应该手动检查 `wp_site` 和 `wp_blogs` 表，以确保所有站点都已正确更改。

此外，手动检查所有 wp_x_options 表，查找以下三个字段，并根据需要进行编辑：

- home
- siteurl
- fileupload_url

如果你正在从子域名迁移到子文件夹，或反之，请记得相应地调整 .htaccess 文件和 `wp-config.php` 文件中 SUBDOMAIN_INSTALL 的值。

### Related Links

- [Moving a blog from wordpress.com to self-hosted blog](https://problogger.com/how-to-move-from-wordpresscom-to-wordpressorg/)
- [Moving WordPress to a new domain or server](https://sltaylor.co.uk/blog/moving-wordpress-new-domain-server/)
- [Italian version of this article – Versione italiana dell'articolo](https://www.valent-blog.eu/2007/09/14/trasferire-wordpress/)
- [Search and Replace for WordPress Databases](https://interconnectit.com/search-and-replace-for-wordpress-databases/)
- [PHP script to replace site url in WordPress database dump, even with WPML](http://blog.lavoie.sl/2012/07/php-script-to-replace-site-url-in.html)
- [The Duplicator plugin helps administrators move a site from one location to another](https://wordpress.org/plugins/duplicator/)
- [Technical tutorial on moving your WordPress blog to Bitnami's AWS configuration](https://agileweboperations.com/2011/01/20/migrate-your-wordpress-blog-to-a-bitnami-ec2-instance/)

# 将多个博客迁移至 WordPress 多站点

本教程将说明如何将多个 WordPress 独立安装迁移至 WordPress 多站点安装。您既可以迁移使用自有域名的站点，也可以迁移使用主域名下子域名的站点。

本教程假设您在使用 cPanel 的服务器上托管 WordPress。如果您使用其他方案管理服务器，则需相应调整操作步骤。

### 操作步骤

#### 备份网站

在 cPanel 中生成完整的网站备份。通过 FTP 复制服务器上的所有文件也可能有所帮助，这样你可以方便地访问插件和主题文件，这在后续步骤中会用到。

#### 从现有 WordPress 安装中导出
在您现有的每个 WordPress 安装中，前往 WordPress 的“工具”>“导出”。下载包含每个站点所有文章和页面的 WXR 文件。请参阅 [工具导出屏幕](https://wordpress.org/documentation/article/tools-export-screen/) 上的说明。

确保您的导出文件确实包含所有文章和页面。您可以使用文本编辑器查看导出文件的最后一条记录来验证这一点。最后一条记录应为最新的文章。

某些插件可能会与导出过程冲突，导致生成空文件或不完整的文件。为稳妥起见，您可能应在执行导出前禁用所有插件。

首先删除所有隔离的垃圾评论也是个好主意，因为这些评论也会被导出，导致文件不必要地变大。

**注意：** 此方法不会导出小工具配置和博客/插件设置。如果您在单个托管账户内迁移，请在此阶段记录这些设置，因为当您删除旧域名时，它们将会消失。

#### 安装 WordPress
安装 WordPress。请按照 [安装 WordPress](https://developer.wordpress.org/advanced-administration/before-install/howto-install/) 的说明进行操作。

#### 启用多站点
在您的 WordPress 安装中启用多站点功能。这需要多次编辑 `wp-config.php` 文件。您需要选择子域名选项，而非子目录选项。请参阅关于如何 [创建网络](https://developer.wordpress.org/advanced-administration/multisite/create-network/) 的说明。

#### 为每个要导入的站点创建博客
为您想要托管在独立域名下的每个站点创建博客。例如，`importedblogdotorg.mydomain.com`。

注意：请仔细选择名称，因为更改名称会导致管理重定向问题。如果您在同一托管账户内迁移站点，这一点尤其重要。

#### 为每个博客导入 WXR 文件
进入每个博客的后台，导入为每个博客导出的 WXR 文件。将作者映射到正确的用户，或创建新用户。务必勾选导入照片和其他附件的选项。请参阅工具导入子面板上的说明。

**注意：** 如果选择将图片从源站点导入到目标站点，请确保它们已上传到正确位置，并在相应的文章或页面中正确显示。

#### 为每个站点编辑 WordPress 配置设置

编辑每个站点的配置设置、小工具等。在此步骤结束时，每个站点应看起来与之前完全一样，只是 URL 变为 `subdomain.example.com` 或 `example.com/subsite`，而非其正确、最终的 URL。

#### PHP 配置的限制
您可能会遇到主机上 PHP 配置的问题。存在两个潜在问题：一是 PHP 的 `max_upload_size` 对于 WXR 文件来说可能太小；二是 PHP 内存限制可能不足以导入所有文章。

有几种解决方法：一是请求您的主机提供商提高限制，即使是临时的；二是在您的 /wp-admin/ 和 /wp-includes 目录中放置一个 php.ini 文件来为您提高限制（php.ini 文件不递归生效，因此必须放在这些目录中）。例如，设置 10 MB 的上传限制和 128 MB 的内存限制应该可行，但请先与您的主机提供商确认，以免违反协议条款。

如需 PHP 配置问题的帮助，请在 [WordPress 论坛支持](https://wordpress.org/support/forums/) 中搜索。

#### 将附加域转换为停靠域

在 cPanel 中删除附加域并用停靠域替换，也会删除与这些域关联的所有域名转发器和电子邮件转发器。请注意这一点，以便在切换后可以恢复这些转发器。

#### 用户导入的限制

虽然存在上述将内容导入多站点博客实例的方法，但在导入多个用户时会遇到严重问题。用户会在导入过程中生成，但新博客不会获得任何角色或附加信息。

#### 设置丢失
如果旧站点已无法访问，且您发现忘记复制某些设置，或想确认所有配置是否正确，请通过谷歌搜索您的站点并点击查看缓存版本。此选项仅在您的新站点被爬取前可用，因此请尽快操作。

另一种选择是使用 [互联网档案馆 Wayback Machine](https://archive.org/web/)。他们可能已存档该站点（或部分内容）的副本。