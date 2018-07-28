---
title: 克服JS奇怪的部分：Day05 變數與函式環境、外部參照
date: 2018-07-28 16:41:00
tags:
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://3.bp.blogspot.com/-2i4JtO_DU_Q/W1wapAcFmXI/AAAAAAAAIaw/kaZqut-L1MAiHHQLvZ8zpxAkcFv4L-1XACLcBGAs/s1600/2018ITMANJS05.png)
<!-- more -->
> 此為筆者 2018IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10190998

執行JavaScript時，接收到翻譯的電腦會先創造一個全域執行環境。當程式呼叫函式，就會在全域環境中創造該函式的(區域)執行環境。而每個執行環境不論全域或區域，都有屬於自己的**變數作用域**存在。

來看看以下程式碼：

```JS
function b() {
	var myVar;
	console.log(myVar);
}

function a() {
	var myVar = 2;
	console.log(myVar);
	b();
}

var myVar = 1;
console.log(myVar); 
a(); 
```

這樣會顯示什麼呢?

![](https://i.imgur.com/ZfyAKoP.png)

在創造與執行階段，發生了什麼事?我們按照順序來看：

1. 首先電腦先創造 **全域執行環境**。
2. 函式a、b被設定進記憶體(但還沒被呼叫執行其內容)變數myVar也被創造設定進記憶體，並且它是全域變數，它的全域是瀏覽器(window)。
3. 接著在執行階段，賦值1給`myVar`。
4. 接著`console.log(myVar);` 印出1。
5. 接著執行(呼叫)a函式。
6. 此時 **函式a執行環境**被創造出來。
7. 在函式a的執行環境中，創造了一個函式a裡的區域變數`myVar`。
8. 函式a的變數`myVar`被賦值2。
9. `console.log`印出函式a的變數`myVar`顯示是2。
9. 接著執行(呼叫)b函式。
10. 此時 **函式b執行環境**被創造出來。
11. 在函式b的執行環境中，創造了一個函式b裡的區域變數`myVar`。
12. `console.log`印出函式b的變數`myVar`，顯示是`undefined`(因為我們沒賦值給它)。

所以雖然`myVar`被宣告了3次，但因為它都在不同的執行環境中，所以這3個其實是 **不同的變數**。
如果我們在剛剛的程式`a();` 下面再加一行`console.log(myVar); `
執行後回發現重新印出`myVar`仍然顯示1，因為這個`myVar`就是原本全域的變數`myVar`，代表其實他沒有被覆蓋掉。


如果函式裡的變數宣告沒有使用`var`呢?
看看以下程式碼

```JS
function a() {
	var myVar = 2;
	b();
}

function b() {
	console.log(myVar);
}

var myVar = 1;
a(); 
```

執行 結果是

![](https://i.imgur.com/LDvRC7C.png)

`console.log(myVar);`這段程式碼是在`function b`中
b這個函式中根本沒有宣告變數
那為何會印出1而非`undefined`?

當函式中使用到沒有定義的變數時，JS會接著到 **外部環境**尋找(外部參照)，注意這裡的外部環境不是指呼叫這個函式的執行環境，而是指定義宣告這個函式的外部環境。
函式b是在全域環境定義，在函式a中呼叫執行，既然是向定義環境找，所以函數b的外部環境就是全域環境，而不是函式a，故它就找到全域環境的變數`myVar`，最後印出1。

再看看以下程式碼
```JS
function a() {

	function b() {
		console.log(myVar);
	}

	var myVar = 2;
	b();
}

var myVar = 1;
a();
b();
```

這樣會顯示什麼呢?
![](https://i.imgur.com/SwabguR.png)

為何b會報錯?
因為全域執行環境找不到`function b`，`function b`是被定義在`function a`的環境之中，而全域執行環境只有看到`function a`的定義存在。

那為何一開始又會先顯示2呢?
當外部環境呼叫執行`function a`，`a`裏頭宣告了變數`myVar`賦值2，再呼叫執行`function b`，此時執行`function b`裡的`console.log(myVar)`，因為b裡面沒有宣告`myVar`這個變數存在，於是便往外部環境查找，也就是外部定義的環境，`b`是定義在`function a`中，所以找到的自然是`function a`的`myVar`，顯示印出2囉。

上述筆記了這麼多，那有沒有強調區域性的宣告方式?
讓變數在使用不會這麼複雜，沒宣告就會報錯，而不是外部查找。

還是有的!那就是使用 **ES6**新增的 **let**宣告!
let具有區塊範圍(block scoping)特性，用let宣告的相同變數名稱，在全域、區域、不同執行環境彼此互不影響。
　

## 小結
今天我們知道全域執行環境就有全域變數、區域執行環境就有區域變數，並且知道當區域內找不到變數時會向外查找，向外查找的外部環境是指 **外部定義的環境**，不能直接認定就是全域環境與呼叫的環境。
　
今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs)2-14~2-16

