---
title: Hugo Commands
bookToc: true
weight: 2
---

## How to insert images and resize images?{#images}
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

> Why we need to add `../` in front of `images` folder, altough `hugo.md` file and `images` folder are at the same level? 
> Because Hugo system sees `hugo.md` as a foler too. However, Hugo system sees `_index.md` as a file! Therefore, If we want to insert iamges in `_index.md` file, the command will be like:
> ```tpl
> {{</* figure src="./images/Nice.png" width="400" alt=" " */>}}
> ```



## How to add emoji?{#emoji}
Directly copy emoji icon 👋 and paste to `.md` file, instead of using `:wave:`. 
> Emoji Cheat Sheet: 
> https://github.com/ikatyang/emoji-cheat-sheet/blob/master/README.md

## How to add a travel map in Hugo?{#map}
Refer to [this article](https://www.thecoffeemachine.net/writing/adding-maps-to-hugo-blogs-with-osm/)
> **_NOTE:_**  
> In `{{</* openstreetmap mapName="<your map name>" */>}}`, the `<your map name>` is not the name you give to your map, but the name in the website link of your map. For example, I created a map whose name is **Travel**, I need to use **travel_1036974** in the hugo shortcode.
> {{< figure src="../images/map.jpeg" width="400" alt="umap">}}

## Why my local website doesn't update after I update my markdown files?{#update}
Good question. I also find my local website does not update although I have save my changes. I am not clear the excat reason, but It works well if I `Pree Ctrl+C to stop` the local host, and `hugo server` again, I can see the updated website in `http://localhost:1313/`.


## How to link pages and tiles?{#link}
I am in `now.md` file now, and there is another `page.md` which has `#Title I want to link`. I want to link to `page.md` and link to `#Title I want to link`.
In `page.md` file, add an anchor:
```tp1
# Title I want to link {#anchor}
```
In `now.md` file, add commands:
```tpl
[text] ( {{</* ref "./pages.md" */>}} ) # link to one page
[text] ( {{</* ref "./pages.md#anchor" */>}} ) # link to the title
```


## I want to add a comment section (via Cactus Comments).{#comment}
1. Register a Matrix account via https://app.element.io/#/login. 
2. Login.
3. `register huilinBlog` by
{{< figure src="../images/comment1.PNG" width="500" alt=" ">}}
{{< figure src="../images/comment2.PNG" width="500" alt=" ">}}
{{< figure src="../images/comment3.PNG" width="500" alt=" ">}}
The name we will use to add comment section is **huilinBlog**.
4. Create and add `chat.html`. In my case, my root directory is `Blogs/` and `chat.html` is added to `Blogs/layouts/shortcodes/` in which `shortcodes` folder is also created by yourself. 
```
├─.github
├─layouts
│  └─shortcodes
└─themes
    ├─hugo-book
    │  ├─.github
    │  ├─layouts
    │  │  ├─shortcodes
```
> **NOTE:** 
> Do not add `chat.html` to `themes/layouts/shortcodes/`, because we are not authorized to modify the the theme github via git push and so on.
5. Copy the following and paste to `chat.html`, and replace `<YOUR-SITE-NAME>` with `huilinBlog`, like `siteName: "huilinBlog",`
```
<script type="text/javascript" src="https://latest.cactus.chat/cactus.js"></script>
<link rel="stylesheet" href="https://latest.cactus.chat/style.css" type="text/css">
<div id="comment-section"></div>
<script>
initComments({
  node: document.getElementById("comment-section"),
  defaultHomeserverUrl: "https://matrix.cactus.chat:8448",
  serverName: "cactus.chat",
  siteName: "<YOUR-SITE-NAME>",
  commentSectionId: "{{ index .Params 0 }}"
})
</script>
```
6. Add the chat comment section to post. 
```
{{</* chat cactus-comments */>}}
```
The **cactus-comments** is the name that will be shown in your cactus chat website. For example, my case `{{</* chat test */>}}`, so my cactus chat website is like

7. Let us test! 
 {{<chat test>}}





## References
1. [hugo book demo](https://hugo-book-demo.netlify.app/docs/shortcodes/hints/)
2. [【Hugo】hugo-book主题使用](https://hongmao.run/blog/post/010-hugo-book/)
3. [hugo博客 文内插入图片](https://lysandert.github.io/posts/blog/blog_insert_pic/) 
4. [How to render markdown url with .md to correct link](https://discourse.gohugo.io/t/how-to-render-markdown-url-with-md-to-correct-link/26372)
5. [Linking pages in Hugo/markdown](https://stackoverflow.com/questions/33225067/linking-pages-in-hugo-markdown)
6. [Added Cactus Comments to Hugo](https://novores.github.io/en/post/cactus-comment/)
7. [Cactus Comments](https://cactus.chat/docs/getting-started/quick-start/)

 ## Thanks!
 Really appreciate your time for reading my blog, and bless you enjoy your day! If you have any suggestions or comments, please leave them below! (You don't need to login to post😉, feel free to add posts.)
 {{<chat hugo>}}