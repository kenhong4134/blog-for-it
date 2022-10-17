---
title: "Day 08 - 高併發架構"
date: 2022-09-23T22:31:17+08:00
draft: false
tags: [ '14th鐵人賽', 'AWS', '系統架構' ]
type: posts

toc:
  enable: true
---


[TOC]

# 傳統的高併發架構

通常架構圖如下，會有一個 Load Balance 負責接收 Client 端的請求，並指派到各個伺服器；伺服器會依照不同的工作內容切分不同群組，群組之間的溝通會用常見的 Message Queue 來溝通，最後為了避免每次都會需要進到 Database 查找資料，會將資料寫入記憶體緩存系統，加快下次的查詢。

![經典高併發架構.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/經典高併發架構.drawio.png)





AWS 可以設定 ***漸進式擴張*** 的方式，動態的調整要開多少台伺服器。





# AWS 的高併發架構

最常見的三種服務: ELB, Auto Scaling, EC2

另外可能會搭配 CloudFront 作為原本的 CDN、DynamoDB 作為資料庫、ElastiCache 這類的分散式記憶體緩存系統。

![AWS-高併發架構.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-高併發架構.drawio.png)



# C10K 問題

關於高併發的架構，有很多討論 C10K 或是 C1000K 的問題。

C10K 就是單機同時處理 1 萬個請求。現在慢慢的是要達成 C10M 個請求的併發。

這個實際達成達成的方法水有點深，若在不討論雲端架構下的前提下，若在本地端達成是需要考慮到許多問題的。

這也是雲端的優點，你不太需要考慮到本地伺服器的問題，只需要專注在自身的軟體開發流程。







# 參考資料

- [35|基础篇：C10K 和 C1000K 回顾](https://zsy619.github.io/post/35%E5%9F%BA%E7%A1%80%E7%AF%87c10k-%E5%92%8C-c1000k-%E5%9B%9E%E9%A1%BE/)