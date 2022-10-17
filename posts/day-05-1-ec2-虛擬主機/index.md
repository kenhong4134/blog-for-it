# Day 05-1 - EC2 虛擬主機


[TOC]

# EC2

Elastic Compute Cloud，是 AWS 提供的運算平台。屬於 IaSS 性質。

基本上各家公有雲都有提供虛擬機的服務。

有些公司對雲端還不熟悉，會先從把地端主機的程式與資料慢慢上雲。

最後再慢慢從主機的程式拉出來到 AWS 的各個服務。當然也有公司最後就只使用 EC2。

同時 AWS EC2 也有支援各種作業系統。如 Linux, Windows, Redhat... 等官方提供的映象檔。



> 在 AWS 使用 Windows 需要另外付授權費用給微軟，所以在一般情況下會比在 Azure 使用 Windows 還貴
>
> 但是依據使用的情境不同跟付費的方案不一樣，外加還有其他零零總總的服務，加起來 AWS 不見得會比較貴



我們建立的 EC2 則通常會被稱為 *執行個體 (Instance )*



# 虛擬映像檔 - AMI

Amazon Machine Image,  基本上就是虛擬映像檔，可從 AMI Marketplace 尋找，分為官方版本，第三方廠商製作的版本，透過映像檔可以建立 EC2 的執行個體。

官方版本為免費使用。第三方廠商製作的會將他們的產品包裝，因此可能需要收取軟體的使用費。



要注意 AMI **沒辦法跨 Region**，例如在北美建立的 AMI 沒辦法拿到日本東部去做使用。

那如果真的需要拿北美建立的 AMI 到日本東部去做使用怎麼辦? AWS 是**透過 Immutable 原則**來達成



## Immuatable 原則

> **不能移動**已建立的的東西，若要改變，得透過複製的方式來完成



如果真的需要拿北美建立的 AMI 到日本東部去做使用，AWS 實作的細節可以參考以下:

- 到北美建立的 AMI，對它進行快照
- 將此快照複製到日本東部
- 在日本東部的快照，還原為 AMI



# 硬碟 Elastic Block Storage 

分為以下兩大種: 

- Instance Store
- Elastic Block Storage (EBS)



*Instance Store* 是**存在 Instance 裡面**的儲存空間，相對於 EBS ，**較為穩定、效能相對來說較好**。但是當 Instance 關掉的時候，裡面的**資料也會跟著消失**。

> 適用的場合在，大量檔案生成與存取。



*Elastic Block Store (EBS)* 所建立的 磁碟區 (Volume) 是**存在在另一個伺服器**，與 Instance 的聯結是透過網路的方式。在資訊世界中，網路速度是相對較不穩定的，因此**相對於 Instance Store 較沒有那麼穩定**、但是**資料能持續保存**，不會因為 Instance 的關閉而消失。

> 適用的場合在，資料需要能持續保存。

> Linux 環境會將 Volume 掛載在 /dev/... 下





## EBS 的種類

前面有提到 EBS 跟 EC2 Instance 的聯結是透過網路。基本上可以分為 SSD 跟 HDD 這兩種，且各自依據不同的使用情境有不同的類型

|              | SSD<br />EBS 佈建 IOPS                                  | SSD<br />EBS 一般用途                                   | HDD<br />輸送量優化                                         | HDD<br />冷存儲                                       | HDD<br />EBS 磁帶                                            |
| ------------ | ------------------------------------------------------- | ------------------------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| **API 名稱** | io1                                                     | gp2                                                     | st1                                                         | sc1                                                   | 標準                                                         |
| **描述**     | 專為對延遲敏感的交易工作附載而設計的最高效能 SSD 磁碟區 | 針對各種交易工作負載，平衡價格效能的一般用途 SSD 磁碟區 | 專為經常存取、輸送量密集型工作負載而設計的低成本 HDD 磁碟區 | 專為存取頻率較低的工作負載而設計的最低成本 HDD 磁碟區 | 可用於具有較小資料集且資料不常存取的工作負載，或效能一致性不是主要考量時 |
| **使用案例** | I/O 密集型 NoSQL 和 關聯式資料庫                        | 開機磁碟區、低延遲互動應用程式、開發與測試              | 大數據、資料倉儲、日誌處理                                  | 不常使用，每天只需較少掃描的資料                      | 低頻率資料存取                                               |

