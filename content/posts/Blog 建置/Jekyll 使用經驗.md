---
title: "???"
date: 2022-09-16T00:20:51+08:00
draft: true
---


# Last Result

20220811 

https://ithelp.ithome.com.tw/articles/10269253
看到有某位大神 https://ithelp.ithome.com.tw/articles/10230046
是用 Hexo 寫部落格

在都有 100 篇文章的情況下，Hugo 的 Build time 只需要 0.08s，而 Jekyll 則需要 3.3s。

Jekyll vs Hexo vs Hugo

考慮用 Hugo 來寫  畢竟想把 Go Lang 加入學習計畫裡面

網路上也有人推坑 Hugo
https://zhuanlan.zhihu.com/p/165271155

Hugo则需要你配置Github Action来做这件事。但实际上，有一些复杂一点的Jekyll主题，以及Jekyll 4版本也要通过Github Action来发布。
https://zhuanlan.zhihu.com/p/350977057

我想每個 Octopress 的使用者一定都有這樣的煩惱──隨著文章數量愈來愈多，檔案建立的速度愈來愈慢。本站目前已累積至 54 篇文章，每次建立檔案時，至少都需要花費一分鐘以上的時間。

或許是因為 Ruby 天生就比較慢，而這種問題未來似乎也不會改善，Jekyll 和 Octopress 已經一段時間沒有更新，那麼唯一的解決方案只有另覓其他 Blog Framework 了。
https://zespia.me/blog/2012/10/11/hexo-debut/




20220801 沒有完成   卡在 bundler execu jekll serve 不能 work，並且出現錯誤訊息

不知道是不是因為 run on ubunttu 22.04

少了什麼鬼設定





---



Jekyll 從 零開始建置

github深度绑定jekyll


Jekyll可以由Github自动编译发布






# 環境
Winodws 10 + WSL2 + Ubuntu 22.04



# 前期提要

Jekyll是基於 Ruby Gem 的解析引擎，能夠將樣板、liquid 語言、markdown轉換為”靜態網頁”的產生器。


# 特色

No more databases 不需要資料庫
Liquid Template 動態模板
Free hosting with GitHub Pages 只要學會git push 就可以丟到github page上
Markdown 好編寫



## 基本上就是透過 Jekyll 產生靜態的頁面



# 會用到的工具與說明
## Ruby

## RVM
管理Ruby這個程式語言版本的套件，由於Ruby有很多個版本，比如1.8.X~2.3.X，
這麼多ruby的版本如果搞不清楚什麼時候用什麼版本或是切換版本就很麻煩
所以就有這套 RVM 來做 Ruby 的版本管理


## Gem
很多用Ruby寫成的套件包或是框架可以用

才能造就Ruby這個程式的多樣與豐富性
有名Rails就是ruby的套件

找尋這些套件並且管理的軟體

安裝Ruby的時候就會內建Gem




## Bundle
用來解決 gem 套件相依性問題的套件

例如有一個Ａ專案，他使用Ｂ套件（1.0.0版），而這個Ｂ套件會用到Ｃ套件（1.0.0版）

今天客戶可能搬到新的主機了

主機上也有其他專案，但是用Ｂ套件（2.0.0版），然後這個Ｂ套件會用到Ｃ套件（2.0.0版）


bundler會依照Gemfile所定義的RubyGems套件去下載

並將當前所有套件的版本做一個快照檔Gemfile.lock存起來

未來專案bundle會根據Gemfile.lock快照決定Gemfile是否有修改來進行套件的更新

# 目錄

*僅列出部分*

|-- index.html   #網站首頁
|-- _config.yml  #網站設定檔
|
|---/_includes  #網站結構資料夾
|    |--/JB      #Jekyll-bootstrap 模組
|    |--/themes  #Jekyll-bootstrap 預設版型
|
|---/_layouts   #版型
|
|---/_posts     #文章資料夾


# 發文




# 問題
jekyll-4.2.2





# bundle exec jekyll -v






# 參考
- ([筆記]Ruby的RVM,GEM,Bundler是什麼？)[http://sayaku.github.io/blog/2016/05/05/rubyde-rvm-gem-bundler/]



- (使用 Jekyll 创建 GitHub Pages 站点)[https://docs.github.com/cn/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll]
  - 寫的蠻完整的



