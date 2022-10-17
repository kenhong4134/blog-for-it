---
title: "Day 19 - Serverless Framework"
date: 2022-10-04T23:21:26+08:00
draft: false
tags: [ '14th鐵人賽', 'AWS', 'AWS Lambda', 'Serverless Framework' ]
type: posts
---

[TOC]



# Serverless Framework 

有沒有想過一件事情，既然我們都已經使用 container 跟 serverless 的架構了，我們真的會需要侷限在某朵公有雲上面嗎?

Serverless Framework 是一個協助串接各個公有雲的產品，它提供了一個通用的介面，可以讓你從 Azure Function 跳轉到 AWS Lambda。

之後你需要跳到阿里雲、騰訊雲的 Serverless Solution 也都是可以的。



它擁有一些特性:

- 參數設定使用 YAML 檔格式
- 監控管理系統

## 參數設定使用 YAML 檔格式

近幾年來 *IaC (Infrastructure as Code)*，基礎設施即代碼越來越盛行，Serverless Framework 因其特性是將各家雲端服務上包一層框架。因此有些設定例如: 記憶體大小、回應逾時時間、依賴相關的服務定義，都是透過 YAML 檔來設定。

也因為是檔案的關係，也可以納入版控，這也是 IaC 其中一項優勢。



## 監控管理系統

Serverless framework 提供了儀錶板畫面，由於它算是 SaaS 服務，是透過 CLI 的方式佈署一些設定，並將資料呈現在他們所提的網頁。

當使用率、錯誤率突然竄升時，會提示警告，同時也可以發出通知或是透過 webhook 綁定你的 API。(如果公司是使用 Slack 來溝通的也有福了~ 他們有直接支援)

Serverless framework 會蒐集 Fucntion 的執行狀況，並根據這些資料建議你更適合的 memorySize、timeout，不會讓你多花錢。



# 安裝 Serverless Framework



首先你需要先有 `npm` 這個 Javascript 的套件安裝工具。

執行以下指令:

```
npm install -g serverless
```

最後可以執行 

```
serverless info
```

確認你安裝是否成功，以及它的版本



# 建立第一個 Serverless Framework 專案

執行 `serverless` 這個 command 會跳出以下讓你選擇:

```

Creating a new serverless project? What do you want to make?
  AWS - Node.js - Starter
  AWS - Node.js - HTTP API
  AWS - Node.js - Scheduled Task
  AWS - Node.js - SQS Worker
  AWS - Node.js - Express API
  AWS - Node.js - Express API with DynamoDB
  AWS - Python - Starter
  AWS - Python - HTTP API
  AWS - Python - Scheduled Task
  AWS - Python - SQS Worker
❯ AWS - Python - Flask API
  AWS - Python - Flask API with DynamoDB
```



選擇你要的專案性質，我們這邊可以選 AWS - Python - Flask API。當然我們也可以直接執行:

````
serverless create aws-python-flask-api
````



這時候會問你要不要登入 serverless dashboard，可以選擇登入後，之後可以查看你佈署的 AWS Lambda 執行的狀況如何。



一旦完成這些步驟，會得到一個下面的目錄架構

```
└── aws-serverless-project
    ├── handler.py
    ├── package.json
    ├── package-lock.json
    ├── README.md
    ├── serverless.yml
```



其中最重要的檔案就是 `serverless.yml`，以下有切一個子章節來說明。

到這邊為止大致上就已經是完成第一個 serverless framework 的應用程式了。







## serverless.yml 檔案說明

檔案的內容結構大致上長的如下:

![img](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/serverless-framework-yml-structure.jpg)

整個設定檔可以分為以下幾個區塊:

- 第一個區塊
  - 是cloud provider 設定，你需要在這邊描述 aws 參數。例如: region、environment variable、vpc ...
- 第二個區塊
  - 是 function 參數設定，這邊會描述你的 lambda function 啟動時的進入點，要用那些 Layer。
- 第三個區塊
  - 是 resource 定義，這邊是 AWS CloudFormation 的描述，定義需要額外部署的 AWS 資源。
- 最後是 custom 區塊，這邊可以讓你定義自己的變數



## 動態變數

大多時候，我們可以用單純的 key-value 來描述 serverless 設定檔

但有時候，我們可以用變數的方式來描述，將你要參照的變數以這樣的方式索引 `${variable-name}`

serverless 提供以下幾個變數來源：

- cli 參數

  透過 deploy --stage 所帶的參數，可能是 dev / uat / prod 其中一個

  ```
  custom:
      deploy_env: ${opt:stage}
  ```

- AWS Parameter Store

  ```
  provider:
    name: aws
    role: ${ssm:/lambda/permission/role-arn}
  ```

- AWS Secret Manager
  ```
  custom:
      DB_USERNAME: ${ssm:/aws/reference/secretsmanager/ambda-${self:custom.deploy_env}}.username
      DB_PASSWORD: ${ssm:/aws/reference/secretsmanager/lambda-${self:custom.deploy_env}}.password
  ```

  > AWS Secret Manager 適合存放金鑰資訊，最大的好處是他整合了任何 AWS 需要 credential 的服務(例如 RDS )，並能夠自動汰換更新，而不需要重新部署服務

- AWS CloudFormation
  ```
  layers:
    - ${cf:mop-lambda-layer-dev.MopLambdaLayerArn}
  ```





## 指定環境變數檔

我們可以在 provider 底下增加一個 *environment* 的參數，定義環境變數的檔案

```
provider:
  environment:
    ${file(./environments.yml)}
```

其定義長得有點像 `.env` 檔案。

```
# environments.yml
CONFIG_ENABLE: true
EVENT_LOG_ENABLE: true
TRACK_ENABLE: true
```



# 佈署 Serverless Framework

執行以下指令即可完成佈署

```
serverless deploy --stage dev
```





# 跟 CloudFormation 的關係

看到這裡，我們用一個簡單的圖來說明 serverless 與 CloudFormation 的關係![https://i.imgur.com/ZDkiUay.jpg](I:\我的雲端硬碟\Blog\IT\content\posts\iThome 鐵人賽\2022\images\serverless-cloudformation.jpg)

AWS 終究只看得懂 CloudFormation，Serverless 就像是一個 compiler 一樣，負責幫我們把把 Serverless config 打包成一個或多個的 CloudFormation Template 然後供 AWS CloudFormation 佈署使用。



# 參考資料

- [Serverless Framework](https://www.serverless.com/)
- [使用 Node.js + serverless framework + AWS Lambda 打造可擴展、更穩定而且更經濟的架構](https://medium.com/visuallylab/%E4%BD%BF%E7%94%A8-node-js-serverless-framework-aws-lambda-%E6%89%93%E9%80%A0%E5%8F%AF%E6%93%B4%E5%B1%95-%E6%9B%B4%E7%A9%A9%E5%AE%9A%E8%80%8C%E4%B8%94%E6%9B%B4%E7%B6%93%E6%BF%9F%E7%9A%84%E6%9E%B6%E6%A7%8B-6a54b51b8988)
- [Serverless。Notifications](https://www.serverless.com/framework/docs/guides/monitoring/notifications)
- [Serverless。serverless.yml 設定檔參考資料](https://www.serverless.com/framework/docs/providers/aws/guide/serverless.yml)
- [Day 17 - 看一下自己寫的東西都去哪了 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天 (ithome.com.tw)](https://ithelp.ithome.com.tw/articles/10223505)







