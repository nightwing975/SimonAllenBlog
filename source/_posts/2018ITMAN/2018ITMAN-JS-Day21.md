---
title: 克服JS奇怪的部分：Day21 閉包
date: 2018-07-29 15:07:03
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://2.bp.blogspot.com/-_cRchTaZoHs/W1watZI8r3I/AAAAAAAAIbs/5vHr6fg8dAQPQF3SaaghuZGxdsYofK3wACLcBGAs/s1600/2018ITMANJS21.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10193423

---

今天來看看closure閉包

先來看看以下程式碼：
```JS
function say(whattosay){
	return function(name){
		console.log(whattosay + ' ' + name);
	}
}
```
設定一個函式陳述句，在裏頭用函式表示式回傳一個函式，並利用範圍練`scope chain`的特性放入whattosay，裏頭這個回傳函式沒有宣告whattosay，於是它會外部(參照)查找，去找設定這個函式的say函式參數whattosay。

關於`外部參照`，可以參考這天的[筆記](https://ithelp.ithome.com.tw/articles/10190998)。

當我們呼叫函式say，會得到一個值，這個值是從函式say裡面return返回的另一個函式。我們可以帶入參數，這樣呼叫函式裡的函式：
```JS
say('Hello')('Simon');
```
![](https://i.imgur.com/ofpA7Eh.png)

目前這樣還OK
接著修改一下程式碼，設定一個變數去接(指向)函式say的回傳值：
```JS
var talk = say('Hello');
talk('Simon');
```
現在talk這個變數，會指向函式say執行時回傳的函式。
再透過呼叫talk，並傳入參數字串Simom。

結果是：
![](https://i.imgur.com/B9lmN46.png)

一樣可以執行，乍看很合理，但好像有點怪怪的？
影片看到這邊我還不曉得為什麼，後面看講者的解釋才驚覺，對啊，為什麼？

> talk(指向的)函式居然能外部參照到whattosay！


**說明**
當函式被呼叫，會在全域執行環境**創造這個函式的執行環境**，在函式執行環境裡執行它的程式，當 **函式結束，其函式執行環境也結束**，而這個例子，程式碼全貌是這樣：

```JS
function say(whattosay){
	return function(name){
		console.log(whattosay + ' ' + name);
	}
}
var talk = say('Hello');
talk('Simon');
```

1. whattosay被創造在`var talk = say('Hello');`這段的`say('Hello')`，say函式執行其程式，然後回傳出裏頭的函式給變數talk指向，接著結束say函式執行環境，就算它的`arguments`有`'Hello'`，它就是結束了。

2. say函式離開JS的執行堆，每個函式都有自己的記憶體空間，函式內的變數與其他程式被設定在那裏，當函式的執行環境結束，JavaScript引擎預設會清除這個記憶體內容，這稱為`garbage collection`**垃圾回收**，即是將多餘的記憶體內容清掉，回收記憶體空間。

3. 理論上say函式的記憶體空間都被清除，whattosay自然也不在電腦記憶體裡了，say函式執行環境結束了，記憶體也被回收，但裏頭的whattosay卻沒有被電腦回收。

4. 透過變數talk去執行其指向的函式，創造一個函式執行環境。當函式發現沒有whattosay變數給它使用，便向外參照，然後居然可以找到記憶體裡的whattosay！

即便say函式已經結束了，talk的函式依然可以找到對應的外部變數，這種現象稱之為`closure`**閉包**
這是JS的特性之一：即便這個變數所在的執行環境已經結束，jS引擎仍會確保，其它函式執行時可以找到它所需的變數。

---
接著來看看另一個閉包範例：

```JS
function buildFunctions(){
	var arr = [];
	for(var i=0; i<3; i++){
		arr.push(function(){
			console.log(i);
		});
	}
	return arr;
}
```

建立一個函式buildFunctions，裏頭宣告陣列arr，並有個for迴圈陳述句，每次迴圈都會
把：
```JS
function(){
	console.log(i);
}
```
塞入arr陣列裡，最後把arr回傳出來。
　
承接程式碼內容，接著
```JS
var fs = buildFunctions();

fs[0]();
fs[1]();
fs[2]();
```
宣告變數fs並指向呼叫呼叫`buildFunctions()`後回傳的值(陣列arr)，接著個別呼叫fs指向的陣列arr，序號0、1、2的函式。

執行結果是什麼？會是印出`0、1、2`或`1、2、3`嗎？
不對，因為fs[0]、fs[1]、fs[2]現在都是：
```JS
function(){
	console.log(i);
}
```
這三個函式在buildFunctions的迴圈裡被創造，**並未執行**，所以函式內都是`console.log(i);`結果只會是印出三個一樣的`i`。接著要找i值，因為執行環境的不同，`i`理論上應隨著`buildFunctions`函式的結束而被記憶體清除，但因為閉包的特性，`i`被留下在記憶體裡，而`i`最後更新的值是3，所以結果是：
![](https://i.imgur.com/26OY8fB.png)
印出3個3
　
　
那麼，有辦法修改程式，讓其印出3個不同的**連號**嗎？
這裡有兩種方法可以處理：

1. 利用**ES6**的`let`宣告變數j，`let`強調在`{}`的區塊執行環境，每次`let`都會保留j在不同的記憶體位置，利用閉包取值時結果自然不一樣囉。
```JS
function buildFunctions(){
	var arr = [];
	for(var i=0; i<3; i++){
		let j = i;
		arr.push(function(){
			console.log(j);
		});
	}
	return arr;
}

var fs = buildFunctions();

fs[0]();
fs[1]();
fs[2]();
```
結果是：
![](https://i.imgur.com/91qDtOp.png)
　

2. 利用`IIFE`：在buildFunctionsc函式內的迴圈，裏頭的push函式改成立即執行函式，每次立即函式被創造出來就立刻執行，而且每次立即函式的執行環境都不一樣，利用此方法可以保留i的值在不同的記憶體空間，利用閉包取值時結果自然不一樣囉。

```JS
function buildFunctions(){
	var arr = [];
	for(var i=0; i<3; i++){
		arr.push(
			(function(j){
				return function(){
				console.log(j);
				}
			})(i)
		);
	}
	return arr;
}

var fs = buildFunctions();

fs[0]();
fs[1]();
fs[2]();
```
結果是：
![](https://i.imgur.com/I6J4z4r.png)
　
　
　
　
## 小結
閉包是JS語言中的一個重要特性，網路上有不少閉包文章，有興趣也可以參考[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Obsolete_Pages/Obsolete_Pages/Obsolete_Pages/%E9%96%89%E5%8C%85%E7%9A%84%E9%81%8B%E7%94%A8)的運用說明。
今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)4-46、4-47