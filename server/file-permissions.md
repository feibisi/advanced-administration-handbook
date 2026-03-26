---
title: "更改文件权限"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Server
    - Repos
    - Data
---

# 更改文件权限

在计算机文件系统中，不同的文件和目录拥有**权限**设置，用于指定谁以及什么可以读取、写入、修改和访问它们。这很重要，因为 WordPress 可能需要访问并写入 `wp-content` 目录中的文件以实现某些功能。

## 简要说明

Linux [文件权限](https://en.wikipedia.org/wiki/File_system_permissions) 主要由三个部分组成——文件或文件夹所有者拥有的权限、拥有该文件或文件夹的组成员拥有的权限，以及其他人访问或修改该文件和文件夹的权限。这三个权限部分通常用三个数字表示，依次是所有者权限级别、组权限级别和所有人的权限级别。_从技术上讲还有第四个部分，但这超出了保护 WordPress 所需了解的范围，此处不予讨论。_

对于用户、组和其他人，各有三种访问权限：读取权限、写入权限和执行权限。读取权限允许您读取文件或目录的内容。写入权限允许您修改文件或目录。执行权限允许您像运行程序或脚本一样运行文件。

## 权限模式

```
 7      5      5
用户   组     其他用户
r+w+x  r+x    r+x
4+2+1  4+0+1  4+0+1  = 755
```

权限模式的计算方法是将用户、文件组和其他所有用户的以下数值相加。图示展示了计算过程。

* **读** 4 – 允许读取文件
* **写** 2 – 允许写入/修改文件
* 执**行** 1 – 读取/写入/删除/修改/目录

```
 7      4      4
用户   组     其他用户
r+w+x  r      r
4+2+1  4+0+0  4+0+0  = 744
```

### Example Permission Modes

| Mode | Str Perms | Explanation |
| ---- | --------- | ----------- |
| **0677** | -rw-rwxrwx | owner has rw only(6), other and group has rwx (7) |
| **0670** | -rw-rwx— | owner has rw only, group has rwx, others have no permission |
| **0666** | -rw-rw-rw- | all have rw only (6) |
| **0607** | -rw—-rwx | owner has rw only, group has no permission and others have rwx |
| **0600** | -rw——- | owner has rw only, group and others have no permission |
| **0477** | -r–rwxrwx | owner has read only (4), other and group has rwx (7) |
| **0470** | -r–rwx— | owner has read only, group has rwx, others have no permission |
| **0407** | -r—–rwx | owner has read only, other has rwx, group has no permission |
| **0444** | -r–r–r– | all have read only (4) |
| **0400** | -r——– | owner has read only(4), group and others have no permission(0) |

## WordPress 权限方案

不同主机的权限设置各不相同，因此本指南仅详述通用原则，无法涵盖所有情况。本指南适用于运行标准配置的服务器（注意：对于使用 "suexec" 方法的共享主机，请参阅下文）。

通常，所有文件应归您 Web 服务器上的用户（FTP）账户所有，并可由该账户写入。在共享主机上，文件绝不应由 Web 服务器进程本身所有（有时是 **www**、**apache** 或 **nobody** 用户）。

任何需要 WordPress 写入访问的文件，应归 WordPress 使用的用户账户所有或组所有（可能与服务器账户不同）。例如，您可能有一个允许您通过 FTP 上传下载文件的用户账户，但您的服务器本身可能使用单独的用户（在单独的用户组中）运行，例如 **dhapache** 或 **nobody**。如果 WordPress 以 FTP 账户身份运行，则该账户需要具有写入权限，即成为文件的所有者，或属于具有写入权限的组。在后一种情况下，这意味着权限设置比默认更宽松（例如，文件夹为 775 而非 755，文件为 664 而非 644）。

对于大多数用户，WordPress 的文件和文件夹权限应相同，具体取决于您执行的安装类型以及安装时系统环境的 umask 设置。

**注意：** 如果由经验丰富的用户为您安装 WordPress，您可能无需修改文件权限。除非您遇到权限错误问题，或者您*想要*修改，否则您可能不应改动此设置。

**注意：** 如果您自行安装 WordPress，您很可能*需要*修改文件权限。某些文件和目录应通过更严格的权限进行"加固"，特别是 wp-config.php 文件。该文件初始创建时权限为 644，保持此状态存在风险。请参阅安全与加固。

通常，所有核心 WordPress 文件应仅可由您的用户账户（或 httpd 账户，如果不同）写入。（但有时，多个 FTP 账户用于管理安装，如果所有 FTP 用户已知且可信，即非共享主机，则分配组写入权限可能是合适的。请咨询您的服务器管理员以获取更多信息。）但是，如果您使用 mod_rewrite 固定链接或其他 `.htaccess` 功能，应确保 WordPress 也能写入您的 `/.htaccess` 文件。

If you want to use the built-in theme editor, all files need to be group writable. Try using it before modifying file permissions, it should work. (This may be true if different users uploaded the WordPress package and the Plugin or Theme. This wouldn't be a problem for Plugin and Themes installed via the admin. When uploading files with different ftp users group writable is needed. On shared hosting, make sure the group is exclusive to users you trust... the apache user shouldn't be in the group and shouldn't own files.)

Some plugins require the `/wp-content/` folder be made writeable, but in such cases they will let you know during installation. In some cases, this may require assigning 755 permissions. The same is true for `/wp-content/cache/` and maybe `/wp-content/uploads/` (if you're using [MultiSite](https://developer.wordpress.org/advanced-administration/multisite/create-network/) you may also need to do this for `/wp-content/blogs.dir/`)

Additional directories under /wp-content/ should be documented by whatever plugin / theme requires them. Permissions will vary.

```
|
|- index.php
|- wp-admin
|   |- wp-admin.css
|- wp-blog-header.php
|- wp-comments-post.php
|- wp-commentsrss2.php
|- wp-config.php
|- wp-content
|   |- cache
|   |- plugins
|   |- themes
|   |- uploads
|- wp-cron.php
|- wp-includes
|- xmlrpc.php
```

### 使用 suexec 的共享主机

上述内容可能不适用于采用 "suexec" 方式运行 PHP 二进制文件的共享主机系统。这是许多网络主机常用的方法。在此类系统中，PHP 进程以 PHP 文件所有者的身份运行，从而为共享主机这一特定场景提供了更简单的配置和更安全的环境。

注意：**切勿**在单站点服务器配置中使用 suexec 方法，它们**仅**在共享主机的特定场景下才更安全。

在这种 suexec 配置中，正确的权限方案很容易理解。

* 所有文件应归实际用户账户所有，而非用于 httpd 进程的用户账户。
* 组所有权无关紧要，除非 Web 服务器进程权限检查有特定的组要求。通常情况并非如此。
* 所有目录应为 755 或 750。
* 所有文件应为 644 或 640。例外：wp-config.php 应为 440 或 400，以防止服务器上的其他用户读取。
* 任何目录都不应设置为 777，即使是上传目录。由于 PHP 进程以文件所有者的身份运行，它拥有所有者的权限，即使对 755 目录也能写入。

在这种特定类型的设置中，WordPress 会检测到它可以直接创建具有正确所有权的文件，因此在升级或安装插件时不会要求提供 FTP 凭据。

系统管理员用于此设置的常用方法包括：

* [suPHP](https://smarsching.github.io/suphp/Home.html)，通过 php-cgi 运行，自 2013 年起已停止维护。
* [mod_ruid2](https://github.com/mind04/mod-ruid2)，Apache 模块，自 2013 年起已停止维护。
* [mpm-itk](http://mpm-itk.sesse.net/)，Apache 模块。
* [mod_fcgid](https://httpd.apache.org/mod_fcgid/)，一个 Apache 模块和 FastCGI 服务器，具有更广泛的配置选项。
* [PHP-FPM](https://php-fpm.org/)，一个替代的 FastCGI 服务器，具有共享 OPCode，可与 Apache 和 Nginx 配合使用。

## 使用 FTP 客户端

[FTP 程序](https://developer.wordpress.org/advanced-administration/upgrade/ftp/)（"客户端"）允许您为远程主机上的文件和目录设置权限。此功能在程序菜单中通常称为 `chmod` 或 `设置权限`。

在 [WordPress 安装](https://developer.wordpress.org/advanced-administration/before-install/howto-install/)中，您可能想要修改的两个文件是索引页和控制布局的 css 文件。以下是更改 index.php 的方法——_对于任何文件，过程都是相同的_。

在下面的截图中，查看最后一列——它显示了权限。看起来有点令人困惑，但现在只需注意字母序列。

[![](https://wordpress.org/documentation/files/2019/02/podz_filezilla_12.gif)](https://wordpress.org/documentation/files/2019/02/podz_filezilla_12.gif)
初始权限

右键单击 'index.php' 并选择 '文件权限'
将弹出一个屏幕。

[![](https://wordpress.org/documentation/files/2019/02/podz_filezilla_13.gif)](https://wordpress.org/documentation/files/2019/02/podz_filezilla_13.gif)
更改文件权限

不用担心复选框。只需删除 '数值：' 并输入您需要的数字——在本例中为 666。然后单击确定。

[![](https://wordpress.org/documentation/files/2019/02/podz_filezilla_14.gif)](https://wordpress.org/documentation/files/2019/02/podz_filezilla_14.gif)
权限已更改。

您现在可以看到文件权限已更改。

### 显示隐藏文件

默认情况下，大多数 [FTP 客户端](https://developer.wordpress.org/advanced-administration/upgrade/ftp/)（包括 [FileZilla](https://sourceforge.net/projects/filezilla/)）都不会显示以点号（.）开头的隐藏文件。但有时您可能需要查看隐藏文件，以便更改其权限。例如，您可能需要使控制 [固定链接](https://wordpress.org/documentation/article/using-permalinks/) 的 [.htaccess](https://wordpress.org/documentation/article/glossary#htaccess) 文件可写。

要在 FileZilla 中显示隐藏文件，需要从顶部菜单选择“查看”，然后选择“显示隐藏文件”。文件显示界面将刷新，之前隐藏的文件应会显示出来。

若要让 FileZilla 始终显示隐藏文件，请在“编辑”>“设置”>“远程文件列表”中勾选“始终显示隐藏文件”复选框。

在最新版本的 FileZilla 中，“显示隐藏文件”选项已移至“服务器”选项卡。请选择“强制显示隐藏文件”。

## 使用命令行

如果您拥有对托管账户的 shell/SSH 访问权限，可以使用 `chmod` 命令来更改文件权限，这是经验用户的首选方法。在开始使用 `chmod` 之前，建议先阅读一些教程，以确保您理解其功能。设置错误的权限可能导致网站离线，请务必谨慎操作。

* [Unix 权限](https://web.archive.org/web/20190715230319/http://www.washington.edu/computing/unix/permissions.html)

您可以通过两个步骤使 `wp-content` 目录中的**所有**文件可写，但在使每个文件和文件夹都可写之前，应先尝试更安全的替代方案，例如仅修改目录权限。请先依次尝试以下每条命令，如果无效再使用递归选项（这会使您的主题图像文件也可写）。请将 **DIR** 替换为您要写入的目标文件夹：

```
chmod -v 746 DIR
chmod -v 747 DIR
chmod -v 756 DIR
chmod -v 757 DIR
chmod -v 764 DIR
chmod -v 765 DIR
chmod -v 766 DIR
chmod -v 767 DIR
```

如果上述命令仍无法写入，请按相同顺序重试所有命令，但这次将 -v 替换为 -R（递归更改文件夹内的每个文件）。如果之后仍无法写入，可尝试使用 777 权限。

### [关于 Chmod](#about-chmod)

`chmod` 是一个 Unix 命令，意为对文件进行“**ch**ange **mod**e”（更改模式）。`-R` 标志表示将更改应用于 `wp-content` 内的每个文件和目录。766 是我们将目录更改到的模式，它意味着该目录对 WordPress 以及您系统上的任何其他用户都是可读和可写的。最后，我们指定要修改的目录名称 `wp-content`。如果 766 不起作用，您可以尝试 777，这会使所有文件、文件夹对所有用户、组和进程都可读、可写和可执行。

如果您使用[固定链接](https://wordpress.org/documentation/article/using-permalinks/)，您还应该更改 .htaccess 的权限，以确保当您更改设置（例如添加新页面、重定向、分类等）时，WordPress 可以更新它，因为在使用 mod_rewrite 固定链接时，需要更新 .htaccess 文件。

1.  转到 WordPress 的主目录
2.  输入 `chmod -v 666 .htaccess`

**注意：** 从安全角度来看，即使少量的保护也比全局可写的目录要好。从低权限设置（如 744）开始，逐步提高直到它正常工作。仅在必要时使用 777，并且希望只是临时使用。

## 777 权限的危险性

此权限问题的核心在于服务器配置方式。您通过 FTP 或 SSH 登录服务器时使用的用户名，很可能与服务器应用程序自身用于提供页面的用户名不同。

```
 7      7      7
用户   组     其他用户
读写执行 读写执行 读写执行
4+2+1  4+2+1  4+2+1  = 777
```

Apache 服务器通常由 **www-data**、**dhapache** 或 **nobody** 用户账户"拥有"。这些账户对服务器文件的访问权限有限，这是有充分理由的。通过将您个人账户拥有的文件和文件夹设置为全局可写，您实际上是在让所有用户都能写入。现在，运行服务器、提供页面、执行 PHP 解释器的 www-data、dhapache 和 nobody 用户将拥有对您账户文件的完全访问权限。

这为攻击者通过劫持服务器上几乎任何进程（包括机器上的其他用户）来获取文件访问权限提供了途径。因此，您应谨慎考虑修改机器权限。我从未遇到过需要超过 767 权限的情况，所以当看到 777 时，请务必询问其必要性。

### 最坏的结果

对文件夹甚至文件使用 777 权限可能导致的最坏情况是：如果恶意攻击者或实体能够上传恶意文件或修改现有文件以执行代码，他们将完全控制你的博客，包括获取你的数据库信息和密码。

### 寻找变通方案

通常可以轻松获取出色的 WordPress 插件提供的增强功能，同时避免将自己置于风险之中。联系插件作者或您的服务器支持团队，请求一个变通方案。

## 寻找安全的文件权限

.htaccess 文件是由运行服务器的进程所有者访问的文件之一。因此，如果权限设置过低，服务器将无法访问该文件并导致错误。这就是寻找最安全设置的方法：从过于严格的限制开始，逐步增加权限直到其正常工作。

### 示例权限设置

以下示例包含一个*自定义编译的 php-cgi 二进制文件*和一个位于 cgi-bin 目录中的*自定义 php.ini* 文件，用于执行 PHP 脚本。为防止解释器和 `php.ini` 文件在 Web 浏览器中被直接访问，它们通过 .htaccess 文件进行保护。

默认权限 (umask 022)

```
644 -rw-r--r--  /home/user/wp-config.php
644 -rw-r--r--  /home/user/cgi-bin/.htaccess
644 -rw-r--r--  /home/user/cgi-bin/php.ini
755 -rwxr-xr-x  /home/user/cgi-bin/php.cgi
755 -rwxr-xr-x  /home/user/cgi-bin/php5.cgi
```

安全权限

```
600 -rw-------  /home/user/wp-config.php
6**0**4 -rw----r--  /home/user/cgi-bin/.htaccess
6**00** -rw-------  /home/user/cgi-bin/php.ini
7**11** -rwx--x--x  /home/user/cgi-bin/php.cgi
**100** ---x------  /home/user/cgi-bin/php5.cgi
```

#### .htaccess 权限

**644 > 604** – 移除了允许 .htaccess 文件所属组拥有读取权限的位。通常建议 .htaccess 文件使用 644 权限。

#### php.ini 权限

**644 > 600** – 之前，所有能访问服务器的组和用户都可以访问 php.ini，甚至可以直接从网站请求它。棘手之处在于，由于 php.ini 文件仅由 php.cgi 使用，我们只需确保 php.cgi 进程有访问权限即可。php.cgi 以拥有这两个文件的同一用户身份运行，因此现在只有该用户能够访问此文件。

#### php.cgi 权限

**755 > 711** 此文件是一个已编译的 php-cgi 二进制文件，用于替代 mod_php 或托管公司提供的默认原生 php。此文件的默认权限为 755。

#### php5.cgi 权限

**755 > 100** – 由于用户账户是运行 php cgi 进程的所有者，因此其他用户或组无需访问权限，所以我们禁除了执行权限之外的所有访问。这很有趣，因为它确实有效。你可以尝试读取文件、写入文件等，但对此文件的唯一访问权限就是运行 php 脚本。作为文件所有者，你随时可以再次更改权限模式。

```
$ cat: php5.cgi: Permission denied
./php5.cgi:  Welcome
```

## SELinux

[安全增强型 Linux](https://en.wikipedia.org/wiki/Security-Enhanced_Linux) 是一个内核安全模块，它提供了一种机制，可以将进程沙盒化到特定的上下文中。这对于限制网页对操作系统其他部分所能执行的操作特别有用。被安全策略拒绝的操作通常很难与常规的文件权限错误区分开来。

selinux 通常安装在 Redhat 系列发行版上（例如 CentOS、Fedora、Scientific、Amazon 等）。

### 如何判断 selinux 是否是问题所在？

如果您使用的是基于 Debian 的发行版，通常不会有问题。

运行以下命令（在基于 rpm 的系统上）：

```
$ rpm -qa | grep selinux
selinux-policy-targeted-3.13.1-166.el7_4.7.noarch
selinux-policy-3.13.1-166.el7_4.7.noarch
libselinux-2.5-11.el7.x86_64
libselinux-python-2.5-11.el7.x86_64
libselinux-utils-2.5-11.el7.x86_64
```

要检查它是否是导致权限拒绝的原因：

```
$ getenforce
Enforcing
```

selinux 导致的一个问题是阻止 wp-admin 工具写出 URL 重写所需的 \`.htaccess\` 文件。有多个命令可以检查此行为：

```
$ audit2allow -w -a
type=AVC msg=audit(1517275570.388:55362): avc:  denied  { write } for  pid=11831 comm="httpd" path="/var/www/example.org/.htaccess" dev="vda1" ino=67137959 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:httpd_sys_content_t:s0 tclass=file
        Was caused by:
        The boolean httpd_unified was set incorrectly.
        Description:
        Allow httpd to unified

        Allow access by executing:
        # setsebool -P httpd_unified 1
```
以及

```
$ ausearch -m avc -c httpd
----
time->Tue Jan 30 01:30:31 2018
type=PROCTITLE msg=audit(1517275831.762:55364): proctitle=2F7573722F7362696E2F6874747064002D44464F524547524F554E44
type=SYSCALL msg=audit(1517275831.762:55364): arch=c000003e syscall=21 success=no exit=-13 a0=55b9c795d268 a1=2 a2=0 a3=1 items=0 ppid=11826 pid=11829 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48 egid=48 sgid=48 fsgid=48 tty=(none) ses=4294967295 comm="httpd" exe="/usr/sbin/httpd" subj=system_u:system_r:httpd_t:s0 key=(null)
type=AVC msg=audit(1517275831.762:55364): avc:  denied  { write } for  pid=11829 comm="httpd" name="bioactivator.org" dev="vda1" ino=67137958 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:httpd_sys_content_t:s0 tclass=dir
----
```
  
您可以临时禁用 selinux 以确定它是否是问题的根源：

```
$ setenforce
usage:  setenforce \[ Enforcing | Permissive | 1 | 0 \]
```