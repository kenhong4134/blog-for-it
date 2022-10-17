# Day 07 - CloudWatch & CloudTrail 監控與稽核


[TOC]



# CloudWatch 

DevOps 倡導的 CI/CD，其中 CD 裡面就包含 *Monitor* (監控) 的部分。

CloudWatch 為 AWS 所推出的 **監控** 服務，能夠監控大多數的 AWS 服務。

另外 CloudWatch 裡面有個 *CloudWatch Logs* 的功能，開放使用者把自製的 Log 傳回 CloudWatch 。

簡單來說，**硬體的監控**，AWS 透過 Cloud Watch  幫你完成了。

**軟體的監控**，你自己可以透過 CloudWatch Logs 蒐集

這樣就可以統一在 CloudWatch 查看所有軟硬體相關的日誌內容了!



## Metrics 視覺化圖表

只有 Log 應該還不夠吧? 而且只有一堆數字也不直覺，這時候你可以使用 Metrics 來建立視覺化圖表，來幫助你快速了解各種數值快速查找問題的原因。





### Metric (指標) & Dimension (維度) 

Metric (指標) 是 CloudWatch 的基本概念，可以是要監控的變數。EC2 Instance 的 CPU 用量就是 AWS 提供給我們指標之一。同時這個變數也可以透過自行開發的應用程式傳入 CloudWatch。



Dimension (維度) 是 Metric 的一部分，類似把資料做分類，比較好的解釋是審視 Metric 用不同角度 (Dimension) 來看

從某一個數值轉換為多種數值。可以將維度連接到每個指標，然後用維度來篩選 CloudWatch 傳回的結果。



## Alarm 機制

蒐集完 Log，然後呢?  如果真的有錯誤的話不太可能等到有人真的來找你才來滅火吧?

錯誤就像火苗一樣，你不理它，它就燒給你看！ 如果你可以即早發現，還能用滅火器消滅，晚了就只能叫消防隊....

在資訊世界也是一樣， 我們可以為蒐集到的 Log 設定一些警示條件，當 Log 達到警示條件時，就會發 Alarm 通知管理人員或是調度 AWS 服務做緊急處理。



可以搭配 SNS 服務對 管理者進行 Email 或是手機推播通知。



## CloudWatch Agent

CloudWatch 是沒有辦法監控機器內的記憶體用量與硬碟內部用量。

但是軟體監控這兩個指標恰好也是個非常重要的觀察指標。

在軟體運作的過程中，會從硬碟讀取資料、並寄放至記憶體，會經常性調度記憶體與硬碟資料。

這時候我們就必須主動在 EC2 的 Instance 安裝 Cloud Watch Agent。



同時我們也可以將軟體的 Log 寫在指定的資料夾位置下，在透過 Agent 將那些 Log 蒐集回去。

這時在 CloudWatch 與 Agent 之間有一個 Log Stream

透過 Log Stream 進行個別監控，當有多個 EC2 Instance 建立 Agent 的時候，各自的 Agent 會有自己的 Log Stream。Log Stream 多起來很難管理怎麼辦? 

可以使用 Log Group 將這些 Log Stream 統整起來。

以後軟體的整體運行狀況，就能透過 Log Group 內的 Log 內的狀況來評估。



## CloudWatch Event

是由應用程式所丟出或是監控的資源來記錄的一些活動。

可以透過 CloudWatch Event 觸發其他 AWS 資源進行後續操作。例如發出 Alarm 通知用戶



也可以使用 Log Stream 日誌串流，共享相同來源的 Cloud Watch Event。

可以用來即時監視的應用程式或 AWS 資源。



## CloudWatch 整體架構圖

![AWS-CloudWatch.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-CloudWatch.drawio.png)

# CloudTrail

用於行為追蹤，追蹤人員與 AWS 服務的行為操作。

用於~~推卸責任用的~~ 追蹤問題發生的根本原因，究竟是應用程式關閉的，還是根本就是某位管理人員關閉的。

若有多個 AWS 帳戶，可以將這些帳戶的用戶行為紀錄通通存入 S3 Bucket 集中管理。

也可以搭配 CloudWatch 做視覺化監控與告警通知。



## CloudTrail 整體架構

![AWS-CloudTrail.drawio](https://raw.githubusercontent.com/kenhong4134/blog-for-it/main/content/posts/iThome%20%E9%90%B5%E4%BA%BA%E8%B3%BD/2022/images/AWS-CloudTrail.drawio.png)








