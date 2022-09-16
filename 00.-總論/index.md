# 

[TOC]

# Question

- 有其他書有達到 23種設計模型? 公司提供的書只有提供到 12 種設計模式? 有哪些是書沒有提供的?
- GoF 書中提 23 個模型



# 學習目標?

- 了解應用情境、為何要使用這個模型

- 分析模式的問題 (Problem) 與作用力 (Force)

- 設計模式的測試要點


# 軟體開發
軟體開發包含以下核心行動:
	- 過程
	- 需求
	- 設計
	- 工程
	- 構造
	- 測試
	- 除錯
	- 部署
	- 維護

# 軟體設計 & 軟體工程
軟體設計與軟體工程
  程式設計不等同於軟體開發


軟體工程與軟體設計的關係:

    軟體工程
        （英語：software engineering），是軟體開發領域裡對工程方法的系統應用。 包含設計模式


    軟體設計
        軟體設計是程式設計師按照特定順序撰寫電腦資料和指令的集合。「軟體設計」可以是撰寫最基礎的二進位0和1位元；
        也可以是建立在位元之上的各類軟體語言、演算法、架構、程式、圖像化程式碼來進行。


# 演算法 ?? 設計模型??
演算法是算數學題

Design Pattern 是一些常見的系統範本



# 何謂設計模型

- 從建築設計領域及人類學引入電腦科學
- 不直接用來完成程式碼撰寫
- 使不穩定依賴於相對穩定，避免會引起麻煩的緊耦合
- 並非所有的軟體模式都是Design Pattern
- 只軟體設計層次上的問題
- 架構模式非Design Pattern
- 演算法是用來解決計算的問題，而非設計上的問題
- 敘述各種不同情況下，要怎麼解決問題的一種方案
- 通常以類別或物件來描述其中的關係與相互作用





# 學會 Design Pattern 的好處及優點

- 加深物件導向分析與設計的理解
  - 在學習物件導向技術的過程中較早較早學習Design Pattern，可以加深物件導向分析與設計的理解

- **找工作很有用。**
  - 許多軟體開發的職缺，尤其是薪水比較高的職缺，都會列上「熟悉設計模式」這一點。就算沒有列出來，在口試的時候，也經常會被問到。把GoF的23個設計模式都學會，相信找工作的時候，鄉民們會比面試官懂得還多。
- 自己設計軟體的時候可以用，設計出比較容易擴充與維護的軟體。
- 比較容易看懂與學會如何使用別人開發的元件或類別庫（例如JDK、.NET或是許多開源軟體，都套用了很多Design Pattern）。
- 再利用解決方案，避免重蹈前人覆轍
- 建立通用術語
- 成為軟體架構師的先修訓練。
  - 每一個Design Pattern都可算是一個迷你版的軟體架構，而許多軟體架構本身也是一種 pattern（architecture pattern，架構模式）。學會 Design Pattern 可以奠定日後成為軟體架構師的基礎。

- **不被同事或屬下欺騙。**
  - 這算什麼好處？曾經有一位專案經理來上Design Pattern的課，他已經不需要自己動手寫程式，他來上課的理由，是希望能夠對於Design Pattern有一定的了解，日後對於程式設計師所訂出來的開發時程，或是和程式設計師溝通軟體開發問題的時候，可以不要差距太遠，或是因為完全不懂而被程式設計師牽著鼻子走。
  - 或者，有時候你的同事或是下屬會跟你「唬爛」，說他套用了多少個pattern，有多麼厲害、多麼偉大。如果完全都不懂，很容易被蒙騙過去。



# 在了解設計模型前

建議需具備以下能力:

- 閱讀 UML 能力，至少類別圖需要會



# 與演算法的差異

演算法主要用來解決 **計算上** 的問題

而非設計上的問題



## 物件導向基本概念

- 繼承、封裝、多型
- 物件、封裝、抽象類別

到底哪個是正確的?






# 設計模式分類

分類是思考的角度不同，可以分為以下:

- 建立型模式
- 結構型模式
- 行為型模式
- 併發型模式

> 前三種可至 Wiki 的 設計範例查看如何實作






- 建立型模式
  - 類別怎麼產生? 怎麼建立?
> 用於管理物件的建立
>
>  By DESIGN PATTERNS

- 結構型模式
  - 組合積木，組合各個類別
> 用於將已有的程式碼集成到新的物件導向設計中 
>
> By DESIGN PATTERNS

- 行為型模式
  - 物件怎麼執行動作? 物件間怎麼交流?
    - 交給下一代? 流水線? 一個指令一個動作?
> 用於封裝行為的變化
>
>  By DESIGN PATTERNS


- 併發型模式










12 個設計模式 1個設計模型

# 12 個設計模式



- Facade  (入門)
- Adapter  (入門)
- Strategy  (入門)
- Bridge  (進階)
- Abstract Factory   (入門)
- Decorator  (進階)
- Observer  (入門)
- Template Method  (入門)
- Singleton  (入門)
- Double-Checked Locking  ??
- Object Pool  ??
- Factory Method  (入門)



# 23 個設計模型
付費課程的分配

## 入門課程涵蓋的內容

1. 核心物件導向設計原則一聽就懂
2. 如何搞懂設計模式的格式框架
3. 套用設計模式前、後之比較
4. 分析模式的問題(Problem)與作用力(Force)
5.  設計模式的測試要點
6. Singleton模式
7. Observer模式
8. Template Method模式
9. Facade模式
10. State模式
11. Factory Method模式
12. Abstract Factory模式
13. Strategy模式
14. Command模式
15. Adapter模式
16. Composite模式

## 進階課程涵蓋的內容

1. 基礎物件導向設計原則快速複習
2. 搞懂進階物件導向設計原則
3. Builder模式
4. Mediator模式
5. Bridge模式
6. Memento模式
7. Proxy模式
8. Prototype模式
9. Decorator模式
10. Chain of Responsibility模式
11. Flyweight模式
12. Iterator模式
13. Visitor模式
14. Interpreter模式

# 延伸字彙

- 反模式
- 模式挖掘
- 軟體重構
- 程式設計實踐



# 參考資料

- Desgin Patterns - Elements of reusable Object-Oriented Software 收錄了 23 種設計模型
- [工商服務] 十、十一月份Design Patterns這樣學就會了入門與進階班
  - http://teddy-chen-tw.blogspot.com/2014/09/design-patterns.html
  - https://teddysoft.tw/courses/design-patterns-1/
  - Design Patterns這樣學就會了：進階實作班
    - http://teddysoft.tw/courses/design-patterns-2/
- [漫談模式](https://openhome.cc/zh-tw/pattern/) 

