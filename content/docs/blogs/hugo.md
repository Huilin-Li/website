---
title: Hugo
bookToc: true
weight: 2
---

# How to use Hugo-book theme

## How to insert images and resize images?
The file tree is like
```
    ├─content
    │  │  _index.md
    │  │
    │  └─docs
    │      ├─blogs
    │      │  │  hugo.md
    │      │  │  _index.md
    │      │  │
    │      │  └─images
    │      │          Nice.png
```

In `hugo.md` file, we add Nice.png and scale it by
```tpl
{{</* figure src="../images/Nice.png" width="400" alt=" " */>}}
```
{{< figure src="../images/Nice.png" width="400" alt=" ">}}

## How to add emoji?
Directly copy emoji icon 👋 and paste to `.md` file, instead of using `:wave:`.






## References
1. [hugo book demo](https://hugo-book-demo.netlify.app/docs/shortcodes/hints/)
2. [【Hugo】hugo-book主题使用](https://hongmao.run/blog/post/010-hugo-book/)
3. [hugo博客 文内插入图片](https://lysandert.github.io/posts/blog/blog_insert_pic/)