# Day 22 - 資安架構


[TOC]

# 資安架構

為什麼資安這麼重要?

因為駭客很簡單，哪邊有利可圖就往哪裡駭！

尤其是對外服務的應用程式，如果又牽扯到一些金流，那更可能遭受到駭客的攻擊。

建議每年可以至 OWASP ( Open Web Application Security Project )，開放網路軟體安全計畫查看每年的 Top10 的資安問題。



以下為 AWS 為了資安所做出來的服務。

## Shield

Shield 是一種可保護 AWS 上執行的應用程式不受 DDoS 攻擊。

> DDoS 針對 Layer 3 網路層以及 Layer 4 傳輸層做攻擊



AWS Shield 有兩種方案 – Standard 與 Advanced。



***Standard*** 無須額外付費，所有的 AWS 資源都自動具有該功能。

舉例來說，我們對外的 EC2、ELB、CloudFront、Global Accelerator 和 Amazon Route 53 。

預設已經具備 Standard 的防護了。



***Advanced*** ，則是提供更高水準的防護，擁有以下特性:

- 攻擊可見性
  - DDoS 攻擊的偵測與防護功能
  - 查看 CloudWatch 指標與攻擊診斷，以便清楚掌握所有 DDoS 事件。
- 整合 Web 應用程式防火牆 
  - 可自定義 AWS WAF 規則防範 Layer 7 應用層的攻擊
- AWS Shield 應變團隊 (SRT) 
  - 全天候提供服務，由他們幫你撰寫規則防範 Layer 7 應用層的攻擊
- 免費使用 AWS WAF 和 AWS Firewall Manager



有些 AWS 服務如 CloudFront、Global Accelerator 和 Route 53 節點都可另外開啟 Advanced 功能。



## WAF

AWS WAF 是一種 Web 應用程式防火牆，不耗用過多資源的幫助你保護 Web 應用程式或 API 不受常見的 Layer 7 應用層的攻擊，如 Web 入侵程式和機器人侵擾。

AWS WAF 包含功能完整的 API，以自動化安全規則的建立、部署和維護。

透過 **規則** 去防範這些攻擊，建立的規則來篩選流量，以保護 Web 應用程式不受到攻擊。

> 例如: 可以篩選 Web 請求的任何部分，諸如 IP 位址、HTTP 標頭、HTTP 本文或 URI 字串。



規則可被快速部屬、快速抵禦攻擊，建立規則的方法有兩種: *使用 AWS 受控的規則*、*自定義的規則*。



***使用 AWS 受控的規則***，可以快速用於解決 OWASP 10 大安全風險或是那種消耗過多資源、偏移指標或可能導致停機時間的自動化機器人...等常見問題。

> 由 AWS 或 AWS Marketplace 賣方所管理的一組 ***預先設定的規則***



你也可以建立自定義的規則，建立防爬蟲的或封鎖常見攻擊模式 (如 SQL Injection 或跨網站指令碼) 。



以下是幾種常見的部署情境:

- 部署在 CloudFront 做為 CDN 解決方案的一部分
- 部署在 Application Load Balancer
- 部署在 REST API 的 Amazon API Gateway
- 部署在 GraphQL API 的 AWS AppSync。



***定價是以您部署的規則數以及  Web 應用程式收到的請求數為依據。***



## Firewall Manager

AWS Firewall Manager 是一種安全管理服務，擁有以下特點:

- 簡化跨帳戶管理防火牆規則的程序
  - 在 AWS Organizations 中跨帳戶和應用程式，集中設定和管理防火牆規則
  - 從中央管理員帳戶建立防火牆規則、建立安全政策，並在整個基礎架構中以一致、分層的方式加以執行。
  - 新應用程式的建立時，強制執行一組通用的安全規則讓新應用程式和資源合規
- 跨帳戶部署工具
  - 可以輕鬆設定分發 Application Load Balancer、API Gateway 和 CloudFront 的 AWS WAF 規則。
  - 可以跨組織中的帳戶和 VPC 部署 AWS Network Firewall。
- 統一設定並稽核安全群組
  - 為 EC2、Application Load Balancer 和 彈性網路界面 (ENI) 設定新的 VPC 安全群組
  - 稽核現有的 VPC 安全群組。

- 規則關聯工具
  - 可以將 VPC 與 Route 53 的 Resolvers DNS Firewall 規則進行關聯




# 結論

看完所有的資安的解決方式，最大問題是怎麼辨識平民跟士兵。平民是正常來存取的流量，也是你服務正常的使用者；士兵則是來者不善，是來竊取、滲透、攻擊你的服務。

有些解決方案會下降服務的可用性，但是可以減少大多數的攻擊。



在 AWS 的資安架構起點是從 Shield 開始，然後搭配 WAF 和 Firewall Manager  來進行嚴密的過濾。



***Shield  過濾掉無效的 DDoS 請求***

***WAF 過濾掉可疑的封包***

***Firewall Manager 來做集中式管理與部署 (設定防火牆規則或是 WAF 規則)***





# 參考資料

- [OWASP。TOP 10](https://owasp.org/Top10/zh_TW/)
- [AWS。AWS Shield](https://aws.amazon.com/tw/shield/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc)
- [AWS。Firewmal Manager](https://aws.amazon.com/tw/firewall-manager/)
- [AWS。WAF – Web 應用程式防火牆](https://aws.amazon.com/tw/waf/)

