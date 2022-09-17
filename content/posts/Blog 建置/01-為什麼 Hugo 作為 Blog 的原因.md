---
title: "01. 為什麼選擇 Hugo 作為 Blog 的原因?"
date: 2022-08-25T15:35:15+08:00
draft: false
tags: [ 'hugo' ]
toc:
  enable: true
---

# 寫文章的心境
我曾經使用過 Google 的部落格，但是發現操作起來不太好用。
再來我那時候寫到後面覺得似乎寫越多，跟這個平台聯結也越多，導致之後被綁死在這個平台。
以下是我整理的一些傳統部落格的缺點:
- 寫了大量的文章之後，然後網站說關就關
- 定時備份的文章哪天想要轉換到另一個部落格很麻煩
- 沒有版本控制
- 無法離線編輯
- 各個平台擴充的彈性不一定，也有點受限，要先了解它的部落格機制才有辦法修改

# 自己架設一個 CMS ?
Content Management System，簡稱 CMS 
我有想過自己架設一個用 WordPress 架設的 Blog
它是一個強大的平台，很多網頁跟電商都是透過它來架設的
同時它也有很多插件可以使用，但是我最終也沒有選擇它作為我的解決方案，主要是有幾下幾種考量:
- 要另外花錢架設主機，還要考慮備份
- 只是換一個綁架對象
- 網路不通，就沒辦法寫文章了

# 挫折
遇到這些問題，加上自己當時的技術力也不太夠，漸漸的也就不怎麼愛寫了...
但是之後我發現事情的轉機。

# 轉機
因為軟體工作的關係。`Readme.md` 這類專案的起始檔案越來越多，Markdown 的文章格式越來越多人使用。
而且使用方便，程式碼可以顯示不說。
數學函示還有簡單的流程圖都能畫出來。
而且也有許多現成的 IDE 可以使用。如 Typora、MarkdownPad，VS Code 也有插件可以安裝。
我把我自己的筆記盡量用 Markdwon 的文字格式寫下來。

另外也發現近幾年來越來越多人使用 Github Page 來架設自己的部落格。
其中 Jekyll 就直接被 Github Page 官方支援。

讓我認識了 Jekyll 這種純文字轉換成 Blog 文章的靜態網頁產生器。
也就是把你寫的 Markdown 文章，透過 Jekyll 這類的工具，轉換部落格的文章跟網頁。
瞬間我就來勁了!

我就開始研究 Jekyll, Hexo, Hugo 這幾個比較熱門的工具。還有幾個我在 Azure App Service 有看到，可以參考一下[此網頁](https://docs.microsoft.com/zh-tw/azure/static-web-apps/publish-gatsby)。



# 過程及結論
我是重度的 Windows 使用者，從最開始接觸 Windows 98 開始到現在已經 Windows 11。
前面一度使用 Jekyll 來當作主要的轉換工具，但是發現問題不少。
光是我要在 Windows 執行 `jekyll serve` 就遇到一些問題了。
另外是效能問題，文章多的時候轉換的時間會拉長。
文章量只會越來越多，如果處理的時間越來越長。
在來 Jekyll 是 Ruby 語言寫的。是屬於我不熟悉的語言。
之後遇到 Hexo 跟 Hugo 這兩個可以替代的方案。
其中 Hugo 是 Go 語言寫的，且執行效能飛快。
且 Go 語言雖然我目前不會，但是我有把它納入我的學習清單裡面。
因此我這邊最終是使用 Hugo 來當作我的轉換工具





# 參考資料
- [Day06] Jekyll vs Hexo vs Hugo
https://ithelp.ithome.com.tw/articles/10269253
- https://ithelp.ithome.com.tw/users/20111879/ironman/1749