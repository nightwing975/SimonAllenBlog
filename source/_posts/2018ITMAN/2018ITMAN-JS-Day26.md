---
title: 克服JS奇怪的部分：Day26 物件型別、Reflection and Extend
date: 2018-07-30 15:12:01
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://4.bp.blogspot.com/-KSia1FSfio8/W1wauunn5OI/AAAAAAAAIcA/vUN5w-sc1NsLzX5oN5bFmnaAFKV3smQkgCLcBGAs/s1600/2018ITMANJS26.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10194568

---

今天來看第5章節後半兩部影片

JavaScript是物件導向語言，基本上所有的東西可以分成兩大類：`Primitive Types`和`Object Type`

> Primitive Types 基本型別

關於基本型別Primitive Types，可以看第7天的[筆記](https://ithelp.ithome.com.tw/articles/10191195)介紹

> Object Type 物件型別

哪些東西是物件型別呢？
**JavaScript除了Primitive Types以外的東西，全都是物件型別！**

來看看程式碼，這次直接在Chrome瀏覽器console輸入

```JS
var a = {};
var b = function(){};
var c = [];
```
建立空的物件、函式、陣列
![](https://i.imgur.com/75erUD1.png)

直接輸入`a.__proto__`看看有什麼？
![](https://i.imgur.com/Zr5sclg.png)
可以看到是一個物件，並且有很多屬性，若我們輸入`a.__proto__.`，就會跳出a物件原型可存取的屬性、方法。
　
若是a物件原型的原型會是什麼？輸入`a.__proto__.__proto__`
![](https://i.imgur.com/lYcwXIi.png)
可以看到原型鏈終點，也就是`null`。
　
接著看看函式，直接輸入`b.__proto__`看看有什麼？
![](https://i.imgur.com/EsSHdVJ.png)
是一個函式，若我們輸入`b.__proto__.`就會跳出b函式原型可存取的屬性、方法。
　
若是b函式原型的原型會是什麼？輸入`b.__proto__.__proto__`
![](https://i.imgur.com/360tDPg.png)
可以看到是是一個物件，並且有很多屬性，若我們再輸入`b.__proto__.__proto__.__proto__`，也就是去看函式b原型的原型的原型：
![](https://i.imgur.com/bHBXnY6.png)
可以看到原型鏈終點，也就是`null`。
　
若是陣列呢？直接輸入`c.__proto__`看看有什麼？
![](https://i.imgur.com/wjpESMn.png)
一樣也是一個物件，並且有很多屬性，若我們輸入`c.__proto__.`，就會跳出c陣列原型可存取的屬性、方法。
　
若是c陣列原型的原型會是什麼？輸入`c.__proto__.__proto__`
![](https://i.imgur.com/Hm9SW3O.png)
可以看到是是一個物件，並且有很多屬性，若我們再輸入`c.__proto__.__proto__.__proto__`，也就是去看陣列c原型的原型的原型：
![](https://i.imgur.com/i5fyPJE.png)
可以看到原型鏈終點，也就是`null`。

這樣看下來可以知道：

> JS物件的原型仍會是物件，而原型鏈的終點，就是`null`。

---

接著來看看 **Reflection and Extend**的介紹

> Reflection 

一個物件列出自己的屬性，然後改變自己的屬性和方法，我們可以藉Reflection，實現`extend`(擴展、繼承)這個模式來幫我們達成目的。

來看看程式碼
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

superman.__proto__ = batman;

for (var prop in superman) {
  console.log(prop + ':' + superman[prop])
}
```
這裡用了Forin語法，Forin可以將目標內的東西全都遍歷、loop個一輪，詳細用法這裡不贅述，而現在要把superman每個屬性(值)都取出來，注意`superman[prop]`的`[]`不是陣列，而是可以存取物件屬性的`計算取用成員運算子`運算子，詳情可看之前的[筆記](https://ithelp.ithome.com.tw/articles/10191723)。

執行結果
![](https://i.imgur.com/qCrq7En.png)
superman其實沒有getFullName這個方法，因為superman原型batman有，Forin還是順著原型鏈把其他屬性都取出來了。


我們可以用每個物件都有的內建方法hasOwnProperty來幫助我們篩選，將Forin程式碼改成這樣
```JS
for (var prop in superman) {
    if(superman.hasOwnProperty(prop)){
        console.log(prop + ':' + superman[prop])
    }
}
```
hasOwnProperty可以檢查屬性是不是該物件本身的成員，若有非物件本身的屬性存在(包含物件原型的屬性)，就回傳false，因此這裡的if判斷，console.log只會印出superman本身有的屬性。
結果是
![](https://i.imgur.com/ZjcemN7.png)
　
接下來內容須使用underscore.js，作者要以underscore.js的extend函式作為範例介紹，extend可以將其他物件的屬性、方法新增給我們的目標物件。

> 當然我們知道ES6有新增extends，各種框架也有出現extends的蹤跡，推測作者錄製影片時，ES6還未正式定義好，畢竟克服JS的奇怪部分也上架很久了。

另外ES6的extends和underscore.js的extend並未衝突，因為差了一個s字母。

首先引入underscore.js(開發者版本)
![](https://i.imgur.com/BlxIrDS.png)

接著我們可以去underscore.js的原始碼找_.extend

![](https://i.imgur.com/T6Dral8.png)

再找createAssigner

![](https://i.imgur.com/yoh525m.png)
可以看到是怎麼寫的，基本上看到`return`，就知道這是利用閉包特性的寫法，現在知道underscore定義了自己的extend函式，來看看使用它會發生什麼事：

```JS
var superman = {
  firstname: 'Clark',
  lastname: 'Kent'
}

var Lois = {
  job: 'reporter',
  getFormalName: function(){
    return this.lastname + ', ' + this.firstname;
  }
}

var Jimmy = {
  getFirstName: function(){
    return this.firstname;
  }
}
_.extend(superman, Lois, Jimmy);
```

使用extend後，物件superman獲得了另外兩個物件Lois、Jimmy的屬性與方法，並不是說superman去原型鏈查找，而是利用extend函式，被新增了原本沒有的屬性，我們現在可以直接操作superman最初沒有的屬性了。
```JS
console.log(superman);
```
結果是
![](https://i.imgur.com/ng5BD1B.png)
```JS
console.log(superman.job);
```
結果是
![](https://i.imgur.com/bZbmMD3.png)
```JS
console.log(superman.getFormalName());
```
結果是
![](https://i.imgur.com/iYLVgXh.png)
```JS
console.log(superman.getFirstName());
```
結果是
![](https://i.imgur.com/MBIaD0l.png)



## 小結

extend(s)是JS物件導向很重要的觀念、語法，可以讓JS物件獲得其他物件的屬性和方法，尤其現在使用Vue、React開發，很常看到ES6的extends存在。
第5章節只有4個影片，到這裡就告一段落了，明天進入第6章節 **建立物件**
今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)5-55、5-56
