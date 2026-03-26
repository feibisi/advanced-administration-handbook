---
title: "站点架构 (v1.5)"
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

# 站点架构 (v1.5)

以下是对 WordPress 1.5 通用站点架构的描述。鼓励但不强制 WordPress 主题开发者沿用 XHTML 标签和 CSS 选择器的核心站点架构。因此，您可以将其视为通用大纲，因为您的主题可能有所不同。

## 模板驱动页面

在深入了解 WordPress 页面架构的[核心结构](#Core_Structure)之前，您需要理解 WordPress 使用[模板文件](https://codex.wordpress.org/Templates)来生成最终页面的"外观"和内容。例如，当您查看 WordPress 网站的前端页面时，实际上看到的是多个模板文件的组合：

* index.php
* header.php
* sidebar.php
* footer.php

当您查看单篇文章页面时，可能涉及以下模板文件：

* single.php
* header.php
* sidebar.php
* footer.php
* comments.php

在分类、归档和搜索等多文章页面中，您可能看到以下模板文件的任意组合：

* index.php
* category.php
* 404.php
* search.php
* header.php
* sidebar.php
* footer.php

在接下来的架构规范中，我们将尽可能明确指定哪些 CSS 选择器属于哪些模板文件。

## 核心结构

WordPress 网站的核心结构代表了承载页面内容的主要容器。一个 WordPress 网站的核心结构至少包含：

*   页眉
*   侧边栏/菜单
*   内容
*   页脚

这些是包含页面最重要部分的主要容器。请记住，核心结构就像一组积木，每个单元都依赖于其他单元。如果你改变其中一个，就必须改变其他部分。

**经典主题**

```
<body>
  <div id="rap">
    <h1 id="header"></h1>
    <div id="content"></div>
    <div id="menu"></div>
    <p class="credit"></p>
  </div>
</body>
```

**默认主题**

```
<body>
  <div id="page">
    <div id="header"></div>
    <div id="content" class="narrowcolumn"></div>
    <div id="sidebar"></div>
    <div id="footer"></div>
  </div><!-- end page -->
</body>
```

请注意，虽然两个主题都使用了侧边栏，但第一个主题将其称为菜单，而另一个主题则称之为侧边栏。

这两个主题核心结构的主要区别可能在于页眉和页脚的使用。对于经典主题，页眉位于 h1 标签内，页脚则包含在段落标签中。而在默认主题中，页眉被放置在一个名为 header 的 div 中，页脚则被放置在一个 footer div 中。

两个主题都有一个容器来包围或"包裹"整个页面。这个容器（通常与 body HTML 标签结合使用）允许对整个结构进行更明确的控制。根据所使用的 WordPress 主题，这个容器也可以被称为：

*   page
*   wrap
*   rap

有些主题可能会添加第二个、第三个甚至第四个侧边栏，以创建列效果。它们也可能在整个页面周围或特定容器周围包含额外的包装器。然而，在所有情况下，基本的核心结构本质上保持不变。

### 模块化模板文件

基于构建块的理念，WordPress 主题将核心结构划分为独立的块，称为[模板文件](https://codex.wordpress.org/Templates)。以下是这些模板文件：

* 页眉 - header.php
* 侧边栏/菜单 - sidebar.php
* 内容 - index.php、single.php、page.php、category.php、author.php、search.php 等
* 页脚 - footer.php

在每个模板文件中，都可以使用 body div 作为内容的整体容器。

当查看使用特定 WordPress 主题的网页时，生成的特定模板文件取决于用户的请求。如果用户点击分类标签，将使用分类模板。如果用户查看[页面](https://wordpress.org/documentation/article/create-pages/)，将使用页面模板。

当这些核心模板文件与[WordPress 循环](https://codex.wordpress.org/The_Loop)和查询结合加载时，可以生成各种模板。这使得网页开发者能够为每个特定模板创建独特且个性化的样式。

## 内部结构

在这些核心结构容器内部，存在着更小的构建模块，它们承载着父容器内的具体内容。WordPress 主题可以包含多种此类结构，但我们将重点关注 WordPress 自带的两个主题。（大多数 WordPress 主题模板都基于这两个主题。）

### 页眉

页眉是传统上位于网页顶部的结构。它包含网站标题。有时也可称为刊头、头部、标题或横幅。在所有 WordPress 主题中，页眉都位于 header.php 模板文件中。

经典主题具有最简单的页眉代码：

```
<h1 id="header"></h1>
```

默认主题的页眉代码更为复杂：

```
<div id="header">
  <div id="headerimg">
    <h1></h1>
    <div class="description"></div>
  </div>
</div>
```

经典主题的样式位于主题的 style.css 文件中，而默认主题的样式既位于 style.css 文件中，也位于 header.php [模板文件](https://codex.wordpress.org/Templates) 的 <head> 部分。文章 [设计页眉](https://codex.wordpress.org/Designing_Headers) 详细介绍了如何处理这些样式。

### 内容

WordPress 中的内容容器扮演着至关重要的角色，因为它承载着 [WordPress 循环](https://codex.wordpress.org/The_Loop)。WordPress 循环会处理当前页面上将要显示的每一篇文章。然后，这些文章会根据它们如何匹配循环标签内的特定条件进行格式化。

经典主题拥有最简单的结构：

```
<div id="content">
  <h2>日期</h2>
  <div class="post" id="post-1">
    <h3 class="storytitle">文章标题</h3>
    <div class="meta">文章元数据</div>
    <div class="storycontent">
      <p>欢迎使用 WordPress。</p>
    </div>
    <div class="feedback">评论 (2)</div>
  </div>
</div>
```

经典主题为日期、标题、文章元数据、文章内容和反馈（评论数量）提供了容器。它还展示了一个强大的功能：能够单独设置单篇文章的外观样式。

```
<div class="post" id="post-1">
```

`post` 这个 CSS 类选择器将文章样式应用到这个容器上。需要注意的是，`post` 类选择器还有一个由 WordPress 自动生成的 ID。以下是一个可用于显示类选择器 ID 的代码示例：

```
<div class="post" id="post-<?php the_ID(); ?>">
```

使用模板标签 [the_ID()](https://developer.wordpress.org/reference/functions/the_ID/) 可以显示文章的 ID 号。这个唯一标识符可以用于内部页面链接以及样式。例如，单篇文章可以拥有针对 `post-1` 以及 `post-2` 的样式。虽然为每篇文章都设置一个样式有点过度，但可能有一两篇文章你需要它们看起来有点不同。一些插件也可能使用这个标识符来自动改变不同文章的外观。

默认主题的内容容器具有**多文章视图**（例如，用于首页、分类、归档和搜索）以及用于单篇文章的**单文章视图**。多文章视图如下所示：

```
<div id="content" class="narrowcolumn">
  <div class="post" id="post-1">
    <h2>文章标题</h2>
    <small>日期</small>
    <div class="entry">
      <p>文章内容。</p>
    </div>
    <p class="postmetadata">文章元数据部分</p>
  </div>
  <div class="navigation">
    <div class="alignleft">上一篇文章</div>
    <div class="alignright">下一篇文章</div>
  </div>
</div>
```

这里包含了很多内容。让我们来分解一下。

### 内容结构示例解析

```
<div id="content" class="narrowcolumn">
```

**多篇文章视图**的内容容器使用名为 `narrowcolumn` 的类，而**单篇文章视图**则使用名为 `widecolumn` 的类。单篇文章视图页面不会生成侧边栏，这使得文章能够占据整个内容区域的宽度。

```
<div class="post" id="post-1">
```

与经典主题类似，这个分区设置了文章的样式和 `post-X` 的标识符，其中 `X` 代表文章的唯一 ID 号。这允许用户自定义特定文章的外观。

```
<h2>文章标题</h2>
```

这部分包含文章的标题代码，由 `<h2>` 标签定义样式。

```
<small>日期</small>
```

日期代码由 `small` 标签包裹并定义样式。

```
<div class="entry">
```

文章内容由 `entry` CSS 选择器内的样式和段落标签组合定义样式。

```
<p class="postmetadata">文章元数据部分</p>
```

[文章元数据部分](https://codex.wordpress.org/Post_Meta_Data_Section)包含关于文章的详细数据，例如日期、时间以及文章所属的分类。

```
<div class="navigation">
```

[上一篇和下一篇链接](https://codex.wordpress.org/Next_and_Previous_Links)在 `navigation` div 中定义样式。它们还包含 `alignleft`（用于上一篇）和 `alignright`（用于按时间顺序的下一篇）类。

在**单篇文章视图**的内容结构中，这些元素的排列发生了变化：

```
<div id="content" class="widecolumn">
  <div class="navigation">
    <div class="alignleft"></div>
    <div class="alignright"></div>
  </div>
  <div class="post" id="post-1">
    <h2>文章标题</h2>
    <div class="entrytext">
      <p>文章内容。</p>
      <p class="postmetadata alt">
        <small>文章元数据</small>
      </p>
    </div>
  </div>
</div>
```

由于没有侧边栏，`widecolumn` 类被设置为让内容横跨整个页面。`navigation` div 被移到了顶部。文章元数据现在被整合到 `entrytext` 父容器中，并且使用了不同的样式，添加了 `alt` 类选择器。

默认主题中的这两个示例，仅仅让您窥见了 WordPress 站点可以进行自定义的无数种方式。

#### 评论

评论可以显示在单篇文章视图中（使用 comments.php 模板文件），也可以显示在弹出窗口中（使用 comments-popup.php 模板文件）。这两种评论的整体样式基本相同。

##### Classic Theme Comments

```
<h2 id="comments">1 Comment
  <a href="#postcomment" title="Leave a comment">»</a></h2>
  <ol id="commentlist">
    <li id="comment-1">
      <p>Hi, this is a comment.</p>
      <p><cite>Comment by Person</cite> </p>
    </li>
  </ol>
  <p>
    <a href='https://example.com/archives/name-of-post/feed/'><abbr title="Really Simple Syndication">RSS</abbr> feed for comments on this post.</a>
    <a href="https://example.com/name-of-post/trackback/" rel="trackback">TrackBack <abbr title="Uniform Resource Identifier">URI</abbr></a>
  </p>
  <h2 id="postcomment">Leave a comment</h2>
  <form action="https://example.com/blog/wp-comments-post.php" method="post" id="commentform">
    <p>
      <input type="text" name="author" id="author" value="" size="22" tabindex="1">
      <label for="author"><small>Name (required)</small></label>
    </p>
    <p>
      <input type="text" name="email" id="email" value="" size="22" tabindex="2">
      <label for="email"><small>Mail (will not be published) required)</small></label>
    </p>
    <p>
      <input type="text" name="url" id="url" value="" size="22" tabindex="3">
      <label for="url"><small>Website</small></label>
    </p>
    <p>
      <small><strong>XHTML:</strong> List of Tags you can use in comments</small>
    </p>
    <p>
      <textarea name="comment" id="comment" cols="100%" rows="10" tabindex="4"></textarea>
    </p>
    <p>
      <input name="submit" type="submit" id="submit" tabindex="5" value="Submit Comment">
      <input type="hidden" name="comment_post_ID" value="1">
    </p>
  </form>
</div>
```

While individual sections of the comments feature styling reference, the Classic Theme has no general comment division or group style reference, one could be easily added.

**#comments h2**

Styles the title at the top of the comments list which says "Comments 4 Leave a Comment", with the latter part of the sentence in a link that jumps to `<h2 id="postcomment">Leave a comment</h2>`.

**#comment-n**

Comments are given a unique ID number, signified here by the letter n. This allows them to be styled individually.

**#comments ol**

This begins the **ordered list** of the comments, counting down from one, and sets the overall style of the comments list.

**#comments li**

Style reference for each comment on the list.

**#comments p**

This paragraph tag styles the actual comments on the comment list.

**#comment cite**

This use of the cite controls the look of the commenter's name. It usually states "Name says:" in the comments list.

**#comments h2** or **#postcomment**

The h2 heading can be styled two ways, as #comments h2 or #postcomment. The latter is used by the "Leave a Comment" link from the top of the comments section, too.

**#commentform**

Style reference for the overall "form" for inputting comments. Each input area has it's own ID.

**#author**

ID reference for the comment author's input area.

**#comments small**

`<small>` 标签在经典主题中的多个位置被使用。这种用法环绕着**评论提交表单**中的文本以及评论中可用的**标签列表**文本。

**#email**

评论作者电子邮件的 ID 引用。

**#url**

评论作者网址的 ID 引用。

**#comment**

评论输入文本区域的 ID 引用。它并不样式化最终生成的评论，仅样式化输入框。

**#comment #submit**

经典主题中有两个提交按钮，分别用于搜索和评论提交。这是提交评论的按钮。

##### 默认主题评论

默认主题的评论功能在 comments.php 和 comments-popup.php 中使用循环查询，根据评论是否开放、关闭以及是否存在来改变部分信息。如果评论开放或关闭且尚未有任何评论，此信息将显示在 `<h3 id="comments">` 标签内。

```
<h3 id="comments">One Response to "Hello world!"</h3> 
<ol class="commentlist">
  <li class="alt" id="comment-1">
    <cite>
      <a href="https://example.org/" rel="external nofollow">Mr WordPress</a>
    </cite> Says:<br>
    <small class="commentmetadata">
      <a href="#comment-1" title="">Date and Time</a>
    </small>
    <p>Hi, this is a comment.</p>
  </li>
</ol>
<h3 id="respond">Leave a Reply</h3>
<form action="https://example.com/blog/wp-comments-post.php" method="post" id="commentform">
  <p>
    <input name="author" id="author" value="" size="22" tabindex="1" type="text">
    <label for="author">
      <small>Name (required)</small>
    </label>
  </p>
  <p>
    <input name="email" id="email" value="" size="22" tabindex="2" type="text">
    <label for="email">
      <small>Mail (will not be published) required)</small>
    </label>
  </p>
  <p>
    <input name="url" id="url" value="" size="22" tabindex="3" type="text">
    <label for="url">
      <small>Website</small>
    </label>
  </p>
  <p>
    <small><strong>XHTML:</strong> You can use these tags:....</small>
  </p>
  <p>
    <textarea name="comment" id="comment" cols="100" rows="10" tabindex="4">
    </textarea>
  </p>
  <p>
    <input name="submit" id="submit" tabindex="5" value="Submit Comment" type="submit">
    <input name="comment_post_ID" value="1" type="hidden">
  </p>
</form>
</div>
```

虽然评论功能的各个部分都有样式引用，但默认主题没有通用的评论分区或组样式引用，不过可以轻松添加。

**h3 #comments**

为"文章回复数量"标题的 `<h3>` 标签设置样式。

**#commentlist ol**

为评论列表的"有序列表"设置样式。

**.alt li** 和 **#comment-n**

评论列表项有两个样式引用。第一个是类 `alt`，第二个是评论 ID 号，此处用字母 n 表示。这允许对它们进行单独样式设置。

**cite**

`cite` 标签包裹"姓名说："以及指向评论作者 URL 的链接。

**.commentmetadata small**

`<small>` 标签具有 `commentmetadata` 类，允许设置文章日期和时间的样式。

**ol #commentlist p**

为评论有序列表内的段落设置样式。

**#respond h3**

为"发表回复"标题设置样式。

**#commentform**

用于输入评论的整体"表单"的样式引用。每个输入区域都有自己的 ID。

**#author**

评论作者输入区域的 ID 引用。

**#comments small**

`<small>` 标签在经典主题中的多个位置被使用。这种用法包裹了**评论提交表单中的文本 _以及_** _标签列表的文本_，这些标签可在评论中使用。

**#email**

评论作者电子邮件的 ID 引用。

**#url**

评论作者网址的 ID 引用。

**#comment**

评论输入文本区域的 ID 引用。它并不样式化最终生成的评论，仅样式化输入框。

**#comment #submit**

经典主题中有两个提交按钮，分别用于搜索和评论提交。这是提交评论的按钮。

##### 弹窗评论

经典主题和默认主题的 `comments-popup.php` 模板文件基本相同。它们都采用[经典主题评论结构](#Default_Theme_Comments)的布局。虽然经典主题使用 `<h2>` 标题，而默认主题在其评论中使用 `<h3>` 标题作为标题，但在 comments-popup.php 模板文件中，两者都使用 `<h2>` 标题标签。

```
<body id="commentspopup">
  <h1 id="header"></h1>
  <h2 id="comments">评论</h2>
  ....经典主题评论部分.....
  ...经典主题页脚....
```

body 标签通过 `#commentspopup` 设置整个页面的样式。`<h2>` 标题开启了评论部分。

如果您对整体主题的页眉和页脚内的标签结构进行了修改，请确保这些结构更改也应用于评论弹窗模板，特别是如果您打算[向公众发布该主题](https://codex.wordpress.org/Designing_Themes_for_Public_Release)。

### 侧边栏

正如您在默认主题中所见，侧边栏是否可见取决于正在使用的模板文件。一般来说，侧边栏可以简单也可以复杂。WordPress 主题通常在侧边栏中以**嵌套列表**的形式展示信息。关于侧边栏的定制，有一份逐步指南：[自定义您的侧边栏](https://codex.wordpress.org/Customizing_Your_Sidebar)，还有更多关于[使用 CSS 样式化列表](https://codex.wordpress.org/Styling_Lists_with_CSS)的信息。

通常，WordPress 侧边栏在一个列表中展示各个部分的标题，而该部分的项目则以嵌套列表的形式显示在标题下方。

经典主题的侧边栏看起来像下面这样（为简化起见，链接已移除）：

```
<div id="menu">
  <ul>
    <li class="pagenav">Pages
      <ul>
        <li class="page_item">Contact</li>
        <li class="page_item">About</li>
      </ul>
    </li>
    <li id="linkcat-1"><h2>Blogroll</h2>
      <ul>
        <li>Blogroll Link 1</li>
        <li>Blogroll Link 1</li>
        <li>Blogroll Link 1</li>
      </ul>
    </li>
    <li id="categories">Categories:
      <ul>
        <li>Category Link 1</li>
        <li>Category Link 2</li>
      </ul>
    </li>
    <li id="search">
      <label for="s">Search:</label>   
      <form id="searchform" method="get" action="/index.php">
        <div>
          <input type="text" name="s" id="s" size="15"><br>
          <input type="submit" id="searchsubmit" value="Search">
        </div>
      </form>
    </li>
    <li id="archives">Archives: 
      <ul>
        <li>Archives Month Link 1</li>
        <li>Archives Month Link 2</li>
      </ul>
    </li>
    <li id="meta">Meta:
      <ul>
        <li>RSS Feed Link</li>
        <li>RSS Comments Feed Link</li>
        <li>XHTML Validator</li>
        <li>XFN Link</li>
        <li>WordPress Link</li>
      </ul>
    </li>
  </ul>
</div>
```

这些内容大多不言自明。每组链接都有其自己的 CSS 选择器：[页面](https://wordpress.org/documentation/article/create-pages/)、分类、归档、搜索和元信息。

#### 页面与链接分类

标记为"博客链接"的[页面](https://wordpress.org/documentation/article/create-pages/)与[链接](https://codex.wordpress.org/Links_Manager)分类，使用[<?php get_links_list(); ?>](https://developer.wordpress.org/reference/functions/get_links_list/)和[<?php wp_list_pages(); ?>](https://developer.wordpress.org/reference/functions/wp_list_pages/)模板标签自动生成标题。

对于**链接**分类，它会为该组链接生成h2标题。这意味着您可以将菜单h2标题的样式设置为与其他标题不同，或者，如果您希望它们看起来一致，请确保菜单h2样式与未自动生成的其他分类样式*匹配*。

**页面**模板标签生成pagenav作为标题，并以新方式标识页面。在多文章和单文章视图中，页面列表项具有class="page_item"来设置这些链接的样式。查看单个页面时，该页面的链接将变为class="current_page_item"，从而可以将其样式设置为与其余页面链接不同。

#### 分类、归档与元数据

其他侧边栏区块的标题，如 _categories_、_archives_、_meta_ 等，并未使用会自行生成标题的模板标签。这些标题是通过 PHP 语句在页面中“打印”出来的。虽然它们可以放入[标题标签](https://codex.wordpress.org/Designing_Headings)内，但 WordPress 使用 `_e()` 函数来显示或 `echo` 这些文本标题，同时标记这些文本为可能的翻译目标。如果你打算[开发主题](https://codex.wordpress.org/Theme_Development)并[公开发布](https://codex.wordpress.org/Designing_Themes_for_Public_Release)，强烈建议使用 echo 函数。

你可以单独或统一为这些标题设置样式。一些主题，如默认主题，将所有这类标题放在 `<h2>` 标签中，使列表标题看起来一致。因此，它们可能为每个区块使用样式引用，也可能不使用。如果你需要改变每个链接区块的外观，可以自行添加样式。

#### Search Form

The search form is found within the searchform.php. It may be found in different locations within the sidebar. To style the overall search form, use the search ID. Here is a list of the individual areas of the search form which may be styled by default. You may add style classes to gain more control over the look of your search form.

```
<li id="search">
  <label for="s">Search:</label>   
  <form id="searchform" method="get" action="/index.php">
    <div>
      <input type="text" name="s" id="s" size="15"><br>
      <input type="submit" id="searchsubmit" value="Search">
    </div>
  </form>
</li>
```

**#search**

The overall style for the search form.

**#search label**

Used to style the label tag, if necessary.

**#searchform**

Used to style the form itself.

**#search div**

This unlabeled div is a child container of the parent container search and maybe styled from within that selector.

**#searchform input**

To style the input area for the search, this selector combination will work.

**#searchsubmit**

_Used by the Default Theme_, this selector may be used to style the **search** or **submit** button.

The search form area, input, and button can be styled in many ways, or left with the default input and "button" look.

#### 元信息源链接

元链接可以显示为文本或代表各类链接的图标。XHTML 和 CSS 验证链接可使用 W3 图标。各类信息源也可用图标表示，或保留为文本形式。具体选择由您决定。侧边栏中使用文本或图标形式的信息源，相关说明详见文章 [WordPress 信息源](https://wordpress.org/documentation/article/wordpress-feeds/)。

### 页脚

页脚位于 footer.php 模板文件中。在默认主题和经典主题中，页脚包含的信息都很少。

**经典主题**

```
<p class="credit">
  <!--15 queries. 0.152 seconds. -->
  <cite>Powered by <a href='https://wordpress.org' title='Powered by WordPress, state-of-the-art semantic personal publishing platform.'> <strong>WordPress</strong></a></cite>
</p>
</div>
```

页脚内容通过 `credit` 类以及 `<p>` 和 `<cite>` 标签进行样式设置。

该标签以 HTML 注释代码的形式显示页面使用的 MySQL 查询次数和页面加载时间。这是为了方便管理员使用，仅在页面源代码中可见。如果您希望将其显示在页面上，请移除[注释](https://codex.wordpress.org/Commenting_Code)。其外观将受到 `<p>` 标签的 `credit` 类样式影响。在模板文件中，它看起来像这样：

```
<!--<?php echo $wpdb->num_queries; ?> queries. 
<?php timer_stop(1); ?> seconds. -->
```

**默认主题**

```
<div id="footer">
  <p>Blogging in the WordPress World is proudly powered by <a href="https://wordpress.org/">WordPress</a><br>
    <a href="feed:http://example.com/feed/">Entries (RSS)</a> and <a href="feed:https://example.com/comments/feed/"> Comments (RSS)</a>.
    <!-- 18 queries. 0.186 seconds. -->
  </p>
</div>
```

默认主题的页脚通过 `footer` ID 和 `<p>` 标签进行样式设置。虽然页脚区域本身可能由 `footer` 设置样式，但 `<p>` 标签控制其中的文本。要为页脚内的 `<p>` 标签设置与页面其他部分不同的样式：

**#footer p {styles}**

## 资源

* [CSS](https://developer.wordpress.org/advanced-administration/wordpress/css/)
* [查找您的 CSS 样式](https://codex.wordpress.org/Finding_Your_CSS_Styles)
* [CSS 故障排除](https://codex.wordpress.org/CSS_Troubleshooting)
* [使用主题](https://wordpress.org/documentation/article/worik-with-themes/)
* [主题开发](https://codex.wordpress.org/Theme_Development)
* [为公开发布设计主题](https://codex.wordpress.org/Designing_Themes_for_Public_Release)
* [WordPress 课程](https://learn.wordpress.org/)
* [博客设计与布局](https://codex.wordpress.org/Blog_Design_and_Layout)
* [深入了解模板标签](https://codex.wordpress.org/Stepping_Into_Template_Tags)