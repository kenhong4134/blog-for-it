---
title: "Day 28 - AWS Devops 解決方案"
date: 2022-10-13T18:42:39+08:00
draft: false
tags: [ '14th鐵人賽', 'DevOps', 'AWS', 'AWS CodePipeline', 'AWS CodeCommit', 'AWS CodeBuild', 'AWS CodeDeploy']

type: posts
---

[TOC]

# 前言 - 何為 DevOps?

這邊還是要說明一下 DevOps 究竟是什麼。

DevOps 為英文 Development (開發) 和 Operation (運營) 的縮寫組合。是一種企業文化，講求 IT 團隊、 IT 運營團隊 、 QA 團隊三者間的溝通，增加彼此合作的效率，達到軟體開發效率的提升及品質的保障。

![DevOps - 維基百科，自由的百科全書](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/1200px-Devops.svg.png)







然後整個 DevOps 從一開始 Plan->Code->Build->Test->Release->Deploy->Operate->Monitor.... 週而復始的下去形成一個循環。

![DevOps infinity loop](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/13429_ILL_DevOpsLoop.png)

其中要如何增加交付的效率以及準確性，最常見的 CI / CD 持續整合、持續佈署的工具就誕生了。

各家廠商都有做類似的工具，例如 Open Source 的 Jetkins、CircleCI、Github Action ... 等。

微軟的 Azure 會搭配 Azure Devops Service 來當作 CI/CD 的工具。

在 AWS 就是使用 **CodePipeline** 來控制整個軟體的發佈流程。





# CodePipeline

是一種全受管持續交付服務，可自動化發行管道，快速、可靠地提供應用程式和基礎設施的更新，可靠地交付功能更新。

CodePipeline 可根據發行模式在每次程式碼變更時自動建立、測試和部署發行程序的各個階段。

可以將 AWS CodePipeline 與 GitHub 等第三方服務或自訂外掛程式來來做整合。

也可以搭配 CloudFormation 去做基礎設備的建立，快速地建立相關的 AWS 服務。

以下是佈署容器化應用程式常見的流程圖:

![AWS-CodePipeline.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-CodePipeline.drawio.png)



# CodeCommit

CodeCommit 是一種版本控制服務，讓您能在 AWS 雲端 私下存放和管理 Git 儲存庫。

你也可以使用 ***Amazon CodeGuru Reviewer*** 對 AWS CodeCommit 中的儲存庫提供分析與建議。





# CodeBuild

CodeBuild 是在雲端的全受管組建服務。CodeBuild 可編譯原始碼、執行單元測試，最終產生可部署的應用程式。

優點在於你不必佈署、管理、擴展自己的伺服器。它提供預先封裝的編譯環境，適用於常見的程式設計語言和編譯工具，例如 Apache Maven、Gradle 等等。







# CodeDeploy

CodeDeploy 也是全受管部署服務，可以自動將軟體部署到各種運算服務，包括 EC2 Instance、AWS Fargate、AWS Lambda 和現場部署伺服器。

發佈應用程式的新功能、避免在部署應用程式時停機、處理複雜的應用程式更新，減少手動操作可能造成的錯誤。

同時它不只可以在 CodePipeline 使用，也可以 GitHub、Jenkins 去做整合。



# 結論

這四個 AWS 的服務，彼此之間沒有相依性。其中 ***CodePipeline*** 的角色更傾向於 ***調度者*** ，安排哪個環節應該用哪個服務。

用簡單的流程來解釋有以下三種重要的流程:

​	***取得原始碼*** -> ***編譯(組建)和單元測試*** -> ***發佈應用程式***



***取得原始碼***

​	可以從 AWS CodeCommit、GitHub、Amazon ECR 或 Amazon S3 直接提取管道的原始程式碼。

***編譯(組建)和單元測試***

​	可以在 AWS CodeBuild 執行編譯(組建)及單元測試。

***發佈應用程式***

​	可以使用 AWS CodeDeploy、AWS Elastic Beanstalk、Amazon ECS 或 AWS Fargate 佈署變更。



我認為好的 CI/CD 的工具，應該是具有彈性的。從取得原始碼到編譯原始碼到發佈應用程式的各個環節應該都是要能抽換成其他工具或是服務。

版本控制不一定要使用 CodeCommit ，也可以使用 Github。

編譯應用程式不一定要用 CodeBuild ，我也可以使用 Jetkins 或是 Github Actions。

最後發佈應用程式我也不一定要用 CodeDeploy，我還是可以繼續使用 Jetkins 搭配 AWS CLI 來發佈應用程式。





# 參考資料

- [Wiki。DevOps](https://zh.m.wikipedia.org/zh-tw/DevOps)
- [51CTO 博客。一文收录16张DevOps ”拍照神图” ](https://blog.51cto.com/u_15127518/2658838)
- [dynatrace。What is DevOps? Unpacking the purpose and importance of an IT cultural revolution](https://www.dynatrace.com/news/blog/what-is-devops/)
- [AWS。AWS CodeDeploy](https://aws.amazon.com/tw/codedeploy/)
