---
title: Hugo
bookToc: true
weight: 9
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

## How to add a travel map in Hugo?
Refer to [this article](https://www.thecoffeemachine.net/writing/adding-maps-to-hugo-blogs-with-osm/)
> **_NOTE:_**  
> In `{{</* openstreetmap mapName="<your map name>" */>}}`, the `<your map name>` is not the name you give to your map, but the name in the website link of your map. For example, I created a map whose name is **Travel**, I need to use **travel_1036974** in the hugo shortcode.
> {{< figure src="../images/map.jpeg" width="400" alt="umap">}}

## Why my local website doesn't update after I update my markdown files?
Good question. I also find my local website does not update although I have save my changes. I am not clear the excat reason, but It works well if I `Pree Ctrl+C to stop` the local host, and `hugo server` again, I can see the updated website in `http://localhost:1313/`.




## References
1. [hugo book demo](https://hugo-book-demo.netlify.app/docs/shortcodes/hints/)
2. [【Hugo】hugo-book主题使用](https://hongmao.run/blog/post/010-hugo-book/)
3. [hugo博客 文内插入图片](https://lysandert.github.io/posts/blog/blog_insert_pic/) 