---
title: Getting Started with Hexo -- Creating Posts
toc: true
category: Hexo
tags: 
  - Hexo
date: 2025-04-18 18:11:27
---




In a [previous post](2025-04-09-Migrating-from-Jekyll-to-Hexo.md), I shared my migration from Jekyll to Hexo for my GitHub Pages blog. In this post, I’ll go over some of the common commands I use to create posts and test my blog locally.

## ✍️ Create a New Post or Draft

The commands in Hexo are quite human-readable. To create a new blog post or draft, use:

```bash
$ hexo new post "My Post"
$ hexo new draft "My Draft"
```

This will create a Markdown file in the `source/_posts` folder for posts, or in the `source/_drafts` folder for drafts. The keywords `"post"` and `"draft"` refer to the layout used for the file. You can customize these layouts in the `scaffolds` folder.

You can also create your own custom layout file inside the `scaffolds` folder and use it when creating a new post. For example:

```bash
$ hexo new mylayout "My Layout"
```

To move a draft to the posts folder, use the `publish` command instead of `new`. This will move the file from `source/_drafts` to `source/_posts` :

```bash
$ hexo publish post "My Draft"
```



---

## 🖥️ Run the Blog Locally

To run your blog locally, you first need to generate the static files:

```bash
$ hexo generate
```

Or use the shorthand:

```bash
$ hexo g
```

Then, start the local development server:

```bash
$ hexo server
```

By default, this will start a server at `http://localhost:4000`. Open that link in your browser and you’ll see your website.

Although Hexo is supposed to watch for local changes and update automatically, there are cases where a simple browser refresh doesn’t reflect your changes — especially when you modify configuration files like `_config.yml`. In those cases, it’s best to regenerate the static files and restart the server.

Also, we can use the `clean` command to remove previously generated files, to keep local repository clean.

```bash
$ hexo clean
```





## ✅ Summary

Here’s a quick cheat sheet:

| Action                                | Command                               |
| ------------------------------------- | ------------------------------------- |
| Create a new post                     | `hexo new post "Post Title"`          |
| Create a new file using other layouts | `hexo new <layout name> "Post Title"` |
| Publish a draft to a post             | `hexo publish post "Draft Name"`      |
| Generate static files                 | `hexo generate` or `hexo g`           |
| Run local server                      | `hexo server`                         |
| Clean old files                       | `hexo clean`                          |
