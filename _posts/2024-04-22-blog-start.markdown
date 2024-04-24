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



#### Quick start —— 快速開始

在本教學中，我將會一步一步教你使用Github pages 建立自己的部落格。Github pages 是一個基於Jekyll的靜態網站生成器，它可以幫助你快速建立一個個人部落格，並且可以免費部署在Github上。這意味著你可以使用Github pages 來建立自己的部落格，並且可以免費使用Github的服務。

很幸運的是，由作者[Huxpro](https://github.com/Huxpro)提供了一個非常好的模板，我們只需要將他的專案fork到自己的Github上，然後修改一些配置，就可以建立自己的部落格了。

**第一步**

首先打開這個[網址](https://github.com/Huxpro/huxpro.github.io)。

**第二步**

點選右上角的`Fork`按鈕，將這個專案fork到你的Github上。

**第三步**

將Repository name改為`username.github.io`，其中`username`必須是你的Github用戶名。

**第四步**

點選`Settings`，在左側`Code and automation`找到`Pages`，並將`Source`用選單設置為`Github Actions`，  
此時網頁會跳出`Jekyll`的選項，點選`Configure`，系統就會自動生成一個`Jekyll`的`Github Actions`配置文件。

**第五步**

點選右上角的`Commit changes`，等待幾分鐘，你的部落格就建立好了，  
可以再選單裡面的`Actions`裡面查看`Github Actions`的運行情況。

#### Customize —— 自定義






## References

[1] [Github Pages](https://pages.github.com/)

[2] [Jekyll](https://jekyllrb.com/)

[3] [Huxpro](https://github.com/Huxpro/huxpro.github.io)

---

本文是我的第一篇文章，如果您喜歡，請繼續關注我的Blog :)