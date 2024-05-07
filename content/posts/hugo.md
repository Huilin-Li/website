+++
title = "Hugo commands"
description = ""
tags = ["script", "issue"]
categories = ["Hugo",]
+++ 

Some commands and issues you might want to know when building the website using Hugo, including inserting images and resize and center images,adding emoji, travel map, linking to pages and titles, and so on. 

# SUMMARY
- [use themes as your own repositories]({{<ref "#git">}})
- [insert and resize images]({{<ref "#images">}})
- [center images]({{<ref "#center">}})
- [add emoji]({{<ref "#emoji">}})
- [add travel map]({{<ref "#map">}})
- [Why my local website doesn't update after I update my markdown files?]({{<ref "#update">}})
- [link pages and tiles]({{<ref "#link">}})
- [Add Giscus comment in Hugo-book theme]({{<ref "#Giscus">}})

<center>{{<figure src="../images/mona.png" alt=" ">}}</center>


## Remove git in themes, and use themes as your own repositories.{#git}
```t
 git rm --cached hugo-book -f
 ```
So that, we could modify, git add, and git push the theme repository as our own.


## How to insert images and resize images?{#images}

The file tree is like
```
    ├─content
    │  │  _index.md
    │  └─posts
    │      │  hugo.md
    │      │  _index.md
    │      └─images
    │           Nice.png
```

In `hugo.md` file, we add Nice.png and scale it by
```tpl
{{</* figure src="../images/Nice.png" width="400" alt=" "  */>}}
```
{{< figure src="../images/Nice.png" width="400" alt=" ">}}

{{< hint info >}}  
**Why we need to add `../` in front of `images` folder, altough `hugo.md` file and `images` folder are at the same level?**

Because Hugo system sees `hugo.md` as a foler too. However, Hugo system sees `_index.md` as a file! Therefore, If we want to insert iamges in `_index.md` file, the command will be like:

```tpl
{ {< figure src="../images/Nice.png" width="400" alt=" " >} }
```
{{< /hint >}}

## Center the image?{#center}
```tpl
<center>{ {<figure src="../images/Nice.png" width="400" alt=" ">} }</center>
```
At the same time, in `hugo.toml`, add
```tpl
[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true
```



## How to add emoji?{#emoji}
Directly copy emoji icon 👋 and paste to `.md` file, instead of using `:wave:`. 
> Emoji Copy&Paste: 
> https://emojidb.org/number-emojis

## How to add a travel map in Hugo?{#map}
Refer to [this article](https://www.thecoffeemachine.net/writing/adding-maps-to-hugo-blogs-with-osm/)
{{< hint info >}}
**NOTE:**  

In `{ {< openstreetmap mapName="<your map name>" >} }`, the `<your map name>` is not the name you give to your map, but the name in the website link of your map. For example, I created a map whose name is **Travel**, I need to use **travel_1036974** in the hugo shortcode.
{{< figure src="../images/map.jpeg" width="400" alt="umap">}}
{{< /hint >}}


## Why my local website doesn't update after I update my markdown files?{#update}
Good question. I also find my local website does not update although I have save my changes. I am not clear the excat reason, but It works well if I `Press Ctrl+C to stop` the local host, and `hugo server` again, I can see the updated website in `http://localhost:1313/`.


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

## Add Giscus comment in Hugo-book theme{#Giscus}
1. Create a new public github repository, e.g. `site-comment`.
2. [Enable the discussions feature](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/enabling-or-disabling-github-discussions-for-a-repository).
3. [Install the giscus app](https://github.com/apps/giscus)

For example,
<center>{{<figure src="../images/giscus1.PNG" width="400" alt=" ">}}</center>

4. Configure giscus codeblock. Go to [Giscus App](https://giscus.app/), and get the anable Giscus.
<center>{{<figure src="../images/giscus2.png" width="500" alt=" ">}}</center>
<center>{{<figure src="../images/giscus3.png" width="600" alt=" ">}}</center>
5. Add the <code>script</code> to <code>themes\hugo-book\layouts\partials\docs\comments.html</code>, like this
<center>{{<figure src="../images/giscus4.png" width="600" alt=" ">}}</center>

6. `bookComments: false` controls whether you want to display comment in this page.











## References
1. [hugo book demo](https://hugo-book-demo.netlify.app/docs/shortcodes/hints/)
2. [【Hugo】hugo-book主题使用](https://hongmao.run/blog/post/010-hugo-book/)
3. [hugo博客 文内插入图片](https://lysandert.github.io/posts/blog/blog_insert_pic/) 
4. [How to render markdown url with .md to correct link](https://discourse.gohugo.io/t/how-to-render-markdown-url-with-md-to-correct-link/26372)
5. [Linking pages in Hugo/markdown](https://stackoverflow.com/questions/33225067/linking-pages-in-hugo-markdown)
6. [Adding comments system to a Hugo site using Giscus](https://www.justinjbird.me/blog/2023/adding-comments-to-a-hugo-site-using-giscus/)


