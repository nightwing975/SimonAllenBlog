---
title: 克服JS奇怪的部分：Day17 傳值by value與傳址by reference
date: 2018-07-29 15:06:54
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://3.bp.blogspot.com/-Ga2-HM8LthU/W1waskr-OrI/AAAAAAAAIbg/zXzHgT1IstAwq6PkE2QVSjwK1chlE2WnACLcBGAs/s1600/2018ITMANJS17.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10192342

---

今天來看傳值與傳址

`call by value`**傳值**與`call by reference`**傳址**指的是電腦記憶體中的東西，與程式的參照傳遞互動的模式。

> call by value

當我們創造變數並給值時，變數會指向值在電腦記憶體中的位置，若我們以這個值為參照，指定另一個變數指向這個值時，電腦會在記憶體中新增(複製)一個新值，**讓後來的這個變數指向新的值**。

> 在JavaScript裡，布林值、字串、數值、null、undefined都是call by value。

來看看以下程式碼
```JS
var a = 100;
var b;

b = a;
a = a - 70;
console.log('a現在是' + a);
console.log('b現在是' + b);
```
1. 先創造一個變數a並設定進電腦記憶體
2. 在電腦記憶體創造數值100
3. 將變數a指向電腦記憶體內的100
4. 再創造一個變數b並設定進電腦記憶體
5. 電腦拷貝a的值，在電腦記憶體中設定另一個100
6. 將變數b指向電腦記憶體內另一個100
7. 變數a指向的值減去70

現在變數a是30，變數b是100。

