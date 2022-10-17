---
title: "Day 09-1 - CloudFront 內容發送網路 CDN"
date: 2022-09-24T23:49:42+08:00
draft: false
tags: [ '14th鐵人賽', 'AWS', 'AWS ELB' ]
type: posts

toc:
  enable: true
---

[TOC]

# Elastic Load Balancer

*Elastic Load Balancer 彈性附載平衡器*，是 AWS 提供的 Load Balance，可以實現 Layer 4 和 Layer 7 的負載平衡，以下簡稱 Elastic Load Balancer 為 ELB



如果當服務很熱門，一天的中午時段有幾十萬個人來使用你的服務，只有一台伺服器，真的有辦法 Cover 這麼多的流量? 

而且真的只能用一台服務器來提供這些處理嗎?

答案是否定的，我們可以開很多台後端主機來協助處理動態的流量。



舉例來說.... 你有看過台鐵的售票口只有一個嗎?  如果有的話，~~可能是服務人員輪班去吃飯了~~ 可能中秋連假回家烤肉只能吃空氣了。



## CloudFront & Elastic Load Balancer 的差異?

前面有提到， CloudFront 流量傳輸上的優化、靜態內容的緩存。

如果流量需要的是動態內容，還是需要丟到伺服器，就需要 ELB 的幫忙了。

因此 ELB 最主要的任務，就是 ***將流量分散導流*** 並有效地到達你要的服務上面。



這邊畫一個表格來說明 CloudFront 與 Elastic Load Balance 的差別:

| CloudFront                     | Elastic Load Balancer          |
| ------------------------------ | ------------------------------ |
| 針對靜態流量做優化             | 針對動態流量去做優化           |
| 針對使用者存取公司服務前的端口 | 開始指派後端服務前的工作分配器 |





## Health Check

還記得哪個篇章也有提到 Health Check 嗎?

沒錯 Route 53 有 Health Check 的功能，跟 Route 53 一樣，能確定後面的 Server 是否還活著。

通常 ELB 後面對應的是一個叢集，透過定期用 HTTP 的方式詢問後面的 Instance。

只要其中有一台 Instance 掛點，ELB 就不會將流量導到那台 Instance。



這個設定需要由我們來設定，提供網頁能執行 health check 的 endpoint 給 ELB 做設定。



## 加解密

ELB 提供 *流量解密 (SSL Negotiation)* 的功能，讓 ELB 後面的 Instance 不用在特別處理解密。

而且相對較為安全，因為不需要將私鑰放到每個 Instance 裡面。



## 黏性會話 - Sticky Session

V2 ELB 才有這個功能，我這邊說明一下當有多個 Instance 的時候，可能會遇到的狀況。

就是使用者登入系統的時候，Session 只記錄某一台 Instance 裡面。這時候使用者操作系統，ELB 是有可能導到另外一個 Instance 上面的，這時候因為 Session 沒有在這台 Instace 儲存導致系統誤以為使用者根本沒有登入。

這時候 Sticky Session 就派上用場了！可以記錄用戶的操作，確保使用者固定使用它登入儲存 Session 的那台 Instance 裡面。



> 僅有 二代 ELB 才有提供

> ※建議採取 Stateless 架構，使用統一的共享儲存體來儲存 Session 資訊。
>
> 現在常見的會用資料庫或是緩存體(如: Redis) 儲存使用者登入訊息，這樣才能達成最大程度的擴充。



## Access Log

在 AWS 上，只要是流量服務相關的，一定有 Log 的應用程式。

例如: VPC 提供 Flow Logs; CloudFront 也有提供流量 Log。

ELB 提供 Access Log 來監控外部流量的存取。



## 封包改動
經過 ELB 的封包，會改動一些內容。譬如會增加一個 Request Header  `X-Forwarded-For` *(XFF)* ，用來判斷客戶端最原始的 IP 位址。




## ELB 移除時，連線中的用戶會不會馬上受到影響?
答案是，不會。在 AWS 可以啟用 Connection Draining (連線耗盡)，避免用戶在 ELB 刪除時馬上受到影響，且預設為 300 秒，最長可以設定到 3600 秒。

當刪除 ELB 時， 註冊在此 ELB 的 Instance 會取消跟此 ELB 的連結。當超過設定的秒數時，ELB 就會強制關閉連線。



# 一代目 & 二代目 ELB

- V1
  - Classic Load Balance
    - TCP
    - SSL/TLS
    - HTTP / HTTPS
- V2
  - Network Load Balance
    - TCP 
    - UDP
    - TLS
  - Application Load Balance
    - HTTP / HTTPS
    - gRPC
  - Gateway Load Balance
    - IP



## 二代目 ELB

比第一代 ELB 提供了以下:

- 將 Port 轉發到同一個 EC2 Instance 但是不同的 container (在 EC2 Instance 安裝 Docker 執行容器化程式)
- 黏性會話 - Sticky Session

Application Load Balance 是  Layer 7 應用層的附載平衡器

Network Load Balancer 是 Layer 4 網路層的附載平行器

> 越偏向上方第七層的負載平衡器可以提供**更多 CPU 運算的能力與彈性**
>
>    越偏向下方第四層的負載平衡器則是可以**針對IP與封包做出有效的分流**





# 參考資料

- [30天鐵人賽介紹 AWS 雲端世界 - 15:　EC2的網路負載平衡服務 Elastic Load Balancing(ELB) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天 (ithome.com.tw)](https://ithelp.ithome.com.tw/articles/10192245)
