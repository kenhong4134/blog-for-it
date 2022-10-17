---
title: "Day 26 - AWS 金鑰儲存 & 共用參數儲存"
date: 2022-10-11T23:35:04+08:00
draft: false
tags: [ '14th鐵人賽', 'AWS', 'AWS KMS', 'AWS Parameter Store', 'AWS Secrets Manager' ]

type: posts
---
[TOC]

# 前言

當我們實作微服務，就會遇到一些問題:

***參數怎麼共用 ?*** ***儲存的參數是否夠安全 ?*** ***是否有版本紀錄每次的異動?***

當微服務架構把業務邏輯抽出來了，但是有些組態、參數我們還是希望能共用方便我們維護以及統一設定。

在 AWS 有幾個服務可以達成共用的參數儲存。



# KMS  (Key Management Service)

KMS (Key Management Service)，金鑰管理服務，是 AWS 儲存憑證金鑰的服務。

基本上就是儲存各種對稱、非對稱金鑰的地方。能夠統一管理各種憑證。它可以跟很多 AWS 的服務去做整合，例如:

EC2 Instance 登入的時候會用到的金鑰， S3 檔案加密會用到的金鑰，還有我們接下來會介紹的 AWS 服務: *Parameter Store*、*Secret Manager*。





# Parameter Store

屬於 AWS Systems Manager (SSM) 的一項功能，擁有高可用性的服務，且可以放在 AWS 各個區域的可用區域裡面。



您可以存放各種參數，例如: 密碼、資料庫字串、Amazon Machine Image ID 、API Token 、授權碼或是 ***任何需要共用統一儲存的資料*** 。



存放的參數可以是 ***純文字或加密資料*** ，建立的參數會有自己的 ARN，可以用在指令碼、命令、SSM 文件以及組態和自動化工作流程中參考 Systems Manager 參數。



當你修改的時候也有版本紀錄，可以追蹤參數的變化。



Parameter Store 也能與等下會介紹的 Secrets Manager 整合。

有些 AWS 服務已經支援 Parameter Store 的服務，用來擷取 Secrets Manager 的秘密。





# Secret Manager

如果你的密碼 ***定時都會去更新、替換*** 的話，建議可以使用 Secret Manager 來達成。

當多個應用程式需要透過憑證來取得資料內容的時候，通常憑證會有過期的問題，當鑲嵌在應用程式的憑證過期時，應用程式就有可能會失敗。也因為有此風險，許多客戶選擇不要定期輪換憑證。

但是憑證存在的目的，在於要達成資安的需求，而要達成資安的需求其中一項就是要有 ***時效性*** 。

但是更換憑證又太麻煩容易出錯，那該怎麼辦 ? 這時候 Secret Manager 存在的價值就出現了!



Secrets Manager 可讓您將程式碼中的憑證 (包括密碼)，***改成透過 API 呼叫 Secrets Manager，以程式設計方法擷取秘密***，用更輕鬆的方式管理、輪換和擷取秘密。

這有助於確保不讓某人研究應用程式的程式碼而盜用秘密，因為 ***秘密不再存在於程式碼中*** 。

也可以設定 Secrets Manager，根據指定的排程自動輪換秘密。***用短期秘密取代長期秘密，進而大幅降低洩漏風險。***







# 參考資料

- [AWS。AWS Secrets Manager 使用 AWS KMS 的方式。](https://docs.aws.amazon.com/zh_tw/kms/latest/developerguide/services-secrets-manager.html)
- [AWS。AWS 服務使用 AWS KMS 的方式。](https://docs.aws.amazon.com/zh_tw/kms/latest/developerguide/service-integration.html)
- [AWS。AWS Systems Manager Parameter Store。](https://docs.aws.amazon.com/zh_tw/systems-manager/latest/userguide/systems-manager-parameter-store.html)
- [AWS。什麼是 AWS Secrets Manager？](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)
