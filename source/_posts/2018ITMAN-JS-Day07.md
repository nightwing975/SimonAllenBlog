---
title: 克服JS奇怪的部分：Day07 型別與運算子
date: 2018-07-29 11:52:06
categories: 2018 IT鐵人賽
tags:
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://3.bp.blogspot.com/-eHv7nqsLDzs/W1waps7FnrI/AAAAAAAAIa0/crNGnjVLhBcABqcPoMKxrzyY4kyuVdu-ACLcBGAs/s1600/2018ITMANJS07.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10191195

---

今天開始，課程影片進入第三章節囉！

JavaScript是動態型別`Dynamic Typing`語言，相較於 **C#**、**JAVA**之類的靜態型別語言，JS的變數不用在編輯時特意宣告型別(例如布林值、字串、數值..等等)，在執行時它就會自動判別。

而說起型別，JS有6種純值`Primitive Types`，(又稱原始型別、基本型別)，「純值」是什麼意思呢？
純值是指一種資料的型別(型態)，換句話說，純值(基本型別)不是物件，因為物件就是名稱/值配對的組合物，為避免與口語上的'值'搞混，下面純值改稱呼原始型別或基本型別。

這6種基本型別分別是:

## undefined

表示未定義，是JS給所有變數的**初始值**，剛宣告的變數在我們賦值之前，其值是 undefined，所以開發者最好不要賦值undefined給變數。

## null

表示空、不存在，開發者在宣告變數並要先表示這個變數沒有值時，可以賦值null，不要手動賦值為 undefined。

另外，JS的null有一點特別的地方，當我們使用typeof(null)來檢查時，居然會回傳 **object**。

![](https://i.imgur.com/9iAQ04u.png)

欸~上面不是才說`Primitive Types`不是物件嗎？

null確實是`Primitive Types`，而非物件型別，畢竟物件的特色就是可以自由增加屬性，而null既不是名稱/值的組合，也沒辦法讓開發者自行修改其屬性，**它確實是Primitive Types原始型別沒錯**。

那為何使用`typeof()`檢查null會回傳object？
原來，這個BUG是JS的歷史包袱之一，這個設計可以追究到JS被創造出來時候，具體典故可以參考阮一峰大的文章：[undefined與null的區別](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)裡的說明。

## boolean

表示true或false其中一個，意即是/否、對/錯、成立/不成立。

## number

表示數字，JS的數字型別只有number，不像其他程式數字還有分整數與其他特定數值型態，另外JS的number是浮點數，表示(實際上)有小數點跟在後面。

## string

表示字串，由字符組成，可以用單引號或雙引號來表示。

## symbol

表示符號，是ES6新增的型別，可以賦予變數獨特性、獨一性。

了解何謂`Primitive Types`後，最後我們來快速看看運算子`Operator`

在JS中有不少運算子種類：算術運算子、賦值運算子、比較運算子、邏輯運算子...等等很多種。
我們最熟悉的就是算術運算子啦，`+`、`-`、`*`、`/`(加、減、乘、除)，至於JS怎麼知道，當我們輸入`+`這個符號就等於相加呢？
可以把運算子想像成是**函式**的一種，這些符號會將前後兩個參數傳入對應的JS內建的函式中，進行個別的運算並回傳。
　
　
　
## 小結
今天複習了何謂Primitive Types與運算子，明天來看看運算子的優先性。
至於今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)3-19~3-21