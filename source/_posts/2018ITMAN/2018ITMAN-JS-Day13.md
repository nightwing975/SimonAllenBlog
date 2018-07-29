---
title: 克服JS奇怪的部分：Day13 物件與物件實體
date: 2018-07-29 12:40:44
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://2.bp.blogspot.com/--V9qD7gj65c/W1warOXdhQI/AAAAAAAAIbM/GGXuXKaVR0MmAWCHxf7kzQvejyL3tZz8wCLcBGAs/s1600/2018ITMANJS13.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10191841

---

今天我們來看 **物件實體語法**`object literals`

JS可以透過`new Object()`來建立物件，但開發時相對少見這種寫法。
這是因為JS還有另一種更快建立物件的方法，就是 **物件實體語法**`object literals`。
什麼是物件實體語法呢?

來看看以下程式碼：
```JS
var Batman = {};
```
用一對大括號`{}`快速建立物件，大家最常用的就是這種寫法(我也是)。
這種寫法的好處在於可以同時建立屬性和方法：
```JS
var Batman = { 
		firstname: 'Bruce',
		lastname: 'Wayne'
};
```
使用物件實體語法和這種寫法
```JS
var Batman = new Object();
Batman['firstname'] = 'Bruce';
Batman['lastname'] = 'Wayne';
```
兩者其實是一樣的

物件實體語法的好處在於可以快速、直觀的建立物件、屬性和方法，早期沒有物件實體語法，使用`[]`計算取用成員運算子來建立物件很常見，而現在最常見的方式則是使用物件實體語法`{}`。
另外`變數[屬性]`這種寫法並非被淘汰掉，有些地方還是看的到，例如我上次見到是在學 **node.js**的時候，這種方式的優點是可以使用變數的方式來替換操作，還是挺有妙用的。

當有函式需要傳參數進去時，也是可以使用物件實體語法：
```JS
var dinner = { 
	meat:'牛肉',
	vegetables:'菠菜'
};

function today(food){
	console.log('今天吃' + food.vegetables);
}

today(dinner)
```
![](https://i.imgur.com/Fg7uY31.png)

除了`today(dinner)`這種呼叫，函式傳值也可以這樣：
```JS
today({ meat:'牛肉',vegetables:'菠菜'})
```
![](https://i.imgur.com/LzsNs8B.png)

結果是一樣的，這種傳物件的方式其實也很常常見到。
　
　
　
　
## 小結
今天複習了建立物件最快速的 **物件實體語法**`object literals`，也知悉函式傳值可以直接用用物件實體語法。
至於筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)4-31
