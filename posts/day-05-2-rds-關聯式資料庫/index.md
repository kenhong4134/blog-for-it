# Day 05-2 - RDS 關聯式資料庫


[TOC]

# 為何要選擇 RDS?

在雲端使用 Database 有兩種方式，其一開台 EC2 自行安裝資料庫軟體，其二是選用 RDS 服務。

在 EC2 架設 DB，後續能連入 EC2 調整系統參數。

選用 RDS，雖然 AWS 不開放我們連入作業系統底層做操作，但是卻提供跟 AWS 其他服務整合性更高的選擇。

資料備援與系統的高可用性都可以快速設定完畢。



支援的資料庫引擎如下:

- Amaazon Aurora
- PostgreSQL
- MySQL
- MariaDB
- Oracle DB
- SQL Server



DB 的高可用性不外乎**執行寫入交易、查詢的結果更快**，**資料庫系統錯誤時轉換到另一個資料庫**確保服務的持續性。



在 RDS 建立的時候，需要指定 Subnet Group，供 RDS 挑選合適的 Subnet 去放置資料庫執行個體。

> Subnet Group 是提供一種子網路集合，需在 VPC 中建立。



# Mutli-AZ

強調高可用性，在同個 Region 不同 AZ 準備資料庫系統。為資料庫常見的 Master-Standby 資料庫架構。

執行個體當中，有一個會是 Master DB，另一個則是 Standby DB。

啟用 Mutli-AZ 的時候，Standy DB 會幫也會幫忙處理 Read Replica 跟 Snapshot。



![AWS-RDS-Multi-AZ.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-RDS-Multi-AZ.drawio.png)

一旦 Master DB 掛點了， Standby DB 就可以馬上代替 Master DB 的身份，把工作接續下去。此時 Standby DB 也會變成 Master DB。

※注意: Mutli-AZ 會負擔相對高額的費用



# Read Replica 

唯獨副本的存在，是資料庫常見的讀寫分離架構。因為寫入的情況較為重要且只有一位。

為了舒緩主資料庫的流量， RDS 提供了唯獨副本，可供純粹的查詢服務。以下是未啟用 Muti-AZ 時的架構圖:

![AWS-RDS-ReadReplica.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-RDS-ReadReplica.drawio.png)





# Snapshot

類似前面 EC2 提到的快照功能。

當沒有啟動 Multi-AZ 的時候，**做 Snapshot，都是向 Master 做請求，且會有 Downtime 時間** (約略幾秒鐘到幾分鐘)

有啟動 Multi-AZ 的時候，做 Snapshot 可以向 Standby DB 請求，就不會有 Downtime 時間

有分兩種*不定期手動備份*、*週期性自動備份*。

當我們刪除 RDS 資料庫的時候，週期性自動備份僅會留下最新的一份。

手動備份的 Snapshot 全部都會留下。





# 效能調教

不開放我們連入作業系統底層做操作，要怎麼優化資料庫效能?  可以參考以下兩種方法:

- 使用 Parameter Group 匯入資料庫參數，提升效能
- 更換硬碟，提升效能
  - EBS Volume -> Provision iops 類型的 EBS Volume





# 加密

只有在建立資料庫才可以啟用加密，想對已經存在的資料庫作加密，需將原資料庫做 Snapshot，在經由 Snapshot 復原並建立一個新的資料庫。

這時候就可以加密了



# Region 移轉

要將 RDS 換到不同的 Region ，不可以直接轉移。

需要先做成 Snapshot，然後透過 Snapshot 在新指定的 Region 建立新的 RDS 執行個體。




