---
title: "Day 31 - 考後感想 & 賽後心得"
date: 2022-11-01T00:20:51+08:00
draft: true
tags: [ '14th鐵人賽', 'AWS' ]
type: posts
---

[TOC]









# 有些在意的 Hands-on

- S3 的加解密實作
- S3 災難復原 (DR) 講求的 RTO 和 RPO 的輩分方案
  - 使用 S3 來 做備份及還原
- VPC 的 Route Table 設定
- C100K 問題
- 書籍上有些跟GCP 的差別
- LRU 快取
- Packer
- Terrform
- Well-Architected Framework
  -  AWS Well-Architected 系統架構精進實戰工作坊
     您是否常常擔心您的網站系統在促銷活動時無法承受大量顧客與訂單的流量？困擾於耗費過多工程師的時間與資源在系統的運作與升級上？抑或是擔憂網站遭受駭客攻擊，珍貴的客戶資料無法復原？為了預防狀況往往需要耗費大量的人力與時間，卻依舊不見得清楚了解到底改善到什麼程度。 
  -  AWS 在多年累積經驗幫助我們建立了一套可以在 AWS 雲端環境評估系統架構的方法稱之為 AWS Well-Architected Framework。這套方法可以協助客戶找出需要改進的部份，並轉換為具有客觀且量化的有效參考數據。由六個不同面向出發，協助客戶評估並建立出低維運成本、安全、穩定可靠、高效能、經濟實惠且具有永續經營特性的雲端系統。
  -  現在就報名Well-Architected 系統架構精進實戰工作坊，協助您有效了解 AWS Well-Architected Framework 的核心概念，並幫助您快速的進行初步的架構評估，獲得立即且有效的系統改善建議。
  -  https://d1.awsstatic.com/whitepapers/zh_TW/architecture/AWS_Well-Architected_Framework.pdf
- Cloud Watch Log Stream 的真正用途與意義
- CloudFront 的 RMTP 怎麼用?  真的有差?
- AWS Glue 整合 S3 跟 Amazon Athena，Athena 可以用 SQL 的方式來查 S3
- ELB 有提到 反向代理。正向代理 反向代理 透明代理
- Serverless Framework 沒有實際 deployee 成功
- 問：AWS Global Accelerator 與 Amazon CloudFront 有何不同？
- GraphQL API 的 AWS AppSync ,  WAF 上看到的??
- Amazon Route 53 Resolvers DNS Firewall
- 零時差弱點 Firewall Manager，您從提供 CVE 修補更新的 AWS Marketplace，預訂 WAF 的 Managed Rule，藉此輕鬆保護整個組織，避免發生零時差弱點
- 您可以使用 AWS Firewall Manager 自動防範各種類型的 DDoS 攻擊，例如 UDP 反射攻擊、SYN 泛洪、DNS 查詢泛洪和跨帳戶的 HTTP 泛洪攻擊。
- SAML 2.0 與 OpenID Connect 
- Cognito Windows AD 整合
- 客戶主金鑰(CMK) 是在 AWS KMS 中建立的金鑰 S3
- SSM SSM 文件 是啥鬼
- DevOps 方法論, DevOps 架構, DevOps 架構演進
- 其他工具如 Chef, Ansible 他們最主要的功能是「配置管理」，亦即是決定要安裝什麼套件、設定什麼環境變數…等之類的，而相較之下 Terraform 則是決定要什麼樣的機器。
- OpsWorks 採用 Chef 和 Puppet 的自動化操作




# 考試的準備心得

首先我先看書把一些服務先了解功能。

之後搭配架構實際 Hands-on 到可以使用的階段。

實際工作上會遇到的情況，比較像是你遇到什麼問題或是需要什麼功能，你能下意識知道要去哪個服務設定，或是增加哪些服務來幫你達成你要的需求。

因此我認為比較重要的服務一定要實際 Hands-on 一遍，其他冷門服務不見得要 Hands-on ，但是遇到了你要知道有什麼東西可以幫助你。

這部分我這 30 天





# 考前準備

模擬考題呀! 公司雖然有模擬考的資源，但是我自己另外推薦這個網站 [Examtopics](https://www.examtopics.com/exams/amazon/aws-certified-solutions-architect-associate-saa-c02/view/) ，他囊括各種考試，而且也會有許多人上來討論，提供正確解答。

考題對應解決方案我也會在筆記上記錄下來。未來遇到類似問題也不會因為這次考試就忘記。



因為這張證照是架構師，所以各個 AWS 的服務用途要非常了解，最後最好是搭配架構圖一起服用。

一定要了解各個服務存在的用意及使用情境。



裡面有談到的 AWS 服務:

- AWS serverless application Model (AWS SAM) template
- AWS Control Tower 
- service control policies (SCPs)
- AWS Cognito
- AWS GuardDuty
- AWS Macie
- AWS Shield
- AWS WAF
- AWS SSO 
- AWS Secrets Manager
- AWS Direct Connect
- AWS Fargate
- AWS Lambda
- AWS Transfer Family
- Amazon Simple Queue Service
- AWS Elastic Container Service
- AWS Elastic Kubernetes Service
- AWS Regions
- AWS Route 53
- AWS Comprehend
- AWS Polly
- AWS RDS Proxy
- AWS X-Ray





- AWS caching services
  - Amazon ElastiCache
  - Amazon API Gateway cache
- CloudWatch



- AppStream





架構應該考慮:

- 安全 Secure 
- 有彈性 Resilient 
- 可擴展 scalable
- 高效能 High-Performing 
- 成本最優化 Cost-Optimized
- 鬆耦合 loosely coupled





架構相關

- Event-driven architectures
- Message Driven ??
- Multi-tier architectures
- Microservice
- 



維運相關

- 災難復原 Disaster recovery  ( DR ) 策略 
  - backup and restore
  - pilot light
  - warm standby
  - active-active failover
  - recovery point objective [RPO]
  - recovery time objective [RTO]
- 故障轉移 Failover
- 



