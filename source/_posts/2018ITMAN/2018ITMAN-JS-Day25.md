---
title: 克服JS奇怪的部分：Day25 古典與原型繼承、瞭解原型
date: 2018-07-29 15:07:03
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://3.bp.blogspot.com/-Tobgbk_ruTo/W1wauidkz1I/AAAAAAAAIb8/LyvyyZ0sqP4fIvkiWmvOZuFJj7HD-QAJACLcBGAs/s1600/2018ITMANJS25.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10194350

---

今天來看看古典繼承、原型繼承與原型的介紹

> 繼承

表示一個物件可以取用另一個物件的屬性或方法

> **Classical Inheritance** 古典繼承

C#、JAVA常用到的物件繼承方式，有一些專有名詞(語法)如：private私用、protected保護、friend夥伴、 interface介面...等術語。

古典繼承很流行，也解決了很多問題，但樹狀結構物件的互動模式，一但數量增加，很容易產生複雜、龐大的集合。

> **Prototypal Inheritance** 原型繼承

JavaScript的物件繼承種類，相對古典繼承簡易、彈性，既然是所謂Prototypal「**原型**」，就代表有相對於目前物件的原型存在。

JS的所有物件(包含函式)都有個隱藏屬性proto，這個屬性會參考、指向另一個(原型)物件，而這個被指向的原始物件，就是我們當下這個物件的原型。

一個物件可以參照一個原型(物件)，這個原型物件可以再參照另一個原型(物件)。
當我們使用的屬性、方法不存在當下操作的物件中，JS就會往該物件的原型去查找，有就存取它，沒有就在往原型的原型物件查找，直到盡頭。

假設今天我們有一個物件obj，它有個屬性prop1，當要存取obj物件的屬性prop1通常會這樣寫：
```JS
obj.prop1
```
若我們突然這樣寫：
```JS
obj.prop2
```
如果obj物件沒有屬性prop2，便會先往obj物件原型`proto`去查找，若`proto`原型物件有屬性prop2，那obj就可以存取prop2而不會報錯，即便obj本身沒有prop2屬性。
若我們突然這樣寫：
```JS
obj.prop3
```
如果obj物件沒有屬性prop3，便會先往obj物件原型`proto`去查找，若連`proto`原型物件也沒有屬性prop2，那就在往`proto`原型物件的`proto`原型物件去找，一直找下去終於找到了，那obj就可以存取prop3而不會報錯。

![](https://i.imgur.com/Q86W4Kd.png)

> 圖片來源：udemy課程-克服JS奇怪的部分 

物件往原型物件查找，再往原型物件的原型物件查找，再往原型物件的原型物件的原型物件查找，再往原型物件的原(下略)一直找，鏈接下來的這個狀態，就稱為**Prototypal Chain**原型鏈。

要注意的是：原型鏈查找與 **範圍鏈**(外部參照)是 **不一樣**的東西，前者是去物件的原型對象找屬性和方法，後者是往外部執行環境找變數，這兩者不可搞混。

另外，JS的原型繼承特性，**不同的物件可以指向同一個原型物件**
![](https://i.imgur.com/rbOLgW9.png)

> 圖片來源：udemy課程-克服JS奇怪的部分 

若今天有個物件obj2，它可以是和obj1有同樣的原型物件`proto`的。

---

接著來看看程式碼
```JS
var batman = {
	firstname: 'Bruce',
	lastname: 'Wayne',
	getFullName: function(){
		return this.firstname + ' ' + this.lastname;
	}
}

var superman = {
	firstname: 'Clark',
	lastname: 'Kent'
}
```
創建物件batman、superman，他們都有屬性firstname與lastname，只有batman有方法getFullName。
接續程式碼
```JS
superman.__proto__ = batman;
```
將superman的原型__proto__指向batman，現在superman的原型是batman了

> 影片警告：這裡範例使用__proto__只是為了方便說明，在現實生活中非常不建議使用__proto__，因為瀏覽器會真的存取物件原型，導致程式的執行速度變慢。


接續程式碼
```JS
console.log(superman.getFullName());
```

我們用`.`運算子，呼叫執行superman的getFullName方法(函式)，由於superman並沒有getFullName方法(函式)，因此變往superman的原型去查找。

現在superman原型是batman，batman有getFullName方法(函式)，所以程式可以執行而不報錯，乍看好像借了batman的函式方法來用，但和操作`bind()`借函式不一樣。

若superman能使用getFullName，執行getFullName函式時的`this`會指向誰？
是getFullName原本所屬的物件batman？還是呼叫執行的物件superman？
![](https://i.imgur.com/rTVKFlt.png)

也就是說：

> 物件使用__proto__改變原型(指向其他物件），操作其他物件的函式方法，函式的this仍會指向存取、執行它的物件，而不是函式方法原本所屬的物件。

那superman的firstname屬性還是Clark嗎？
```JS
console.log(superman.firstname);
```
![](https://i.imgur.com/6APq9G8.png)
並沒有印出Wayne，一樣是顯示Clark，只有當我們存取該物件沒有的屬性與方法，才會到原型去查找。

接續內容，再看看以下程式碼
```JS
var robin = {
	firstname: 'Damian'
}
robin.__proto__ = batman;
console.log(robin.getFullName());
```
建立物件robin，將robin的原型指向batman，現在也可以用robin呼叫getFullName方法了，robin並沒有lastname，那結果會是？

![](https://i.imgur.com/Zz8dQ6y.png)

getFullName的`this`指向robin，接著找不到lastname，於是JS順著原型鏈取查找，找到robin原型batman屬性lastname，印出Damian Wayne。　
　
　
　
　
## 小結
每個物件都有原型物件對象、範圍練和原型練是不一樣的東西、__proto__是物件函式的內建屬性，實際開發不要使用它。
今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)5-53、5-54
