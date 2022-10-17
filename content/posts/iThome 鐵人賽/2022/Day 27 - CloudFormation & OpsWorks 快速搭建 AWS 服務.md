---
title: "Day 27 - CloudFormation & OpsWorks 快速搭建 AWS 服務"
date: 2022-10-12T23:05:54+08:00
draft: false
tags: [ '14th鐵人賽', 'AWS', 'AWS CloudFormation', 'AWS OpsWorks' ]

type: posts
---
[TOC]

# 前言

你有一個很棒的 AWS 基礎設備架構，你花了一個禮拜設定在 AWS Console 建立並拉出這樣的架構，再花了一個月測試這樣的架構沒問題。

之後，在準備上正式環境的時候，發現一切的設定都要重新來過，再花一個禮拜設定，再花一個月測試，然後可能很不幸的，有些服務的設定已經忘記怎麼設定了，又得再花一個禮拜找原因。

這樣會不會太沒效率了！我們有沒有更有效率的方式來建立這些服務，以避免花這麼多時間呢?

CloudFormation，就是可以解決我們的需求的服務！





# CloudFormation

CloudFormation 是一個可以幫助我們快速建立 AWS 資源的服務。採用 ***基礎架構程式碼 (IaC)*** 的敘述方式，描述一個 Template，來告訴 CloudFormation 你想建立的資源，例如: EC2 Instance、RDS Instance。

之後 CloudFormation 就會依照你的描述，幫你建立、配置這些資源，所以你不用到各個資 源頁面，去個別建立和設定。這個服務幫助了我們以下:

- 簡化基礎設備的管理
- 快速複製基礎設備到其他環境
- 追蹤基礎設備地的改變





## 核心元件 Template、Stack、Change Set

CloudFormation 有以下幾個重要元件:

- Template
  - 設定檔，也是 CloudFormation 的基礎
  - 支援 JSON、YAML 檔格式

- Stack
  - 推疊，也就是使用到的 AWS 服務

- Change Set
  - 變更集，負責追蹤環境內變更的 AWS 資源以及新增刪除的 AWS 服務




我們會透過 Template 開啟 Stack，在使用 Template 的時候，可以觀測 Change Set，確認會影響到的服務後建立出 Stack。



## DependsOn  & WaitCondition  

我們來說實際的使用情境:

> 首先需要先開 RDS，再來開 EC2 Instance，等待 EC2 Instance 裡面的應用程式完成後，再來啟用 ELB



上述的要求，主要有兩點: 

- ***先開哪個資源? 哪個資源要等待它?***
- ***開完之後誰要等待?***



***DependsOn***:  決定先後順序，**在誰之後建立**

***WaitCondition***:  **等待前面完成**，在尚未收到完成訊號前阻擋其他 AWS 服務建立資源 

透過這兩個屬性來決定 AWS 服務之間的關聯性。





## Stack & Template 

使用 CloudFormation 時，會把相關的資源透過 Stack 來表達跟管理。***一個 Template 可以有多個 Stack***，可以透過刪除 Stack 刪除一系列的 AWS 資源。



以下的 Template 就會**建立一個 Stack 並有兩個 AWS 資源**。


```
AWSTemplateFormatVersion: "2010-09-09"
Description: A sample template
Resources:
  MyEC2Instance:
    Type: "AWS::EC2::Instance"
    Properties: 
      ImageId: "ami-0ff8a91507f77f867"
      InstanceType: t2.micro
      KeyName: testkey
      BlockDeviceMappings:
        -
          DeviceName: /dev/sdm
          Ebs:
            VolumeType: io1
            Iops: 200
            DeleteOnTermination: false
            VolumeSize: 20
  MyEIP:
    Type: AWS::EC2::EIP
    Properties:
      InstanceId: !Ref MyEC2Instance
```

這個 template 描述了一個 `EC2 Instance` 及 `Elastic IP`



### Template 結構

在 Template 主要會分為以下幾個區塊

- Resources
  - 要建立的 AWS 資源
- Parameters
  - 使用此 Template ，需要提供怎麼樣的參數
- Mappings
  - 設定區域或是其他條件相關的。例如: 歐美地區開的 EC2 Instane 的 AMI 用 *ami0001*；亞太地區開的 EC2 Instane 的 AMI 用 *ami0002*
