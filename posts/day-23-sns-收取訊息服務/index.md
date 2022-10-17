# Day 23 - SNS 收取訊息服務






[TOC]



# SNS 

其主要的使用場景在於通知，如寄簡訊、Email 通知、APP 推播給用戶。

更進一步你可以透過 SNS 發起一個 HTTP 的請求至指定的 URL。

幫助你集中化管理，也可以透過觸發的方式整合一些 AWS 服務，如 Lambda、SQS。

最常見的使用情境是在CloudWatch ，搭配 SNS，在達到警示閥值的時候傳送訊息。

用戶端的訊息傳送與協調、管理訂閱端點。



![AWS-SNS-Features.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-SNS-Features.drawio.png)







## SNS 核心元件

SNS 採用常見的 Pub & Sub 系統架構，最重要的三個核心元件如下:

- Publisher 發佈者
- Subscriber 訂閱者
- Topic 主題

Topic 是訊息頻道的邏輯存取點，不論是 Publish 要發佈訊息或是 Subscriber 要訂閱訊息都需要確定 Topic 的內容為何才有辦法繼續下去處理。

![](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-SNS-Core.png)





## 與 SQS 結合打造消息重用系統

SNS 蠻常搭配 SQS 做出一個消息可以重複使用的系統。如果收到一個訊息，同時有不同的系統需要接收做對應的處理，那應該怎麼做?

我們舉例來說，假設銀行收到一個警示訊息，需要將某個可疑的用戶凍結，這時候可能有幾種系統需要做對應的處理:

- *銀行帳戶管理系統: 將警示帳戶的帳戶凍結*
- *客服系統: 通知被凍結的帳戶擁有人*



這時候我們可以在 SNS 發出一個 Topic 的時候切出三條 SQS 的 Queue 去接收，每個 Queue 後面可以各自接不同的系統。

![AWS-SNS-Intergration-With-SQS.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-SNS-Intergration-With-SQS.drawio.png)



這也是分散式架構的優點，有些功能為了開發效率與系統的安全及可用性，會另外切割出來，一旦獨立的切割出來，也不一定要侷限於某種程式語言。



> 住要要做 **雙向綁定**，在 SNS 建立 SQS 的 訂閱以外，在 SQS 頁面也要允許 SNS


