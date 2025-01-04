---
title:       "Hugo Quick Start"
subtitle:    ""
description: "A quik start guide for Hugo"
date:        "2025-01-04T16:35:35Z"
author:      "Hao Xu"
image:       ""
tags:        ["Web development", "Hugo"]
categories:  ["Tech" ]
draft:       false
---

# Create the directory structure for your project in the quickstart directory.
在quickstart目录中为您的项目创建目录结构。
```bash
hugo new site quickstart
```

# Start Hugo’s development server to view the site.
启动Hugo的开发服务器来查看站点。

```bash
hugo server
```

# Add content   
添加内容

Add a new page to your site.
向您的网站添加新页面。
```bash
hugo new content content/posts/my-first-post.md
```

Hugo created the file in the content/posts directory. Open the file with your editor.
Hugo 在content/posts目录中创建了该文件。使用编辑器打开文件。

```markdown
+++
title = 'My First Post'
date = 2024-01-14T07:07:07+01:00
draft = true
+++
```

Notice the draft value in the front matter is true. By default, Hugo does not publish draft content when you build the site. Learn more about draft, future, and expired content.
请注意，前面的内容中的draft值是true 。默认情况下，Hugo 在您构建网站时不会发布草稿内容。了解有关草稿、未来和过期内容的更多信息。

Add some Markdown to the body of the post, but do not change the draft value.
在帖子正文中添加一些Markdown ，但不要更改draft值。

```markdown
+++
title = 'My First Post'
date = 2024-01-14T07:07:07+01:00
draft = true
+++

## Introduction

This is **bold** text, and this is *emphasized* text.

Visit the [Hugo](https://gohugo.io) website!
```

Save the file, then start Hugo’s development server to view the site. You can run either of the following commands to include draft content.
保存文件，然后启动Hugo的开发服务器来查看站点。您可以运行以下任一命令来包含草稿内容。

```bash
hugo server --buildDrafts
hugo server -D
```

View your site at the URL displayed in your terminal. Keep the development server running as you continue to add and change content.
通过终端中显示的 URL 查看您的站点。当您继续添加和更改内容时，请保持开发服务器运行。

When satisfied with your new content, set the front matter draft parameter to false.
当您对新内容感到满意时，请将 front Matter draft设置为false 。

# Reference
https://gohugo.io/getting-started/quick-start/
