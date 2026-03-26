---
title: "常见问题故障排除"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Resources
    - Repos
    - Data
---

# 常见问题故障排除

## 为什么我看不到我的文章？只显示“抱歉，没有符合您条件的文章”？

清除浏览器缓存和 Cookie 可能解决此问题。同时，请检查您的 `search.php` 和 `index.php` 模板文件是否存在错误。

另请参阅：

* [我进行了更改但没有任何反应](https://wordpress.org/support/topic/i-make-changes-and-nothing-happens/)

## 如何获取更多帮助？

除了这些[常见问题](https://codex.wordpress.org/FAQ)外，还有多种资源可帮助您获取更多关于 WordPress 的帮助。

* [故障排除](https://codex.wordpress.org/Troubleshooting)
* [寻找 WordPress 帮助](https://wordpress.org/documentation/article/where-to-find-wordpress-help/)
* [使用支持论坛](https://wordpress.org/support/welcome/)
* [关于 WordPress 的资源和技术文章](https://codex.wordpress.org/Technical%20Articles)
* [安装问题](https://codex.wordpress.org/Troubleshooting#Installation_Problems)

## 我在哪里可以找到 CSS 问题的帮助？

以下文章将帮助您排查和解决许多 [CSS](https://wordpress.org/documentation/article/css/) 问题：

* [博客设计与布局](https://codex.wordpress.org/Blog_Design_and_Layout)
* [查找您的 CSS 样式](https://codex.wordpress.org/Finding%20Your%20CSS%20Styles)
* [CSS 修复浏览器错误](https://codex.wordpress.org/CSS%20Fixing%20Browser%20Bugs)
* [CSS 故障排除](https://codex.wordpress.org/CSS%20Troubleshooting)
* [WordPress CSS 信息与资源](https://wordpress.org/documentation/article/css/)

## Why do I get an error message about Sending Referrers?

If you got this message when trying to save a post, consider checking [Administration](https://wordpress.org/documentation/article/administration-screens/) > [Settings](https://wordpress.org/documentation/article/administration-screens/) > [General](https://wordpress.org/documentation/article/settings-general-screen/) and make sure both your **WordPress address (URI)** and the **Blog address (URI)** do not use 'www'. For example, instead of https://www.example.com use https://example.com in those fields.

See also:

* [Enable Sending Referrers](https://codex.wordpress.org/Enable%20Sending%20Referrers)

## How do I empty a database table?

See also:

* [Emptying a Database Table](https://developer.wordpress.org/advanced-administration/server/empty-database/)

## How do I fix the following error SQL/DB Error errcode 13 Can't create/write to file?

**Problem:** The MySQL variable `tmpdir` is set to a directory that cannot be written to when using PHP to access MySQL.

To verify this, enter MySQL at the command line and type `show variables`;

You'll get a long list and one of them will read: `tmpdir = /somedir/` (whatever your setting is.)

**Solution:** Alter the `tmpdir` variable to point to a writable directory.

**Steps:**

1. Find the `my.cnf` file. On *nix systems this is usually in `/etc/`. On Windows system, Find the `my.ini`.
2. Once found, open this in a simple text editor and find the `[mysqld]` section.
3. Under this section, find the `tmpdir` line. If this line is commented (has a `#` at the start), delete the `#` and edit the line so that it reads: `tmpdir = /writable/dir` where `/writable/dir` is a directory to which you can write. Some use `/tmp`, or you might also try `/var/tmp` or `/usr/tmp`. On Windows, use `C:/Windows/tmp`.
4. Save the file.
5. Shutdown MySQL by typing `mysqlshutdown -u -p shutdown`.
6. Start MySQL by going to the MySQL directory and typing `./bin/safe_mysqld &`. Usually the MySQL directory is in `/usr/local` or sometimes in `/usr/` on Linux systems.

If none of this make sense and you have someone to administrate your system for you, show the above to them and they should be able to figure it out.

**Description:** You get a warning message on your browser that says:

```
Warning: Cannot modify header information – headers already sent by (output started at
```

**Reason and Solution:**

It is usually because there are spaces, new lines, or other stuff before an opening `<!--?php &lt;/code&gt;&lt;/strong&gt; tag or after a closing &lt;strong&gt;&lt;code&gt;?-->` tag, typically in `wp-config.php`. This could be true about some other file too, so please check the error message, as it will list the specific file name where the error occurred (see "Interpreting the Error Message" below). Replacing the faulty file with one from your most recent backup or one from a fresh WordPress download is your best bet, but if neither of those are an option, please follow the steps below.

Just because you cannot see anything does not mean that PHP sees the same.

1. 通过 [FTP](https://developer.wordpress.org/advanced-administration/upgrade/ftp/) 或您主机控制面板提供的文件管理器下载错误信息中提到的文件。
2. 在[纯文本编辑器](https://wordpress.org/documentation/article/wordpress-glossary/#text-editor)中打开该文件（**不要**使用 MS Word 或类似软件。Notepad 或 BBEdit 都可以）。
3. 检查文件**最**开头的字符，确保其后面没有空行或空格。
4. 在保存之前，或使用“另存为”对话框时，确保文件编码不是 `UTF-8 BOM`，而是纯 `UTF-8` 或任何不带 `BOM` 后缀的编码。

要确保文件末尾正确，请执行以下操作：

1. 将光标置于 `?` 和 `>` 之间
2. 现在按下您计算机上的 DELETE 键 **给 MAC 用户的提示**：PC 上的 "DELETE" 键删除光标**右侧**的字符。这里指的就是这个键。
3. 按住该键
4. 至少持续 15 秒
5. 现在输入 `>` 然后
6. **保存**，期间不要按任何其他键。
7. 如果您按了其他键，问题将会重现。
8. **不要**将代码放在不必要的代码块中，请将它们放在一个单一的 PHP 块中。

错误示例：

```
<?php
some code;
?>

<?php
some other codes;
?>
```

正确示例：

```
<?php
code;

some other code;
?>
```

编辑并保存文件后，将其上传回您的服务器。

**注意**：同时检查文件的编码。如果文件以带 BOM 的 UTF-8 编码，BOM 会被视为开始输出的字符。

**解读错误信息：**

如果错误信息显示：`Warning: Cannot modify header information - headers already sent by (output started at /path/blog/wp-config.php:34) in /path/blog/wp-login.php on line 42`，那么问题出在 `wp-config.php` 的第 `34` 行，而不是 `wp-login.php` 的第 `42` 行。在这种情况下，`wp-login.php` 的第 `42` 行是受害者。它受到了 `wp-config.php` 第 `34` 行多余空白字符的影响。

如果错误信息显示：`Warning: Cannot modify header information - headers already sent by (output started at /path/wp-admin/admin-header.php:8) in /path/wp-admin/post.php on line 569`，那么问题出在 `admin-header.php` 的第 `8` 行，而不是 `post.php` 的第 `569` 行。在这种情况下，`post.php` 的第 `569` 行是受害者。它受到了 `admin-header.php` 第 `8` 行多余空白字符的影响。

**可能导致该错误的其他问题：**

如果您使用了函数：[wp_redirect()](https://developer.wordpress.org/reference/functions/wp_redirect/) 或在头部（或任何内容）已发送后尝试使用头部重定向，该错误信息就会出现。如果需要，请改用 JavaScript 重定向。

## 为什么我的“发布”或“保存草稿”按钮不起作用？

要解决此问题及类似问题，请逐一禁用您的插件，直到找到问题根源。通常，这是由于两个或多个插件试图使用相同资源（例如 JQuery 或其他基于 Java 的工具）造成的。

此外，也可能是您的浏览器存在问题。常见的解决方法是清空浏览器缓存。请查阅您常用浏览器的文档以了解如何操作。

## 为什么在使用苹果 Safari 浏览器时看不到可视化富文本编辑器或快速标签按钮？

请更新您的 Safari 浏览器。早期版本的 Safari 不受支持。

## 未收到通过电子邮件发送的密码

**描述：** 当用户尝试注册您的博客或通过输入用户名和/或电子邮件来更改密码时，WordPress 会提示已将密码通过电子邮件发送给他们，但他们从未收到。

**原因和解决方案：** WordPress 使用标准的 PHP [mail()](https://www.php.net/manual/en/function.mail.php) 函数，该函数使用 sendmail。不需要账户信息。如果您使用的是托管服务，这通常不是问题，但如果您使用自己的服务器且没有 SMTP 服务器，邮件将永远无法发送。如果您使用的是 *NIX 服务器，您的机器上应该安装有 postfix 或 sendmail；您只需要设置它们（在互联网上搜索操作方法）。如果您不想在 *NIX 服务器上设置完整的邮件服务器，您可能会发现 [ssmtp](https://packages.debian.org/stable/mail/ssmtp) 很有用——它提供了一种“安全、有效且简单的方式将邮件从系统发送到您的邮件中心”。在 Windows 机器上，可以尝试使用 sendmail 模拟器，例如[支持 TLS v1.2 的 Windows 版 Fake sendmail](https://github.com/sendmail-tls1-2/main)。

更多帮助可以在 WordPress 支持论坛的这个主题中找到：[WordPress 如何发送用户注册电子邮件？](https://wordpress.org/support/topic/how-does-wp-send-the-user-registration-email/)。对于基于插件的替代方案，您可以尝试 [Configure SMTP](https://coffee2code.com/wp-plugins/configure-smtp/)：“在 WordPress 中配置 SMTP 邮件发送，包括支持通过 SSL/TLS（例如 GMail）发送电子邮件。”

**Windows 主机服务器特定：** 检查 SMTP 虚拟服务器上的“中继”设置。授予 `127.0.0.1` 访问权限。然后在您的 `php.ini` 文件中，将 `SMTP` 设置设置为相同的 IP 地址。同时将 `smtp_port` 设置为 `25`。

**确保使用正确的返回地址：** 默认情况下，WordPress 邮件程序将发件人字段填充为 _wordpress@example.com_，发件人名称填充为 _WordPress_。

如果这是一个有效的电子邮件地址，这没问题。例如，如果您的真实电子邮件是 _wordpress@example.com_，您的主机应该传递该电子邮件进行投递。只要 _example.com_ 设置为发送和接收邮件，即使 _wordpress_ 不是有效的邮箱，您的邮件也可能会发送。但是，如果您将真实电子邮件设置为发件人地址，并且它是类似 _wordpress@example.com_ 的地址，邮件可能无法发送，因为 _gmail.com_ 不是邮件服务器处理的域。

**被视为垃圾邮件：** 您的电子邮件可能已被路由到垃圾邮件文件夹，甚至更糟，直接被当作恶意邮件丢弃。您可以采取一些措施来说服收件人的邮件服务器，您的邮件是合法的，应该按地址投递。

**SPF:**（发件人策略框架）这是最常用的反垃圾邮件措施。如果您使用的是托管系统，很可能您的主机已为您使用的邮件服务器设置了此功能。让 WordPress 向您发送邮件，并检查邮件头中是否有通过 SPF 检查的证据。您可以通过登录页面的“忘记密码”链接获取发送的邮件。要保留旧密码，请不要点击邮件中的链接。

如果您的系统邮件未通过 SPF 检查，并且您有权访问 DNS 记录且邮件服务器的域名属于您，则可以设置 SPF 凭据。检查系统发送邮件的返回路径。如果其中列出的邮件服务器包含您的域名，您就可以设置 SPF 凭据。互联网上有一些相关教程。

**DKIM:**（域名密钥识别邮件）此系统也被使用。您可以在同一封邮件中同时使用 SPF 和 DKIM。同样，与 SPF 一样，您可以通过检查邮件头来确认接收邮件服务器是否验证了您主机的域名密钥。很可能没有提供签名密钥，这表明您的主机选择不使用此协议。与 SPF 类似，如果您可以编辑 DNS 记录且邮件服务器属于您的域名，您可以自行设置 DKIM 凭据。在互联网上搜索可以找到一些教程。

要让 WordPress 发送正确的 DKIM 密钥，请挂载 `'phpmailer_init'` 操作。系统会传递 `$phpmailer` 对象给您。设置必要的属性并返回该对象。更多信息请参阅类源代码，位于 _wp-includes/class-phpmailer.php_。

## 我在文章中使用了 Quicktag `nextpage`，为什么它不起作用？

在某些[主题](https://wordpress.org/documentation/article/worik-with-themes/)中，例如 WordPress 经典主题，您可能会看到 `<!–nextpage–>` 在主页上正常工作，但其他[主题](https://wordpress.org/documentation/article/worik-with-themes/)，例如 WordPress 默认主题，可能只在单独查看文章时显示*分页符*。您可能需要修改主题的[模板](https://codex.wordpress.org/Templates)文件 *page.php* 或 *index.php*，以使此功能按您的意愿工作。您需要添加以下代码：

```
<?php wp_link_pages(); ?>
```

## MySQL 错误 28

可能原因：

* `/tmp` 目录（无论 tmpdir 指向何处）空间不足，或
* `/tmp` 目录中文件过多（即使有大量可用空间）

这是 MySQL 错误，与 WordPress 无直接关联；您应联系主机服务商。部分用户反馈在 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/) 中执行“修复表”命令可解决此问题。

## 为什么引号被转义或未被转义？

如果你编写插件或制作高级自定义模板，最终可能会发现自己需要处理数据库中的数据。WordPress *通常*会为你管理这些数据，使其立即可用。但在某些情况下（尤其是如果你在不使用 WordPress 的情况下直接操作数据库），你可能会遇到奇怪的现象。

例如，引号不能直接存储在 MySQL 数据库中。MySQL 在其 SQL 语言中使用引号。当引号被使用时（例如在文章中），文章保存到数据库时，每个引号都会被转义。这意味着会在前面添加一个反斜杠字符，表示下一个字符应被视为输入的一部分，而不是 SQL 命令的一部分。

例如，如果你在文章中添加以下内容：

```
...an article about "Happiness" is at
<a href="https://example.com/happy" title="Happiness">Happiness</a>
if you would like to read it...
```

实际上导入数据库后会变成这样：

```
...an article about \"Happiness\" is at
<a href=\"https://example.com/happy\" title=\"Happiness\">Happiness</a>
if you would like to read it...
```

从数据库中提取数据时，反斜杠可能并不总是自动移除。如果这成为问题，你可以对文本使用 PHP 函数 [stripslashes()](https://www.php.net/manual/en/function.stripslashes.php)。

**描述：** 当任何人尝试评论文章时，窗口变为空白，并且评论似乎未被 WordPress 识别。

**原因和解决方案：** 你使用的主题缺少评论表单的关键部分，因此 WordPress 不知道评论指的是哪篇文章。你需要检查主题中的 comment.php 文件，并确保表单内包含以下代码。

```
<input type="hidden" name="comment_post_ID" value="<?php echo $id; ?>" />
```

相关讨论主题：

* [无法评论](https://wordpress.org/support/topic/cant-comment-3/)

有时可能需要停用所有插件，但你无法访问管理菜单来执行此操作。有两种方法可用于停用所有插件。

使用 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/) 停用所有插件。

1. 在表 wp_options 中，在 `option_name` 列（字段）下找到 `active_plugins` 行
2. 将 `option_value` 字段更改为：`a:0:{}`

或者通过 [FTP](https://developer.wordpress.org/advanced-administration/upgrade/ftp/) 或主机控制面板提供的文件管理器重置插件文件夹。此方法会保留插件选项，但需要手动重新激活插件。

1. 通过 FTP 或主机文件管理器，导航到 `wp-contents` 文件夹（目录）。
2. 通过 FTP 或主机文件管理器，将 `plugins` 文件夹重命名为 `plugins.hold`。
3. 登录 WordPress 管理插件页面 (`/wp-admin/plugins.php`) —— 这将禁用所有“缺失”的插件。
4. 通过 FTP 或主机文件管理器，将 `plugins.hold` 重命名回 `plugins`。

## 如何清除自动升级后显示的“计划维护暂时不可用”消息？

作为自动升级的一部分，WordPress 会在博客的**根**文件夹（包含 `wp-admin` 文件夹的目录）中创建一个名为 `.maintenance` 的文件。如果该文件存在，访问者将看到消息**计划维护暂时不可用。请稍后再试。**

要停止向访问者显示此消息，只需删除 `.maintenance` 文件。建议重新执行自动升级，以防升级失败。

请注意，核心自动升级功能是在 [2.7 版本](https://wordpress.org/documentation/wordpress-version/version-2-7/)中引入的。

## 如何修复使用固定链接时出现的 404 错误？

如果在使用 [固定链接](https://wordpress.org/documentation/article/introduction-to-blogging/#pretty-permalinks) 选项（例如 **日期和名称型**）时出现 404 错误，该选项位于 [管理后台](https://wordpress.org/documentation/article/administration-screens/) > [设置](https://wordpress.org/documentation/article/administration-screens/#permalinks) > [固定链接设置](https://wordpress.org/documentation/article/settings-permalinks-screen/)，这可能是由于 **mod_rewrite** 模块未激活/安装所致。解决方案是为 Apache 网页服务器激活 **mod_rewrite**。检查 `apache/conf/httpd.conf` 文件中的 `# LoadModule rewrite_module modules/mod_rewrite.so` 这一行，并删除行首的 `#`。然后停止并重新启动 Apache。**注意：** 您可能需要联系您的主机提供商来激活 **mod_rewrite**。

另请参阅：

* [使用固定链接](https://wordpress.org/documentation/article/customize-permalinks/)

相关讨论主题：

* [本地主机：固定链接问题](https://wordpress.org/support/topic/localhost-permalink-question/)

不确定此问题发生的原因，但可以尝试以下两种解决方案之一。

这通常可以解决问题：

1.  创建一个具有管理员角色的新管理员用户（例如 newadmin）
2.  以 'newadmin' 身份登录
3.  将旧的 'admin' 用户降级为订阅者角色并保存
4.  将旧的 'admin' 用户重新提升为管理员角色并保存
5.  以旧的 'admin' 身份登录

如果上述方法无效，请尝试：

1.  创建一个具有管理员角色的新管理员用户（例如 newadmin）
2.  以 'newadmin' 身份登录
3.  删除旧的 'admin' 用户，并将其所有文章分配给 'newadmin'
4.  创建一个具有管理员角色的 'admin' 用户
5.  以 'admin' 身份登录
6.  删除 'newadmin' 用户，并将其文章分配给 'admin'

## 为什么博客文章显示的作者名称不正确？

此问题通常与上一个问题的解决方案相同：
[为什么编辑文章时管理员用户未列为作者？](https://wordpress.org/documentation/article/faq-troubleshooting/#why-isnt-the-admin-user-listed-as-an-author-when-editing-posts)

## 刚刚发布了更新，为什么我的博客没有检测到可用更新？

当有更新发布时，管理界面顶部会显示发布通知，提示 **WordPress x.x.x 现已可用！请立即更新。** 但并非所有博客都会同时看到这条消息。您的博客被设定为每 12 小时检查一次更新，但检查的时间点是完全随机的。因此，如果您的博客恰好在更新发布前几分钟刚完成检查，那么您需要等到 12 小时后博客再次检查时才会看到更新提示。

如果您希望博客立即检查更新，可以删除 `wp_options` 表中名为 `update_core` 的记录。请注意，插件和主题各有独立的检查和更新周期，分别由 `wp_options` 表中的 `update_plugins` 和 `update_themes` 记录控制。

相关讨论帖：

* ["2.7.1 请立即更新" 未显示](https://wordpress.org/support/topic/271-please-update-now-not-showing-up-1/)

## 为什么在上次自动升级中，我对 WordPress 默认主题的自定义修改丢失了？

核心升级会将发行版中的所有新文件复制并覆盖旧文件，因此如果你修改了 WordPress 默认主题中的现有文件（例如 `wp-content/themes/twentysixteen/style.css`），这些修改会被该文件的新版本覆盖。

请注意，核心升级会遍历 `wp-admin/includes/update-core.php` 中定义的“旧文件”列表，并删除这些文件。任何不在列表中且不在发行版中的文件将被保留。

**请记住，在升级之前，无论是自动还是手动升级，都应按照 [WordPress 备份](https://developer.wordpress.org/advanced-administration/security/backup/) 中的说明备份 WordPress 文件和数据库。**

修改默认主题的更好方法是使用[子主题](https://developer.wordpress.org/themes/advanced-topics/child-themes/)。设置起来需要稍多一些工作，但值得付出努力，因为当主主题更新时，你的自定义修改将是安全的。

另请参阅：

* [WordPress 备份](https://developer.wordpress.org/advanced-administration/security/backup/)
* [子主题](https://developer.wordpress.org/themes/advanced-topics/child-themes/)

## 如何修复 MySQL 数据库表？

有时可能需要修复一个或多个 MySQL 数据库表。根据 [dev.mysql.com 上的《如何修复 MyISAM 表》](https://dev.mysql.com/doc/refman/8.0/en/myisam-repair.html)，修复表的原因有多种，包括诸如 _"tbl_name.frm 被锁定无法更改"_、_"找不到文件 tbl_name.MYI (错误代码: nnn)"_、_"意外的文件结尾"_、_"记录文件已损坏"_ 或 _"从表处理器收到错误 nnn"_ 等错误。

以下是使用 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/) 修复 MySQL 数据库中表的步骤：

1.  登录托管账户。
2.  登录 [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/)。
3.  选择受影响的数据库。如果只有一个数据库，默认会选中，无需操作。
4.  在主面板中，会看到数据库表列表。勾选需要修复的表旁边的复选框。
5.  在窗口底部、表列表下方，有一个下拉菜单。选择“修复表”。

请记住，建议始终保留数据库的最新备份。

另请参阅：

*   [WordPress 备份](https://developer.wordpress.org/advanced-administration/security/backup/)