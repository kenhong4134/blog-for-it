---
title: "Day 24 - 混合雲架構"
date: 2022-10-09T22:35:16+08:00
draft: false
tags: [ '14th鐵人賽', '系統架構', 'AWS', 'AWS Direct Connect', 'AWS Storage Gateway', 'AWS FSx', 'AWS VM Import/Export' ]

type: posts
---



[TOC]

# 混和雲架構

我們先說說看混和雲的存在必要。

天下合久必分，分久必合。

全部的服務上公有雲，不就結束了?

但是實際情況是，有些資料真的不能上雲，也因為一些法規讓資料可能無法保存在國外。

再來是搬遷成本，企業累積 10 年以上的資訊系統要在一朝一夕全部上雲是不可能的。

混和雲具備彈性，讓你有需要的時候在來使用雲端資料，也可以跟本地伺服器現有的資料與服務做溝通。



這裡書上提到了，混和雲的三個階段:

​	*網路穩定* -> *資料同步* -> *應用程式同步*





# Multi-Site 解決方案

有沒有想過當流量來了，公有雲的應用程式跟各個服務都不會壞掉嗎?

我想這個答案應該沒有人能保證，如果服務失效造成的商業損失，第一是損失估計難以估計，第二是就算估算出來了跟公有雲打官司勞財傷民又浪費時間，還不一定能拿到賠償。

與其這樣公司不如也準備一個機房，除了讓公有雲壞掉的時候能不讓公司服務掛掉增加可用性，在平常的時候做分流分擔雲端的運算。

Multi-Site 會遇到的挑戰如下:
- 本地應用倒入雲端
- 資料同步傳輸
- 網路傳輸穩定與安全



這幾項 AWS 都有對應的服務可以解決。



## Direct Connect 

首先需要確保 **網路品質** 有這幾種: 傳輸穩定、封包安全。AWS 與本地 ISP 廠商推出了 Direct Connect。

可以將本地機房與 ISP 業者拉一條專線，然後由 ISP 將此專線的流量直接上雲，其簡易的流程如下:

![](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-混合雲-DirectConnect.png)



可以確保重要資料直接輸入 AWS 或是從 AWS 傳出，繞過 ISP 且同時避免網路壅塞。



## Storage Gateway

確保網路品質後，接下來就是確保 **資料同步** 。通常公司一定會有 NFS 系統，透過 Storage Gateway 可以在本地環境模擬一個 NFS，當資料存到這個模擬的 NFS 就會透過 Storage Gateway 同步資料到雲端。

有以下三種閘道:

- File Gateway
  - 將檔案上傳至 S3。
- Volume Gateway
  - 將檔案自動上傳到 EBS 的 Volume 或 Snapshot。
- Tape Gateway
  - 將檔案自動上傳至 Glacier。

## FSx

通常公司都有已經現有的文件系統，Amazon FSx 讓您可以在四種廣泛使用的文件系統進行選擇：

- NetApp ONTAP
- OpenZFS
- Windows File Server 
- Lustre。

通常基於您對給定文件系統的熟悉程度，或將文件系統的功能集、性能配置文件和數據管理功能與應用程式匹配。

若你的應用程式牽扯到一些特定文件系統的功能或函式庫，這時候你就應該使用 FSx ，可以保持應用程序的兼容性而不改變管理數據的方式。



## VM Import/Export

可以將本地機房跟 EC2 的 Instance 做交換或同步。

可以匯入 VMware ESX 或 Workstation、Microsoft Hyper-V 以及 Citrix Xen 虛擬化格式的 Windows 與 Linux VM。匯出可以使用 AWS CLI 作為工具來匯入到 AWS 雲端。

也可以將 EC2 執行個體匯出為 VMware ESX、Microsoft Hyper-V 或 Citrix Xen 格式。



# AWS 混和雲架構圖

![AWS-混合雲架構.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-混合雲架構.drawio.png)



# 參考資料

- [AWS。FSx](https://aws.amazon.com/tw/fsx/when-to-choose-fsx/?refid=30ef8e27-201f-4959-b6d2-de9e186bb48f)
- [AWS。VM Import/Export](https://aws.amazon.com/tw/ec2/vm-import/)

