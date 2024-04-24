---
layout:     post
title:      "Blog 建置 - Github pages 的使用"
subtitle:   "個人部落格快速建置"
date:       2024-04-22
author:     "PCLiu"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - Blog
---


> 在當今的寫作世界中，創作不只是文字的堆砌，而是如同繪制數字世界的設計師，使用語言的筆觸繪製心靈的圖景。作家從日常生活的點滴中提取靈感，如同探索宇宙的科學家，用文字搭建一個個內心世界。

## 前言

在這個信息爆炸的時代，每個人都有故事要講，每個聲音都值得被聽見。寫部落格不僅是分享知識和經驗的一種方式，更是一種自我表達和創造影響的工具。無論是專業知識、個人生活，還是對世界的獨特見解，通過部落格，我們可以建立連接，影響他人，甚至改變世界。在這本指南中，我們將探索寫部落格的動機，技巧，以及它如何成為個人和職業成長的強大推動力。讓我們一起揭開寫作的力量，看看如何通過文字塑造思想，連接世界。



## Quick start —— 快速開始

在本教學中，我將會一步一步教你使用Github pages 建立自己的部落格。Github pages 是一個基於Jekyll的靜態網站生成器，它可以幫助你快速建立一個個人部落格，並且可以免費部署在Github上。這意味著你可以使用Github pages 來建立自己的部落格，並且可以免費使用Github的服務。

很幸運的是，由作者[Huxpro](https://github.com/Huxpro)提供了一個非常好的模板，我們只需要將他的專案fork到自己的Github上，然後修改一些配置，就可以建立自己的部落格了。

### 第一步

首先打開這個[網址](https://github.com/Huxpro/huxpro.github.io)。

### 第二步

點選右上角的`Fork`按鈕，將這個專案fork到你的Github上。

### 第三步

將Repository name改為`username.github.io`，其中`username`必須是你的Github用戶名。

### 第四步

點選`Settings`，在左側`Code and automation`找到`Pages`，並將`Source`用選單設置為`Github Actions`，  
此時網頁會跳出`Jekyll`的選項，點選`Configure`，系統就會自動生成一個`Jekyll`的`Github Actions`配置文件。

### 第五步

點選右上角的`Commit changes`，等待幾分鐘，你的部落格就建立好了，  
可以在選單裡面的`Actions`裡面查看`Github Actions`的運行情況。

## Customize —— 自定義

在Github上建立好部落格之後，我們可以進行一些自定義的操作，比如修改部落格的樣式，或是添加新的文章，  
這裡我將會示範一些比較重要的自定義操作。

你可以在repo裡面的`_config.yml`文件中修改大部分部落格的配置。

**網站基本屬性設定**
```
# Site settings
title: PCLiu's Blog
SEOTitle: 邦均的部落格 | PC's Blog
header-img: img/home-bg.jpg
email: kevin95120@gmail.com
description: "這裡是 @pcpcliu 的部落格，和你分享我的所見所聞"
keyword: "PCLiu, Machine Learning, Deep Learning, ML, DL, Large Language Model, LLM"
url: "https://cmosinverter.github.io/" # your host, for absolute URL
```

**社群軟體設定**
```
# SNS settings
RSS: false
instagram_username: pcpcliu
github_username: cmosinverter
```
**討論區設定**
```
# Disqus settings
disqus_username: cmosinverter
```
這邊的步驟比較複雜一點，我們需要到[Disqus](https://disqus.com/)註冊一個帳號，然後在這裡填入你的`disqus_username`，  
這樣就可以在部落格上添加討論區了。

**注意**：在開始使用之前必須要先驗證註冊帳號時填入的e-mail，否則討論區無法正常運行。

**Side bar設定**
```
# Sidebar settings
sidebar: true # whether or not using Sidebar.
sidebar-about-description: "做一個有趣的靈魂 <br> Master Student @ NTHU"
sidebar-avatar: https://github.com/cmosinverter.png # use absolute URL, seeing it's used in both `/` and `/about/`
```
**友人連結設定**

```
# Friends
friends:
  [
  #   { title: "乱序（Midare）", href: "http://mida.re/" },
  #   { title: "Ebn Zhang", href: "https://ebnbin.dev/" },
  #   { title: "Kun Qian", href: "http://kunq.me" },
  #   { title: "Sherry Woo", href: "https://sherrywoo.me/" },
  #   { title: "SmdCn", href: "http://blog.smdcn.net" },
  #   { title: "JiyinYiyong", href: "http://tiye.me/" },
  #   { title: "DHong Say", href: "http://dhong.co" },
  #   { title: "尹峰以为", href: "http://ingf.github.io/" },
  ]
```
這邊我是直接把這些設定註解掉，因為邊緣人沒朋友哈。

## Post —— 發表文章

如果想要發表文章，只需要在`_posts`資料夾裡面新增一個`.markdown`文件，文件名的格式建議是`yyyy-mm-dd-title.markdown`，
然後在文件的頭部添加以下的格式：

```
---
layout:     post
title:      "Blog 建置 - Github pages 的使用"
subtitle:   "個人部落格快速建置"
date:       2024-04-22
author:     "PCLiu"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - Blog
---
```
系統就會自動根據你所使用的tag，將比較常使用到的tag自動渲染到部落格的tag頁面，  
下面就可以開始寫文章了，文章的格式可以參考[Markdown](https://markdown.tw/)。

## References

[1] [Github Pages](https://pages.github.com/)

[2] [Jekyll](https://jekyllrb.com/)

[3] [Huxpro](https://github.com/Huxpro/huxpro.github.io)

---

本文是我的第一篇文章，如果您喜歡，請繼續關注我的Blog :)