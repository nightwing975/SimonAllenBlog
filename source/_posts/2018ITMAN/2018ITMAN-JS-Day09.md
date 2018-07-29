---
title: 克服JS奇怪的部分：Day09 強制型轉與比較運算子
date: 2018-07-29 12:11:27
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://3.bp.blogspot.com/-CzWCO7Lv9tY/W1waplo7sCI/AAAAAAAAIa4/S-CJuKPMOZ8-C11d8R9KsbhANarx4m_6QCLcBGAs/s1600/2018ITMANJS09.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10191375

---

今天我們來看看強制型轉與比較運算子

JavaScript是動態型別`Dynamic Typing`語言，故非常容易發生強制型轉這件事。

> 強制型轉`Coercion`，JavaScript自動轉換值的型別。

例如說：

```JS
var a = 1 + 2;
console.log(a)
```
![](https://i.imgur.com/AYAwxvG.png)
結果是3，合理！

那這樣呢?
```JS
var a = 1 + '2';
console.log(a)
```
![](https://i.imgur.com/yY5KfzQ.png)

發生了什麼事?

這裡我們可以用`typeof()`這個內建函式來查看一下a現在的型別
![](https://i.imgur.com/ETq5JUQ.png)
`string`(字串)

對JS而言，當數值與字串相加時，它會將數值自動轉型為字串，所以1變成'1'。
實際執行可以想像變成：
```JS
var a = '1' + '2';
```
字串'1'與字串'2'相加就是字串'12'，而不是數學意義上的相加。

以此聯想，那：
```JS
var b = 'ithome' + 30;
console.log(b)
```

console會印出什麼?
![](https://i.imgur.com/s4iHZ5K.png)
字串'ithome'與數值30相加結果是字串'ithome30'
用`typeof()`來查看一下b現在的型別
![](https://i.imgur.com/tQoqa8b.png)
`string`(字串)

...不過
很些人會以為**「阿~在JS只要數值與字串計算，數值就會被轉成字串」**
或是拿數值類比成硬幣，字串類比成物品，說硬幣(錢)可以數學計算(買賣)也可以當成物品，但物品不一定能拿來數學計算(買賣)，以此比喻數值與字串的計算狀況。

這麼想容易落入JS的陷阱裡！
看看以下程式碼：

```JS
var c = '10' * 8
console.log(c)
```

console會印出什麼?
![](https://i.imgur.com/fEhsHZn.png)

'10'在計算時型別從字串轉型成數值
用typeof()來查看一下c現在的型別
![](https://i.imgur.com/vcRPn9S.png)
`number`(數值)

```JS
var d = '10' - 8
console.log(d)
```
console會印出什麼?
![](https://i.imgur.com/DSk85jw.png)

'10'在計算時型別從字串轉型成數值
用typeof()來查看一下d現在的型別
![](https://i.imgur.com/7r4NmPj.png)
`number`(數值)
　
所以撇開`+`不說，字串和數值進行**計算**時，不是只有數值會轉成字串，字串也有可能轉成數值。


接著來看看以下程式碼
```JS
console.log(1 < 2 < 3); 
```
console會印出什麼?
![](https://i.imgur.com/Z8c6OyI.png)


合理!

那
```JS
console.log(3 < 2 < 1); 
```
console會印出什麼?
![](https://i.imgur.com/MjQA7SN.png)


疑?為什麼也是true?

誠如昨天筆記，當運算子**優先性**相同，就依據**相依性**來判斷是左到右執行還是右到左執行。
現在有兩個`<`運算子，所以**優先性**是一樣的，那就再來看`<`的相依性：
`<`相依性為左到右，所以`3 < 2 < 1` 的執行順序是先比較3和2，再比較1。
`3 < 2`相比結果是false，接著再運行`false < 1`的相比較。

在JS中，true若被強制型轉成數值會是1，false被強制型轉成數值會是0。
這裡的false因為與數值進行比較判斷，被JS自動型轉成0，變成`0 < 1`，而`0 < 1`成立，回傳結果true。
要判斷哪些東西會被轉型成數字可以用`Number()`這個內建函式：

![](https://i.imgur.com/EqgSKCA.png)

`NaN`在JS中表示 **Not a Number**
表示 **不是數值**
它也不是原始型別`Primitive Types`

所以若是今天筆記前面的例子
```JS
var b = 'ithome' + 30;
console.log(b)
```
會印出字串ithome30

那
```JS
var b = 'ithome' - 30;
console.log(b)
```
字串`string`減數值`Number`，執行時字串ithome無法被自動型轉成數值
結果自然是
![](https://i.imgur.com/0BABVsE.png)

拉回 **比較**的主題，要在JS中進行數學的比較(兩邊是否相等)，可以使用`==`運算子。

若是用`==`運算子來比較`數值`0與`布林值`false，會發生什麼事呢?
![](https://i.imgur.com/DODbSRz.png)
比較時被強制自動轉型(型別轉換)了
上面說過false與數值互動時會自動型轉成0，執行時實際變成是
```JS
0 == 0
```
0與0相等嗎?
當然囉，所以回傳true。

只不過我們都知道，0和false根本是不同的東西，它們連型別都不同呀！
這個時候，可以使用`===`運算子：

```JS
0 === false
```

![](https://i.imgur.com/jR4WFIL.png)

false！
同樣是比較左右是否相等，`==`和`===`差異是什麼？
因為`==`運算子只比較值，故型別會有轉型來轉型去的狀況，而使用三個`=`是**嚴格比較**，`===`運算子不只比對左右兩邊的值，**連型別也會一起比較**，0的型別是數值，false的型別是布林，比較過程中不轉型，兩邊不相等就回傳false囉。

引用自影片作者對三個`===`的評語

> **That is going to save your life.**

　
　
　
## 小結
JS是動態語言，會有自動轉型(型轉)的狀況發生，除非我們就是故意要讓它轉換型別來比較、運算，否則進行相等比較時，最好使用===嚴格比較。
關於JS的比較表格可以參考MDN的[相等性比較表格](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)3-24~3-26