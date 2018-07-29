---
title: 克服JS奇怪的部分：Day10 存在與布林
date: 2018-07-29 12:20:01
categories: 2018 IT鐵人賽
tags:
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://2.bp.blogspot.com/-VuYF1DggcIg/W1waqRRZXFI/AAAAAAAAIbA/LmF6U5MZGQgPDdwJhfUjts-OZCVlCb2owCLcBGAs/s1600/2018ITMANJS10.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10191498

---

今天來看 **存在**`existence`與 **布林**`boolean`的關係

由於JS常發生型別轉換這件事，開發者可以用`Boolean()`這個內件函式，來判斷型別轉為boolean的結果。
![](https://i.imgur.com/Cn4HY13.png)
`undefined`、`null`、`""`這些代表沒有的值，都會被被型轉成false。
開發者可以利用此特性，用if()敘述來判斷是否成功取值。

例如：
```JS
var a;

if(a){
	console.log('這裡有東西');
}
```
![](https://i.imgur.com/O3bRPMO.png)
結果什麼也沒發生，也沒有語法錯誤，發生了什麼事？

我們都知道宣告變數，變數預設值是`undefined`。
而`if()`敘述句()內看的是布林值`true`或`false`，來決定敘述句`{}`裏頭的程式碼要不要執行。
故a在`if()`敘述句被型轉為`false`，所以這個條件判斷內的程式碼不執行。
如果是有值的狀況，例如：
```JS
var a = 'IT鐵人賽';

if(a){
	console.log('這裡有東西');
}
```
結果會是？
![](https://i.imgur.com/GhqrBeB.png)

a有值(字串)，所以型轉為`true`，這就是開發者對js強制型轉的妙用之一，可以判斷這個值是否有存在，**當有值存在時，若值不是代表「沒有、無、未定義」的值，就會被型轉成true**。

但如果賦值給a的值是數值`0`，那怎麼辦呢？
```JS
var a = 0;

if(a){
	console.log('這裡有東西');
}
```
瀏覽器console結果是？
![](https://i.imgur.com/rODsN9z.png)
沒東西

雖然a有值(數值`0`)，但`0`被自動型轉為`false`，導致這個if條件判斷程式碼不成立，只是a確實有開發者賦予的值呀！

這個時候可以這樣寫：
```JS
var a = 0;

if(a || a === 0){
	console.log('這裡有東西');
}
```
結果是：
![](https://i.imgur.com/TbRGHGv.png)
來看看`if()`內發生了什麼事

`||`運算子可以想成中文的「或」的意思，`===`這個運算子的特性則是嚴格相等比較，前面[這篇筆記](https://ithelp.ithome.com.tw/articles/10191292)有說，運算子執行順序，先看 **優先性**，再看 **相依性**，`===`這個運算子優先性比`||`還高
(可參考MDN的[運算子優先等級](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence))

所以`if()`內的執行順序，先從`a === 0`這段程式碼開始，把這段單獨抽出來看，結果是： **true** 成立
![](https://i.imgur.com/zxcShS3.png)
再來看`a || true`
a的值`0`在`if()`內被自動型轉為`false`
所以變成是`false || true`
`||`或的意思，若有true就選擇true，假或真，選擇真。
![](https://i.imgur.com/SaTFAx4.png)
是故`if()`內結果`true`，條件判斷成立，印出`這裡有東西`



這種寫法很少見嗎？不，很常見(我最近上班也有看到)
有的框架、函式庫、JS套件會用這種寫法，舉個例子，現在給變數a一個函式：
```JS
var a = function() {
	console.log("某套件某功能的函式")
}
if(a){
	console.log("我有成功匯入某JS套件")
}

```
結果是：
![](https://i.imgur.com/1c9foIQ.png)

用這種方式，能判斷的不是只有字串或數值，連**函式**也可以，當然**物件**也是可以的。
```JS
var a = {it30day:'一天一筆記'}
if(a){
	console.log("我有成功匯入某JS套件")
}
```
結果是：
![](https://i.imgur.com/Wy6AJVR.png)
　
　
　
## 小結
某次使用JS套件(別人寫好的Plugin)時，看原文件發現這種寫法，當時的反應是欸欸~~這樣也可以喔！後來想一下、console確認可行就用了，現在在udemy看到這部影片，**存在與布林**，感覺真是心有戚戚焉。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)3-27