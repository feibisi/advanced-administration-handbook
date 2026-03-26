---
title: "文件编辑器界面"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Plugins
    - Repos
    - Data
---

# 文件编辑器界面

在标准 WordPress 安装中，众多用户可编辑文件包括[插件](https://wordpress.org/plugins/)文件。虽然很少需要修改插件代码，但**插件文件编辑器界面**允许您编辑这些插件文件。

根据您使用的主题，可以在以下位置找到此编辑器：

*   如果使用[区块主题](https://wordpress.org/documentation/article/block-themes/)，此编辑器将列在"工具"菜单下。
*   如果使用经典主题，此编辑器将列在"外观"菜单下。

![编辑插件界面](https://raw.githubusercontent.com/WordPress/Advanced-administration-handbook/main/assets/edit-plugins.png)

![编辑插件警告](https://raw.githubusercontent.com/WordPress/Advanced-administration-handbook/main/assets/edit-plugins-warning.png)

## 编辑插件

内置的插件文件编辑器允许您在占据屏幕主要区域的大文本（编辑）框中查看或修改任何插件的 PHP 代码。

如果特定文件可写入，您可以在此进行更改并保存文件。否则，您将看到提示信息**您需要将此文件设为可写才能保存更改**。

正在编辑的插件文件名显示在文本框顶部。由于插件文件是纯文本格式，无法在文本框中插入图像或图片。

您可以通过右上角的下拉菜单选择要编辑的插件。只需找到插件名称并点击"选择"。

### 插件文件

插件选择菜单下方列出了可编辑的插件文件列表。点击任意文件链接，即可将该文件的文本内容载入文本框。

编辑已激活的插件文件时请务必谨慎。编辑器不会创建备份副本。如果因错误导致网站崩溃，您将无法使用编辑器修复问题。这种情况下，请使用 FTP 上传问题文件的功能性备份，或更改 _/wp-content/_ 目录下 _/plugins/_ 文件夹的名称（这将使所有插件失效，直到文件夹被正确重命名）。

### 文档查询

编辑器下方有一个下拉菜单，列出了正在编辑的插件文件中包含的函数名称。选择函数并点击"查询"按钮，即可在[代码参考](https://developer.wordpress.org/reference/)或[PHP 文档](https://www.php.net/)中查看其文档。

## 更新文件

请记住点击此按钮以保存对插件文件所做的更改。点击后，您将在屏幕顶部看到提示消息"文件编辑成功"。如果未看到该消息，则说明更改未保存！请注意，如果文件不可写，更新文件按钮将不可用。