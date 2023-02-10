---
title: "Hugo01 How to modify my website"
date: 2023-01-01
draft: false
description: "About my Hugo Website: Gokarna"
type: "post"
showTableOfContents: true
tags: ["Hugo"]
---

## 1. Gokarna Template

### Introduction

My website is based on a beautiful and simple website framework: [Gokarna](https://github.com/526avijitgupta/gokarna). Developers are [Yash Mehrotra](https://yashmehrotra.com/) and [Avijit Gupta](https://twitter.com/526avijit).

Gokarna focuses on ultimate minimalism and simplicity. 😊

### Basic Guide

- Gokarna Website Template: https://gokarna-hugo.netlify.app/
- [Theme Documentation - Basics](https://main--gokarna-hugo.netlify.app/posts/theme-documentation-basics/)
- [Theme Documentation - Advanced](https://main--gokarna-hugo.netlify.app/posts/theme-documentation-advanced/)

### Contribution

I really love the design style of this website. Thus I will continue [reporting issues](https://github.com/526avijitgupta/gokarna/issues) and providing suggestions. Gokarna's developers are keen on revising, and they accept [pull requests](https://github.com/526avijitgupta/gokarna/pulls) (Though is not that fast, they keep doing it 💕).

## 2. Modify Gokarna

### Standard Page Layout

#### Footer

Footer defines my website footer part `themes/gokarna/layouts/partials/footer.html` 

I simply decorate the footer, and add the [site counter](http://busuanzi.ibruce.info/).

Check the revise of bottom corner of the page. Once updating Gokarna submodule from Github, pay attention to update content the `footer.html`.

```html
<span>.............................................................................................................</span>
    <!-- import counter code -->
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
<span>&copy; {{ now.Year }} {{ .Site.Params.Footer}} | Total Visitors: <a id="busuanzi_value_site_pv">...</a></span>
<span>Made with &#10084;&#65039; using <a target="_blank" href="https://github.com/526avijitgupta/gokarna">Gokarna</a></span>
```

### Music

Register one of the [music player plugin](https://player.lzti.com/admin/#/).

If there is new update, it is required to change the music list in the website of music player plugin.

Add two lines of scripts to `themes/gokarna/layouts/partials/footer.html` 

```html
<script src="https://player.lzti.com/player/js/jquery.min.js" type="text/javascript"></script>
<script src="https://player.lzti.com/api/player/167601724233" id="myhk" key="167601724233" m="1"></script>
```