- Outputs
  - 在 Stack 完成後產生輸出值。 可以在 Console 看到，或是在 CLI 下 `aws cloudformation describe-stacks`





# 使用情境

- A / B 測試
  - 避免功能改錯了或是新功能會必會被用戶買單而建立的測試
  - ***需要將一部分的流量導入舊系統，另一部份導入新系統***，蒐集測試資料去做後續評估
  - 需要快速搭建擁有新功能的新環境
- 快速搭建各種用途的環境
  - 針對不同用途，可能會有 DEV、UAT、PROD 環境，這三個環境在沒有新功能的情況下最終基礎設備都會一模一樣
  - 哪一天 UAT 可能要增加一個環境供客戶測試，或是 PROD 環境需要建置在另外一個客戶的 AWS 帳號上面。




# CloudFormation Designer

我們可以透過視覺化的方式，把 AWS 服務彼此之間要建立的資源關聯起來。

而 Designer 也可以自動生成 Template ，可以選擇輸出 JSON 或是 YAML，並且可以切成不同的組成條件去顯示(畢竟整個 Template 下來其實很不好閱讀)。

以下就是 CloudFormation Designer 的設計畫面。

![image-20221013165858859](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-CloudFormation-Designer.png)





# OpsWorks

採用 Chef 和 Puppet 的自動化操作，我們需要了解宜下 Chef 和 Puppet 是做什麼用途的，這兩個軟體可使用程式碼自動設定伺服器組態的自動化平台。

什麼意思呢? 也就是你為了新環境建立的 EC2 Instance，你需要更新、安裝新的套件，設定系統環境變數。這時候就輪到 OpsWorks 的幫忙了。

OpsWorks 支援以下三種:

- AWS Opsworks for Chef Automate
- AWS OpsWorks for Puppet Enterprise 
- AWS OpsWorks Stacks



# 結論

在學習 AWS 的初學者，我認為 CloudFormation 還是有它存在的價值。你還是可以透過 CloudFormation 去確認你建立的 AWS 服務。

但是許多元件非常的不直覺，WaitCondition  需要搭配 WaitConditionHandle 才能達成，也沒有搭配自動完成的功能。

相較於 CloudFormation 我更喜歡 ***Terraform*** 這個軟體。因為我不會被綁死在 AWS 這朵公有雲上但是又能達成建立 AWS 資源的功能。

在 2019 年時，AWS 推出了全新的開發套件 AWS Cloud Development Kit (AWS CDK)，透過利用 AWS CDK 套件，開發者可以定義整個雲端基礎架構使用程式的方法，同時提升可讀性，並可撰寫測試程式來減少失誤的可能性，也可以達到快速複製的效果。



# 參考資料

- [Medium。Chi-Hsuan Huang。Terraform 自動化的基礎架構介紹](https://medium.com/@chihsuan/terraform-%E8%87%AA%E5%8B%95%E5%8C%96%E7%9A%84%E5%9F%BA%E7%A4%8E%E6%9E%B6%E6%A7%8B%E4%BB%8B%E7%B4%B9-f827e8975e98)
- [Medium。Chi-Hsuan Huang。使用 CloudFormation 快速部署 AWS 資源](https://medium.com/@chihsuan/cloudformation-%E5%BF%AB%E9%80%9F%E5%BB%BA%E7%AB%8B-aws-%E8%B3%87%E6%BA%90-d3378b096249)
- [AWS。什麼是 AWS CloudFormation？](https://docs.aws.amazon.com/zh_tw/AWSCloudFormation/latest/UserGuide/Welcome.html)
- [博客園。亚马逊云服务之CloudFormation](https://www.cnblogs.com/huang0925/p/3384596.html)
- [iThome。Day 17 - 看一下自己寫的東西都去哪了](https://ithelp.ithome.com.tw/articles/10223505)
- [AWS。AWS::CloudFormation::WaitCondition](https://docs.aws.amazon.com/zh_tw/AWSCloudFormation/latest/UserGuide/aws-properties-waitcondition.html)
- [AWS。AWS OpsWorks](https://aws.amazon.com/tw/opsworks/)
