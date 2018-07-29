---
title: 克服JS奇怪的部分：Day15 函式就是物件
date: 2018-07-29 12:40:50
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://1.bp.blogspot.com/-C0rTkp9fWnU/W1warnzKxVI/AAAAAAAAIbU/7isaXLAelUs7_MOhkIKd4uACdeF8573VQCLcBGAs/s1600/2018ITMANJS15.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10192003

---

今天來看看函式`Functions`
在JS這個物件導向語言裡，其函式的特性被稱為`一級函式`。

> 一級函式First Class Functions

開發者對別的基礎型別做的事，也可以對函式去做，因為 **函式就是物件**，而物件就是 **名稱/值**的組合。

開發者可以指派函式給變數，將函式傳入另一個函式，也可以用實體語法建立函數。
函式可以有屬性和方法，因為他就是物件，所以它可以連結到物件、屬性、其他函式(Methood)


關於一級函式的介紹，可以參考：
> 維基百科:[頭等函式](https://zh.wikipedia.org/wiki/%E5%A4%B4%E7%AD%89%E5%87%BD%E6%95%B0)
> ITome專欄[物件導向語言中的一級函式](https://www.ithome.com.tw/node/75500)　by林信良

而JS的函式還有隱藏的物件屬性，其中最重要的有兩個：

> NAME

即名子屬性，然而名子不是必須的，函式也可以沒有名子(匿名函式)

> CODE 

即程式屬性`code property`，這個屬性的內容，就是開發者在編輯器上開發時，寫在函式內的程式碼。
我們寫的程式碼會成為這個屬性的值，而CODE屬性的特性是，它的內容是可以呼叫執行的。


來看看以下程式碼

```JS
function sport(){
	console.log('運動');
}
```
因為函式就是物件，所以我們可以接著這樣寫

```JS
sport.basketball  = '打籃球';
console.log(sport.basketball);
```
結果是
![](https://i.imgur.com/CIM2ifz.png)

為何沒有報出錯誤訊息？
因為JS函式就是物件，物件可以自由擴增屬性，我們等於對`sport`這個函式物件，新增一個`basketball`屬性，其值為字串`打籃球`。

那這個函式，它的名子、物件屬性NAME是什麼呢？
就是`greet`
當我們欲呼叫執行函式，在編輯器寫上`greet()`，其實就是透過函式屬性名子`greet`去呼叫這個函式物件的定義，而`()`就會創造執行環境，執行這個函式物件`程式屬性`裡的內容，也就是`console.log('hi');`這段程式碼。　
　
　
　
　
## 小結
今天複習了 **函式就是物件**這個觀念
先前我只曉得函式是物件型別，可以自由擴增屬性，但沒想深入想過函式的名子也是屬性(NAME)這件事。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)4-34