---
title: 克服JS奇怪的部分：Day12 物件與點
date: 2018-07-29 12:40:38
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://4.bp.blogspot.com/--A9CCFbm4dI/W1waqTUg64I/AAAAAAAAIbE/eDf5vVi4dRomsCPX2_NglgqbPQl37oytgCLcBGAs/s1600/2018ITMANJS12.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10191723

---

今天開始進入第四章節，物件與函式的部分囉。
第二天的筆記有提到，物件是一群**名稱/值**的組合

例如：
```JS
{
	rice: '米飯',
	soup: '海鮮濃湯'
}
```

而其值可以是另一個**名稱/值**的組合，也可以是數值、字串、布林、物件、函式....等等。

例如：
```JS
{
	rice: '米飯',
	soup: {
		seafood: '蝦子',
		hot: true,
		cook: function(){
			console.log('海鮮濃湯');
		}
	}
}
```

關於物件還有幾件事：
* 物件可以有個原始的設定，又稱**屬性**`Properties`，
* 物件可以連結至另一個物件(物件裡包含著物件)，這個被連結的物件也是屬性。
* 物件裡可以有函式，當函式在物件裡，稱為**方法**`Methods`，它既是函式，又與物件相關。

接下來看看以下程式碼：
```JS
var Batman = new Object(); //宣告變數Batman並建立個新物件

Batman['firstname'] = 'Bruce';
Batman['lastname'] = 'Wayne';
```

`Batman['firstname']`的`[]`是運算子「**計算取用成員運算子**」。
它是由**左至右相依**的，會把物件當成一個參數，把字串當成另一個參數，以此字串去尋找物件的屬性並回傳值。

可以這樣想：
`Batman['firstname']='Bruce'`就是對物件Batman取用firstname屬性，因為firstname不存在，所以這邊存取建立它並賦值字串Bruce

`Batman['lastname'] = 'Wayne';`就是對物件Batman取用lastname屬性，因為lastname不存在，所以這邊存取建立它並賦值字串Wayne

這時Batman會是：

```JS
{
  firstname:'Bruce',
  lastname:'Wayne'
}
```

console.log印出來看看 
```JS
console.log(Batman);
```
![](https://i.imgur.com/84wkeG3.png)

這個運算子還可以這樣的用，宣告個變數Who並賦值字串firstname
```JS
var Who = 'firstname';
console.log(Batman[Who]);
```
結果會怎麼樣？
其實`Batman[Who]`就視同`Batman['firstname']`，所以當然是印出
![](https://i.imgur.com/b89f5KK.png)

這個用法好處在於可帶入變數來存取物件屬性，在開發上會有妙用，除了使用`[]`計算取用成員運算子來查找、取用屬性外，使用物件最簡單、最常用的運算子就是`.`了，`.`運算子也可以用來存取物件的屬性。

使用點這個運算子，會印出什麼?
```JS
console.log(Batman.firstname);
```
![](https://i.imgur.com/2eRAaRm.png)

`.`運算子也是個函式，它叫做**成員取用運算子**，可以取用物件中的屬性與方法，使用上很直觀，是前端開發者最常用的運算子，且也不用像`[]`還要替後面的屬性名稱加上引號。


當我們看到`Batman.firstname`，通常會這樣想：「阿不就是Batman物件裡的firstname屬性」，這個口語上OO物件裡的XX屬性，在電腦上其實是透過.運算子去作用的，不管是`[]`運算子還是`.`運算子，基本上都是透過查找左邊物件(記憶體)，去參照尋找右邊屬性(記憶體)位置。
　
　
　
　
## 小結
今天瞭解了物件與其存取屬性的`[]`運算子和`.`運算子，明天來看看**物件實體語法**。
至於筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)4-30