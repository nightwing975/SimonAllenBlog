---
title: 克服JS奇怪的部分：Day28 內建的函式建構子
date: 2018-07-30 15:12:07
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://2.bp.blogspot.com/-DNtPFxlrJWc/W1wavPtR7_I/AAAAAAAAIcI/Lm7-KcFAYUEQEMDtzBcEGd8UG9jh1TqzACLcBGAs/s1600/2018ITMANJS28.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10195074

---

今天來看看內建的函式建構子

什麼是內建的函式建構子？
其實是將JS的內建的`Number()`、`String()`、`Date()`....等其他內建函式，與`new`函式建構子一起使用。

來看看看程式碼，直接在Chrome瀏覽器console上操作。
![](https://i.imgur.com/jipX1Yk.png)
`Number()`是JS的內建函式之一，在程式碼Number的開頭前多了一個`new`建構子，根據[昨天筆記](https://ithelp.ithome.com.tw/articles/10194795)，我們知道`new`這個`function constructor`**函式建構子**會創建新物件，並將後方函式內的`this`指向它，依照函式內的設定創造該新物件的屬性。

現在變數a指向的東西，並不是 **基本型別**`Primitive Types`，而是**物件型別**`Object Type`，在這個物件裏頭，有一個基本型別數值3的存在。(別忘記物件是名稱與值配對的組合)

所以a是物件，原型是Number，用`typeof`檢查a的型別：
![](https://i.imgur.com/WO6zCm4.png)

我們在console打`Number.prototype.`可以看到一些prototype的方法：
![](https://i.imgur.com/fxyuW5X.png)
prototype不是用來取用函式自己的原型，而是用來取用`new`建構子創造出來的物件原型，
這代表我們可以對物件a做一些操作 例如：
![](https://i.imgur.com/XPCAjlt.png)

重點在於，我們可以透過prototype去取用方法、新增方法，來看看以下程式碼：
![](https://i.imgur.com/KSZZBqB.png)
字串`"Simon"`後面用`.`運算子去呼叫方法`isLengthGreaterThan()`並帶入參數3，字串的原型被我們新增了isLengthGreaterThan方法，透過原型鏈的查找，我們可以讓字串使用我們定義的函式，這等於我們可以改造JS程式，添加一些自己的函式進原型中。

那如果是數值呢？
![](https://i.imgur.com/zM3bYmU.png)
報錯，因為JS動態轉型，字串有時候能被被轉成物件，數值卻沒辦法，我們需要用`new`來幫助我們建立物件才可以使用這個方法。
![](https://i.imgur.com/haFmJ3e.png)

雖然函式建構子非常自由，可以新增自行設定的方法到數值、字串、陣列上，但一般不建議建立函式方法到看起來像 **基本型別**`Primitive Types`的物件上。

透過new建立像 **基本型別**`Primitive Types`的**物件型別**`Object Types`，如今天講的字串、數值，可能導致我們開發時搞混，因而造成一些bug出現，畢竟`new`建立建立出來的是**物件型別**`Object Types`，在瀏覽器console試驗看看就知道了：
![](https://i.imgur.com/9i1mY9x.png)
`c == d`，因為自動轉型的關西，回傳`true`。
`c === d`，用三等號嚴格比較，連型別也一起比，c是數值d是物件，故回傳`false`。

順帶一提，如果有用JS處理時間的需求，作者不建議使用JS內建的時間方法，他推薦使用JS函示庫[moment.js](https://momentjs.com/)，這可以幫助開發者迴避一些因建構子產生的問題。
![](https://i.imgur.com/XfhjKnU.png)
　
　
　
　
## 小結
使用內建的函式建構子(JS內建函式 + new函式建構子)需要非常小心。
開發者往往覺得`new Number()`、`new String()`、`new Date()`...等寫法和`Number()`、`String()`、`Date()`的結果一樣：只是多了一個`new`建構子，一樣會建立新的 **基本型別**`Primitive Types`，但只要使用`new`在函式前面，就會創建一個新的 **物件**(型別)`Object Types`出來，而我們往往不會意識到...自己操作的其實是物件。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)5-60、5-61