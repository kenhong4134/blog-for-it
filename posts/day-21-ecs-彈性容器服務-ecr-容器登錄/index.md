# Day 21 - ECS 彈性容器服務 & ECR 容器登錄



[TOC]





# ECS 彈性容器服務

Elastic Container Service (Amazon ECS)，是一種由 AWS 全受管容器協同運作服務，可輕鬆地讓您部署、管理和擴展容器化應用程式，執行安全、可靠且可擴展的容器。

它與 AWS 平台的其餘部分深度整合，例如可以用 IAM 來進行權限管控，管理不同的 Container 能存取何項 AWS 資源。



Amazon ECS 是一種區域等級的服務，可在一個區域內跨多個可用區域以高可用性的方式執行容器。



## 元件

ECS 有以下幾個重要的元件:

- Clusters 叢集
  - 可以在新的或現有的 VPC 中建立 Amazon ECS 叢集。
  - 是任務或服務的邏輯分組。
  - 使用叢集隔離應用程式，它們就不會使用相同的底層基礎設施。
- Task Definitions 任務定義
  - 任務定義用作您應用程式的藍圖。其會指定您應用程式的各種參數。
  - 用來指定作業系統的參數、要使用的容器、應用程式開啟的連接埠，任務中的容器要使用的資料磁碟區。
- Tasks 任務
  - 在叢集內將任務定義執行個體化，可以指定叢集上要執行的任務數量。
- Services 服務
  - 執行和維護您所需的 Tasks 任務數量。如果有任何任務因任何原因而出現故障或停止會根據您的任務定義啟動另一個執行個體。確保服務需要保持的任務數量。
- Container agent 容器代理
  - 在叢集內的每個容器執行個體上執行。
  - 代理程式會將目前正在執行的任務和您容器的資源使用率的相關資訊傳送至 Amazon ECS。
  - 接收到 Amazon ECS 的請求即開始和停止任務。



## 啟動類型

可以選擇兩種模式來執行容器:

- Fargate
- EC2 Instance



AWS 提供了幾種建議，讓你判斷應該使用哪一種啟動方式:

- Fargate 遇到以下狀況適合使用：
  - 需要最佳化以降低額外負荷的大型工作
  - 偶爾會爆量的小型工作
  - 微小的工作負載 或是 批次工作負載
- EC2 遇到以下狀況適合使用：
  - 需要持續高 CPU 核心和記憶體用量
  - 需要針對價格進行最佳化的大型工作任務
  - 應用程式需要存取持久性儲存
  - 必須能直接管理基礎設施





### Fargate 啟動模式

叢集資源由 Fargate 管理，不需要管理 EC2 Instance，無須選擇伺服器類型、決定何時擴展叢集，或最佳化叢集壓縮。

Fargate 也有支援 Linux 容器跟 Windows 容器。(要注意不是所有 AWS 區域都支援)

每個 Fargate 的 Task 都有自己的隔離界限，不會與另一個任務共用基礎核心、CPU 資源、記憶體資源或彈性網路界面。





# ECR 容器登錄

Amazon Elastic Container Registry (Amazon ECR) 是 AWS 管理的容器映像登錄服務，具安全性、可擴展性和可靠性。

以下是它的特性:

- 支援私人的用途
  - 可搭配 Policy 做資源許可，讓指定的使用者或 EC2 Intance 存取儲存庫。
- 存儲庫
  - 能推送 Docker 映像、(OCI) 映像以及與 OCI 相容的成品
- 彈性高的存取方式
  - 可以使用偏好的 CLI 來推送、提取和管理映像檔
- 映像掃描
  - 在推送時掃描並識別容器映像中的軟體漏洞
- 跨區域、跨 AWS 帳戶複寫。
  - 可以設定私有登入檔設定，授予複寫與提取快取功能的許可



# 參考資料

- [AWS。Amazon ECS 元件](https://docs.aws.amazon.com/zh_tw/AmazonECS/latest/developerguide/welcome-features.html)
- [淺談 Container 實現原理, 以 Docker 為例(I)](https://www.hwchiu.com/container-design-i.html)
- [什麼是 Amazon Elastic Container Service？](https://docs.aws.amazon.com/zh_tw/AmazonECS/latest/developerguide/Welcome.html)
- [什麼是 Amazon Elastic Container Registry？](https://docs.aws.amazon.com/zh_tw/AmazonECR/latest/userguide/what-is-ecr.html)


