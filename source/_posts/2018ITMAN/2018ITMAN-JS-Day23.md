---
title: 克服JS奇怪的部分：Day23 函式內建方法：bind()、call()與apply()
date: 2018-07-29 15:07:03
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://3.bp.blogspot.com/-m8NdjiHIQN4/W1wat6KB-UI/AAAAAAAAIbw/rWnm2xQOq6g9flHdvr2CI90S67GHPnqTQCLcBGAs/s1600/2018ITMANJS23.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10193895

---

今天來看看`bind()`、`call()`、`apply()`這三個函式內建方法。

當函式執行環境被創造出來，會一併創造`arguments`關鍵字，保存帶入自己的參數;
也會一併創造出`this`關鍵字，指向函數目前所處的物件，關於this可以參考[這天的筆記](https://ithelp.ithome.com.tw/articles/10192737)。

若我們希望修改this指向的對象，有辦法達到這個目的嗎？

我們可以利用`bind()`、`call()`、`apply()`這些函式內建方法來達成目的


先來看看`bind()`的例子

```JS
var superMan = {
    earthName: 'Clark Kent',
    KryptonName: 'Kal-El',
    getAllName: function(){
  
       var allname = this.earthName + '　and　' + this.KryptonName;
       return allname;

    }
}
console.log(superMan.getAllName())
```
結果是
![](https://i.imgur.com/oVQLA5Y.png)

接著在設定一個函式表示式logName
```JS
var logName = function(log1,log2){
	console.log('Logged:' + this.getAllName())
	console.log('Arguments:' + log1 + ' ' + log2)
	console.log(this)
}
```
直接呼叫logName函式會發生錯誤，因為第一個`console.log()`裡的`this.getAllName()`有問題，現在logName函式的this指向全域，而不是superMan指向的物件。

```JS
logName();
```
![](https://i.imgur.com/K4pgoUf.png)

現在可以利用bind的方法來解決這個問題：

```JS
var logPersonName = logName.bind(superMan);
logPersonName('en');
```

這裡logName用了`.`運算子去取用函式內建方法`bind()`，同時把superMan當參數傳入，此時電腦記憶體會複製一個logName函式，這個複製版logName函式的this指向superMan物件，也就是說，變數logPersonName指向的是新的(另一個)logName函式。
　
接著呼叫並帶入參數'en'
結果是：
![](https://i.imgur.com/qD3cHgq.png)

現在logPersonName函式，this指向物件superMan而不是全域，所以可以順利印出`Clark Kent　and　Kal-El`，而`en undefined`的`undefined`，是因為我們只有傳一個參數en進去的緣故，另一個參數自然就是undefined囉。

logPersonName函式(logName函式複製版)的this指向superMan，那...原本的logName函式有受到影響嗎？再次呼叫執行logName()看看
```JS
logName();
```
![](https://i.imgur.com/swhnKOt.png)

可以看到logName函式還是不受影響，因為用`bind()`綁定this的函式...也就是變數logPersonName指向的函式，並不是真正的logName，而是logName函式的複製版本。

> 也就是說`bind()`可以創造函式的拷貝版，並且可以透過傳入物件來綁定(指定)它的this是誰


---

接著來看看`call()`這個內建方法
`call()`和`bind()`有點類似，可以透過傳入物件來綁定(指定)它的this是誰，但`call()`會馬上執行該函式，所以在`()`傳入物件當參數綁定時，也可以一併傳入這個函式執行所需的其他參數。

將上面bind例子修改成call版本
```JS
var superMan = {
    earthName: 'Clark Kent',
    KryptonName: 'Kal-El',
    getAllName: function(){
  
       var allname = this.earthName + '　and　' + this.KryptonName;
       return allname;

    }
}

var logName = function(log1,log2){
  console.log('Logged:' + this.getAllName())
  console.log('Arguments:' + log1 + ' ' + log2)
  console.log(this)
}

logName.call(superMan);

```
![](https://i.imgur.com/HGuMz6c.png)

一執行時就印出東西來，因為`call()`會馬上執行使用它的函式，我們也可以帶入函式所需參數
```JS
logName.call(superMan,'en','es');
```
![](https://i.imgur.com/Qrqa15e.png)

也就是說
> 同樣修改this對象，`bind()`會複製函數，但不會馬上執行，`call()`則會直接執行

至於logName，再次單獨呼叫看看，它的this會指向誰呢？
```JS
logName();
```
![](https://i.imgur.com/FTPP5NO.png)


---

接著再看看`apply()`這個內建方法

`apply()`也可以透過傳入物件來綁定(指定)它的this是誰，並且和`call()`一樣會直接執行呼叫它的函式，使用時也可以帶入函式所需參數，只是這個參數必須是陣列形式。

將bind的例子修改成apply版本
```JS
var superMan = {
    earthName: 'Clark Kent',
    KryptonName: 'Kal-El',
    getAllName: function(){
  
       var allname = this.earthName + '　and　' + this.KryptonName;
       return allname;

    }
}

var logName = function(log1,log2){
  console.log('Logged:' + this.getAllName())
  console.log('Arguments:' + log1 + ' ' + log2)
  console.log(this)
}

logName.apply(superMan,'en','es');

```
執行結果是
![](https://i.imgur.com/tOUIooL.png)

這是因為使用`apply()`除了綁定的物件外，其他傳入的參數必須是陣列，所以要改成這樣
```JS
logName.apply(superMan,['en','es']);
```
![](https://i.imgur.com/ONDbWgU.png)

至於原本的logName，我們一樣呼叫看看，它的this會指向誰呢？
```JS
logName();
```
![](https://i.imgur.com/irYHwqp.png)

---

恩...那麼什麼時候會用到`bind()`、`call()`、`apply()`呢？

影片裡有兩個例子：

> function borrowing　可以跟別的物件借方法來操作

我們再創建一個物件superMan2，和物件superMan一樣，只是少了一個getAllName
```JS
var superMan = {
    earthName: 'Clark Kent',
    KryptonName: 'Kal-El',
    getAllName: function(){
  
       var allname = this.earthName + '　and　' + this.KryptonName;
       return allname;

    }
}

var superMan2 = {
    earthName: 'Clark Kent',
    KryptonName: 'Kal-El'
}
```
當superMan2物件也需要用到函式getAllName時，就可以跟superMan「借」來用：
```JS
console.log(superMan.getAllName.apply(superMan2));
```
![](https://i.imgur.com/oQwl7iY.png)


> function currying

建立一個函式的拷貝，並設定要傳入的參數預設值，有許多數學運算需處理時很有妙用


`bind()`除了可以帶入欲綁定this的物件，也可以帶入函式的參數，但bind帶入的參數會成為這個函式參數的預設值，例如：

```JS
function multiply(a, b){
  return a*b;
}

var multiplyByTwo = multiply.bind(this, 2); 
console.log(multiplyByTwo(4)); 
```

multiply.bind(this, 2);的this，就是欲綁定this的物件(在這裡範例沒什麼作用)，反倒是2，會對應到multiply(a, b)函式的a，所以綁定後的函式只要帶入一個參數，這個參數就會對應到multiply(a, b)的b。

結果是
![](https://i.imgur.com/HCwr1Xc.png)
　





## 小結
平常頂多用變數保留this，以解決this指向的問題，而不會特別改變this指向的對象，至少目前是如此啦。
這節影片看下來，覺得若要撰寫框架、函式庫、plugins才會頻繁用到這幾個方法，尤其克服JS的奇怪部分，在第七章以後會進入研究框架的章節，或許講師到時候會介紹它們的其他用法。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)4-50