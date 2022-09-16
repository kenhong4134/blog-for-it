# 



[TOC]





AWS 有提供免費方案，基本上有分三種:

- 免費試用
  - 依每個服務不同，提供不同的試用時間，例如: Redshift 是 AWS 的資料倉儲服務，就只提供 2 的月的試用

- 12 個月免費
  - 12 個月內讓你免費使用，但是有免費使用的額度

- 永遠免費
  - 雖然免費，但是有額度限制。例如 Lambda 可每個月收 100 萬個請求， SNS 可發佈 100 萬個訊息




基本上考試內容大部分都是 12 個月讓你有限制的使用，但是以 Hands-on Lab 來說已經很夠用了。

例如 EC2 可以讓你**每個月使用 750 小時**， S3 可以讓你**存 5 G 的檔案**。



# 建立 AWS root 帳號

首先進入: https://aws.amazon.com/tw/free/，點選建立免費帳號。

![image-20220912145830826](.\img\image-20220912145830826.png)



這邊建立需要驗證 Root user 的電子郵件，這邊我輸入我在 Google 建立臨時帳號

帳戶名稱可以隨便打

![image-20220912161505821](.\img\image-20220912161505821.png)

接下來就是驗證電子郵件能不能使用，去你的信箱收信吧~ (要注意超過 10 分鐘後要重新驗證)



Step 1: 輸入 root 密碼

Step 2: 輸入聯絡人資訊，因為我是 For 自己的專案使用，這邊選擇 `Personal`

地址的部分不知道可以到 google map 查到你地址後，改 url 後面的參數，把 *hl* 改成 `en-us`，之後複製地址就好。

![image-20220912162309149](H:\My Drive\Blog\IT\content\posts\iThome 鐵人賽\2022\img\image-20220912162309149.png)



![image-20220912162156059](H:\My Drive\Blog\IT\content\posts\iThome 鐵人賽\2022\img\image-20220912162156059.png)



最終長得像這樣

![image-20220912163006147](H:\My Drive\Blog\IT\content\posts\iThome 鐵人賽\2022\img\image-20220912163006147.png)

Step 3: 輸入信用卡資訊，這邊會有 1 美元的刷卡紀錄驗證你的信用卡，這邊不用慌張，之後會退給你。



Step 4: 輸入你的電話號碼，供驗證使用。

我這邊很不幸手機收不到驗證碼，如果有遇到類似問題的。可以參考

[如果我没有接到 AWS 的电话来验证我的新账户，该怎么办？](https://aws.amazon.com/cn/premiumsupport/knowledge-center/phone-verify-no-call/)

按照上面的操作做完， Support Team 會打電話給你，跟你確認 Account ID 跟你的 Email。

之後會寄一封 OTP 的信請你驗證。全程考驗你的英文溝通能力。

確認完後，30分鐘你的帳號就已經啟用了。



Step 5: 選擇你的 Support 計畫，這邊我選擇 Free，畢竟我們只是來試用的。



> ※ 建議建立完啟用多因子驗證 MFA，增加日後駭客攻擊的難度



# (Optional) 設定 Account Alias

日後公司應該都是一群人或分不同的團隊使用 IAM 登入，不太可能每次都使用 Account ID 進行登入

這時候可以設定 Account Alias，避免要輸入一個落落長但是又難記的數字。

1. 點選 Web Console 的右上角，選擇 *Security credentials*
![image-20220913160101039](H:\My Drive\Blog\IT\content\posts\iThome 鐵人賽\2022\img\image-20220913160101039.png)
1. 點選 Dashboard 後，在 AWS Account 的 Account Alias 點選 Create
![image-20220913165121232](H:\My Drive\Blog\IT\content\posts\iThome 鐵人賽\2022\img\image-20220913165121232.png)

1. 建立完之後，可以提供以下網址給團隊成員登入
```
https://{{YOUR_ACCOUNT_ALIAS}}.signin.aws.amazon.com/console
```





# 建立 AWS IAM 帳號

拿到 root 首先應該要建立另外一個帳號，盡量避免 root 帳號濫用。

1. 點選 Web Console 的右上角，選擇 *Security credentials*
![image-20220913160101039](H:\My Drive\Blog\IT\content\posts\iThome 鐵人賽\2022\img\image-20220913160101039.png)

1. 點選 Menu 的 Users，並按下 Add users 
![image-20220913160235054](H:\My Drive\Blog\IT\content\posts\iThome 鐵人賽\2022\img\image-20220913160235054.png)

1. 設定 AWS 的存取種類，有分 Access Key 跟 Password 兩種。建議可以先兩個都打開
![image-20220913161403605](H:\My Drive\Blog\IT\content\posts\iThome 鐵人賽\2022\img\image-20220913161403605.png)

1. 設定 Permission，我選擇 ***AdministratorAccess*** 這個受 AWS 管理的權限。

1. 建立後，麻煩將 Access Key Id、Secret access key、Password 記下來，之後就可以不需要再使用 Root 帳號了。![image-20220913162451923](H:\My Drive\Blog\IT\content\posts\iThome 鐵人賽\2022\img\image-20220913162451923.png)






