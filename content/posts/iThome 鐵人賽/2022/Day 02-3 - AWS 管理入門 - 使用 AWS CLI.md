---
title: "Day 02-2 - AWS 基本管理 & 常見名詞"
date: 2022-09-17T23:45:17+08:00
draft: false
tags: [ '14th鐵人賽', 'AWS', 'AWS CLI' ]
type: posts

toc:
  enable: true
---

[TOC]




以前大學老師說過。

> 新手工程師都愛用 GUI，高手工程師都用 CLI

沒錯，身為高手的我們怎麼可以只用 GUI 畫面來操作呢!

但是對於初心者在前面學習的階段，我覺得 Web Console 的操作可以快速了解服務的功能。

就算日後工作上真的需要，Web Console 也可以滿足 80% 的工作需求。



之前在管理 Azure 的資源時發現，有些功能沒有提供在網頁畫面，只能用指令更新。

為了避免在 AWS 也出現這種窘境，我覺得兩邊都需要略有涉略才會比較保險。



# AWS CLI

AWS 有 Version 1, Version 2 的差異。官方也有提供如何從 Version 1 改用 Version 的轉移文件。



## 安裝 CLI

Linux 安裝方式如下:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

然後下已下 command`,

```
aws --version
```



如果有顯示已下文字內容，即代表安裝成功

> aws-cli/2.7.31 Python/3.9.11 Linux/5.10.16.3-microsoft-standard-WSL2 exe/x86_64.ubuntu.22 prompt/off



## 設置 Configure

使用已下指令快速設定

```
aws configure
```

會需要輸入以下四種資料:

- AWS Access Key ID 
- AWS Secret Access Key 
- Default region name 
- Default output format

設定完後，~/.aws 會增加兩個檔案 ~/.aws/，`credenitals` 和 `config` 因



## 設定多筆 Profile

可參考以下指令

```
aws configure --profile <YOUR_PROFILE_NAME>
```

ex: `aws configure --profile exam_practice`



使用 `aws configure list` 或 `aws configure list-profiles` 來顯示目前你設定了那些 Profile



***Question: 每次下指令都需要在後面指定 Profile，是否可以設定預設Default?***

在 Shell 下，設定環境變數 `AWS_DEFAULT_PROFILE`、`AWS_PROFILE` 都可以。



```bash
# 指定 Default Prfile (For Windows)
set AWS_DEFAULT_PROFILE=exam_practice
set AWS_PROFILE=exam_practice

# 指定 Default Prfile (For Linux)
export AWS_DEFAULT_PROFILE=exam_practice
export AWS_PROFILE=exam_practice

# 回復原本預設 (For Linux)
unset AWS_DEFAULT_PROFILE
unset AWS_PROFILE
```



> 如果希望每次修改都能運作，請修改 ~/.bashrc





# (Hands-on) S3 操作

顯示目前目前所有的 bucket

```
aws s3 ls
```





# 參考資料

- [AWS CLI 官方文件](https://aws.amazon.com/tw/cli/)
- [AWS CLI Version 1 to Version 2 Migration 文件](https://docs.aws.amazon.com/cli/latest/userguide/cliv2-migration.html)
- [Set your Default Profile's Name in AWS CLI](https://bobbyhadz.com/blog/aws-cli-set-default-profile)





