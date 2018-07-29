---
title: 克服JS奇怪的部分：Day14 JSON與物件實體
date: 2018-07-29 12:40:50
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://4.bp.blogspot.com/-RlK8LI2urow/W1warqHWfaI/AAAAAAAAIbQ/5L9Gav-Thi8R-zTi10i2k0uFGTUnpjfBgCLcBGAs/s1600/2018ITMANJS14.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10191901

---

今天來看看JSON

前端工程師在串接資料，常常是接後端發出的JSON檔案(API)，再將其內容渲染到網頁上，那什麼是JSON呢？

> JSON，全名JavaScript Object Notation
> 是受到JavaScript物件實體語法啟發的傳輸格式，比起使用XML傳輸資料，JSON格式在檔案大小上更為輕量，也是現在主流的傳輸格式。

一個JavaScript物件可以長這樣
```JS
    {name: 'Simon',isF2E: true}
```

而JSON會是這樣
```JS
    {'name':'Simon','isF2E':'true'}
```
看起來好像差不多？

物件的屬性名name，在JSON中被引號包起來變成`'name'`，也就是說原本的屬性名到了JSON變成字串，JavaScript利用物件實體語法時，物件屬性名稱也可以是字串(對物件實體語法，屬性的引號`'`或`"`可加可不加)
```JS
var object = {
	'name': 'Simon',
	'isF2E': true
}
```
這樣的JavaScript物件實體語法是OK的，但不是說物件的屬性名稱`'name'`和`'isF2E'`真的變成字串了，我們用console印出來看看。
```JS
console.log(object)
```
![](https://i.imgur.com/nDWpE0B.png)

可以看到當用物件實體語法創造物件時，屬性名稱可以加上引號`'`，實際創造出來時，物件屬性的引號`'`不見了！
相較之下，JSON格式的屬性名與值一定要加引號`'`成為 **字串**，這裡要有個認知，雖然JSON是受到JavaScript啟發，但 **JSON和JavaScript是不一樣的東西**。

關於JSON，有興趣可以參考[W3C](https://www.w3schools.com/js/js_json_intro.asp)與[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/JSON)的介紹
　
另外JSON格式再傳輸時(前端到後端、後端到前端)，有時也會以 **字串**的形式處理。
來看看以下程式碼，假設前端從後端接到一個JSON **字串**
```JS
'{"name":"Simon","isF2E":"true"}'
```

前端AJAX接到後，現在要取值Simon來用
```JS
var data = '{"name":"Simon","isF2E":"true"}';
console.log(data.name+'挑戰第14天')
```
結果是
![](https://i.imgur.com/BPNPtgd.png)

因為JSON被引號包著，現在是**字串**，這時`JSON.parse()`就派上用場
```JS
var data = '{"name":"Simon","isF2E":"true"}'
var data2 = JSON.parse(data);
console.log(data2.name+'挑戰第14天')
```
![](https://i.imgur.com/LcGEyGZ.png)
　
關於物件與JSON字串的轉換，有兩個重要函式：

> JSON.stringify()

將JS物件轉為JSON字串　

> JSON.parse();

將JSON字串轉為JS物件

而這裡用`JSON.parse();`函式來處理，讓JSON格式字串轉為JavaScript物件，如此一來前端接收到JSON資料就可以繼續用JavaScript處理其內容。
　
　
　
　
## 小結
今天我們認識了JSON格式，與JavaScript物件的差異。
題外話，前端開發者在串接JSON時，如果直接用瀏覽器點開，就會變成這樣：
![](https://i.imgur.com/mxX7UdP.png)

> JSON來源：政府開放資料[戲劇表演資訊](https://data.gov.tw/dataset/6016)

這個時候可以使用Chrome的擴充套件[JSONView](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=zh-TW)，就會變成這樣
![](https://i.imgur.com/QihrpGr.png)

我個人比較習慣JSONView，不過[JSON Editor Online](http://jsoneditoronline.org/)這個網頁也可以達成類似的效果
![](https://i.imgur.com/KV0hi9y.png)

視覺上是不是易讀多了呢?

今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)4-33