> IOPS: Input/Output Operations Per Second，常用在電腦儲存裝置的效能量測結果，可以視為是每秒的讀寫次數



我們可以從幾種指標去選擇，首先是選擇 SSD 跟 HDD。

SSD 首要的效能屬性是 **IOPS**；HDD 首要的效能屬性是 **網路吞吐量**

首先是 **磁碟區 (Volumn)** 跟 **EC2 執行個體 (Instance)**這兩個方面來選擇，個別擁有幾項評估標準

磁碟區的大小、最大 IOPS、最大網路吞吐量

執行個體的最大 IOPS、最大網路吞吐量

如果是大型資料庫，需要選擇讀寫效能較好的 SSD，且選擇能增強讀寫效能最大化的選擇，追求磁碟區 *最大IOPS* 跟 最大網路吞吐量的 *io1*。



如果是大數據應用場景下，注重的高傳輸量優化的 HDD，僅需要注重磁碟區 *最大網路吞吐量* 的 *st1*。



如果不常存取，考慮冷儲存的 sc1 的 HDD，使成本最佳化。



> 對於頻寬、網路吞吐量可以參考常見問題集



# 快照 - Snapshot

為避免多種原因導致毀損，因此需要做資料備份。

快照 (Snapshot)，如果有管理過 VMWare 或是 Hyper-V 等虛擬機器的人應該都知道。可以透過快照快速回復虛擬機的環境。



將 EBS 做成 Snapshot 以後，可以透過此 Snapshot 複製出新的 EBS 或是 AMI。

通常會需要將 Snapshot 轉成 AMI 有兩種情境:

- 將現有 Instance 的內容與軟體做一個封裝，未來可用此封裝的 AMI 建立新的 Instance
- 需要將機器做移轉。例如從東京搬到新加坡



另外可以使用 AWS 提供 `Lifecycle manager` 協助我們定期做資料備份。



# 防火牆 - Security Group

為避免惡意流量或是非預期流量存取，也需要因應不同 OS 的網路防範上設計不同，因此有了這個外掛式的防火牆 ─ 安全群組 (Security Group)



規則能設定 *輸入規則 (Inbound Rule)* 以及 *輸出規則 (Outbound Rule)*



**Inbound Rule**: 從外部進入 EC2 的網路封包規則，需要設定以下:

- 允許存取的協定、我方的 Port 號、允許來源網段與 IP



**Outbound Rule**: 內部網路封包發送出來的封包規則，需要設定以下:

- 對外聯絡用的協定、存取對方的 Port 號、允許的目標網段與 IP



另外還以下幾點特性:

- 兩個 EC2 Instance 之間的連線，可以設定 Inbound Rule / Outbound Rule 允許或拒絕特定的 Security Groups 進行存取
- 安全群組，是帶狀態的防火牆，若先前成功傳出去，傳回來的不用考慮 Inbound Rule；反之先前若通過 Inboud Rule，輸出的時候不用考量 Outbound Rule 的規則



> 網路流量的設計是 Request-Response 的設計，只要有封包丟給服務端，服務端就必須進行回應



# 網路卡 - Elastic Network Interface

*彈性網路接口*是 VPC 中的一個邏輯網路元件，也代表了一張虛擬網卡。

每個 EC2 的 Instance 都**至少**有一張 ENI，可將他們連叫到 VPC 中的執行個體。



AWS 在針對公有 IP 的 Assign 可以有兩種:

- 綁定公有 IP

  - 使用 Elastic IP，從 `Public IP Pool` 取得一組固定 IP 綁定到 Instance

- 不綁定公有 IP

  - Instance 啟動的時候，會去 `Public IP Pool` 取得一組公有 IP
  - Instance 關機的時候會收回 IP，下次開機的時候公有 IP 的位置會變

  







