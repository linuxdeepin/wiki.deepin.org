---
title: How to contribute documents
description: providing reliable development documentation and user guides to the deepin Community
published: true
date: 2023-10-19T04:22:23.084Z
tags: contribute, contribute documents, how to contribute, how to participate
editor: markdown
dateCreated: 2023-10-19T04:22:23.084Z
---

There are many ways to contribute to the deepin Community.  In addition to the well-known [code contribution](https://wiki.deepin.org/en/About_Deepin/contribute_code) path, providing reliable development documentation and user guides to the community is also one of the ways to contribute to the open source community.

There are currently 3 main documentation platforms for the deepin community:
- [wiki](https://wiki.deepin.org/en/About_Deepin/Deepin_Community)：All documentation related to deepin, including but not limited to explanations of terms, operating instructions, development documentation, and other types of documentation that tend to be descriptive in nature; [wiki贡献说明](https://wiki.deepin.org/zh/00_wiki/02_wiki%E7%BC%96%E8%BE%91%E8%A7%84%E5%88%99%E8%AF%B4%E6%98%8E)
- [Blog](https://blog.deepin.org/)：Mainly collect and publish some technical blog articles and insights with deep content; [如何写博客](https://blog.deepin.org/about/)
- [Developer Platform](https://docs.deepin.org/)：Empower developers to build the ecosystem of deepin, including but not limited to DTK docs, Qt docs, Linglong docs, and more;

## Contribute in the deepin Wiki
https://wiki.deepin.org is the official encyclopedia site of the deepin Community for sharing knowledge about deepin and Linux.

### Fundamental Principle
1. There is no restriction on the content of the entries, and any content that is helpful in using deepin (including but not limited to personal experience, works, tutorials, Q&A, etc.) can be included;
2. Created content is forbidden to contain any advertisements, insulting words, sensitive words;
3. Any content shall not violate national laws and regulations;

### Entry Creation Requirements
- Title: Try to make it easy to understand, and it is best to be able to see at a glance what knowledge points are explained in the content;
- Content: There is no uniform format, as long as it is readable and easy for users to read;
- Image: The recommended image formats are jpg and png, with a size not exceeding 150K.  To avoid disputes, please use personal original images or copyright-free images that comply with the CC0 license;

### Moving and Deleting entries
The movement and renaming of pages may render references to other locations ineffective, so it is recommended to carefully consider appropriate titles when creating pages and place them in the correct locations. If relocation or renaming is indeed necessary, move the page to the appropriate location to minimize the need for further movement/renaming of the page.

### Apply for Editing privileges of deepin Wiki
Currently, the deepin Wiki is open to the public, and all interested parties who are committed to providing documentation for the deepin Operating System can apply. [wiki编辑人员申请](https://wiki.deepin.org/zh/00_wiki/01_wiki%E7%BC%96%E8%BE%91%E4%BA%BA%E5%91%98%E7%94%B3%E8%AF%B7)

**Notice for Account Application:**
1. Please make sure to fill in the deepin forum Nickname, not forum ID (through the forum - account center, you can see the nickname information)；
2. Please make sure your forum account is tied to an email address；
3. Please use your forum account to log in once on the wiki;

## Participate in deepin Community Blog contributions
### Environment Configuration
Currently, deepin community blog platform uses [HUGO](https://gohugo.io/) as the article platform tool for static generation of blog articles, and eventually use GitHub Pages to present web pages. So the whole process you just need to follow the normal way of using HUGO.

For more information about how to configure and use HUGO, you can refer to the official HUGO tutorials [Quick Start](https://gohugo.io/getting-started/quick-start/)

### Writing blogs
After HUGO installation is complete, the common commands are as follows (executed in the root directory of this source repository):

```
$ hugo server # Start a local server to preview the content of the site in its current state
$ hugo new posts/my-post.md # Create a new article for editing
$ hugo server -D # Enable local servers and be able to preview articles with a status of draft (`draft: true`)
$ hugo -D # Generate static pages, the generated files will be located in the public directory
```

When you create a post, you create it in the format <category>/<filename>. <format>, in the example above, the category is posts, the filename is my-post, and the format is md (markdown). When you create an article, you will use [YAML front-matter](https://gohugo.io/content-management/front-matter/) to markup some meta information of the article by default, please note that articles in draft status will not be shown eventually.

### Suggestions for Contributions
Articles submitted to this blog need to be reviewed by the relevant staff, and once they have been approved, it means that they have been submitted. It is important to note that articles submitted must be related to the development of the deepin Community. For blogs with personal thoughts or essays related to deepin Community contributions, you can consider posting them to the planet.deepin.org aggregation platform, and the articles on this blog platform will also appear on planet.deepin.org.

When posting your articles, it is recommended to use tags to mark the topics that your articles are related to, so that readers can access your articles more easily. For example tags: ["Continuous Integration"] or tags: ["Guide Documentation", "CMake"]. It is also recommended to include the author's information in the article's meta information, e.g. authors: ["Zhangsan"] (more than one person is allowed).