![](https://i.imgur.com/HboJqo4.png)

挺合理的，a和b指向的值在電腦記憶體裡不一樣，當a修改時，b並不會被影響，這種特性就是call by value。

---

> call by reference

當我們創造變數並給值(物件)時，變數會指向物件在電腦記憶體中的位置，若我們以這個物件為參照，指定另一個變數指向這物件，**這個變數就會指向電腦記憶體中同樣的物件**，不會有新的物件在記憶體中被創造出來。

> 在JavaScript裡，物件、陣列、函式都是call by reference。


來看看以下程式碼
```JS
var c = { hello : '安安' };
var d;
d = c;
c.hello = '你好';

console.log(c);
console.log(d);
```

如果用上面a、b的例子(call by value)來看c、d，那麼：
`console.log(c)`應該是顯示出`{ hello : '你好' }`
`console.log(d)`應該是顯示出`{ hello : '安安' }`

畢竟d和c指向的記憶體物件應該不一樣，彼此會互不影響，來看看結果

![](https://i.imgur.com/MP3dPRl.png)

疑?兩個都一樣?

當我們給變數一個物件，其實賦予變數**指向這個物件在電腦記憶體的位址**，而`d = c`，變數d也指向 **同一個物件**，**同一個該物件在記憶體的位址**。

當`c.hello = '你好'`時，c指向的物件，hello屬性變成'你好'，因為d指向的物件和c是同一個，d自然也是`{ hello : '你好' }`囉。


承接程式碼，若用函式傳參數(物件)，結果也是一樣的
```JS
function changeHello(obj){
	obj.hello = '天氣真好';
}

changeHello(d); 
console.log(c.hello);
console.log(d.hello);
```

上面說過，物件是`by reference`，c、d都是指向記憶體中的同一個物件，結果自然是：

![](https://i.imgur.com/89rzXIq.png)

兩個都一樣


另一個例子：
```JS
var e = { hello : '安安' };
var f = { hello : '安安' };
e.hello = '哎呀';

console.log(e);
console.log(f);
```
![](https://i.imgur.com/B0PNetW.png)
疑？按照上面的說明，此時e和f應該都是`{ hello : '哎呀' }`才對啊？

這是因為，雖然他們的值字面上看一樣，但在電腦記憶體位置中，這兩個物件在記憶體中是獨立分開的，`=`運算子會建立一個新的命名空間，而且用到了 **物件實體語法**來創造物件，是故e和f指向各自指向不同的記憶體位置，彼此自然不影響囉。

---

這邊再給大家看看call by value和call by reference的差別

## call by value
```JS
var byValue1 = 100;
var byValue2 = byValue1;
var byValue3 = byValue1;
var byValue4 = byValue1;
console.log(byValue1,byValue2,byValue3,byValue4);
```
![](https://i.imgur.com/3waALvf.png)

都是100
```JS
byValue1 = byValue1 - 30;
console.log(byValue1,byValue2,byValue3,byValue4);
```
![](https://i.imgur.com/vBqBRGv.png)

只有byValue1的值受到了影響，這代表byValue1、byValue2、byValue3、byValue4指向的數值都是電腦記憶體中獨立分開的值，所以彼此互不影響。

## call by reference
```JS
var obj1 = { ByReference : 100 };
var obj2 = obj1;
var obj3 = obj1;
var obj4 = obj1;
console.log(obj1,obj2,obj3,obj4);
```
![](https://i.imgur.com/Qu7hCuk.png)

現在電腦記憶體中的`{ByReference:100}`其實只有一個，只是同時4個變數指向它。
```JS
obj3.ByReference = obj3.ByReference - 30;
console.log(obj1,obj2,obj3,obj4);
```
![](https://i.imgur.com/41nwNpc.png)

變數obj1、obj2、obj3、obj4都指向同一記憶體內的物件，所以透過變數obj3去修改物件，全部指向它的變數都受影響。

---

最後，來加碼分享面試時有被考到的題目，當時已經知道`by value`與`by reference`的概念，但還是被陷阱坑到。
## 求a.x和b.x，console分別顯示出來的內容

```JS
var a = { n : 1 };
var b = a;
a.x = a = { n : 2 };
console.log(a.x)
console.log(b.x)
```


---
###  解析
可以分三部分來看

#### 第一部分

* 宣告變數a、b
* 變數a指向`{ n : 1 }`
* `var b = a;`，變數b也指向變數a指向的`{ n : 1 }`

> 因為by reference的關係，a、b此時都指向憶體中的同一個`{ n : 1 }`

#### 第二部分

* `a.x = a = { n : 2 }`

`=`運算子是右相依性，所以這行乍看是先從右邊看到左邊....嗎？
別忘記決定運算子順序的是優先性與相依性，可以參考MDN的[運算子優先性表格](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

> `.`運算子的優先性高於`=`運算子

所以先看最左邊的`a.x`，但因為a物件`{ n : 1 }`並沒有x屬性的存在，
於是就創造x這個屬性，`a.x`值`是undefined`。

> a、b此時共同指向的物件變成`{ n : 1 , x : undefined }`

然後才因為`=`運算子，由右看到左。

* `a = { n : 2 }`

> a改指向記憶體中的新物件`{n:2}`，因為by reference的關係，此時b仍指向記憶體中的物件
> `{ n : 1 , x : undefined }`。

* `a.x = a`

將a現在指向的物件`{ n : 2 }`賦予給`a.x`，這邊的`a.x`其實是先前與b一同指向的
`{ n : 1 , x : undefined }`的x屬性，也就是`undefined`，
此時`{ n : 1 , x : undefined }`這個物件變成`{ n : 1 , x : { n : 2 } }`。

> 現在的狀況
> a指向`{ n : 2 }`
> b指向`{ n : 1 , x : { n : 2 } }`

#### 第三部分

* `console.log(a.x)`
這個時候的a指向`{n : 2 }`，console.log卻是要看`a.x`，`{ n : 2 }`並沒有屬性x，於是創造x屬性，a物件變成是`{n : 2 , x : undefined }`，`a.x`就是`undefined`囉!


* `console.log(b.x)`
這個時候的b指向`{ n : 1 , x : { n : 2 } }`，`b.x`自然是`{ n : 2 }`


瀏覽器console，看看結果：
![](https://i.imgur.com/GKsPDtb.png)



　
　
　
## 小結
JS同時具有`call by value`和`call by reference`的特性，這種傳遞特性稱作`call by sharing`。理解這種特性是很重要的，畢竟~~面試會考~~它是「 **物件導向語言** 」啊，瞭解JS物件、函式的`call by reference`，可以幫助我們避掉一些開發上的bug。

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)4-36

