---
title: "安装多个 WordPress 实例"
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

# 安装多个 WordPress 实例

如果您需要多个 WordPress 实例，根据系统架构或 WordPress 实例与数据库的组合方式，有三种安装类型：

1. WordPress 多站点网络：单个 WordPress 实例（在同一 WP 实例内创建多个站点）共享单个数据库实例。
2. 单一数据库：多个 WordPress 实例共享单个数据库实例。
3. 多个数据库：多个 WordPress 实例，每个实例使用自己的数据库实例。

![](https://wordpress.org/documentation/files/2022/06/multisite_db_layout-1024x469.jpg)

让我们首先看看第三种类型，即具有多个数据库的多个 WordPress 实例，因为其安装过程与单个 WordPress 实例相同。

## 多个 WordPress 实例与多个数据库 {#multiple-wordpress-instances-with-multiple-databases}

您计划安装的每个实例都需要一个独立的 [MySQL 数据库](https://wordpress.org/documentation/article/wordpress-glossary/#mysql)。如果您尚未创建这些数据库，[可以在此处找到基本说明](https://developer.wordpress.org/advanced-administration/before-install/howto-install/#step-2-create-the-database-and-a-user)。

为确保每个 WordPress 实例连接到正确的数据库，您需要将这些信息添加到 [wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件中。需要修改的行如下：

```
define('DB_NAME', 'wordpress');    // 数据库名称
define('DB_USER', 'username');     // 您的 MySQL 用户名
define('DB_PASSWORD', 'password'); // 用户的密码
define('DB_HOST', 'localhost' );  // 数据库主机
```

`DB_NAME` 是为托管在 `DB_HOST` MySQL 服务器上的博客创建的独立数据库的名称。如果您为每个数据库使用不同的用户登录，也请相应修改 `DB_USER` 和 `DB_PASSWORD`。

将每个 [wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件上传到其特定的根目录/安装目录，并运行安装。更多信息请参阅 [安装 WordPress](https://developer.wordpress.org/advanced-administration/before-install/howto-install/)。

## 多站点功能 {#the-multisite-feature}

若需多个站点使用 WordPress，可利用多站点功能创建所谓的站点**网络**。该功能涉及安装单一的 WordPress 实例和数据库。

多站点功能看似比其他类型的多 WordPress 安装更简单，但存在一些注意事项和限制。更多详细信息请参阅以下文档：

* [创建网络前的准备](https://developer.wordpress.org/advanced-administration/multisite/prepare-network/)
* [创建网络](https://developer.wordpress.org/advanced-administration/multisite/create-network/)
* [多站点网络管理](https://developer.wordpress.org/advanced-administration/multisite/administration/)

## 使用单一数据库运行多个 WordPress 实例 {#multiple-wordpress-instances-with-a-single-database}

与上文描述的多数据库解决方案类似，每个安装的 [wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件都会有所不同。然而，在这种情况下，每个博客只有一行代码是唯一的：

```
$table_prefix = 'wp_'; // 例如：'wp_' 或 'b2' 或 'mylogin_'
```

默认情况下，WordPress 为其 [MySQL 数据库](https://wordpress.org/documentation/article/wordpress-glossary/#mysql) 表分配表前缀 `wp_`，但这个前缀可以是您选择的任何内容。这允许您为数据库中的每个博客创建唯一的标识符。例如，假设您要设置三个博客，名称分别为 _主站_、_项目_ 和 _测试_。您应该替换每个博客 [wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件中的 `wp_` 前缀：

**主站博客：**
```
$table_prefix = 'main_';
```

**项目博客：**
```
$table_prefix = 'projects_';
```

**测试博客：**
```
$table_prefix = 'test_';
```

如前所述，您可以使用自己创建的前缀。此处提供的仅作示例。

将每个 [wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件上传到其特定的根目录/安装目录，并运行安装。更多信息请参阅 [安装 WordPress](https://developer.wordpress.org/advanced-administration/before-install/howto-install/)。

为了增强安全性，您还可以向同一数据库添加多个用户，并为每个 WordPress 实例分配其自己的 MySQL 用户。

## 多数据库，相同用户 {#multiple-databases-same-users}

您可以通过定义 `CUSTOM_USER_TABLE` 常量（以及可选的 `CUSTOM_USER_META_TABLE` 常量）指向相同的 `wp_your_blog_users` 和 `wp_your_blog_usermeta` 表，从而为同一域名下的所有博客使用相同的用户库。  
详情请参阅 [编辑 wp-config.php/自定义用户和用户元表](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/#custom-user-and-usermeta-tables)。