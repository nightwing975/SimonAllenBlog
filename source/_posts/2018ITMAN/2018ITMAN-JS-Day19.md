---
title: 克服JS奇怪的部分：Day19 陣列、arguments、spread與分號
date: 2018-07-29 15:07:00
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://4.bp.blogspot.com/-xHqt9uDhMbU/W1wasxRbtNI/AAAAAAAAIbk/npAhaUKLa8szIhFSZSWAdxURTvSBiLtcQCLcBGAs/s1600/2018ITMANJS19.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10192906

---

今天的筆記內容比較雜一點。

> 陣列——任何東西的集合

要建立一個JS陣列可以這樣寫
```JS
var arr = new Array();
```
也可以使用**陣列實體語法**來建立
```JS
var arr = [];
```
此外，JS的陣列與物件很像，可以放各種資料，例如：布林值、物件和函數..等等。
例如：
```JS
var arr = [
	24,
	true,
	{ name: 'Simon', isF2E: true}, 
	function(name){
		console.log(name + '挑戰鐵人賽第19天');
	},
	"hello"
];
```
陣列是從0開始數，如果要執行陣列序號3的函式，並帶入陣列序號2的物件屬性值，可以這樣寫：
```JS
arr[3](arr[2].name);
```
結果是
![](https://i.imgur.com/w4nUCcr.png)

---

> arguments與spread

先來看`arguments`

arguments與this一樣，當一個函式的執行環境被創造出來時，也會一併創造出`arguments`這個關鍵字，它會保存所有傳進函式(當參數)的值。

```JS
function JusticeLeague(hero1, hero2, hero3){
	console.log(hero1 + ',' + hero2 + ',' + hero3);
	console.log(arguments);
}

JusticeLeague('Batman','Superman','Wonderwoman')
```
結果是
![](https://i.imgur.com/avFrScv.png)

arguments雖然有`[]`符號，但**並不是真正的陣列**，仔細看它也有點斜斜的，我們可以用`typeof`來檢查它
```JS
function JusticeLeague(hero1, hero2, hero3){
	console.log(typeof arguments);
}
JusticeLeague('Batman','Superman','Wonderwoman')
```

![](https://i.imgur.com/j6csUuU.png)
可以看到，它其實是一個物件。

有興趣可以參考[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Functions/arguments)和[MSDN](https://msdn.microsoft.com/zh-tw/library/87dw3w1k(v=vs.94).aspx)的介紹

---

> spread運算子

(這節影片裡的說的`spread operator`其實更像是`rest operator`，不曉得有沒有人注意到。)

`spread operator`**展開運算子**和`rest operator`**其餘運算子**都是`...`符號，並且是 **ES6**以後才新增的內容，這兩者根據使用的狀況和情境有很大的差別。

可能是因為作者錄製影片時，`...`運算子的規範還不齊全，所以影片快速帶過，這邊有興趣可以參考：
* [MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Functions/rest_parameters)
* [從ES6開始的JavaScript學習生活](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/rest_spread.html)
* [[筆記] JavaScript ES6 中的展開運算子（spread operator）和其餘運算子（rest operator）](https://pjchender.blogspot.tw/2017/01/es6-spread-operatorrest-operator.html) by PJCHEN

---

> 重載函式

其他程式語言如JAVA、C#有重載函式的特性，可以創建名稱一樣，功能卻略有差別的函式。**JS並沒有這種特性**，只能在函式裡寫判斷，並用複數函式互相呼叫，來達成類似的效果。

關於重載函式可以參考[維基百科](https://zh.wikipedia.org/wiki/%E5%87%BD%E6%95%B0%E9%87%8D%E8%BD%BD)的介紹。

---

> 分號

JS會幫我們自動加上分號，更確切的說是**語法解析器**幫我們加上的。
大部分的程式語言需要在程式碼結尾加上`;`分號，但是JS卻可以不用加，並不是說它不需要分號表示結束，而是語法解析器在轉譯我們寫的程式碼給電腦時，會自動幫我們在結尾加上`;`分號。

只是自動加上分號的特性，偶爾會造成bug，例如：
```JS
function hero(){
	return
	{
		Batman: 'Wayne'
	}
}

console.log(hero());
```
![](https://i.imgur.com/ognVq2o.png)

如果語法解析器發現`return`後面有鍵盤Enter換行，會以為開發者忘記加上分號而替我們補上，程式碼被翻譯給電腦時會變成這樣：
```JS
function hero(){
	return;
	{
		Batman: 'Wayne'
	}
}

console.log(hero());
```
![](https://i.imgur.com/XHtlrrA.png)

函式執行到return就結束了，那這種情況該怎麼處理？
只要`return`後面還有其他字符的存在，JS就會知道我們程式還沒結束。
```JS
function hero(){
	return {
		Batman: 'Wayne'
	}
}

console.log(hero());
```
![](https://i.imgur.com/On6Obuc.png)

作者認為物件實體語法的`{`符號最好接在上一行的句尾，例如for迴圈的`{`、if陳述句`{`，樣才可以避免一些地方因為被自動加上`;`而產生bug。
　
　

　
## 小結
今天內容稍嫌雜亂，因為跨好幾個影片章節(影片內容也比較短)，明天來看看何謂IIFE。
  
至於筆記對象可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)4-38~4-42的部分