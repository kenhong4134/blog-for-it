# Day 17 - API Gateway



[TOC]

# API Gateway

通常一間大型企業，一定會有很多自己的系統，財務、人事、物料、ERP、CRM、行政..... 等各種系統。

當我要跑某個內部流程的時候，員工不太可能登到各自的系統去做申請。假設公司有 10 個系統我就要記住這 10 個系統的網址，哪天增加第 11 個系統的時候....。

這時候一定會有一個 EIP 企業入口網站。

而 API Gateway 就是統一對外服務的窗口。

也是使用者使用服務時，唯一的一個整合性接口。

![傳統 EIP 系統架構.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/傳統EIP系統架構.drawio.png)


整合性接口的好處是，可以集中做監控，查看資料傳輸內容，做出限制流量或是通知管理員。

API Gateway 本身也已經有做高併發與容錯機制

同時也可以整合擁有 API 接口的 AWS 服務。例如: Lambda、EC2、DynamoDB

甚至如果你有透過 VPN Connection 連線的網路，也可以連線到企業內部系統。

![AWS API Gateway.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-API-Gateway.drawio.png)



## Cache 資料緩存

這算是 API Gateway 最主要的功能，可以減少用戶需要向後端存取資料的次數。

啟用快取時必需選擇快取容量。通常來說，容量越大效能越佳，不過成本也越高。



若需要頻估 Cache 是否真的有作用，可以在 CloudWatch 把 `CacheHitCount` 和 `CacheMissCount` 指標納入監控。





## 整合 Lambda 製作客製化驗證

每個系統他們的安全驗證機制都不太一樣，有些可能還是傳統的 Session，也些可能已經走 JWT 。

這時候可以寫一個 Lambda，撰寫你的驗證程式碼，當網路封包經過 API Gateway 的時候，就能觸發 Lambda 去做驗證。

或是針對需要較強安全性的系統，去額外撰寫 Lambda 也是一種使用方法。





# 參考資料

- [AWS。使用 API Gateway Lambda 授權方 - Amazon API Gateway](https://docs.aws.amazon.com/zh_tw/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html)
- [小信豬的原始部落。AWS Serverless 學習筆記 - API Gateway)](https://godleon.github.io/blog/Serverless/AWS-Serverless_API-Gateway/)


