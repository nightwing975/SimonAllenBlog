---
title: 克服JS奇怪的部分：Day24 函數程式設計
date: 2018-07-29 15:07:03
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://3.bp.blogspot.com/-RnJNWoqX6t4/W1wauL9B0DI/AAAAAAAAIb4/O8Lbu4xlYawH4zjrj7mLrCiB-on1SE15QCLcBGAs/s1600/2018ITMANJS24.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10194133

---

今天來看函數程式設計的兩節影片
(udemy這系列影片稱呼Function為 **函數**，但我個人比較習慣稱呼 **函式**)

函數程式設計課程不可能在一兩節影片就講完，作者解釋他只是希望在這系列課程引入這個概念，以簡短影片讓人先體會它的威力。

先來看看以下程式碼

```JS
var arr1 = [1, 2, 3];
var arr2 = [];
```
現在有陣列arr1、arr2，我們想要複製arr1陣列的內容給arr2，並讓arr2陣列的值變arr1的2倍。
一般接續內容可能這樣寫：
```JS
for(var i=0; i < arr1.length; i++){

	arr2.push(arr1[i] * 2);

}
console.log(arr2);
```
結果是
![](https://i.imgur.com/TgipMci.png)

因為JS具有`一級函式`的特性，我們可以將程式步驟拆成一個個函式，透過函式來讓程式碼更有結構性。

改成這樣
```JS
var arr1 = [1, 2, 3];

function mapForEach(arr, fn){
	var newArr = [];
	for (var i=0; i < arr.length; i++){
		newArr.push(
			fn(arr[i])
		)
	};
	return newArr;
}

var arr2 = mapForEach(arr1, function(item){
	return item * 2;
});
console.log(arr2);

```

**說明**
將建立arr1以外的新陣列程式碼，改在mapForEach函式裏頭處理，然後用變數arr2指向呼叫mapForEach函式後的的回傳值。

呼叫mapForEach函式時帶入兩個參數，一是參考對象陣列，二是匿名函式，告訴mapForEach函式要處理什麼，這裡告訴mapForEach要將陣列參數乘與二並回傳。

結果是
![](https://i.imgur.com/hL58QSp.png)

這種做法比原本的方式複雜，優點在哪？
若我們突然需要比對陣列的值(例如是否有值大於2)，就可以接續程式碼很快地新增內容
```JS
var arr3 = mapForEach(arr1, function(item){
	return item > 2;
});
console.log(arr3);
```
![](https://i.imgur.com/bAKhRZp.png)

也就是說，利用獨立出mapForEach函式這種寫法，我們可以更方便的對陣列做各種運算。
我們也可以用bind()來修改比較是否超過2這個函式，這次將函式修改得更具彈性。
```JS
var checkPastLimit = function(limiter, item){
	return item > limiter;
}
```
建立變數指向checkPastLimit指向一個匿名函式，檢查傳入的item參數是否有超過傳入limiter參數。
接著
```JS
var arr4 = mapForEach(arr1, checkPastLimit.bind(this, 2));
console.log(arr4);
```
建立變數arr4並指向呼叫mapForEach後的回傳值，這裡一樣帶入arr1當參數，另一個參數帶入checkPastLimit，這裡用bind並不是要修改this，而是為了用`bind`的特性預設參數，來達成function currying的目的，關於`bind`可以看[昨天的筆記](https://ithelp.ithome.com.tw/articles/10193895)。

結果是
![](https://i.imgur.com/B5vP338.png)

那有沒有更簡潔的方法，有！
```JS
var checkPastLimitSimplified = function(limiter){
	return function(limiter, item){
		return item > limiter;
	}.bind(this, limiter);
};

var arr5 = mapForEach(arr1, checkPastLimitSimplified(2));
console.log(arr5);
```

**說明**
建立變數checkPastLimitSimplified指向一個匿名函式，這個函式可以帶入參數limiter，若是執行它會回傳這個匿名函式
```JS
function(limiter, item){
	return item > limiter;
}.bind(this, limiter);

```
這個被回傳的匿名函式，可以再帶入另一個參數limiter和item，然後回傳item參數是否大於limiter參數的布林值，我們用bind來預設一個參數limiter。

現在建立變數arr5，並指向呼叫mapForEach函式的回傳值，帶入arr1陣列與呼叫`checkPastLimitSimplified(2)`的回傳值。

結果是
![](https://i.imgur.com/d98P44y.png)

基本上就是利用閉包特性，`return`再`return`，但太簡潔的話，易讀性就不一定好了，這裡只是做為一個範例。


到這邊只用了幾行程式碼，就能快速創建不一樣的陣列，而且沒有功能重複的程式碼，
作者認為函數程式設計，一開始很難，但只要習慣分割程式到函式，讓函式互傳這種寫法，不僅程式碼簡潔，開發時速度也會快很多。

---


接下來的影片，作者提到了兩個函式庫：

> [underscore.js](http://underscorejs.org/)

![](https://i.imgur.com/MLzhgvo.png)

> [lodash.js](https://lodash.com/)

![](https://i.imgur.com/gFYCKfZ.png)

他們都用到了函數程式設計的概念，並且語法簡潔、速度快，影片中很推崇。
作者推薦除了使用外，也可以下載js原始碼(開發者版本)來研究好的程式碼是怎麼撰寫、思考的。

接著的範例需要下載[underscore.js](http://underscorejs.org/)才能繼續，在網頁引入[underscore.js](http://underscorejs.org/)函式庫後，繼續程式碼的內容。

![](https://i.imgur.com/SUq1GHG.png)

來看看以下程式碼
```JS
var arr6 = _.map(arr1, function(item){
	return item * 3;
});
console.log(arr6);
```

`_.map()`是underscore函式庫定義的函式，功能類似今天筆記的mapForEach函式
第一個參數一樣帶入arr1，結果是

![](https://i.imgur.com/rEafJTo.png)



再看看以下程式碼
```JS
var arr7 = _.filter([2,3,4,5,6,7], function(item){
	return item % 2 === 0;
});
console.log(arr7);
```
`_.filter`這個函式可以幫我們篩選陣列的值，帶入陣列`[2,3,4,5,6,7]`作為filter函式第一個參數，然後第二個參數函式，內容是回傳第一個參數`[2,3,4,5,6,7]`個別能夠被2整除的值。

結果是
![](https://i.imgur.com/JT3QLyo.png)
　
　
　
　
　
## 小結
今天影片這樣看下來，使用函數程式設計的優點有
* **簡潔**
* **速度快**
* **維護性好**
* **可重複利用**

雖然內容好像和我還有點距離，但的確也是一個往前邁進的目標，到這邊課程第4章節總算結束了，明天進入第5章節：**javaScript的物件導向與原型繼承**。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)4-51、4-51