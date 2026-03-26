---
title: "编辑文件"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Wordpress
    - Repos
    - Data
---

# 编辑文件

有时您需要编辑 WordPress 文件，特别是当您想更改 [WordPress 主题](https://wordpress.org/documentation/article/worik-with-themes/)时。WordPress 提供了一个[内置编辑器](https://developer.wordpress.org/advanced-administration/wordpress/edit-files/#using-the-theme-file-editor-and-plugin-file-editor)，允许您使用任何网络浏览器在线编辑文件。您也可以编辑复制或存储在计算机上的文件，然后使用 [FTP 客户端](https://developer.wordpress.org/advanced-administration/upgrade/ftp/)将其上传到您的网站。

在编辑任何 WordPress 文件之前，请务必执行以下操作：

* 尽可能使用备份文件的副本进行工作，并确保您频繁地[备份信息](https://developer.wordpress.org/advanced-administration/security/backup/)——在工作期间以及每次进行更改时。请记住将备份保存在安全的地方！
* 在线工作时，您需要设置适当的[文件权限](https://developer.wordpress.org/advanced-administration/server/file-permissions/)，以便修改和保存文件。如果您在 WordPress 编辑器面板底部看到一条提示，写着 *如果此文件可写，您可以编辑它…*，这意味着您需要先更改文件权限才能进行任何修改。
* 在内置插件和主题编辑器之外修改文件时，请使用[文本编辑器](https://wordpress.org/documentation/article/wordpress-glossary/#text-editor)。**强烈建议不要使用文字处理程序**。文字处理程序会将引号转换为特殊字符，有时会转换特定字符，并且还可能添加不需要的代码。这些更改可能导致文件损坏。出于类似原因，也不建议使用某些 HTML 生成器程序。

## 使用主题文件编辑器和插件文件编辑器

WordPress 内置了两个编辑器，允许您直接从浏览器编辑主题文件。它们被称为**主题文件编辑器**和**插件文件编辑器**。

请注意，根据您拥有的用户权限级别，您可能无法在博客管理面板中访问这些功能。请联系您的博客或网站管理员以调整您的权限。

### 在哪里可以找到这些编辑器？

根据您使用的主题类型，可以在以下位置找到这些编辑器：

* 如果您使用的是[区块主题](https://wordpress.org/documentation/article/block-themes/)，主题文件编辑器和插件文件编辑器都会列在“工具”菜单下。
* 如果您使用的是经典主题，主题文件编辑器会列在“外观”菜单下，而插件文件编辑器会列在“插件”菜单下。

您可以通过右侧边栏导航访问任一编辑器来查看文件。

有关编辑主题的更多信息，请参阅[主题开发者手册](https://developer.wordpress.org/themes/)。

请注意，如果您编辑的主题被更新，您的更改将被覆盖。为了更好地组织您的更改并防止其被更新覆盖，建议创建一个[子主题](https://developer.wordpress.org/themes/advanced-topics/child-themes/)来保存所有修改。

### 哪些文件可以被编辑？

以下文件类型（如果[可写](https://developer.wordpress.org/advanced-administration/server/file-permissions/)）可以在 WordPress 管理面板内置的插件编辑器中编辑：

* [HTML](https://wordpress.org/documentation/article/glossary#html)
* [PHP](https://wordpress.org/documentation/article/glossary#php)
* [CSS](https://wordpress.org/documentation/article/glossary#css)
* TXT（以及相关的类文本文件，如 RTF）

在主题编辑器中，只能编辑可写的 PHP 和 CSS 文件。

### 须知事项

#### 即时变更

使用 WordPress 编辑器对文件所做的更改是即时生效的。这些变更在线实时发生，您和网站访客将立即看到变更效果。

鉴于变更的即时性，通常更安全的做法是：离线编辑文件副本，测试副本文件，待验证无误后再上传更改。

编辑文件前，请务必确保您拥有最新的备份。

#### 编辑器功能

内置的 WordPress 插件和主题文件编辑器非常基础，允许您轻松查看和编辑网站上的插件和主题文件。请注意，这些编辑器不具备高级功能，例如搜索替换、行号显示、语法高亮或代码补全。

提示：您可以使用浏览器的内置搜索栏，在可视化编辑器中查找代码。

#### 文件权限

要使用内置的 WordPress 插件和主题编辑器编辑文件，该文件的权限必须设置为可写（至少 604）。您可以通过使用 [FTP 客户端程序](https://developer.wordpress.org/advanced-administration/upgrade/ftp/)、主机提供的基于 Web 的文件管理器，或通过 SSH（安全外壳）从[命令行](https://wordpress.org/documentation/article/glossary#shell)来[更改文件权限](https://developer.wordpress.org/advanced-administration/server/file-permissions/)。具体选项取决于您的主机提供的访问类型。

#### 操作失误？使用备份文件

编辑前请备份所有文件。如果操作失误导致错误、网站崩溃、出现空白屏幕或无法访问 WordPress 仪表盘，请删除已更改的文件，并用备份中的完好副本替换。

没有备份？从原始来源下载你编辑过文件的新副本，替换后重新开始。务必先备份。

#### 安全警告

默认情况下，任何拥有管理员权限的用户都可以访问 WordPress 插件和主题编辑器，并实时更改您网站上的任何主题或插件文件。

为防止意外操作、错误甚至黑客攻击，您可能希望禁用 WordPress 主题内编辑文件的功能。为此，只需将 [DISALLOW_FILE_EDIT](https://wordpress.org/documentation/article/editing-wp-config-php/#disable-the-plugin-and-theme-editor) 函数添加到您的 `wp-config.php` 文件中即可。

## 离线编辑文件

要离线编辑文件，你可以使用任何[推荐的文本编辑器](https://developer.wordpress.org/advanced-administration/wordpress/edit-files/#text-editors)来创建和编辑文件，并使用[FTP客户端](https://developer.wordpress.org/advanced-administration/upgrade/ftp/)上传它们。确保在浏览器中查看结果，以确认是否达到了预期的更改效果。

**注意**：不建议修改除[wp-config.php](https://wordpress.org/documentation/article/editing-wp-config-php/)之外的WordPress核心文件。如果必须修改其他文件，请记录你的更改，并将这些记录的副本存储在WordPress根目录的文本文件中。你还应该备份WordPress核心文件，以供将来参考和升级使用。

## 使用文本编辑器

### 应避免的编辑器

应避免的编辑器包括任何自助式即时网页制作软件（如 Adobe Dreamweaver）或文字处理软件（如 Google Docs 或 Microsoft Word）。

**注意：** 如果使用外部编辑器（如文字处理器）来创建和编辑文件，可能会损坏正在编辑的文件。请参阅术语表中的[文本编辑器](https://wordpress.org/documentation/article/wordpress-glossary/#text-editor)条目，了解为何应避免使用这些编辑器的简要说明。

### 文本编辑器

以下[文本编辑器](https://wordpress.org/documentation/article/wordpress-glossary/#text-editor)适用于文件编辑：

* [BBEdit](https://www.barebones.com/products/bbedit/) (Mac, 免费)
* [Crimson Editor](http://www.crimsoneditor.com/) (Windows, 免费)
* [CodeLobster](https://codelobster.com/) (Mac, Linux, Windows, 免费)
* [EditPad](https://www.editpadpro.com/) (Windows)
* [EditPlus](https://www.editplus.com/) (Windows)
* [emacs](https://www.gnu.org/software/emacs/emacs.html) (Mac, Linux, Windows) 开源, 免费
* [JEdit](https://jedit.org/) (Mac, Linux, Windows)
* [Notepad++](https://notepad-plus-plus.org/) (Windows) 开源, 免费
* [PSPad](https://www.pspad.com/) (Windows) 免费
* [Smultron](https://www.peterborgapps.com/smultron/) (Mac) $
* [SubEthaEdit](https://apps.apple.com/us/app/subethaedit/id728530824) (Mac) 开源, 免费
* [Sublime Text](https://www.sublimetext.com/) (Mac, Linux, Windows) $
* [TextMate](https://macromates.com/) (Mac) $
* [TextPad](https://www.textpad.com/home) (Windows)
* [UltraEdit-32](https://www.ultraedit.com/) (Mac, Linux, Windows) $
* [vim](https://www.vim.org/) (Mac, Linux, Windows) 开源, 免费
* [Visual Studio Code](https://code.visualstudio.com/) (Mac, Linux, Windows)
* [NetBeans](https://netbeans.apache.org/) (Mac, Linux, Windows)