---
title: 克服JS奇怪的部分：Day29 Object.create與class
date: 2018-07-30 15:12:10
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://1.bp.blogspot.com/-3oIT7I1LHCo/W1wavq9z47I/AAAAAAAAIcM/O3vn5wwDRlsuoMhgZEkn5ALwjo2SmaPFACLcBGAs/s1600/2018ITMANJS29.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10195322

---

今天來看看其他建立物件的方法

除了`new`建構子和物件實體語法，JS還有別種建立物件的方法，那就是ES5新增的`Object.create`和ES6新增的類別`class`。

先來看`Object.create`，以下為程式碼：
```JS
var Flash = {
	firstname: '預設值',
	lastname: '預設值',
	run: function(){
		return 'Run,' + this.firstname + ',Run!';
	}
}
var Barry = Object.create(Flash);
console.log(Barry);
```
首先用物件實體語法創建物件並以變數Flash指向它。
接著使用使用`Object.create()`方法並傳入Flash當參數，並以變數Barry指向它。
現在用`console.log`印出Barry，結果是：
![](https://i.imgur.com/ZAVRE4T.png)

真的只是一般的空物件嗎?
點開瀏覽器console印出的這個空物件
![](https://i.imgur.com/icvTMYX.png)

我們可以看到，Barry(指向的)這個物件，它的原型是Flash物件。
也就是說

> 用`Object.create()`這個方法建立物件，傳入給它的參數會成為創建出物件的原型


這種模式的好處，在於可以給物件屬性設定預設值，例如現在的Barry空物件，如果取用firstname和lastname屬性會發生什麼事？

```JS
console.log(Barry);
console.log(Barry.firstname);
console.log(Barry.lastname);
```
結果是：
![](https://i.imgur.com/oX21XWJ.png)
Barry物件明明是空物件，為何還可以顯示出東西？別忘記當物件存取不存在的屬性與方法時，會透過原型鏈向物件原型查找，可以看[第25天筆記](https://ithelp.ithome.com.tw/articles/10194350)回顧。

如果想要隱藏預設值，只要設定同樣的屬性、方法覆蓋過去就好：
```JS
Barry.firstname = 'Barry';
Barry.lastname = 'Allen';
console.log(Barry.run());
```
![](https://i.imgur.com/tdcuC4t.png)

`Object.create`是ES5以後新增的語法，某些舊版瀏覽器並不支援(例如IE8)，這時我們可以用 **polyfill**來處理。

> polyfill

有些舊版引擎會缺少開發者要用的程式語法，開發者可以用程式檢驗JS引擎(瀏覽器)是否有目標程式語法，如果沒有就由開發者撰寫程式添加進去。

如果要幫瀏覽器手動添加`Object.create`，可以這樣寫：
```JS
if(!Object.create){
	Object.create = function(o){
		if(arguments.length > 1){
			throw new Error('Object.create implementation' + ' only accepts the first parameter.');
		}
		function F() {};
		F.prototype = o;
		return new F();
	}
}
```
**說明**
先用if判斷`Object.create`是否存在，如果對`if()`內放`Object.create`這種寫法感到困惑，可以看[第10天筆記](https://ithelp.ithome.com.tw/articles/10191498)回顧。

如果`Object.create`不存在，就將一個函式表示式(並傳值o)賦予給`Object.create`。若賦予`Object.create`的函式傳入參數超過1個以上，就報錯。

接著設定空函式F，將o傳入F的prototype，最後if陳述句回傳一個物件，這個物件是用new函數建構子建立，原型為函式F的物件。

以上就是手動設定`Object.create`的程式寫法，但現在是2018年，主流瀏覽器應該都支援`Object.create`，就當是作者在影片的補充技巧吧。
　
　

---

接著是ES6新增的`class`**類別**介紹，來看看以下程式碼：
```JS
class TheFlash{

	constructor(firstname, lastname){
		this.firstname = firstname;
		this.lastname = lastname;
	}

	run(){
		return 'Run,' + this.firstname + ',Run!';
	}
}

var Barry = new TheFlash('Barry', 'Allen');
console.log(Barry);
```
我們設定一個class類別TheFlash，在裏頭有constructor建構子，和函式建構子一樣，我們可以預先設定物件值，也可以設定函式方法在class裏頭。

設定好class，要創建新物件仍然使用new並帶入參數，參數會對應到constructor的設定，以此創建物件，故上面程式碼，`console.log`結果是：
![](https://i.imgur.com/nkUJJcW.png)

在其他的語言，類別是創建物件的藍圖，但在ES6，**類別本身是被建立出來的物件**，`class TheFlash`是一個實際被建立的物件，開發者是從物件建立新物件，也就是說，ES6的JS，本質仍然是原型繼承，而非變成古典繼承。

那class要怎麼設定原型呢？可以使用ES6新增的extends來處理。
(注意：不是第26天介紹的underscore.js函式庫extend，是extends，字尾多了一個s)

延續`class TheFlash`程式碼，在程式碼下面新增另一個class：
```JS
class SpeedRunner extends TheFlash{

	constructor(firstname, lastname){
		super(firstname, lastname);
	}
}
var RedRunner = new SpeedRunner('Barry', 'Allen')
console.log(RedRunner.run());
```
設定類別SpeedRunner並`extends TheFlash`(extends會 **繼承**類別TheFlash)，在constructor內，透過super方法去獲得父類別TheFlash的建構式參數設定。

RedRunner此時的父類別是類別TheFlash，它能透過原型鏈取用方法`run()`嗎？
來看看結果
![](https://i.imgur.com/cjZyJvT.png)
　
　
　
　
## 小結
class語法本質上還是JS原型繼承，也就是所謂語法糖，extend(擴展、繼承)的概念也很容易在現今JS框架中看到，例如Vue.js容易見到extend、React容易見到extends，可以說JS的框架都是以JS原型繼承的概念為基礎並加以擴展的。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)5-63、5-64

