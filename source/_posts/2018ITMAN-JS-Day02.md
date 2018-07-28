---
title: 克服JS奇怪的部分：Day02 幾個名詞小觀念
date: 2018-07-28 16:14:35
tags:
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://4.bp.blogspot.com/-tQQ8tog2G-I/W1wan49c2CI/AAAAAAAAIac/6PI_O8WGQQwV89sv8pRZjhakhw-dg1KYwCLcBGAs/s1600/2018ITMANJS02.png)
<!-- more -->
> 此為筆者 2018IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10190637


今天的筆記比較偏觀念性質，畢竟是[克服JS的奇怪部分](https://www.udemy.com/javascriptjs/)這堂課開頭的部分，但對我這個非資訊本科的人來說也很受用了。

## 語法解析器(Syntax Parser)

程式、電腦科學的世界中，電腦並不會直接看懂JavaScript在(以下簡稱JS)寫什麼，而是會將JS轉譯成電腦看得懂的語言，可以這樣想：

當我們寫好JS程式時，在執行時，我們宣告的變數、函式，就會呈現在記憶體中，由電腦去運作使用。而中間負責將我們寫的JS轉換給電腦的，就是 "直譯器、轉譯器"。

它會逐字閱讀，當讀到var、let、const時，他就會知道我們要宣告個什麼東西，當讀到function，它會知道這是一個函式，只要我們在撰寫時遵守JS語法規則，執行時直譯器、轉譯器就會依照規則將其轉換成電腦認識的語言，而這個中介的引擎被稱為直譯、轉譯器。

## 詞彙環境(Lexical Environment)

代表程式碼在程式中的實際所在位置
蛤?什麼意思?

Lexical代表的是和程式的語法有關，特定的程式寫在哪裡是很重要的，例如：
當我們撰寫出一段宣告變數的JS語法

```JS
var food = "potato";
```

它被寫在哪呢?它在一個函數中嗎?

```JS
function kitchen(){
    var food = "potato";
    //下略...
}
```

它的周圍的環境是什麼?它被包在物件、陣列中嗎?

```JS
var myhome = {
    kitchen:function(){
        var food = "potato";
        //下略...
    },
    bedroom:{
        //下略...
    }
}
```

當我們在編輯器存檔，在瀏覽器重新整理時，JS不會給電腦直接執行，而是透過引擎和語法解析器轉譯、翻譯給電腦執行，而程式語法所寫在的位置，可以影響執行階段時，它對應的記憶體位置，也能影響它和其他變數函式的互動。
所以談到詞彙環境(Lexical Environment)就是指開發環境中的程式位置。
　
## 執行環境(Execution Context)

JS在執行時，其實並不會完全照我們開發時期寫的JS逐字逐行去執行。
是的，解析器、直譯器會逐行逐字讀我們寫的code去讀，但轉譯給電腦時並不會逐行逐字按順序丟給電腦記憶體和執行。

我們的程式碼會在執行時，會先依照詞彙環境(Lexical Environment)被解析器轉換，在電腦中被創造並擺到該放的記憶體位置去，最後電腦才執行。尤其JS某些特性(例如提升)就是在創造階段產生，因此理解JS在執行環境中的狀態是很重要的一件事。
　
　
## 名稱/值的配對(Name/Value pair)

名稱/值的配對，代表一個名稱會對應到一個值。
在執行時期(執行環境中)一段正在運行的程式碼，同樣的名稱只會有一個。
一個名稱只能被一個值定義，而這個值可以是更多名稱/值的配對，這是什麼意思呢?

例如：
```JS
var ithome = "30day"
```
var       ->宣告
ithome    ->變數名
"30day"   ->值(字串)

這就是一個名稱/值的配對

而討論到物件時也可以用一樣的概念，**物件也是名稱與值配對的組合**，值可以是字串、數值、陣列、函式、另一個物件，可以這樣想：
```JS
{
	Superman:{
        gender: '男',
        color: 'blue',
        superPower: {
            speed: 90,
            strong: 200
	    }
	}
}
```
Superman也是**名稱/值的組合**，名子是Superman，值是物件，往內看：
gender是名稱，'男'是gender的值；color是名稱，'blue'是color的值；superPower是名稱，值則是另外一個名稱/值的配對。

反過來看，Superman也可以被包在某個名稱/值組合裡，是另外一個名稱/值的配對(好饒口)。
```JS
{
    JusticeLeague:{
        Superman:{
            color: 'blue',
            superPower: {
                speed: 90,
                strong: 200
            }
        },
        Batman:{
            color: 'black',
            power: function(){
                console.log("I am rich!")
            }
        },
        Wonderwoman:{....},
        Flash:{....},
        Aquaman:{....}
        }
}
```

現在JusticeLeague的值也是個物件，裏頭有名稱Superman、Batman、Wonderwoman...等等，而Superman還可以在往下看，它的值裡又有其他名稱/值的組合，可以瞭解就算這樣一層一層的往下衍生，在JS中，物件本質還是名稱/值的配對組合。
　
## 全域環境與全域物件

不論何時執行JS程式，JS都會在執行環境(execution context)裡執行，而全域環境就是指在執行時，把我們的程式碼包在裏頭執行的環境(像個包裹一樣)。JS在執行時都會創造一個基礎執行環境作為他的全域環境，以網頁來說全域通常就是瀏覽器window，而Node.js就是Global。

在電腦執行JS時(執行時期)，全域環境被創造時也會跟著產生全域物件(Global Object)和「this」。
「全域」指的是我們在程式的任何地方都可以取用、呼叫它，可以想像成「公共」比較好理解，舉個例子：

我在公司上班，公司對於我就是全域環境。
我可以自由的使用茶水間的飲水機，飲水機對我來說就是全域物件，公共都可以使用，不只我可以用，別的部門也可以用。以此概念來發想，即便函式寫得再複雜，一層包一層，在裏頭也是可以取用全域的變數與物件的。


## 小結
今天認識、複習到了幾個觀念與名詞(慘了好像都在打中文字比較多....)
* 語法解析器
* 詞彙環境
* 執行環境
* 名稱/值的配對
* 全域環境與全域物件
　

筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)2-6至2-9