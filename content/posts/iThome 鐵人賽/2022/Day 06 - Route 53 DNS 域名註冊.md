---
title: "Day 06 - Route 53 DNS 域名註冊"
date: 2022-09-21T21:25:26+08:00
draft: false
tags: [ '14th鐵人賽', 'AWS', 'AWS Route53', '免費 DNS' ]
type: posts

toc:
  enable: true
---

[TOC]



# 註冊免費的 DNS 服務

這邊我使用 Freenom 註冊一個免費的網域，可以免費使用 12 個月。

這就已經很夠我們做測試用途了。

建立方式請參考 *參考資料*。



# AWS Route 53

是 AWS 提供的 DNS 服務。可以透過 Route 53 購買網域名稱，將網域名稱與指定的伺服器做綁定。

DNS 最重要的資源紀錄可參考下面的 *參考資料*，這裡我僅說明 **A Record、CName** 這兩種。

外加 Route 53 透有的 **Alias**。





## A Record、CName、Alias

A Record: 把網域名稱直接指向伺服器的 IP。

CName: 把網域名稱轉換成另外一個網域名稱。

Alias: 指派 AWS 服務，其底層是 A Reocrd。



特別說一下 Alias，有些 AWS 服務會提供一個隨機域名，有時我們會希望用我們自家的域名來做導引。此時就會有一段轉發的過程，大約可能會多個 10~50 ms。

這時候，我們就可以使用 Alias 來減少轉發的過程。





## 導流策略

Route 53 擁有 Health Cehck 的功能，能確定對應的 Server 是否還活著。也因為這個機制，可以把同一個 DNS 指向到多個伺服器。



### 路由策略

在導流的時候，可以依據一些策略去決定網路路由怎麼走，可以參考以下資料表:



| 公司客製                                    | 地理                                            | 效能                                    |
| ------------------------------------------- | ----------------------------------------------- | --------------------------------------- |
| Simple routing policy (簡便路由)      | Geolocation routing policy (地理位置路由) | Latency routing policy (延遲路由) |
| Weighted routing policy (加權路由)    | Geoproximity routing policy (地理位置鄰近性)    | Failover routing policy (故障轉移)      |
| Multivalue answer routing policy (多值回答) |                                                 |                                         |



常見以下三種:

- Failover routing policy 
  - 指向主要與備用位置，當主要位置失效時，會自動切換到備用位置。
- Latency routing policy
  - 依據對 Client 端而言，最少連線延遲，轉發流量到不同伺服器。
- Geolocation routing policy 
  - 可依據 Client 的地理位置進行流量的分發。
  - 希望不同地理位置的用戶體驗一致，可以採用此策略。

其他的策略說明如下:

- Simple routing policy
  - 為此網域執行指定功能的單一資源，如果希望所有用戶端收到相同的回應。例如 web.ken-hong.tk 提供網站服務。
- Weighted routing policy 
  - 有多個執行相同任務的資源，且要指定移至每個資源的流量比例。例如，兩個或多個 EC2 執行個體。

- Multivalue answer routing policy
  - 想要隨機回傳最多 8 個正常的紀錄來回應 DNS 查詢。

- Geoproximity routing policy 
  - 當你想根據你的資源的地理位置來決定路由，可選擇將資源的流量轉移到另一個位置的資源。




# 建立託管區域 (Hosted Zones)

類型有兩種: 公有託管區域、私有託管區域。

私有託管區域可以在 VPC 中建立私有 DNS，供內部 Instance 使用，不會將 DNS Record 公開。







# 參考資料

- [freenom 免費網域申請](https://lab.twidc.net/freenom-%E5%85%8D%E8%B2%BB%E7%B6%B2%E5%9F%9F%E7%94%B3%E8%AB%8B/)
- [DNS資源紀錄(Resource Record)介紹](http://dns-learning.twnic.net.tw/bind/intro6.html)
- [What is the maximum number of CNAME records per hosted zone in Route 53?](https://stackoverflow.com/questions/72391032/what-is-the-maximum-number-of-cname-records-per-hosted-zone-in-route-53)
- [How to Use GoDaddy Domains with AWS Route 53 Hosted Zones](https://www.youtube.com/watch?v=zFuluVTsF14)