# Instance 管理方法

最主要的論點是 ***要不要讓使用者登入操作***，可以分為以下兩種:

- 可以讓使用者進入 Instance 操作，可以透過 **連線金鑰** 的方式達成
- 盡量不讓使用者進入 Instance 操作，可以透過 **Userdata** 來達成



針對 **不讓使用者登入操作** 這個案例我們來說明一下，因為公有雲相對於私有雲運算資源相對充足，壞了直接開一台新的就好了，但是如果每次都開一台新的，已經部署的應用程式怎麼辦? 這時候就是透過 ***Userdata*** 來達成！



## Userdata

在 EC2 啟動完 Instance 以後，設定 `Userdata` 可以快速調整作業系統內的環境。如安裝 Python、nginx 等服務。

優點在於**不需要人工手動**連入安裝環境。

使用方法就是**撰寫腳本語言**。例如: Linux 就寫 Shell Script；Windows 就寫 Powershell 的腳本



## 連線金鑰

使用者產生非對稱金鑰，將私鑰自己收著，公鑰放到 EC2 Instance 上，參考下圖登入方式:

![AWS-EC2-KeyPair-Login.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-EC2-KeyPair-Login.drawio.png)





## 透過  SSH 登入 EC2 執行個體

我前面有提過我是透過 wsl 來操作，我在 AWS 開了一台 Ubuntu 22.04 的 EC2 Instance，設定完金鑰後 AWS 會自動下載 *.pem，先將檔案放到



AWS 會建立一個非 root 的使用者，各個 OS 的 Default User 可以參考 [這個](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html)

```
sudo ssh -i {{PEM FILE PATH}} ubuntu@{{YOUR EC2 INSTANCE DNS}}
```









# 執行個體監控 & 異常通知

AWS 針對 Instance，預設每五分鐘監控一次，如果需要縮短監控時程，可以啟用***詳細監控功能*** ，設定每分鐘都監控一次。

之後可使用 *AWS SNS*，發出信件通知異常情況，例如: 設定連續三個異常數據時當作異常狀況。









# Placement Group

![AWS-EC2-PlacementGroup.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-EC2-PlacementGroup.drawio.png)

指定*一群 EC2 Instance 啟動位置*的策略，有分三種策略:

- 叢集
  - 將執行個體們**包裝在一個可用區域**內，降低延遲網路效能。
  - 用在 高效能運算 (High Performance Computing, HPC) 應用程式典型緊密耦合，節點對節點通訊的情境上。
- 分區
  - 將執行個體分散到各個邏輯分區，不會與不同分區的執行體群組共用底層硬體。
  - 每個邏輯分區具有自己的一組機架，每個機架有自己的網路跟電源。
  - 大量分散和複寫的工作負載(例如: Hadoop、HDFS、HBase、Cassandra 和 Kafka) 通常會採用此策略

- 分散
  - 嚴格的將執行個體放到不同的底層硬體，減少相互關聯的故障
  - 不會因為壞一台機器，導致壞好幾個 Instance








# Metadata

執行個體的中繼資料 *Metadata*，是關於執行個體的資料，可以用來**設定**或**管理**執行中的執行個體。包括主機名稱、主機 IP 等資料。

若真的有需要取得 Metadata，不易定要使用 AWS Web Console 或 CLI 

可以在執行個體中存取此網址取得能取得的 meta data

```
http://169.254.169.254/latest/meta-data
```

![image-20220922153916132](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/image-20220922153916132.png)

之後我如果想取得 MAC 位置，我可以下:

```
curl http://169.254.169.254/latest/meta-data/mac
```

結果如下

![image-20220922154141656](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/image-20220922154141656.png)









# 常見問題集

## 怎麼選擇付費方案?

AWS 的付費方案如下:

- On Demand (隨需)
  - 效能穩定，且用多少付多少
  - 適用在**臨時需求**且機器需要穩定

- Reserved (保留)
  - 長期保留機制，至少一年
  - 適用在**長期運行**的需求

