---
title: HugoBook+GithubAction
bookToc: true
---

# In Windows10, build personal website via Hugo-book and Github Actions

This is blog is about how to use Hugo to create the local website, and then deploy it on GitHub via Github Action. Everything is step by step.

![Baked snailes](snails.png)

## Prerequisites
1. Git
2. Windows 10
3. VScode

## Install Hugo
1. In Windows10, right click **Windows Powershell** and click **run as administrator**.
2. Refer to [How to install chocolatey in Windows](https://www.youtube.com/watch?v=-5WLKu_J_AE) to **install chocolatey**.
3. Refer to [Install Hugo on Windows.](https://gohugo.io/installation/windows/) to **install Hugo** by 
```
choco install hugo-extended
```
4. Make sure Hugo works well by
```
hugo version
```
and we will get the following
```
hugo v0.123.7-312735366b20d64bd61bff8627f593749f86c964+extended windows/amd64 BuildDate=2024-03-01T16:16:06Z VendorInfo=gohugoio
```


## Create the website using Hugo themes
For example, I want to locally work on my website in `C:\myweb` folder. Still in **Windows Powershell**, 
1. Navigate back to `C:` driver, and create a new folder by typing `mkdir myweb`.
2. Create the website by 
```
hugo new site Blogs
```
*Blogs* will be in your github website link, like this *https://huilin-li.github.io/Blogs/*
3. `cd Blogs` \
4. `git init` \
5. Refer to [Hugo Book Theme](https://themes.gohugo.io/themes/hugo-book/) to use hugo-book theme by
```
git submodule add https://github.com/alex-shpak/hugo-book themes/hugo-book
```
6. Till now, if we run `hugo server`, we will get a plain website locally.

In **VS code**, open `Blogs` folder.

7. 




## References
1. [How to install chocolatey in Windows](https://www.youtube.com/watch?v=-5WLKu_J_AE)
2. [Hugo with Git Hub Pages on Windows](https://www.youtube.com/watch?v=JvvFd1JBeQU&t=127s)
3. [Using GitHub Pages with Actions to deploy Hugo sites in seconds - Tommy Byrd // HugoConf 2022](https://www.youtube.com/watch?v=Z_7RIuf_Z-Q)
4. [Hugo Book Theme](https://themes.gohugo.io/themes/hugo-book/)
5. [Host on GitHub Pages](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
6. [【Hugo】hugo-book主题使用](https://hongmao.run/blog/post/010-hugo-book/)

