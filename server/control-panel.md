---
title: "控制面板"
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

# 控制面板

## 适用于 cPanel & WHM 和 Plesk 的 WP Toolkit

WP Toolkit 是一个统一的管理界面，可让您轻松安装、配置和管理 WordPress®。WP Toolkit 能够安装、配置和管理 4.9 或更高版本的 WordPress。

cPanel & WHM 102 及更高版本默认安装 WP Toolkit。如果服务器管理员已安装 WP Toolkit 扩展，则 Plesk 中也可使用 WP Toolkit。

## cPanel & WHM

本教程介绍如何使用 WP Toolkit 在 cPanel & WHM 控制面板中安装 WordPress。如需了解在 cPanel & WHM 中安装 WordPress 的其他方法，请阅读 cPanel 的 [How to Install WordPress with cPanel](https://docs.cpanel.net/knowledge-base/third-party/how-to-install-wordpress-with-cpanel/) 文档。

要通过 WP Toolkit 安装 WordPress，请执行以下操作：

1.  使用托管公司提供的信息登录 cPanel 界面。
2.  从左侧菜单栏中选择 _WP Toolkit_。_WP Toolkit_ 界面将出现。
3.  点击 _Install WordPress_ 以创建新的 WordPress 安装。

### 安装 WordPress

在 _安装 WordPress_ 界面中，点击 _安装_ 以使用默认设置。或者，提供以下可选信息以自定义安装，然后点击 _安装_：

   * 安装目录。默认情况下，安装使用账户的 `public_html` 目录。
   * 网站标题。
   * 插件或主题集。
   * 网站语言。
   * 要安装的 WordPress 版本。默认情况下，安装使用当前的 WordPress 版本。
   * WordPress 管理员用户名和密码。
   * 数据库名称、表前缀、用户名和密码。
   * WordPress 软件、插件和主题的自动更新设置。

如果您想安装插件和主题，请点击 _安装插件_，或者点击 _不，谢谢_ 稍后选择性地安装它们。

## Plesk

本教程提供使用 Plesk Web Installer 安装 Plesk WP Toolkit 的逐步示例。

### 使用 Web Installer

要使用 Web Installer 安装 Plesk，请打开浏览器并访问 [get.plesk.com](https://get.plesk.com/)。安装 Plesk 需要一台能访问互联网的全新 Linux 服务器。您可以在任何受支持的基于 Linux 的操作系统上安装 Plesk。

![Image_1](https://user-images.githubusercontent.com/19301688/189542599-4fce4d63-8060-416e-9fdf-f21ae62c87e1.png)

提供您服务器的 IP 地址或主机名，输入 root 密码，或直接添加私钥。

* 接受最终用户许可协议的条款，然后单击安装按钮。
* 放松并等待一段时间，让安装完成。
* 点击登录链接。无需担心“安全连接警告”，只需添加例外即可。

### 安装 WordPress

![image_2](https://user-images.githubusercontent.com/19301688/189542665-78f52a1c-e92b-4d70-bb5d-899ac02cc57e.png)

* 如需快速安装，请点击“安装（快速）”。这将安装最新版本的 WordPress 并使用默认设置。如果为该域名启用了 SSL/TLS 支持，新实例将通过 HTTPS 访问。
* 如需更改默认安装设置，请点击“安装（自定义）”。这允许您设置管理员用户、选择所需的 WordPress 版本、指定数据库名称、选择自动更新设置等。

### 管理 WordPress 实例

前往 WordPress 查看所有 WordPress 实例。WP Toolkit 将每个实例的信息分组在我们称为卡片的区块中。

![image_3](https://user-images.githubusercontent.com/19301688/189542692-5d6f38b5-1b32-4de8-8f40-2abe9a5d1d86.png)

卡片显示您网站的截图，并提供多个控件，方便您访问常用工具。截图会实时更新，反映您对网站所做的更改。例如，如果您开启维护模式或更换 WordPress 主题，网站截图将立即更新。

### 工具

在“工具”部分，点击可访问以下 WP Toolkit 功能：

![image_4](https://user-images.githubusercontent.com/19301688/189542713-abf476de-fcbd-4113-9975-1c2961765190.png)

* “同步”可将您的网站内容与另一个网站同步。
* “克隆”可创建网站的完整副本。
* “管理文件”可在文件管理器中管理网站文件。
* “备份/恢复”可为网站创建备份并在必要时进行恢复。

下方的控件让您能便捷访问以下设置和工具：

* “搜索引擎索引”使您的网站显示在搜索引擎的搜索结果中。
* “缓存 (nginx)”可加快网站加载速度并降低服务器负载。
* “调试”可帮助您调试尚未准备好供查看、测试或开发的网站。
* “维护模式”可对访客隐藏您的网站内容。
* “密码保护”指定您将从 Plesk 登录 WordPress 时使用的密码。