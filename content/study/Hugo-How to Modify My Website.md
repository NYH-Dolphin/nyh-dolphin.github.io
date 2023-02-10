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

Footer define my website footer part `themes/gokarna/layouts/partials/footer.html` 

I simply decorate the footer, and add the [site counter](https://gitee.com/qiushaocloud/site-counter#%E5%89%8D%E7%AB%AF%E7%AE%80%E6%98%93%E4%BD%BF%E7%94%A8).

Check the revise of bottom corner of the page. Once updating Gokarna submodule from Github, pay attention to update the `footer.html`.

```html
<footer class="footer">
	<!-- add dashline -->
    <div style="margin: 0 auto; width:60%; border:1px dashed #000000e1"></div> 
    </br>

    <span>&copy; {{ now.Year }} {{ .Site.Params.Footer}} |  Made with &#10084;&#65039; using <a target="_blank" href="https://github.com/526avijitgupta/gokarna">Gokarna</a></span>

    <!-- add website counter -->
    <script async src="//githubcdn.qiushaocloud.top/gh/qiushaocloud/site-counter@master/dist/qiushaocloud_site_counter.min.js"></script>
    <script>
        window.localStorage.setItem('qiushaocloud_sitecounter_max_session_duration', 1 * 60 * 60 * 1000);
    </script>
    <span>
        Total visitors: <a id="qiushaocloud_sitecounter_value_site_pv">...</a> | Today's visitors: <a id="qiushaocloud_sitecounter_value_today_site_pv">...</a>
    </span>
</footer>
```

