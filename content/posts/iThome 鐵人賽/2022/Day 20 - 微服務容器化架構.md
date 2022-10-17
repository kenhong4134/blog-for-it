---
title: "Day 20 - 微服務容器化架構"
date: 2022-10-05T23:42:09+08:00
draft: false
tags: [ '14th鐵人賽', 'AWS', '系統架構', '微服務架構' ]
type: posts
---

[TOC]



# 微服務架構

微服務架構如果說到很細其實會是一個蠻大的篇章，甚至也有專門書籍來說它。說完篇幅會太長，而且也不會是考試的重點。這邊我用微軟提供的圖來說明，以下是最常見、簡易的架構圖。

![Logical diagram of microservices architecture style.](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/microservices-logical.png)





微服務怎麼切? 一直都是很熱門的討論話題，最常見就是依據不同商業模組去切微服務。例如: 財務系統、人事系統、訂貨管理系統可能都有各自的微服務。

我認為可以參考 ***葉志銘先生寫的微服務設計模式心得淺見***  裡面對於微服務的架構設計模式更基礎的認識





# 容器化 (Containerized)

在執行微服務架構的時候，最常使用容器 (Container) 來當作微服務架構的架構底層。

我們可以將應用程式容器化大致幾個重要的元件:

- **Image Registry**
  - 存放 Image 的儲存庫，供需要的人下載，Docker Hub 就是最大的一個儲存庫

- **Image**
  - 映像檔，可以把它想像成一個小型的 OS 

- **Container**
  - 實際從 Image 產生出來，實際的執行個體。


且最常使用 Docker 來作為管理工具，其關係圖可參考:

![A diagram showing the basic taxonomy in Docker.](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/taxonomy-of-docker-terms-and-concepts.png)



另外我認為上圖缺少了一個蠻重要的東西，也就是 ***儲存體 (Storage)***。我們在設計架構的時候也必須考慮持續性質的檔案應該要怎麼去做存放。



# AWS 微服務容器化架構

在 AWS 實現微服務容器化架構有兩種方式 ***EKS*** 和 ***ECS***。

EKS 是 AWS 雲端的 K8S 架構，K8S 內容比較複雜，沒辦法在這一兩篇就說明完畢，以下會使用以 ECS 作為基礎的微服務架構。

![AWS-微服務容器化架構.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-微服務容器化架構.drawio.png)





基本上有以下幾個重要的 AWS 服務跟傳統的容器化去做對應:

- Elastic Container Registry (ECR)  
  - 對應 Image Registry

- Elastic Container Service (ECS)
  - 對應 Container

- Elastic File System
  - 對應 儲存體




其中你可以使用 Fargate 或是 EC2 的 Instance 作為 Container 的載體。







## EBS 、 EFS 、 S3 的差異 ?

這邊有提到 EFS ，我們統整一下之前有提到的儲存服務。



EBS (Elastic Block Store) 是之前建立 EC2 時會需要建立的，是 ***區塊等級*** 的服務，因此需要將其進行格式化。



EFS (Elastic File System) 是 ***檔案等級*** 的服務，在 AWS 可以分散到不同的可用區域，使用的時候透過 *掛載磁碟區 (Mount Volume)* 的方式來使用，也可以被多個 EC2 Instance 使用，同時訪問 EFS 上的檔案。



S3 也可以算是 ***檔案等級*** 的服務，但它相較於 EFS 的使用情境不太一樣，更傾向於對外的功能，因此不能像傳統的文件系統那樣能設定擁有者、權限等。***但是儲存費用是三個裡面最便宜的。***









# 參考資料

- [昕力資訊TPIsoftware。葉志銘。微服務設計模式心得淺見](https://www.tpisoftware.com/tpu/articleDetails/2114)
- [閱坊。微服務架構中 10 個常用的設計模式](https://www.readfog.com/a/1633275464939311104)
- [克里斯·理查森。微服務架構設計模式](https://www.books.com.tw/products/CN11642128)
- [Microsoft。微服務架構樣式](https://learn.microsoft.com/zh-tw/azure/architecture/guide/architecture-styles/microservices?source=recommendations)
- [昕力軟體。葉志銘。微服務設計模式心得淺見](https://www.tpisoftware.com/tpu/articleDetails/2114)
- [IT 人。《微服務架構設計模式》讀書筆記 | 第1章 逃離單體地獄](https://iter01.com/610620.html)
- [Microsoft。Docker 容器、映像和登錄](https://learn.microsoft.com/zh-tw/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries)
- [Geeksforgeeks。Difference Between Amazon EBS and Amazon EFS](https://www.geeksforgeeks.org/difference-between-amazon-ebs-and-amazon-efs/)
- [EBS vs EFS vs S3 – when to use AWS’ three storage solutions](https://www.justaftermidnight247.com/insights/ebs-efs-and-s3-when-to-use-awss-three-storage-solutions/)
- [AWS EFS vs EBS vs S3 (differences & when to use?) [closed]](https://stackoverflow.com/questions/29575877/aws-efs-vs-ebs-vs-s3-differences-when-to-use)
