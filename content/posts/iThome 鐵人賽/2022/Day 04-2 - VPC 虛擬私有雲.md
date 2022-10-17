---
title: "Day 04-2 - VPC 虛擬私有雲"
date: 2022-09-19T23:23:32+08:00
draft: false
tags: [ '14th鐵人賽', 'AWS', 'AWS VPC', '網路拓樸' ]
type: posts

toc:
  enable: true
---

[TOC]



# 經典網路拓樸架構

以下為在傳統在地端的網路架構

![經典網路架構拓樸圖.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/經典網路架構拓樸圖.drawio.png)

> ※NACL: Network Access Control List



# VPC

Virtual Private Cloud，虛擬私有網路，類似網路環境中的子網域

VPC 擁有以下幾點特性:

- 單一 Region 中，橫跨所有 Availability Zone
- Availability Zone 可建立一個或多個子網路
- 子網路 (Subnet) **只能存在於單一** Availability Zone

- 預設 VPC 之間無法彼此互連

- 子網路可設定多個公網段 (Public Subnet) 跟 私網段 (Private Subnet)

- 預設 VPC 私網段內部的機器無法直接對外

- 每個子網路都會配置 NACL 防火牆

  



最常見的網路拓樸圖如下:

![AWS-簡易網路拓樸圖.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-VPC-簡易網路拓樸圖.drawio.png)







# 服務對接方式

常見的有以下四種對接方式:

- 私有網段連外網
- VPC 以內網方式操作其他 AWS 服務
- VPC 串接 VPC
- 本地網路環境串接 VPC 





## 私有網段連外網

在預設情況下，Private Subnet 的機器無法連外網。

因此有可能需要安裝一些套件的時候，是無法安裝的。

這時候就需要透過 `NAT Gateway`，可參考以下架構圖。

![AWS-VPC-私有網段連外網.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-VPC-私有網段連外網.drawio.png)





## VPC 以內網方式操作其他 AWS 服務

正常情況下，如果 VPC 內部網路需要存取 AWS Service 時，是需要走外網去存取 AWS Service 的。如下圖 Instance 需要存取 S3 需要走 Internet 去做存取。

![AWS-VPC-VPC 以內網方式操作其他 AWS 服務-Default.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-VPC-VPC 以內網方式操作其他 AWS 服務-Default.drawio.png)



當 VPC 內部的主機需要跟這些服務溝通時會走外網，但是聽起來有點繞遠路。

同樣都是 AWS 的服務，為什麼需要特地繞出去 AWS 再回到 AWS? 

這時候可以透過 `VPC Endpoint` 讓 VPC 內的機器可以 **以內網的方式** 與 AWS 服務串接，如下圖。

![AWS-VPC-VPC 以內網方式操作其他 AWS 服務-VPC Endpoint.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-VPC-VPC 以內網方式操作其他 AWS 服務-VPC Endpoint.drawio.png)



## VPC 串接 VPC
前面有提到，在預設情況下。VPC 之間是沒辦法互相連線，只能走外網存取其他 AWS VPC。如下:

![AWS-VPC-VPC 串接 VPC-Default.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-VPC-VPC 串接 VPC-Default.drawio.png)

但是公司內可能會有多個部門，分別都使用獨立的 VPC，若需要做資料共享

一樣同樣都是 AWS 的服務，不能走更快速的方式直接連線嗎? 

這時候我們可以使用 `Peering Connection` 將兩個子網段進行配對，透過內部網路存取其他 AWS VPC，如下:

![AWS-VPC-VPC 串接 VPC-PeeringConnection.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-VPC-VPC 串接 VPC-PeeringConnection.drawio.png)



## 本地網路環境串接 VPC 

通常建立雲端環境不會馬上把所有服務直接轉換至雲端

或是因為一些資安限制，只能從公司網路去進行佈署 AWS 服務或是存取資料。

可以參考以下網路拓樸圖:

![AWS-VPC-本地網路環境串接 VPC .drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-VPC-本地網路環境串接 VPC .drawio.png)





# 元件特性與說明

- Internet Gateway
  - 水平擴展、備援且高可用性
  - 允許 VPC 中的執行個體與外部溝通


- Route Table
  - 路由表，讓網路封包能夠有效流出流入
- Network access control list
  - 介於 VPC 與各個子網路之間，可以視為防火牆的概念
  - 每個子網路都會配置 NACL 防火牆
  - 屬於靜態防火牆，**每次**進出都需要審核


- NAT Gateway

  - 網路位址轉譯，讓私網段內的機器**可對外連線**，且**不被外部網路**所認識。
  - 將私網段的流量導流到公網段後，轉發到外部網路。

- VPC Endpoint

  - AWS 有些服務不會在 VPC 裡面(例如 S3)
  - VPC Endpoint 可以走內網的方式存取 AWS 服務

- Peering Connection

  - 將兩個不同的 VPC 進行配對，使雙方可以用內網的方式存取對方的資料
  - 兩個 VPC 各自使用**對方的私有 IP** 進行資料交換

- VPN Connection

  - 透過加密的方式從外網連入 VPC
  - 以**內網 IP 存取** AWS 資源

- Customer Gateway


  - 在 Site-to-Site VPN 的架構中，連接在地端的網路
  - 需要設定兩個通道，若任何一端有問題，VPN Connection 會自動容錯移轉至第二個通道
  - IPsec 的加解密

  


# 監控 VPC 流量

可使用 VPC Flow Logs 追查網路流量。

另外可以將資料內容發佈到 CloudWatch Logs 或 Amazon S3。