- Schedule Reserved (時程保留)
  - 特定時間才運作
  - 適用在**特定時間**需要執行計算任務

- Spot (競標)
  - 誰出更高的價格，誰就有使用權
  - 你的運算是運算任務會被中斷，但是可以從被中斷後的地方進行
  - 適用在**需要運算較強**、**運算任務可以被中斷**的情境

- Dedicate (專用主機)
  - 需要使用到特殊軟體的情境，會依據主機規格來進行授權
  - 適用於**特殊情況**，例如 SQL Server 會依據 CPU 核心數量來決定授權費用






## Security Groups 和 NACL 都擁有阻擋封包的功能，這兩者的差異在哪?
我們參考以下翻譯的內容:



**Security Groups:**

你附加到一個 Instance 或 Load Balance 的 Stateful 防火牆。這些自動創建臨時規則，允許從TCP連接返回的流量。你可以根據數據包來自的IP地址/端口，或者更有趣的是，根據數據包來自的實例所附的安全組，來允許/禁止流量。



**NACL:**

連接到子網的無狀態防火牆。這些只能根據IP和端口允許/阻止數據包。由於它們是 Stateless  的，你必須創建規則以允許返回流量。

這些規則使用起來很麻煩，但對獲得安全沒有必要。



**路由表:**

這就是[路由](https://en.wikipedia.org/wiki/Router_(computing))的方式，並與子網相連。



**WAF:**

它可以附加到負載均衡器上，做更複雜的過濾類型，如禁止來自某些國家的流量。



舉例:

在一個子網內，只有 Security Groups 可以產生影響。如何防止同一子網中的兩個 Instance 使用 22 號端口 SSH 進入對方，方法是建立一個 Security Groups，只允許來自你的堡壘主機(可能在不同的子網)的 22 號端口的流量。

在子網之間，路由表指定數據包應該如何流動，而 NACL 是允許數據包流動的。

因此，路由表中必須有一個條目，以允許從基站主機到兩個 Instance 的流量。

但是，如果你使用NACL，你就必須允許 22 號端口的流量從堡壘所在的子網進入每個子網，同時也允許流量從TCP 1024到65535端口返回，作為短暫的端口。



結論: 

**Security Groups** 是有狀態的，只需要設定輸入或輸出其中一邊規則就好，設定某些服務對外的時候可以直接用這個，是進入 EC2 Instance 前最後的防火牆。

**NACL** 是無狀態的，需要同時設定兩個才有辦法正確運作，是 VPC 底下的功能。細節內容請參考參考資料。





> 堡壘主機: bastion host, 網絡上專門設計和配置為抵禦攻擊的專用主機。或是，Private Subnet 的機器沒辦法對外連線，需要 SSH 進入 Private Subnet 的機器進行部屬或診斷，也能稱為 Bastion Host 或 Jump Server



## 能不能自己做映像檔?

可以，透過 Snapshot 轉換成 AMI。





## 頻寬, 吞吐量的差別?

![img](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/throughput-bandwidth.png)

頻寬 (Bandwidth)：水管的大小
吞吐量 (Throughput)：從水管實際流出的水量
延遲 (Latency)：水從一端到另一端所花費的時間

資料傳輸換算
理論傳輸公式：Time (s) = File Size / Bandwidth
實際傳輸公式：Time (s) = File Size / Throughput






# 參考資料

- [Wiki - IOPS](https://zh.m.wikipedia.org/zh-tw/IOPS)
- [Difference between Security Groups, Route Tables, and NACLs?](https://www.reddit.com/r/aws/comments/cab816/difference_between_security_groups_route_tables/)
- [bandwidth-and-throughput](https://blog.pichuang.com.tw/20190527-bandwidth-and-throughput/)
- [什麼是堡壘機 Bastion Host](https://blog.cptsai.com/2021/07/19/access-from-teleport/#%E4%BB%80%E9%BA%BC%E6%98%AF%E5%A0%A1%E5%A3%98%E6%A9%9F-Bastion-Host)

