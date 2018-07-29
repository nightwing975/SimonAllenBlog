---
title: 克服JS奇怪的部分：Day08 運算子的優先性
date: 2018-07-29 12:01:02
categories: 2018 IT鐵人賽
tags:
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://2.bp.blogspot.com/-AlP52UWbvJc/W1wap5U2eEI/AAAAAAAAIa8/yMTbQ30ZbWQvKnMtZi_ytYOAeLf-t853gCLcBGAs/s1600/2018ITMANJS08.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10191292

---

今天繼續來看運算子，昨天提到：運算子可以想成是一個函式，這個函式會將前後兩個參數，傳入對應的JS內建函式中，進行運算並回傳。

那是什麼決定運算子的執行順序呢？
這就看運算子的兩個特性，`優先性`與`相依性`來決定。

## 優先性
> 表示哪一個運算子被優先運算，當同一行程式有不只一個運算子時，具高優先性的運算子會先計算，然後依序算到排序等級低的運算子。

~~原來運算子也有分社會階級~~

## 相依性
> 表示運算子被計算的順序，若是左到右計算的運算子就稱為左相依性，右到左則稱為右相依性。

若運算子的優先性都相同，那就是依據`相依性`來判斷順序，決定是左到右還是右到左運算。

來看看以下程式碼：
```JS
var a = 3 + 4 * 5;
console.log(a)
```

現在有`+`和`*` 兩個運算子透過`=`賦值給a
JavaScript會先執行哪一個呢?
在JavaScript中，`*`運算子優先性比`+`還高(先乘除後加減)，所以會等於23，輸出結果看看：
![](https://i.imgur.com/9j1zxFP.png)

一般常見的運算子如`+-*/`(加減乘除)與物件的`.`運算子都是左相依性的。
那有沒有右相依性的運算子呢?

有的！
例如我們常用的`=`就是，在JS中一個`=`符號也是運算子，它並不是數學運算上的「等於」，它可以把右邊的東西賦值、指向(或傳址)給左邊的東西。

例如：
```JS
var Tony = "IronMan"
```

~~X 宣告變數`Tony`，它和字串`IronMan`相等。~~
O 宣告變數`Tony`，接著=運算子右邊的字串`IronMan`賦值給左邊變數`Tony`

所以`=`運算子是右相依性。
若是要判斷數學上的「**等於**」，左右兩邊是否是一樣的東西，在JS中就要用 `==` 和 `===` 來比。

拉回來，再看看以下程式碼：

```JS
var a = 2, b = 3, c = 4;
a = b = c;
console.log(a);
console.log(b);
console.log(c);
```
結果會是多少？
![](https://i.imgur.com/D6qM7Gc.png)

為何都是4呢？

看到`a = b = c;`時，我們習慣從左邊看到右邊。
(先看`a`，再看 `=`，再看到`b`，再來看到 `=`，最後是`c`)

但是`=`運算子是右相依性的，實際上是右到左：
1.c先賦值給b
2.b再賦值給a

也就是說，執行時程式可以這樣想：

```JS
var a = 2, b = 3, c = 4;
b = c;
//此時b也是4了

a = b;
//此時a也是4了
console.log(a);
console.log(b);
console.log(c);
```

因此abc自然都是4囉！
　
　
　
## 小結
關於運算子的優先順序，我們可以參考MDN的[運算子優先等級](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)
裏頭有清楚列出JS對不同運算子的執行順序。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)3-22、3-23
