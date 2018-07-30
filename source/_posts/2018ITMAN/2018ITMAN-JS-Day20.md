---
title: 克服JS奇怪的部分：Day20 立即呼叫的函式表示式(IIFE)
date: 2018-07-29 15:07:03
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://2.bp.blogspot.com/-26nLD68_qfQ/W1watHvD5jI/AAAAAAAAIbo/_TTa1fhUeSM7IIixTIhc9lpoi6DXcZgOgCLcBGAs/s1600/2018ITMANJS20.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10193313

---

今天來看看IIFE

> IIFE全名為`Immediately Invoked Functions Expressions`
指的是可以立即執行的`Functions Expressions`**函式表示式**，中文多譯為立即(執行)函式。

來看看以下程式碼
```JS
var hello = function(name){
	console.log('Hello ' + name);
};
```
這是一個`Functions Expressions`函式表示式，要呼叫它通常會寫成`hello()`
```JS
hello();
```
![](https://i.imgur.com/v3u8pAG.png)

目前沒有傳值進去，所以函式印出Hello undefined
若把`hello()`這句刪掉，把程式碼改成這樣：

```JS
var hello = function(name){
	console.log('Hello ' + name);
}();
```
![](https://i.imgur.com/gJILwyO.png)

電腦在函式表示式後面讀到`()`，就知道要立刻呼叫這個函式，這種立刻執行的函式寫法就稱為IIFE。


若要傳值進去可以加參數在最後面的`()`
```JS
var hello = function(name){
	console.log('Hello ' + name);
}('Simon');
```
![](https://i.imgur.com/R2CAZxZ.png)

再看看另一個例子，將函式裏頭的`console.log`改成`return`，透過另一個`console.log()`呼叫變數指向的函式。

> 一般的函式表示式

```JS
var hello2 = function(name){
	return 'Hello ' + name
};
console.log(hello2);
```
![](https://i.imgur.com/HDvo1vQ.png)

函式結尾的`}`後面沒有`()`表示立即執行，所以`console.log`印出hello2時，是印出hello2指向的函式

> IIFE

```JS
var hello2 = function(name){
	return 'Hello ' + name
}();
console.log(hello2)
```
![](https://i.imgur.com/GwKch5r.png)

函式結尾的`}`後面有`()`表示立即執行，當`console.log`印出hello2時，是印出hello2指向函式立即執行的結果值。

若要傳值進去，一樣加參數在函式最後面的`()`
```JS
var hello2 = function(name){
	return 'Hello ' + name
}('Simon');
console.log(hello2);
```
![](https://i.imgur.com/PkGpbBX.png)


再來，透過變數指向立即執行函式，要呼叫它只要透過變數名`hello2`就好，不用像一般的`Functions Expressions`寫成`hello2()`，那現在如果寫成這樣會發生什麼事呢？
```JS
var hello2 = function(name){
	return 'Hello ' + name
}('Simon');
console.log(hello2());
```
![](https://i.imgur.com/y9tFS1R.png)


hello2乍看指向立即執行函式，其實可視為指向立即執行函式的返回值`Hello Simon`，現在`console.log()`的hello2後面多加一個`()`，電腦就會往return回傳的`'Hello ' + name`去找函式，現在`'Hello ' + name`是字串`Hello Simon`，並不是函式，自然就報出錯誤囉！


---
再看看以下程式碼，在[第16天筆記](https://ithelp.ithome.com.tw/articles/10192146)，我們知道`Expression`表示式會產生(回傳)一個值，而這個值不一定會被開發者用變數指著。
現在我們不用變數指著IIFE，這種寫法可以嗎？
```JS
function(food){
	console.log('大俠愛吃' + food)
}('漢堡包');
```

![](https://i.imgur.com/vJ5bUuF.png)

因為沒有變數指向它(不是`Functions Expressions`)，它也沒有名子(不是`Function Statement`)，所以語法解析器認為這段程式寫錯了，自然報錯。

解決方法是，最外面再用一個`()`把它包起來
```JS
(function(food){
	console.log('大俠愛吃' + food)
}('漢堡包'));
```
執行結果
![](https://i.imgur.com/SYaaiEs.png)

這樣就不會報錯了，這也是IIFE，也是最常在JS框架、套件看到的寫法。
至於放在後面，立即呼叫的`()`，要放在裡面或外面都可以。

```JS
(function(food){
	console.log('大俠愛吃' + food)
})('漢堡包');
```
這樣可以，兩個結果都一樣，不過作者認為開發者擇一使用就好，因為要保持撰寫風格的一致。

---

另外，ES5版本的JS有全域執行環境(作用域)、函式執行環境(作用域)兩種，直到ES6版才出現塊級作用域，在ES6出來前，為了避免設定太多的全域變數，開發者往往會將變數設定在函式中，使其成為區域變數，尤其是設定在IIFE中，確保不會汙染到全域環境的變數。

例如
```JS
var seafood = '波士頓龍蝦';
(function(name){
	var seafood = '北海道帝王蟹';
	console.log(name + '好~想吃' + seafood);
}('Simon'));
```

結果是
![](https://i.imgur.com/ZvDUUxl.png)

因為執行環境的不同，'北海道帝王蟹'只存在IIFE裡，不會影響到外部環境的變數。
也就是說，我們可以將程式碼包在被`()`包住的IIFE裡，當IIFE和全域環境有宣告相同變數名稱時，IIFE不會蓋掉全域的變數。

不少JS框架、套件的開頭與結尾被`()`包住，程式碼被立即函式包著，其目的是怕污染到使用者(開發者)的全域環境。
反過來說，若全域和IIFE內都有重複的變數名，那這些框架、套件該如何取用全域的變數呢？

```JS
var food = '水牛城雞翅';
console.log(food);
(function(global){
    var food = '雞塊';
	global.food = '麥脆雞';
    console.log(food);
})(window);
console.log(food);
```
![](https://i.imgur.com/JMPzFuw.png)
為避免無意義的外部查找，將window當參數傳入，使其成為這個IIFE的區域物件，確保在IIFE內的程式能故意取用到全域的特定變數。
　
　
　
　
## 小結

使用立即函式的好處，除了可以立即執行程式碼，省略多餘的呼叫，還可以用來避免汙染全域執行環境的東西，減少開發時因相同命名相衝的bug。
今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)4-44、4-45