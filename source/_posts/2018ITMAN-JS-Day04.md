---
title: 克服JS奇怪的部分：Day04 undefined與not defined
date: 2018-07-28 16:35:25
tags:
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://4.bp.blogspot.com/-ujC_t_jn8bM/W1waoo-OThI/AAAAAAAAIao/8upC6aL0wQITKLJopMwLig2Eak-ctW7PgCLcBGAs/s1600/2018ITMANJS04.png)
<!-- more -->
> 此為筆者 2018IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10190904

昨天的筆記出現`undefined`與`not defined`，今天我們來看看兩者的差異。

`undefined`和`not defined`「字面上」來看都是未定義、無定義、沒有定義..之類的。
但對JavaScript(以下簡稱JS)而言，兩者的意涵完全不相同。在JS的世界裡，當我們宣告變數時，JS會給變數預設的值，就是`undefined`。

```JS
var a = 100;
// 或你要用ES6語法也可以
// let a = 100;
```

其實JS背後是這樣運作的，執行程式時，電腦先建立全域執行環境，接著創造階段把變數創造進記憶體。

```JS
var a; // 建立變數a
// 此時a為undefined
```

創造階段結束後JS才把100賦值給a
那如果直接這樣寫：

```JS
console.log(b);
```
沒宣告b就直接用console.log印出來，結果是：
![](https://i.imgur.com/K1aOVad.png)
這是怎麼一回事呢?

因為我們並沒有宣告b這個變數存在，在電腦或瀏覽器執行JS程式碼時，會先創造全域執行環境，然後創造我們宣告的變數進記憶體。

接著執行程式的運算敘述與判斷，因為我們沒宣告b，電腦執行`console.log(b)`這段時，發現找不到b這個變數。既然記憶體裡沒有變數b，那自然返回`b is not defined`，因為b這個東西並沒有被定義其存在。

若是
```JS
console.log(a);
var a = 100;
```

誠如[昨天筆記](https://ithelp.ithome.com.tw/articles/10190700)，執行時建立全域執行環境，創造宣告的東西，於是變數a被創造了，並且JS預設這個a值是`undefined`，表示其值還未被定義，我們可以賦值、指向其他東西給a。

另外，`undefined`不代表什麼都沒有，在JS中`undefined`既是個型別，也是一個值，這個型別`undefined`的值就叫`undefined`，既然不代表什麼都沒有，那僅宣告變數自然也會佔據記憶體空間。

課程影片的最後，提到盡量避免這種寫法：

```JS
var a = undefined;
```

程式運行可以拆成這樣想

```JS
var a //創造變數，此時a為undefined
a = undefined //此時a賦值為undefined
```
不要給變數手動賦值`undefined`，因為當JS創造變數時，預設就已經是`undefined`了。
不管是要debug還是預先宣告變數而不賦值，這樣多此一舉，可能會導致除錯時，分不清楚`undefined`是一開始執行時給的，還是自己賦值進去的。
若要先宣告變數又暫時不給值，或要表示後續程式碼才會用該變數時，可以先這樣寫：

```JS
var a;
```
或
```JS
var a = null;
```

`null`有空空如也的含意，它和`undefined`一樣，既是個值也是一個型別。
　
　
　
　
## 小結
`undefined`和`not defined`是不一樣的東西：
`undefined`則是建立後尚未賦值時，**預設的值**，而這個值表示**值還沒被定義**。
`not defined`是變數、函式..等未被宣告建立、定義，在電腦執行程式碼時找不到這個東西，可以想成 **在程式與記憶體中未被定義**。
　
今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)2-11