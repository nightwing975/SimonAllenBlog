---
title: 克服JS奇怪的部分：Day27 函式建構子與new
date: 2018-07-30 15:12:04
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://2.bp.blogspot.com/-JDpc7KX15fY/W1wau2FSrAI/AAAAAAAAIcE/l9r5RBbQDw4Y6BoSRkHqNJaTdudyTHQuwCLcBGAs/s1600/2018ITMANJS27.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10194795

---

今天進入第6章節 **建立物件**

JavaScript剛被創造出來時，為了吸引JAVA開發者借鑒了不少東西，包含名子`Java`Script，而在物件部分，向JAVA和C++借鑒了`new`這個關鍵字，`new`和物件實體與法一樣，都可以讓使用者快速建立物件，與之一起出現的用法就是`function constructor`函式建構式。

> function constructor 函式建構式(或譯函式建構子)

能用來新建物件的一種函式，透過與new運算子一起使用，能創建出新物件並設定該物件的屬性與方法


來看看程式碼
```JS
function batman(){
  this.color = 'black';
  this.city = 'Gotham City';
}
var Bruce = new batman();
console.log(Bruce);
```
結果是
![](https://upload.cc/i/13dLGZ.png)

我們建立了一個物件給變數Bruce，在函式batman裏頭的`this.color`、`this.city`，現在和物件Bruce的屬性、值一樣，發生了什麼事？

在[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)的運算子表格裡，我們可以知道new也是一個運算子，當我們使用`new`時，電腦會先建立一個空物件`{}`，接著呼叫`new`後面的函式，當函式被呼叫時，創造函式執行環境，`this`關鍵字也隨之被創造出來，接著，**this關鍵字指向了剛被new創造出來的空物件**，所以函式的`this.color`和`this.city`，就會等於在新增color和city屬性給該空物件。


我們先前知道，全域函式的this會指向全域物件，現在因為new關鍵字，this指向了Bruce，我們可以將程式碼改成這樣，多加一個`console.log(this);`，看看會多印出`window`還是Bruce指向的這個物件。
```JS
function batman(){
  this.color = 'black';
  this.city = 'Gotham City';
  console.log(this);
}
var Bruce = new batman();
console.log(Bruce);
```
![](https://upload.cc/i/yr6DT7.png)
可以了解new會改變this的對象，另外，使用new的狀況下，new後方的函式預設不會回傳值，故意在函式裡寫上`return`的話，函式反而會因為return回傳東西，依狀況決定是否把this創造的物件屬性蓋掉，看看例子

```JS
function batman(){
  this.color = 'black';
  this.city = 'Gotham City';
  console.log(this);
  return 'The Dark Knight';
}
var Bruce = new batman();
console.log(Bruce);
```
![](https://upload.cc/i/nLekta.png)
`return`返回字串'The Dark Knight'，物件看似未受影響，console印出兩個一樣的內容。
那`return`**返回一個物件呢？**

```JS
function batman(){
  this.color = 'black';
  this.city = 'Gotham City';
  console.log(this);
  return {'The Dark Knight':2008};
}
var Bruce = new batman();
console.log(Bruce);
```
![](https://upload.cc/i/ImoG8u.png)
第一個印出來的是batman函式內`console.log(this);`此時物件還沒被蓋掉，接著因為`return`一個物件出來，`console.log(Bruce);`的Bruce反而指向了最後被回傳的物件。

使用`new` 也可以快速很多建立屬性一樣，但實則各自獨立在記憶體不互相影響的物件。

```JS
function batman(){
  this.color = 'black';
  this.city = 'Gotham City';
}
var Bruce = new batman();
var Grayson = new batman();
console.log(Bruce);
console.log(Grayson);
```
![](https://upload.cc/i/WNRm2Y.png)

需要注意的是，若是使用函式建構式不用`new`，就會變成一般的呼叫函式。
```JS
var Bruce = new batman();
var Bruce = batman();
```
這兩個是不一樣的，一個是用`new`建立物件並指向變數Bruce;一個是呼叫執行函式並將結果指向變數Bruce。


`function constructor`**函式建構子**可以幫我們設定新物件，那**原型**呢？
函式就是物件，它有一些隱藏屬性，其中有個在用`function constructor`**函式建構子**才會用到屬性`prototype`。
`prototype`這個屬性名稱很令人困惑，乍看會以為是用來設定或取用原型物件的屬性，但昨天筆記看到，取用物件原型的內建屬性是`__proto__`而不是這裡的`prototype`，函式的內建屬性`prototype`不是用來取用函式自己的原型物件，而是用來取用**建構子創造出來的物件原型**。

來看看以下程式碼
```JS
function today(food1,food2){
  this.lunch = food1;
  this.dinner = food2;
}
today.prototype.eat = function(){
	return this.lunch + ' ' + this.dinner;
}

var Duke = new today('雞腿飯', '麻醬麵');
console.log(Duke);
```
我們用函式建構子新建了一個物件，並且用prototype新增eat這個方法給透過today創造出來的物件原型，檢查console印出來的物件Duke
![](https://upload.cc/i/modi3a.png)
![](https://upload.cc/i/3Hf1LD.png)
可以看到，`__proto__`裡面多了一個eat，這個eat方法便是透過prototype新增的，現在可以使用eat方法了

```JS
console.log(Duke.eat())
```
Duke並沒有eat方法，但它的原型透過prototype新增了這個函式，透過原型鏈，JS找到了eat，讓物件Duke也可以執行、存取它。
![](https://upload.cc/i/kxqP5g.png)
　
　
　
　
## 小結
透過function constructor與new建立物件，是JS很常見的用法，尤其是使用某些plugins，很常看到類似的寫法。
今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)5-57~5-59