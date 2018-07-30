---
title: 克服JS奇怪的部分：Day22 Function Factories、閉包與回呼
date: 2018-07-29 15:07:03
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://2.bp.blogspot.com/-HDyRxmLiOnA/W1wat6uQN7I/AAAAAAAAIb0/Ft7ChmeglBofg9xz7vhZhyiDG8SHaS7TQCLcBGAs/s1600/2018ITMANJS22.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10193656

---

今天來看Function Factories、閉包與回呼這兩個章節

> Function Factories

JS並沒有 **重載函式**的特性，但是可以用函式傳入的參數，在函式裡用if判斷達成類似的效果。
例如這樣：
```JS
function hello(firstname, lastname, language){
	language = language || 'en';

	if(language === 'en'){
		console.log('Hello ' + firstname + ' ' + lastname);
	}else if(language === 'es'){
		console.log('Hola ' + firstname + ' ' + lastname);
	}
}

hello('Simon' , 'Duke', 'en')
hello('Simon' , 'Duke', 'es')
```
![](https://i.imgur.com/QNOrpvm.png)

這種作法的缺點是，若函式裡的判斷變多、傳進去的參數變多(光現在就要帶入三個參數..再更多呢？)，那呼叫時的 **易讀性**會降低不少，開發者可以用閉包的特性，修改其內容。

```JS
function makeHello(language){

	return function(firstname , lastname){

		if(language === 'en'){
			console.log('Hello ' + firstname + ' ' + lastname);
		}else if(language === 'es'){
			console.log('Hola ' + firstname + ' ' + lastname);
		}
	}
}


var englishHello = makeHello('en');
var spanishHello = makeHello('es');

englishHello('Simon', 'Duke')
spanishHello('Simon', 'Duke')
```
結果是
![](https://i.imgur.com/P8J4pr8.png)

說明
1. 定義一個函式敘述句makeHello
2. 宣告變數englishHello指向`makeHello('en');`的回傳值(函式)

> 此時創造makeHello函式執行環境，帶入參數`'en'`，執行函式內程式，回傳結果給englishHello後，結束函式執行環境，電腦回收多餘記憶體。

3. 宣告變數spanishHello指向`makeHello('es');`的回傳值(函式)

> 此時創造makeHello函式執行環境，帶入參數`'es'`，執行函式內程式，回傳結果給spanishHello後，結束函式執行環境，電腦回收多餘記憶體。

4. 呼叫執行變數englishHello指向的函式，並帶入字串Simon、Duke。
5. 呼叫執行變數spanishHello指向的函式，並帶入字串Simon、Duke。

在2、3步驟，變數englishHello和變數englishHello指向的函式內容，都是：
```JS
function(firstname , lastname){

	if(language === 'en'){
		console.log('Hello ' + firstname + ' ' + lastname);
	}else if(language === 'es'){
		console.log('Hola ' + firstname + ' ' + lastname);
	}
}
```
由於makeHello函式的執行環境分別創造、結束兩次，現在變數englishHello和變數spanishHello指向的函式其實存在不同的記憶體位置(它們仍然是`by reference`)。

現在呼叫執行`englishHello()`，可以得到英文的問好；呼叫執行`spanishHello()`，可以得到西班牙文的問好，需帶入的參數不用理`en`、`es`而減少至兩個了。

藉由此方法與閉包的特性，我們可以創造新函式，用閉包設定函式內的預設參數，來設定、優化重載函式的效果。

---

> 閉包與回呼

我們常用的setTimeout，既是**非同步**，也用到了 **函式表示式**和 **閉包**的特性。
```JS
function sayHi(){

	var hi = '你好';

	setTimeout(function(){
		console.log(hi);
	}, 3000);
}
sayHi();
```
結果是..3秒後顯示出
![](https://i.imgur.com/i2LNcQu.png)

`setTimeout()`這個方法，接受我們傳入兩個參數(匿名函式、3000)，這個被傳入的函式就是 **函式表示式**(因為 **一級函式**的特性，可以把函式被當成值傳來傳去)，而關於非同步事件則可以參考這天[筆記](https://ithelp.ithome.com.tw/articles/10191094)

呼叫執行sayHi函式，setTimeout註冊事件，3秒後執行傳給setTimeout當參數的匿名函式，印出變數hi的值你好。
但3秒後sayHi函式的執行環境早就結束了，這個hi變數理應隨著sayHi執行環境結束而從記憶體清除，只是因為 **閉包**的特性，hi仍存在記憶體，所以才可以正常顯示出來。

> 回乎函式
當我們呼叫、使用了一個函式，可以當成「call」一個函式，當這個函式執行到一定程度，在裏頭呼叫「call」另一個函式，這個被叫來的函式就是`callback function`**回乎函式**。

```JS
function tellMeWhenDone(callback){

	console.log('已完成!');

	callback();
}

tellMeWhenDone(function(){
	console.log('全部完成囉!');
});
```

設定一個函式陳述句tellMeWhenDone，若執行時傳入函式當參數給它，在`console.log('已完成!');`這段程式碼執行完畢，它就會呼叫執行這個被傳入的函式，回應tellMeWhenDone呼叫的函式就是回乎函式。
結果是
![](https://i.imgur.com/8yNZ38n.png)
　
　
　
　
## 小結
原來setTimeout有利用到閉包的特性，之前只知道它是非同步、需要傳入函式表示式，但還沒有深入去思考它閉包的關聯。今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)4-48、4-49