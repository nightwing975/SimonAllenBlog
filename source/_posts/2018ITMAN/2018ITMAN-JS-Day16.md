---
title: 克服JS奇怪的部分：Day16 函式陳述句與函式表示式
date: 2018-07-29 15:06:44
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://1.bp.blogspot.com/-2Fdw7ZuXIms/W1war-PDghI/AAAAAAAAIbY/BB05yGZPKKcTWd0w8lDueHsJjYnD4jgvgCLcBGAs/s1600/2018ITMANJS16.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10192146

---

今天來看`Function Statement`(函式陳述句)與`Function Expression`(函式表達式、表示式)

等等...`Statement`與`Expression`又是什麼東西呢?

> Statement

程式碼的單位，這段程式碼不會產生一個值

> Expression

程式碼的單位，這段程式碼最終會產生(回傳)一個值，而這個值不一定會被開發者賦予變數。

看看以下程式碼
我們先宣告一個變數a，然後直接在瀏覽器console做後續動作
![](https://i.imgur.com/KEixJZ5.png)

* 輸入`a = 3`，瀏覽器console回傳3，這代表這段程式碼是`Expression` 
* 輸入`10 + 5`，瀏覽器console回傳15，它也是`Expression`
* 輸入`a === 3`，回傳true(成立)，它也是`Expression`
* 若我們直接用物件實體語法創造物件，將物件指向變數`a`，瀏覽器console也回傳一個物件，它也是一個`Expression`


接著看看以下程式碼
```JS
if(a === 3){
    console.log(a);
}
```

`if()`的`()`，需要布林值`true`、`false`來決定這個程式判斷會不會成立，故`()`裡面會放表示式，但`if(){}`這段程式碼本身是陳述句，本身不會回傳值，我們也無法將它賦予、指向給變數。
```JS
var app = if(a === 3){
    console.log(a);
}
```
![](https://i.imgur.com/qP9G5Wj.png)


現在拉回來函式，函數的定義也有分為`Statement`與`Expression`

例如：
```JS
function cat(){
    console.log("貓咪");
}
```

當電腦在執行轉譯的JS程式碼時，上面這段程式並不會報錯，因為它是`Function Statement`函式陳述句，程式碼本身不會回傳值，除非我們呼叫執行使其執行。

而這種先宣告函式定義，等待我們(開發者)呼叫執行的寫法，也稱為`Function Declaration`函式宣告，它會在電腦的創造階段它就會被設定進記憶體，發生**提升**`Hoisting`現象。

我們可以這樣寫不會報錯
```JS
cat()

function cat(){
    console.log("貓咪");
}
```
![](https://i.imgur.com/Mv7lW42.png)

呼叫執行cat的`cat()`雖然寫在函式定義上面，但電腦在`創造階段`就先把函式設定進記憶體，在執行階段呼叫其內容，創造其函式執行環境，關於`Hoisting`可以參考第三天的[筆記](https://ithelp.ithome.com.tw/articles/10190700)。

由於函式就是物件，`cat()`這段程式碼，還可以理解成透過cat去查找在記憶體中的function，找到名子NAME屬性cat的函式物件，接著執行其程式屬性`console.log("貓咪");`，關於這部分，可以參考昨天筆記。


那`Function Expression`函式表示式呢？

```JS
var dog = function(){
    console.log("狗");
}

dog();
```
![](https://i.imgur.com/7qNlAUe.png)
當程式執行到`=`運算子右邊的程式碼，**才建立這個函式物件**，將其指向變數dog，並且它可以當成一個**值**，所以是`Function Expression`表示式。

這裡的函式並沒有名子，也就是說它是一個`匿名函式`，`var dog = function()`可以理解是將這個逆名函式在記憶體中的`位址`傳給變數dog。

而呼叫執行它的程式碼`dog()`，則可以理解是透過變數dog有的地址，去呼叫查找在記憶體中的該匿名函式，並執行其程式屬性`console.log("狗");`



那這樣寫呢?

```JS
dog();

var dog = function(){
    console.log("狗");
}
```
![](https://i.imgur.com/nyixNzG.png)

變數會在創造階段被設定進記憶體，所以程式碼實際上會像這樣：

```JS
var dog
dog();

dog = function(){
    console.log("狗");
}
```
發生了 **提升**現象，呼叫函式時變數dog其實沒有函式，自然報錯`dog is not a function`

而函式在呼叫執行時，也可以傳參數進去。
接續上面的內容，這裡建一個`Function Statement`
```JS
function log(a){
	console.log(a);
}
```
然後也可以把`Function Expression`表示式當成參數傳進去

透過變數
```JS
log(dog)
```

或直接放入匿名函式
```JS
log(function(){
    console.log("狗");
})
```
結果
![](https://i.imgur.com/FPnwYxK.png)


若是想要直接執行這個傳進去的匿名函式，原本的Function Statement可以改成這樣
```JS
function log(a){
	a();
}
log(function(){
    console.log("狗");
})
```
![](https://i.imgur.com/lngOhIw.png)
　
　
　
　
## 小結
今天複習了`Statement`與`Expression`的差別，也複習了函式表示式、函式陳述句，瞭解函式可以當參數傳入其他函式。
明天來看JavaScript的 **傳值**`by value`與 **傳址**`by reference`。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)4-35