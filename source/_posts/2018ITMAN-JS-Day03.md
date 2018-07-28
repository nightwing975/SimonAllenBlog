---
title: 克服JS奇怪的部分：Day03 執行環境:創造與提升
date: 2018-07-28 16:28:38
tags:
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://2.bp.blogspot.com/-vuFXuz8wvrM/W1waoPQRDFI/AAAAAAAAIak/KE7E9Zpz3q0IskEuEsIOCPK0WjJUfj_nACLcBGAs/s1600/2018ITMANJS03.png)
<!-- more -->
> 此為筆者 2018IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10190700

JavaScript在電腦上要執行時，會經過創造階段，這會對我們的程式碼造成什麼影響?
來看看以下程式碼：

```JS
var a = 'Hello World!';

function b() {
    console.log('Called b!');
}

b();
console.log(a);
```
用瀏覽器Console來看
![](https://i.imgur.com/C8ZG6K0.png)
嗯!很正常

那這樣呢?下面這種寫法，有些程式語言會報錯，但JS可不會呦
```JS
b(); 
console.log(a);

var a = 'Hello World!';

function b() {
    console.log('Called b!');
}
```
用瀏覽器Console來看
![](https://i.imgur.com/HuThyMY.png)
Called b!正常被印出來，代表函式b有正常執行，但是`undefined`呢?

當我們在編輯器存檔，並在網頁重新整理(執行時)，我們寫的JS在執行前會先經過一個階段：

> 創造(creation)：直譯器將我們程式所寫的變數與函式創造並設定到電腦的記憶體裡。

實際上語法解析器將我們的程式碼轉換給電腦時，它會知道我們在程式的哪裡宣告變數與函式，在**創造**(給電腦)階段會先將變數與函式的定義按順序設定在記憶體裡，然後才會**執行**我們其他的程式。
　
那為什麼變數a會是`undefined`?
在轉換給電腦執行的創造階段時，函數與其函數內容(定義)會先存至記憶體，所以可以正確的呼叫，呼叫後建立這個函式的執行環境，印出Called b!
　
而變數雖然也會被創造存至記憶體，但其賦值內容並沒有再創造時跟著被進去，這邊可以把創造變數與賦值給變數當成兩件事，所以呼叫變數a才會印出`undefined`
　
上面的一樣的程式碼，電腦執行時可以想像成是這樣的順序：
```JS
var a;

function b() {
    console.log('Called b!');
}

b();
console.log(a);
a = 'Hello World!';
```
結果會是一樣的，再看看以下程式碼：
```JS
boy();
function boy() {
    var a = 'Yoo!';
	console.log(a);
}
```
結果會是怎麼樣?
用瀏覽器Console來看
![](https://i.imgur.com/t1ehlSQ.png)

和變數有點不一樣，function是呼叫才創造執行環境並在裏頭創造他的區域變數，故可以正常顯示出來。
那如果是這樣呢?

```JS
boy();
var boy = function() {
    var a = 'Yoo!';
	console.log(a);
}
```
好像差不多，結果是?
用瀏覽器Console來看
![](https://i.imgur.com/bZgLtLK.png)


執行時我們可以這樣想：

var boy先被提升到最上面(創造變數boy並設定進記憶體)
接著呼叫執行boy函式
給變數boy賦值(指向)函式

呼叫執行boy函式時報錯，是因為變數boy這個時候**根本沒有被賦值**，何況是指向函式呼叫呢？所以自然報錯`boy is not a function`，可以想像成是這樣：

```JS
var boy
boy();
boy = function() {
    var a = 'Yoo!';
	console.log(a);
}
```
結果會是一樣的
　
　
當然這不代表我們寫在編輯器或IDE上的JS程式被竄寫成這樣，而是電腦執行JS的順序變成這樣，這種乍看變數與函式被拉到作用域最前面的現象稱為**Hoisting**
這裡也可以參考[MDN](https://developer.mozilla.org/zh-TW/docs/Glossary/Hoisting)與[W3C](https://www.w3schools.com/js/js_hoisting.asp)

## 小結
今天複習了執行環境的創造與Hoisting，Hoisting是JS奇妙的特性之一，也是初級前端工程師面試常考的題目之一，實際開發時最好注意宣告變數與定義函式的部分，可以降低雷點的發生。 
　
今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)2-10
