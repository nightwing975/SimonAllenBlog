---
title: 克服JS奇怪的部分：Day11 設定函式內的預設值
date: 2018-07-29 12:40:30
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://4.bp.blogspot.com/-pkhOjZdwszc/W1waqQFCKjI/AAAAAAAAIbI/nqI0lQGDL_oXqSuFjCKd7DwhGIuWN3oAQCLcBGAs/s1600/2018ITMANJS11.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10191599

---

今天筆記是昨天內容的衍生

開發者用JS程式呼叫函式時，傳參數進去處理是很常見的方式，如果呼叫時沒有帶入參數，會發生什麼事呢?
來看看以下程式碼：

```JS
function greet(name){
	console.log('Hello ' + name);
}
greet()
```
結果是：
![](https://i.imgur.com/H8N5OS1.png)

發生了什麼事？
我們沒有帶值進去，JS在呼叫函式時也沒報錯，因為傳入的值在呼叫階段被設定進記憶體，但我們什麼都沒有傳，所以JS把這個空的東西，在記憶體被設定成`undefined`並傳入。
當`undefined`與字串相加時被轉型成字串`'undefined'`，自然印出`'Hello undefined'`囉！

但是這並不符合開發者預期的狀況，有沒有辦法讓我們呼叫函式，在未帶入值的時後，函式內有個預設值存在嗎？
這個時候可以使用昨天[筆記](https://ithelp.ithome.com.tw/articles/10191498)的方法。

```JS
function greet(name){
	name = name || '<Your name here>';
	console.log('Hello ' + name);
}
greet();
```

利用了先前提到，`||`運算子與型別轉換的特性，在JS自動型轉時，`undefined`會被轉成`false`，而有值的東西會被轉為`true`，`||`運算子可以想成中文「**或**」，當`||`運算子左邊的東西成立時(被轉型為`true`)，左相依性的`||`運算子就不會去處理右邊的東西(因為已經找到`true`了)，若`||`左邊是`false`，才去檢查右邊是不是`true`。

例如：
![](https://i.imgur.com/GBusic7.png)

拉回greet這個函式，利用此一特性，若是呼叫函式沒帶值，此時帶入的是`undefined`
```JS
name = name || '<Your name here>';
```
變成
```JS
name = undefined || '<Your name here>';
```
當`undefined`與有值(且非`null`、`''`、`false`)的東西比就會是
```JS
name = undefined(false) || true
```
`||`運算子會選擇`true`
結果就是：
![](https://i.imgur.com/7ubUP9v.png)

若我們呼叫函式時有帶值
```JS
greet('Tony');
```

此時函式內的運作變成這樣
```JS
name = 'Tony' || '<Your name here>';
```

`||`運算子先看左邊的東西，左邊一判斷為`true`就回傳(不往右邊看)，因此會賦值的是`'Tony'`，而不是`'<Your name here>'`。
![](https://i.imgur.com/xGuHGrB.png)

也就是說用這種方法，開發者可以在不影響結果的狀況，讓JS函式內有有預設值，不管有沒有帶值進去，都不會被`undefined`影響呼叫函式的結果。
　
　
　
　
## 小結
與昨天筆記內容相似，使用`||`與JS自動轉換型別的特性，可以做出預設值的效果，這種方式通常會在JS函式庫、別人寫好的Plugin看見，有時也挺有妙用。

鐵人賽一轉眼就進入第11天，剛好課程第三章節也在這邊告一段落，明天進入第四章節 **物件**的部分，這部分也是影片最多、最重要的，加油加油。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)3-28