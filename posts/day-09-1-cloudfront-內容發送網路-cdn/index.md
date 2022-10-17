# Day 09-1 - CloudFront 內容發送網路 CDN


[TOC]

# CloudFront

***簡單來說，就是雲端的前門。***



在軟體的系統架構，很多會採用三層式架構 (網路、運算、儲存)。

可以參考前一天所討論到的架構圖。

但是我們可能會遇到以下挑戰:

- DDOS 攻擊
- 部分流量都在瀏覽靜態頁面



所以我們就會思考:

- 如何快速解決殭屍流量 DDoS 攻擊?
- 瀏覽靜態網頁的資料，真的有需要每次都連後端主機?
- 如何在於不改動程式碼的情況下，調整架構就可以達成?



因此 CloudFront 的三個特性就是: **多點接收、緩存、傳輸加速**



其中傳輸加速是指流量傳輸上的優化，以現實生活來舉例，就是從原本的省道直接上高速公路的概念，從時速 40-50 公里直接飆上時速 100 公里。





# Distribution, Edge Location, Origin



Distribution 會在各大洲，各個 Distribution 會有各自的 Edge Location。

Edge Location 會以優化過的連線方式向我們的內容伺服器( Web, AP)取得內容，並作緩存。

加快下次使用者讀取重複內容時，可直接從緩存內取得資料。

就以網路流量來說，就是:

​	***User*** →  ***Edge location*** → ***Origin***



以架構圖來看，就是長的如下:

![AWS-CloudFront.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-CloudFront.drawio.png)



***Distribution*** 我們可以視為 ***Edge Location*** 的群組

提供服務的伺服器，我們稱為 ***Origion***，只有在需要存取動態內容 (例如取得資料庫資料) 才會用到。







# 協定

CloudFront 提供以下兩種協定:

- RTMP 影片串流
- HTTP 網頁應用

你可以在 Distribution 視情況建立其中一種。



## 限制特定地區用戶存取 & 緩存

我們可以限制某些地區限制訪問 *Distribution* 來解決 DDoS 攻擊

若在 Edge location 存有 Edge cache ，也可以用來直接回傳用戶端，減少靜態頁面需要到 Origion 重複讀取資源的問題



# 整合 S3 並設定 OAI 

前面在 S3 的章節有提到，S3 不只有儲存的功能，也可以作為網頁伺服器。

若是靜態網頁，可以考慮把靜態網頁的檔案放入 S3 的 Bucket 裡面，並且設定 OAI 存取。

透過這樣子的方式跟其他網頁伺服器相比，可以使用 CloudFront 來監控以及做更精準的流量限制。



> 另外也有一個好處，就是***成本比較便宜***。對公有雲來說，運算貴於儲存、儲存貴於傳輸



## 什麼是 OAI ?

OAI, Origin Access Identity，原始存取身份。

在 CloudFront 建立 OAI 這個特殊的使用者。並且設定 Bucket Policy 讓 Bucket 允許 CloudFront 讀取資料。

另外我們將 S3 設定為不對外開放，可以讓使用者無法使用 URL 來存取 S3 的 Object，提高安全性。

只要我們有設定 CloudFront 的 OAI，就可以透過 OAI 來存取 S3 的檔案並提供給使用者。



# 限制

CloudFront 很美好，但不是萬能。有些事情他也是做不到的，我們可以將前端下載的資源加快，但是資料來源是動態需要去存取後端的服務時，就不在 CloudFront 的範疇了，還是得搭配系統架構還有軟體設計來達成。

以現實生活舉例:

> *我們只能減少消費者到大賣場的時間。*
>
> *但是消費者要到大賣場逛多久就是另外一個議題了。*




