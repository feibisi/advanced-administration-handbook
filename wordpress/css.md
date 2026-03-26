---
title: "CSS"
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

# CSS

[tip]自 WordPress 6.2 起，您可以在站点编辑器的样式区域添加自定义 CSS。了解更多关于[全站和按块自定义 CSS 编辑器](https://wordpress.org/documentation/article/styles-overview/#applying-custom-css)的信息。[/tip]

WordPress 高度依赖 CSS 中的呈现样式。通过使用[主题](https://wordpress.org/documentation/article/using-themes/)，您几乎拥有无限的选择来定制布局。WordPress 主题让更改网站外观变得简单，并为您[创建自己的主题](https://codex.wordpress.org/Theme_Development)和页面布局提供了广阔空间。

[CSS](https://wordpress.org/documentation/article/glossary/#css) 代表**层叠样式表**。它允许您将样式呈现信息（如颜色和布局）与 HTML 结构分开存储。这使您能够精确控制网站布局，并使页面更新更快、更容易。

本文简要描述了 CSS 在 WordPress 中的使用，并列出了进一步信息的参考资料。有关 CSS 本身的信息，请参阅[在 WordPress 上构建的资源#CSS](https://developer.wordpress.org/advanced-administration/resources/#CSS)。

## [WordPress 与 CSS](#wordpress-and-css)

WordPress 主题通过结合使用[模板文件](https://codex.wordpress.org/Templates)、[模板标签](https://codex.wordpress.org/Template_Tags)和 CSS 文件来生成您 WordPress 网站的外观。

### [模板文件](#template-files)

[模板文件](https://codex.wordpress.org/Stepping_Into_Templates)是构建您网站的基本模块。在[WordPress主题结构](https://developer.wordpress.org/advanced-administration/wordpress/site-architecture/)中，页眉、侧边栏、内容和页脚都包含在单独的文件中。它们组合在一起构成您的页面。这使您可以自定义这些构建模块。例如，在默认的WordPress主题中，您网站首页、分类、[归档](https://codex.wordpress.org/Creating_an_Archive_Index)和[搜索](https://codex.wordpress.org/Creating_a_Search_Page)网页上的多文章视图包含侧边栏。点击任何文章，您将进入单文章视图，侧边栏则会消失。您可以[选择页面上显示哪些部分](https://codex.wordpress.org/The_Loop_in_Action)，并单独自定义它们，例如在特定分类的所有页面上显示不同的页眉或侧边栏。更多内容请参阅[深入了解模板](https://codex.wordpress.org/Stepping_Into_Templates)。

### [模板标签](#template-tags)

模板标签是用于提供指令和请求存储在 WordPress 数据库中的信息的代码片段。其中一些标签具有高度可配置性，允许您自定义网站上显示的日期、时间、列表和其他元素。您可以在[深入了解模板标签](https://codex.wordpress.org/Stepping_Into_Template_Tags)中了解更多信息。

### [样式表](#stylesheet)

CSS 文件是所有设计元素的汇集之处。您网站中的每个模板文件都包含包裹着模板标签和内容的 HTML 元素。每个主题的样式表中都包含控制这些 HTML 元素设计和布局的规则。没有这些规则，您的页面将仅呈现为简单的长文本页面。通过这些规则，您可以调整页面结构模块的布局，使页头变得非常长且充满图形或照片，或者保持简洁窄小。您的网站可以“浮动”在浏览者屏幕中央，左右留有空白，也可以拉伸至全屏，填满整个页面。侧边栏可以位于右侧或左侧，甚至可以从页面中部开始。如何设计页面样式完全由您决定。但样式设计的指令都位于每个主题文件夹内的 `style.css` 文件中。

## [WordPress 中的自定义 CSS](#custom-css-in-wordpress)

自 WordPress 4.7 起，您可以直接通过[外观自定义屏幕](https://wordpress.org/documentation/article/appearance-customize-screen/)为您的主题添加自定义 CSS，无需额外插件或直接编辑主题及子主题。只需在自定义当前主题时选择 **附加 CSS** 标签页即可开始！

您所做的任何 CSS 修改都会在预览中显示，就像在定制器中进行的其他更改一样。这意味着您有充足时间调整和完善网站外观，在实际应用更改前可反复修改直至满意！

请注意，CSS 修改与您的主题绑定。这意味着如果您切换到新主题，您的自定义 CSS 样式将不再生效（当然，如果切换回之前的主题，它们会再次生效）。

### [为何使用自定义 CSS？](#why-use-custom-css)

原因如下：

* 若直接修改主题且主题后续更新，您的修改可能会丢失。使用自定义 CSS 可确保修改得以保留。
* 使用自定义 CSS 能加快开发速度。
* 自定义 CSS 在主题原始 CSS 之后加载，因此无需从头编写整套 CSS 即可覆盖特定样式声明。

## [WordPress Generated Classes](#wordpress-generated-classes)

Several classes for aligning images and block elements (`div`, `p`, `table` etc.) were introduced in WordPress 2.5: `aligncenter`, `alignleft` and `alignright`. In addition the class `alignnone` is added to images that are not aligned, so they can be styled differently if needed.

The same classes are used to align images that have a caption (as of WordPress 2.6). Three additional CSS classes are needed for the captions, and one more for accessibility. Together, the classes are:

```
/* WordPress Core
-------------------------------------------------------------- */
.alignnone {
  margin: 5px 20px 20px 0;
}

.aligncenter,
div.aligncenter {
  display: block;
  margin: 5px auto 5px auto;
}

.alignright {
  float:right;
  margin: 5px 0 20px 20px;
}

.alignleft {
  float: left;
  margin: 5px 20px 20px 0;
}

a img.alignright {
  float: right;
  margin: 5px 0 20px 20px;
}

a img.alignnone {
  margin: 5px 20px 20px 0;
}

a img.alignleft {
  float: left;
  margin: 5px 20px 20px 0;
}

a img.aligncenter {
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.wp-caption {
  background: #fff;
  border: 1px solid #f0f0f0;
  max-width: 96%; /* Image does not overflow the content area */
  padding: 5px 3px 10px;
  text-align: center;
}

.wp-caption.alignnone {
  margin: 5px 20px 20px 0;
}

.wp-caption.alignleft {
  margin: 5px 20px 20px 0;
}

.wp-caption.alignright {
  margin: 5px 0 20px 20px;
}

.wp-caption img {
  border: 0 none;
  height: auto;
  margin: 0;
  max-width: 98.5%;
  padding: 0;
  width: auto;
}

.wp-caption p.wp-caption-text {
  font-size: 11px;
  line-height: 17px;
  margin: 0;
  padding: 0 4px 5px;
}

/* Text meant only for screen readers. */
.screen-reader-text {
  border: 0;
  clip: rect(1px, 1px, 1px, 1px);
  clip-path: inset(50%);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute !important;
  width: 1px;
  word-wrap: normal !important; /* Many screen reader and browser combinations announce broken words as they would appear visually. */
}

.screen-reader-text:focus {
  background-color: #eee;
  clip: auto !important;
  clip-path: none;
  color: #444;
  display: block;
  font-size: 1em;
  height: auto;
  left: 5px;
  line-height: normal;
  padding: 15px 23px 14px;
  text-decoration: none;
  top: 5px;
  width: auto;
  z-index: 100000;
  /* Above WP toolbar. */
}
```

Each Theme should have these or similar styles in its `style.css` file to be able to display images and captions properly. The exact HTML elements and class and ID values will depend on the structure of the Theme you are using.

## [模板与 CSS](#templates-and-css)

为了帮助您更深入地理解 CSS 与网页之间的关系，建议您阅读以下列表中的相关文章：

* [使用主题](https://wordpress.org/documentation/article/using-themes/) – 此列表中还包含许多进阶文章。
* [模板](https://codex.wordpress.org/Templates) – WordPress 主题与模板文章的完整列表。
* [主题开发](https://codex.wordpress.org/Theme_Development) – WordPress 主题开发指南与代码标准。

## [WordPress 布局帮助](#wordpress-layout-help)

如果您在使用 WordPress 主题或布局时遇到问题或有疑问，请首先查看主题作者的网站，看看是否有升级版本或您的问题是否有答案。以下是一些其他资源：

* [WordPress 网站设计教程](https://wordpress.org/documentation/article/wordpress-lessons/)
* [网站设计与布局](https://codex.wordpress.org/Site_Design_and_Layout) – 与 WordPress 网站设计相关的综合资源列表。
* [常见问题：布局与设计](https://codex.wordpress.org/FAQ_Layout_and_Design)

## [CSS 资源](#css-resources)

* [查找您的 CSS 样式](https://codex.wordpress.org/Finding_Your_CSS_Styles)
* [CSS 故障排除](https://codex.wordpress.org/CSS_Troubleshooting)
* [CSS 修复浏览器错误](https://codex.wordpress.org/CSS_Fixing_Browser_Bugs)
* [CSS 编码标准](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/)
* [CSS 简写](https://codex.wordpress.org/CSS_Shorthand)
* [WordPress 开发资源#CSS](https://developer.wordpress.org/advanced-administration/resources/#CSS)
* [条件注释 CSS](https://codex.wordpress.org/Conditional_Comment_CSS)
* [验证网站](https://codex.wordpress.org/Validating_a_Website)