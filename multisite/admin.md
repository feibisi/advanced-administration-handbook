---
title: "网络管理员"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Multisite
    - Repos
    - Data
---

# 网络管理员

## 网络管理员

**网络管理员界面**是管理 WordPress [多站点（或网络）](https://wordpress.org/documentation/article/glossary#multisite)功能所需各项设置的中心入口。以下信息专门面向网络管理员。其他用户应查阅[管理界面](https://wordpress.org/documentation/article/administration-screens/)了解 WordPress 使用信息。

网络管理员链接仅在您[创建网络](https://developer.wordpress.org/advanced-administration/multisite/create-network/)后可见。该菜单将出现在菜单栏右上角，且仅对超级管理员可见。它已被移至管理区域内的独立位置，只要您以超级管理员身份登录，即可从任何站点的管理区域访问。

访问网络管理员界面时，您将看到仪表盘界面。其外观与站点仪表盘相似，但增加了一个额外的小工具，并移除了站点特定的小工具。"当前概况"小工具提供快速链接至"创建站点"和"创建用户"界面，同时包含可快速查找站点和用户的搜索框。

每个界面均通过主导航菜单访问，如下方框所示。这些框中的链接将引导您至本文描述对应界面的章节。通过这些章节，您可以导航至详细说明每个界面的文章。

### 仪表盘 {#dashboard}

仪表盘是信息中心，展示您的网络站点信息，提供 WordPress 社区动态，允许管理插件，并显示其他 WordPress 相关资讯。

### 站点 {#sites}

使用[网络管理站点屏幕](https://developer.wordpress.org/advanced-administration/multisite/admin/#network-admin-sites-screen)来查看和管理网络中各个站点。这些站点根据网络配置方式，可能是子目录站点或子域名站点。在此屏幕中，您可以访问网络中每个站点的信息、用户、主题和设置。

使用[添加新站点屏幕](https://developer.wordpress.org/advanced-administration/multisite/admin/#add-site)向网络添加新站点。

### 用户 {#users}

[网络管理员用户界面](https://codex.wordpress.org/Network_Admin_Users_Screen)是网络管理员人员管理用户的地方，而[添加新用户界面](https://codex.wordpress.org/Network_Admin_Users_Screen#Add_User)则用于添加新用户。

### 主题 {#themes}

[网络管理员主题屏幕](https://codex.wordpress.org/Network_Admin_Themes_Screen) 允许您控制站点管理员可以为每个站点使用哪些主题。它不会激活或停用站点当前正在使用的主题。如果网络管理员禁用了正在使用的主题，该主题仍可在该站点保持选中状态。如果选择了另一个主题，被禁用的主题将不会出现在站点的外观 > 主题屏幕中。网络管理员可以在站点编辑屏幕（通过站点屏幕上的编辑操作链接进入）上逐个站点启用主题。

要添加新主题，请参阅[添加新主题](https://codex.wordpress.org/Network_Admin_Themes_Screen#Add_New_Theme)以了解为您的网络查找和安装新主题的流程。

使用主题编辑器来编辑构成您主题的各种文件。[主题编辑器屏幕](https://codex.wordpress.org/Network_Admin_Themes_Screen#Theme_Editor)允许您指定要编辑的主题，然后显示该主题中的文件。主题中的每个文件（模板和CSS）都可以在大文本框中编辑。

### 插件 {#plugins}

[网络管理插件界面](https://codex.wordpress.org/Network_Admin_Plugins_Screen) 允许您为 WordPress 网络添加默认安装中未包含的新功能。WordPress 拥有丰富多样的可用插件，插件的安装和管理非常简便。

请参阅 [添加新插件](https://codex.wordpress.org/Network_Admin_Plugins_Screen#Add_New_Plugins) 来添加新插件。有关下载和安装插件的信息，请参见 [管理插件](https://wordpress.org/documentation/article/manage-plugins/)。

使用 [插件编辑器](https://codex.wordpress.org/Network_Admin_Plugins_Screen#Plugin_Editor)，您可以修改所有插件的源代码。

### 设置 {#settings}

[网络管理员设置界面](https://wordpress.org/documentation/article/network-admin-settings-screen/)是网络管理员为整个网络设置和修改配置的地方。网络中的第一个站点是主站点，网络设置是从该原始站点的选项中获取的。

此外，可以访问在[创建网络](https://developer.wordpress.org/advanced-administration/multisite/create-network/)时使用的[网络设置](https://wordpress.org/documentation/article/network-admin-settings-screen/)信息。

### 更新 {#updates}

[网络管理员更新屏幕](https://developer.wordpress.org/advanced-administration/multisite/admin/)控制网络和站点的更新流程。在[可用更新屏幕](https://developer.wordpress.org/advanced-administration/multisite/admin/#available-updates)中，您可以更新 WordPress 核心、主题和插件。在您更新到最新版本的 WordPress 后，可以从[升级网络屏幕](https://wordpress.org/documentation/article/network-admin-updates-screen/#upgrade-network)升级网络上的所有站点。

## 网络管理站点界面

**网络管理站点界面** 允许您在[网络](https://wordpress.org/documentation/article/glossary#network)中添加新站点并管理现有站点。

[![](https://i1.wp.com/wordpress.org/support/files/2019/02/superadmin-sites.png?fit=1277%2C443&ssl=1)](https://wordpress.org/documentation/superadmin-sites-2/)
_超级管理员站点_

### 站点 {#sites}

列出此网络上的所有站点。
- **编辑**：点击此链接进入编辑站点界面，以查看/编辑站点设置并添加用户。
- **仪表盘**：将管理界面切换至该站点的仪表盘。
- **停用 / 激活**：停用或激活站点。
- **归档**：归档站点（效果等同于停用）。
- **标记为垃圾**：将站点标记为垃圾站点。这将使其对所有人不可用。
- **删除**：删除站点。
- **访问**：前往该网站。

#### 添加站点 {#add-site}

填写各项并点击 _添加站点_ 按钮，将新站点加入您的网络。

- **站点地址**：仅允许使用字符 a-z 和 0-9。  
- **站点标题**：站点的名称。  
- **管理员邮箱**：新站点管理员的电子邮件地址。如果上述邮箱地址不在数据库中，将创建一个新用户。  
用户名和密码将发送到此邮箱地址。

#### 编辑站点 {#edit-site}

编辑站点界面分为多个标签页以便于数据管理。强烈建议除非您确定知道自己在做什么，否则不要编辑这些字段。

**基本信息**

此部分数据为站点的基本信息。包括域名、注册日期、最后更新时间，以及是否公开或包含成人内容。

[![](https://i2.wp.com/wordpress.org/support/files/2019/02/superadmin-sites-edit.png?fit=1277%2C633&ssl=1)](https://wordpress.org/documentation/superadmin-sites-edit/)
_编辑站点 – 超级管理员_

- **用户**：用户列表显示站点的所有用户。还提供添加新用户的选项，可通过“添加现有用户”（即网络中已有的用户）或“添加新用户”（即网络中的新用户）实现。
- **主题**：主题显示站点上的所有主题。网络启用的主题不会在此界面显示。
- **设置**：所有站点设置都存储在此表中。除非您知道自己在做什么，否则请勿在此处编辑任何内容。

## 网络管理员更新界面

**网络管理员更新界面**控制网络中的更新流程。当有可用更新时，工具栏和导航菜单中会显示通知。保持站点更新对安全至关重要，也能为您和读者营造更安全的网络环境。在[网络管理员](https://developer.wordpress.org/advanced-administration/multisite/admin/) > [更新](https://developer.wordpress.org/advanced-administration/multisite/admin/#updates)下有两个界面：默认的[可用更新界面](#available-updates)可更新WordPress、主题和插件；升级至最新WordPress版本后，可通过[网络升级界面](#upgrade-nework)升级网络中的所有站点。

### 可用更新 {#available-updates}

在此[可用更新屏幕](#available-updates)上，您可以更新到最新版本的 WordPress，并更新来自 [WordPress.org](https://wordpress.org/) 存储库的主题和插件。

[![](https://wordpress.org/documentation/files/2019/04/network-available-updates-1024x590.png)](https://wordpress.org/documentation/files/2019/04/network-available-updates.png)

#### 如何更新 {#how-to-update}

**WordPress**：更新您的 WordPress 安装是一个简单的单步操作：当收到新版本可用通知时，只需点击“立即更新”按钮。在大多数情况下，WordPress 会自动在后台为您应用维护和安全更新。

**主题和插件**：要在此界面更新单个主题或插件，请使用复选框进行选择，然后点击相应的“更新”按钮。要一次性更新所有主题或插件，您可以在点击更新按钮前，勾选该部分顶部的复选框以全选。

**翻译**：翻译文件会在需要时更新。当收到新翻译可用通知时，点击“更新翻译”按钮。

### 升级网络 {#upgrade-network}

此[升级网络页面](#upgrade-network)用于在[WordPress 升级](#available-updates)完成后，升级[网络](https://wordpress.org/documentation/article/glossary#network)中的所有站点。WordPress 升级后，系统会提示您访问**升级网络**，并显示类似“感谢更新！请访问升级网络页面以升级您的所有站点。”的消息。

[![](https://i1.wp.com/wordpress.org/support/files/2019/04/superadmin-update.png?fit=1024%2C584&ssl=1)](https://wordpress.org/documentation/files/2019/04/superadmin-update.png)

升级网络功能将逐步处理每个站点（每次五个），确保应用所有数据库更改。此菜单项仅当您以超级管理员角色用户登录时才可见。您可以从网络中的任何站点访问它。

如果因任何原因某个站点未升级，当该站点的管理员登录其管理后台时，每个站点都应被升级。已停用的站点不会通过此过程升级，但如果站点被重新激活，当该站点的管理员登录其仪表盘时，站点将得到升级。

**升级网络** 点击此按钮开始升级过程。点击升级网络按钮将逐步处理网络中的每个站点（每次五个），确保应用所有数据库更新。

如果核心版本未发生更新，点击此按钮不会产生任何影响。