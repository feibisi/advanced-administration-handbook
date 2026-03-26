---
title: "为 WordPress 创建数据库"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Before Install
    - Repos
    - Data
---

# 为 WordPress 创建数据库

如果您要在自己的 Web 服务器上安装 WordPress，请按照以下说明之一创建 WordPress 数据库和用户账户。

## 使用 phpMyAdmin

如果您的 Web 服务器安装了 [phpMyAdmin](https://wordpress.org/documentation/article/wordpress-glossary/#phpMyAdmin)，您可以按照这些说明创建 WordPress 用户名和数据库。如果您在自己的计算机上操作，在大多数 Linux 发行版上可以自动安装 PhpMyAdmin。

_**注意：** 这些说明是针对 phpMyAdmin 4.4 编写的；不同版本的 phpMyAdmin 用户界面可能略有不同。_

如果左侧的**数据库**下拉列表中尚不存在与 WordPress 相关的数据库，请创建一个：

为您的 WordPress 数据库选择一个名称：'wordpress' 或 'blog' 是不错的选择，但大多数托管服务（尤其是共享托管）会要求名称以您的用户名和下划线开头。因此，即使您在自己的计算机上操作，我们也建议您检查托管服务的要求，以便在自己的服务器上遵循这些要求，并能够无需修改地迁移数据库。在**创建数据库**字段中输入选定的数据库名称，并为您的语言和编码选择最佳的排序规则。在大多数情况下，最好在 "utf8_" 系列中选择，如果找不到您的语言，请选择 "utf8mb4_general_ci"（参考：[\[1\]](https://make.wordpress.org/core/2015/04/02/the-utf8mb4-upgrade/)）。

[![phpMyAdmin 语言编码下拉菜单，选中了 utf8mb4_general_ci](https://i1.wp.com/wordpress.org/support/files/2018/11/phpMyAdmin_create_database_4.4.jpg?fit=688%2C411&ssl=1)](https://i1.wp.com/wordpress.org/support/files/2018/11/phpMyAdmin_create_database_4.4.jpg?fit=688%2C411&ssl=1)

phpMyAdmin 语言编码下拉菜单

点击左上角的 **phpMyAdmin** 图标返回主页，然后点击 **用户** 选项卡。如果用户列表中尚不存在与 WordPress 相关的用户，请创建一个：

[![phpMyAdmin 用户选项卡被选中](https://wordpress.org/documentation/files/2018/11/users-768x500.jpg)](https://wordpress.org/documentation/files/2018/11/users-768x500.jpg)

phpMyAdmin 用户选项卡

1. 点击 **添加用户**。
2. 为 WordPress 选择一个用户名（推荐使用 'wordpress'）并填入 **用户名** 字段。（确保从下拉菜单中选择了 **使用文本字段：**）
3. 选择一个安全的密码（最好包含大小写字母、数字和符号的组合），并填入 **密码** 字段。（确保从下拉菜单中选择了 **使用文本字段：**）在 **重新输入** 字段中再次输入密码。
4. 记下您选择的用户名和密码。
5. 保持 **全局权限** 下的所有选项为默认设置。
6. 点击 **执行**。
7. 返回 **用户** 屏幕，点击您为 WordPress 刚创建的用户旁边的 **编辑权限** 图标。
8. 在 **数据库特定权限** 部分，从 **为以下数据库添加权限** 下拉菜单中选择您为 WordPress 刚创建的数据库，然后点击 **执行**。
9. 页面将刷新并显示该数据库的权限。点击 **全选** 以选择所有权限，然后点击 **执行**。
10. 在结果页面上，记下页面顶部 **服务器：** 后列出的主机名。（通常为 **localhost**。）

[![数据库服务器选择显示 'localhost'](https://i1.wp.com/wordpress.org/support/files/2018/11/phpMyAdmin_server_info_4.4.jpg?fit=682%2C107&ssl=1)](https://i1.wp.com/wordpress.org/support/files/2018/11/phpMyAdmin_server_info_4.4.jpg?fit=682%2C107&ssl=1)

## 使用 MySQL 客户端

您可以通过在 shell 中运行 mysql 来快速轻松地创建 MySQL 用户和数据库。语法如下所示，美元符号是命令提示符：

```
$ mysql -u adminusername -p  
输入密码：  
欢迎使用 MySQL 监视器。命令以 ; 或 \\g 结尾。  
您的 MySQL 连接 id 是 5340，服务器版本：3.23.54  
  
输入 'help;' 或 '\\h' 获取帮助。输入 '\\c' 清除缓冲区。  
  
mysql> CREATE DATABASE databasename;  
查询成功，1 行受影响 (0.00 秒)

mysql> CREATE USER "wordpressusername"@"hostname" IDENTIFIED BY "password";
mysql> GRANT ALL PRIVILEGES ON databasename.* TO "wordpressusername"@"hostname";
查询成功，0 行受影响 (0.00 秒)

mysql> FLUSH PRIVILEGES;  
查询成功，0 行受影响 (0.01 秒)   
  
mysql> EXIT  
再见  
```

示例显示：

*   root 也是 _adminusername_。更安全的做法是选择一个所谓的“普通”账户作为您的 mysql 管理员，这样您就不会以系统 root 用户的身份输入“mysql”命令。（任何时候避免以 root 身份操作都能降低被利用的风险。）您使用的名称取决于您使用 mysqladmin 分配的数据库管理员名称。
*   wordpress 或 blog 是 _databasename_ 的良好选择。
*   wordpress 是 _wordpressusername_ 的良好选择，但您应该意识到，既然这里使用了它，全世界也会知道它。
*   _hostname_ 通常是 localhost。如果您不知道这个值应该是什么，如果您不是 WordPress 主机的管理员，请咨询您的系统管理员。如果您是系统管理员，请考虑使用非 root 账户来管理您的数据库。
*   _password_ 应该是一个难以猜测的密码，最好包含大小写字母、数字和符号的组合。避免使用字典中单词的一个好方法是使用您容易记住的短语中每个单词的首字母。

如果您需要将这些值记录在某个地方，请避免将它们写在包含受其保护内容的系统中。您需要记住用于 _databasename_、_wordpressusername_、_hostname_ 和 _password_ 的值。当然，既然它们已经（或即将）在您的 wp-config.php 文件中，就没有必要再把它们放在其他地方了。

## 使用 Plesk

如果您的托管服务提供商提供 [Plesk](https://www.plesk.com/) 托管控制面板，并且您想手动安装 WordPress，请按照以下说明创建数据库：

1.  登录 Plesk。
2.  在"网站与域名"页面上，找到您网站的"自定义网站"区域，点击 **数据库**：

[![Plesk 面板截图，高亮显示了自定义网站区域中的数据库按钮](https://wordpress.org/documentation/files/2018/11/plesk-db-768x558.png)](https://wordpress.org/documentation/files/2018/11/plesk-db-768x558.png)

Plesk 自定义网站数据库

3.  点击 **添加新数据库**，如果需要可以更改数据库名称，通过提供凭据创建数据库用户，然后点击 **确定**。完成！

## 使用 cPanel

如果您的托管服务提供商提供 [cPanel](https://wordpress.org/documentation/article/wordpress-glossary/#cPanel) 主机控制面板，您可以按照以下简单说明来创建您的 WordPress 用户名和数据库。关于使用 cPanel 创建数据库和用户的更完整说明集，可以在 [使用 cPanel](https://wordpress.org/documentation/article/using-cpanel/) 中找到。

1.  登录您的 [cPanel](https://wordpress.org/documentation/article/wordpress-glossary/#cPanel)。
2.  在“数据库”部分下，点击 **MySQL 数据库向导** 图标。
3.  在 **步骤 1. 创建数据库** 中输入数据库名称，然后点击下一步。
4.  在 **步骤 2. 创建数据库用户** 中输入数据库用户名和密码。请务必使用强密码。点击创建用户。
5.  在 **步骤 3. 将用户添加到数据库** 中，勾选“所有权限”复选框，然后点击下一步。
6.  在 **步骤 4. 完成任务** 中，记下数据库名称和用户。请写下 _主机名_、_用户名_、_数据库名_ 以及您选择的密码的值。（请注意，_主机名_ 通常为 **localhost**。）

## 使用 DirectAdmin

如果您是单站点虚拟主机账户的普通用户，可以正常登录。然后点击 **MySQL 管理**。（如果此选项不易见，可能需要您的主机商修改您的“套餐”以激活 MySQL。）接着按照下面的“c”部分操作。

经销商账户或管理员账户可能需要点击 **用户级别**。如果相关域名是经销商的主域名，他们必须首先以经销商身份登录……如果域名不是经销商的主域名，则以用户身份登录。如果是经销商的主域名，那么以经销商身份登录后，直接点击 **用户级别**。但如果相关域名不是经销商的主域名，则必须以用户身份登录。然后点击 **MySQL 管理**。（如果不易见，可能需要返回经销商或管理员级别，修改“管理用户套餐”或“管理经销商套餐”以启用 MySQL。）

在 MySQL 管理中，点击小字：**创建新数据库**。此处需要提交数据库及其用户名的两个后缀。为获得最高安全性，请使用两组不同的 4-6 位随机字符。密码字段有一个随机按钮，可生成 8 位密码。您也可以为获得最高安全性而添加更多字符。点击 **创建**。下一个屏幕将汇总数据库、用户名、密码和主机名。请务必将这些信息复制粘贴到文本文件中以备将来参考。