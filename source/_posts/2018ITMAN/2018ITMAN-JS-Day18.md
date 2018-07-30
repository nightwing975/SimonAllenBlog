---
title: 克服JS奇怪的部分：Day18 物件、函式與「this」
date: 2018-07-29 15:06:57
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://4.bp.blogspot.com/-UMpYJ2Vufko/W1wasZPOmzI/AAAAAAAAIbc/FBHaaHgtO7AOzrwZ2_IJuscqyM-F2JD0ACLcBGAs/s1600/2018ITMANJS18.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10192737

---

今天來看看`this`

JavaScript在建立執行環境時，不論是全域、區域執行環境，在創造時會一併建立一個變數 `this`。而this會指向呼叫函式的執行環境，更進一步的說，this會指向函式目前所在物件。

如果我們直接這麼寫，這段程式碼的this會指向誰?
```JS
console.log(this);
```
![](https://i.imgur.com/8Wn26Mt.png)

我們在全域執行環境呼叫this，此時它會指向全域物件，也就是window

那這樣呢？
```JS
function a(){
	console.log(this);
}

var b = function(){
	console.log(this);
}

a();
b();
```
呼叫a、b函式，電腦創造函式a、b的執行環境，此時a、b函式內的this也一併被創造出來，那this會分別指向誰呢？

![](https://i.imgur.com/1a2j2rJ.png)

還蠻合理的，畢竟`a()`、`b()`也視同`window.a()`、`window.b()`，這代表不管用`函式表示式`或`函式陳述句`，只要在全域定義、呼叫創造其執行環境，這時的this會指向全域物件。

但也因為函式a、b內的this指向全域物件，我們在函式a、b內使用this，就可以影響全域物件的內容(屬性)，這樣很容易與我們在全域宣告的變數名稱相衝到。

例如：
```JS
var tomato = "牛番茄"
function a(){
	this.tomato = "聖女番茄"
}

a();
console.log(tomato)
```

![](https://i.imgur.com/IEcIPS1.png)

1. 我在全域宣告一個變數tomato，賦值`"牛番茄"`。

2. 呼叫函式a時，函式a的執行環境被創造出來，同時函式a的執行環境也創造了它自己的this。

3. 賦值`"聖女番茄"`給函式a的this.tomato，因為函式a自己的this指向全域，這下可好了，在全域的變數tomato，其值被蓋過從`"牛番茄"`變成了`"聖女番茄"`。

所以使用this必須要小心，得清楚它指向的物件對象是誰，否則可能會產生一些開發上的bug。

若是物件的方法(Method)裡的this呢？

```JS
var c = {
	name: "我是物件c",
	log: function(){
		console.log(this);
	}
}

c.log();
```
`c.log()`的結果是？

![](https://i.imgur.com/SMok0K6.png)

當函式變成連結到物件的方法，函式的this關鍵字就會指向這個物件。
是故this指向了c這個物件，合理！

可以這樣衍生
```JS
var c = {
	name: "我是物件c",
	log: function(){
	this.name = "更新物件c"
		console.log(this);
	}
}

c.log();
```
![](https://i.imgur.com/ltghdgP.png)

可以看到物件的name，其值被替換成`更新物件c`，這意味著我們在開發時，可以使用方法中的this，影響包住這個方法的物件，或是取用同一物件的其他方法或屬性。


然而，JS的this特性，有一點很奇怪，常常被認定是bug

```JS
var c = {
	name: "我是物件c",
	log: function(){
		this.name = "更新物件c";
		console.log(this.name);

		var otherLog= function(e){
			this.name = e;
		}
		otherLog("現在第二次更新c");
		console.log(this.name);
	}
}
c.log();

```
上面這一段程式碼，在執行`c.log()`時，預期會看到瀏覽器console顯示出兩次`this.name`：
第一個`this.name`會是`更新物件c`
第二個`this.name`會是`現在第二次更新c`

預期看到這樣，那結果會是？

![](https://i.imgur.com/WCOblPS.png)

字串`"現在第二次更新c"`，確實被傳入`otherLog()`，也改變了`this.name`，那為何console印出來的都是`更新物件c`？

這是因為，log方法裡的函式表達式otherLog，其this指向了`全域物件`！，我們可以從瀏覽器console來檢查現在的window。

![](https://i.imgur.com/4sgFUtU.png)

有看到物件c了，但還是沒看到`現在第二次更新c`這段文字，再往下找找看window 。

![](https://i.imgur.com/SMeBCVj.png)

可以看到現在window多了一個屬性name，其值為`現在第二次更新c`，這代表

> 在函式內定義的函式，裏頭函式的this會指向全域物件。

通常開發者會用個方法避免這種bug，那就是 **用個變數保存this**

來看看以下程式碼
```JS
var c = {
	name: "我是物件c",
	log: function(){
		var self = this;
		this.name = "更新物件c";
		console.log(this.name);

		var otherLog= function(e){
			self.name = e;
		}
		otherLog("現在第二次更新c");
		console.log(this.name);
	}
}
c.log();

```
和上一個例子差不多，只是這次在log函式內寫了這一段`var self = this`，並且將log裡面的otherLog函式，原本的`this.name = e`改成`self.name = e`。

在第五天的[筆記](https://ithelp.ithome.com.tw/articles/10190998)提到，當內部執行環境找不到指定的東西時，便會向外查找，一層一層找，直到找到全域為止。

現在otherLog函式內沒有self的存在，程式碼卻寫了`self.name`，於是電腦便往外部執行環境找，在物件c的方法：log函式內找到了self，這個self保存的this指向物件c(別忘記物件函式是`by reference`)，也就是說，用這種保存this的方式，可以讓物件方法(函式)內的函式，也能存取該物件的屬性。

所以上面這段改良版程式碼，結果是：

![](https://i.imgur.com/hgi725q.png)
　
　
　
　
## 小結
this關鍵字是JavaScript很常被拿來討論的特性，並不是說不會this就無法開發下去，除了利用`外部查找`特性，上面的`this.name`和`self.name`改成`c.name`也是通的，但this確實給予開發者很大的彈性，理解其特性對我們的開發會很有效率。

若是ES6的箭頭函式，其this的狀況又不一樣，很常會需要先在外部環境用變數保存this，在箭頭函式內再調用，這30天的筆記基本上是照克服JS的奇怪部分去記，箭頭函式的部分就先略過囉。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)4-37