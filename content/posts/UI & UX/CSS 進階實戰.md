---
title: "???"
date: 2022-09-16T00:20:51+08:00
draft: true
---



你在寫 CSS 時是否也遇過以下困擾？

😱 任何變動都要找出程式碼一個一個手工改
😱 一種命名各自表述，階層越寫越多，網站效能越來越差
😱 網站不好管理，開發跟維護都得花很多時間，又很容易出問題
 

姚韋禎 ( Misa ) 老師用超過 15 年的豐富業界實務經驗，讓你的能力不再止於入門，快速提升 CSS 技能，晉升到設計方法與設計模式的大師領域！

老師將在一堂課教你秒懂 CSS 底層邏輯和原理，並用 BEMIT 手刻框架實戰打造你的完整作品，提升維護效率 x 實戰經驗！
 

▍經手再大的網站也能系統化管理 CSS 的秘訣​​​

🎯 使用業界主流預處理器 Sass 定義變數，輕鬆做到一鍵修改，又快又簡單！
🎯 了解 OOCSS、SMACSS、Atomic CSS、BEM 等設計模式，統一命名規則，程式碼變得乾淨好懂，檔案還變小了！
🎯 用國外最先進的 ITCSS 設計心法，CSS 架構分層清楚，可複用性大幅提升，獨立分離，局部想搬移到其他專案也 OK！








CSS 進階實戰｜靈活運用設計模式 x ITCSS 心法，打造最好維護的專案！

帶你認識業界最常用的 CSS 預處理器 Sass 以及 5 種熱門設計模式 
OOCSS、SMACSS、Atomic CSS、BEM 與 ITCSS，

 一門讓你同時掌握工具與設計思維，並實作手刻框架的進階課程。

另外想請問 sass 的 @import 在官網上有建議使用 @use , @forward， 想請問目前實際專案上主要還是使用@import嗎?   
https://sass-lang.com/documentation/at-rules/import

目前開發團隊計畫是將原訂2022年10月 將@import 棄用 ，不過最新的Timeline 是可能需要延後一年或更長時間才棄用，那麼想問老師當在開發的時候，繼續選用@import 的原因是為何呢?     
https://github.com/sass/sass/blob/main/accepted/module-system.md#timeline
**July 2022**: In light of the fact that LibSass was deprecated before ever adding support for the new module system, the timeline for deprecating and removing `@import` has been pushed back. We now intend to wait until 80% of users are using Dart Sass (measured by npm downloads) before deprecating `@import`, and wait at least a year after that and likely more before removing it entirely.



目前 Misa 老師慣用以及課程中所使用的都是屬於常用的 Ruby Sass。另有搭配 C/C++ 的 LibSass 以及 JavaScript 的 Dart Sass。由於 JavaScript 的效能愈來愈好，開發者也比 Ruby 來得多很多，我猜 Sass 有意將重心挪往 Dart Sass，不過目前成效似乎不好。

 

而棄用 @import 的原因正是因為，@import 會傻傻地將所有 CSS 解譯出來，不分變數範圍，也不管程式是否有重複，效能及安全性遠不如 @use。但目前包括 Bootstrap 5 都還是持續使用 Sass 的 @import，Sass 官網也自己說：Dart Sass 的使用率還沒達到一定程度，所以貿然放棄 @import 對現有開發者肯定是個不小的傷害。

抱歉，更正上篇回覆：Misa 老師慣用及課程中教學的是 Node Sass（誤寫為 Ruby Sass），亦不支援 @use 及 @forward。

希望有機會能請老師解釋一下，Sass 的樣式結構分類，雖然有看過網路上的資料夾分類，但是在實際分類的時候又不太清楚這樣分類是好或不好。 以及使用設計模式命名時，如BEM，對於怎麼命名有點障礙， Modifier 實際常用在什麼樣的狀況下? 摸索了一陣子，還是有種不確定感~ 謝謝
這就是這堂課的目的囉！其實大多數學員（包含我自己），在認識各種設計模式的時候一定都會有定義的模糊地帶，光是命名也是一門很大的學問了！

 

課程中我會先傳達一個觀念：設計模式不是非 A 即 B，我們甚至可以混用，或是創造出自己（或團隊）的習慣與模式。課程尾端的示範，就是帶入 BEM + ITCSS 的設計方法，手把手地做出幾個頁面的切版。然而課程中 Misa 老師還是混了自己的習慣進去，說是 BEM + ITCSS 嗎？也不完全！



https://hiskio.com/fundraising/1458/comments?utm_term=misa_css_20220911

