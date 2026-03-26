---
title: "多语言 WordPress"
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

# 多语言 WordPress

WordPress 当前并不原生支持双语或多语言博客。不过，WordPress 社区开发的插件使得创建多语言博客变得简单。WordPress Gutenberg 项目的第四阶段也是最后阶段，将为核心添加多语言站点支持，具体内容已列入 [WordPress 路线图](https://wordpress.org/about/roadmap/)。

创建多语言博客，本质上是安装多个语言版本的 WordPress，并让插件在它们之间切换。这包括安装 .mo 语言文件，大多数插件会要求您手动完成此操作。更多详情，请参阅[以您的语言安装 WordPress](https://developer.wordpress.org/advanced-administration/before-install/in-your-language/)。

## 多语言插件的不同类型 {#different-types-of-multilingual-plugins}

多语言插件主要有以下几种基本类型：

1. 每种语言使用单独的文章来管理多语言内容，然后将翻译版本相互链接，表明一篇文章是另一篇的翻译。
2. 将每种语言的文章变体都存储在同一篇文章中。
3. 在生成的页面上管理翻译，而不是在文章上下文中处理。
4. 通过相互引用，将网络（多站点）安装中不同语言的独立 WordPress 站点链接在一起。

### 每篇文章一种语言 {#one-language-per-post}

支持多语言的插件若采用每篇文章对应一种语言的方案，会允许用户选择文章的语言，并将翻译内容作为新文章添加（页面、标签和分类同理）。

随后，同一内容的不同版本会被链接在一起，形成翻译组。这种分组方式使用户能够切换显示语言。

优点：

1. 文章在数据库中的内容保持不变（易于安装和卸载）。
2. 默认情况下所有内容都会被翻译。如果文章包含自定义字段，这些字段会附加到该文章上，因此它们已与语言关联。
3. 部分插件会使用可本地化主题自带的语言文件（.mo）来翻译主题显示的术语。在 WordPress 中，本地化基于 [GNU gettext](https://make.wordpress.org/polyglots/handbook/#Localization_Technology) 技术。因此，当单篇文章为法语时，插件会将主题的所有术语切换为同一语言（此处为法语）。这些文件可以补充网站特有的术语（分类标题、小工具、链接等）。无需全部重新翻译，只需添加特定术语及其目标语言翻译即可。
4. 其他分析内容的插件（如相关文章推荐）仍能正常工作。

缺点：

1. 架构更复杂。插件需要挂钩到许多 WordPress 函数并进行过滤，以确保仅返回与语言匹配的内容。
2. 部分插件需要额外的数据表——通常用于存储翻译分组信息。较新的插件可能改用自定义分类法或文章元字段。
3. 可能导致数据库过度增长并拖慢性能。例如，一个拥有 10 万种产品的 WooCommerce 网站在翻译成 5 种语言后，将产生 50 万条记录。所有产品元数据（每个产品可能有数十个，临时数据也会被复制）都会重复存储，数据库可能变得非常庞大。

### 单篇文章包含所有语言 {#all-languages-in-a-single-post}

在同一篇文章中存储所有语言内容的多语言插件，使用语言元标签来区分不同语言的内容。当文章显示时，会先进行处理，只保留当前激活语言的内容。

优点：

1. 易于实现并排编辑。
2. 出错可能性更低。没有额外的数据表，WordPress 中需要修改的内容也更少。
3. 无论使用哪种语言的关键词进行搜索，都能找到同一篇文章。
4. 数据库中的记录数量保持不变。

缺点：

1. 卸载可能比较复杂，因为需要从数据库中清理多语言内容。
2. 文章固定链接可能无法翻译。

### 管理生成页面的翻译 {#manage-translations-on-the-generated-page}

多语言插件利用 WordPress 生成的内容页面，并在这些页面上执行翻译。当任何页面在 WordPress 上显示时，插件（无论是离线还是在线）会尝试使用机器翻译创建该页面的翻译版本。之后，该翻译可以手动更改或修改。

优点：

1. 安装简单，并提供页面上任何内容的翻译。
2. 可以轻松编辑翻译。

缺点：

1. 自动翻译效果不够好，网站上的页面可能翻译得很差。
2. 内容与翻译之间存在强耦合，原始内容的更改可能会破坏翻译。

### 将您导向外部翻译服务的插件 {#plugins-that-direct-you-to-external-translation-services}

这类多语言插件通常用于创建一个小组件，该组件为使用在线翻译服务（如 Google 翻译）提供快捷方式。内容由第三方引擎按需自动翻译。

优点：

1.  安装简单，并提供页面上任何内容的翻译。
2.  翻译过程是自动化的这一点非常明确，因此用户的期望值会降低。

缺点：

1.  自动翻译质量不够好，网站上的页面可能翻译得很差。
2.  由于无法修改翻译，这些插件限制了内容发布者提供高质量翻译内容的能力。

### 每种语言使用独立的 WordPress 安装 {#each-language-in-its-own-wordpress-installation}

为每种需要翻译的语言创建独立站点（例如在 [WordPress 多站点](https://developer.wordpress.org/advanced-administration/multisite/create-network/) 安装中）。所有站点需运行相同的主题和插件。当翻译保存时，源文章会收到翻译文章的关联通知，系统会通过独立数据表维护翻译关系。

优势：

1. 每个语言站点都是标准的 WordPress 安装，使用常规文章格式（翻译数据通过文章元数据和外部数据库存储）
2. 停用插件后内容仍可正常使用，仅失去与源文章/翻译的关联信息

劣势：

1. 独立站点会增加管理需求，这可能带来不便

## 语言协商 {#language-negotiation}

语言协商是指如何确定用户查看网站时使用的语言。

无论采用何种存储多语言内容的解决方案，多语言插件也需要能够选择显示哪种语言。

通常，URL 会指示显示语言。编码语言信息的不同 URL 策略包括：

* 将语言名称作为参数添加：example.com/?lang=en 或 example.com/?lang=es
* 添加虚拟“目录”作为语言名称：example.com/en/ 或 example.com/es/
* 对不同语言使用不同的域名：www.example.com 或 es.example.com

## 如何选择正确的多语言解决方案 {#how-to-choose-the-right-multilingual-solution}

为您的需求选择最合适的多语言插件需要一些时间。请查看 [WordPress 插件目录](https://wordpress.org/plugins/search/multilingual) 以获取多语言插件列表。

没有唯一的方法，只有适应内容策略、数据模型、文章和页面数量以及访客预期行为/体验的方法。对于 WordPress 网络（多站点），还需要具备良好的服务器管理知识。

无论如何，安装多语言插件对任何网站来说都是一项重大变更。最好先创建一个测试站点，验证所有必需的插件与主题之间一切运行正常，然后再进行安装。

由于许多多语言插件会显著改变数据库，因此在尝试前需要进行[数据库备份](https://developer.wordpress.org/advanced-administration/security/backup/database/)。

## 相关 {#related}

* [WordPress 本地化版本](https://developer.wordpress.org/advanced-administration/before-install/in-your-language/)