---
title: 克服JS奇怪的部分：Day06 JS是同步還是非同步?
date: 2018-07-29 11:44:04
categories: 2018 IT鐵人賽
tags:
 - 網頁前端
 - IT鐵人賽
 - JavaScript
 - 學習筆記
---
![](https://2.bp.blogspot.com/-JXDRY0NYcys/W1waoy9p6wI/AAAAAAAAIas/nVx-9niwq-Aan0i37T7KWV02envXOhMnACLcBGAs/s1600/2018ITMANJS06.png)
<!-- more -->
> 此為筆者 2018 IT鐵人賽 筆記備份：https://ithelp.ithome.com.tw/articles/10191094

---

今天來看看JS，非同步背後的原理，在課程影片中提到：
> JavaScript是一個 **單執行緒**、**同步**的程式，它逐行執行程式碼，並不會非同步的執行程式。

等等，同步?
JS的特色之一不就是 **非同步**嗎?
繼續看下去：

撇開node.js先不說，JS在瀏覽器執行時依賴引擎來驅動，而瀏覽器不是只有JavaScript引擎的存在，還有其他引擎在處理瀏覽器的其他狀態、程式。有可以呈現東西到瀏覽器畫面上的渲染引擎(Rendering Engine)，也有其他專門處理http請求的東西存在。

JS引擎可以和瀏覽器的其他引擎、處理器互動，這個互動行為是非同步的，但JS引擎 **自己**在運作JavaScript是同步的。如果有非同步的操作，那會發生什麼事呢?

JS引擎內的等待列稱為事件佇列`Event Queue`，如果有事件發生，如滑鼠點擊事件，JS引擎會先將事件放在事件佇列`Event Queue`，當執行環境的程式都執行完成後，JS才會開始注意事件佇列，此時檢查是否有函式被事件觸發，當有事件發生如點擊事件，它會創造執行環境給這個對應的函式執行。

JS在瀏覽器的非同步行為，其實是指 **將非同步事件放到事件佇列**，而自己執行同步程式結束後，才處理事件。如果事件觸發函式，就創造執行環境給對應函式，所以說，當瀏覽器的其他引擎或處理器有事件、狀態發生並與JS引擎互動時，這些事件就會被註冊進事件佇列裡。

看看影片中的程式碼範例

```JS
function waitThreeSeconds() {
    var ms = 3000 + new Date().getTime();
    while (new Date() < ms){}
    console.log('finished function');
}

function clickHandler() {
    console.log('click event!');
}

document.addEventListener('click', clickHandler);

waitThreeSeconds();
console.log('finished execution');
```

說明：
`function waitThreeSeconds()`
呼叫它會在3秒後印出finished function

`function clickHandler()`
對應下面的`document.addEventListener('click', clickHandler);`
當瀏覽器監聽到點擊事件，執行clickHandler()，印出click event!

`waitThreeSeconds();`
呼叫函式waitThreeSeconds

`console.log('finished execution');`
印出finished execution

當在影片聽到`function waitThreeSeconds()`的`3000 + new Date().getTime();`3秒說明時，瞬間想了一下，為何這裡不使用常見的`setTimeout`來處理?
...下一秒瞬間回神，阿`setTimeout`也是非同步的(敲頭)，作者是簡化例子中的同步、非同步判斷，好讓觀眾能易懂，如此一來上述程式中只有click點擊事件是非同步的。

拉回來，當瀏覽器載入後，顯示印出的東西順序會是如何?
點擊事件印出的內容，有辦法比`waitThreeSeconds`(3秒後才印出東西)或最下面的`console.log('finished execution')`更早印出來嗎?

執行程式，在網頁載入後快速點擊頁面，結果為：

![](https://i.imgur.com/F7GJMAF.png)

> 3是我猛按的結果，不要理這個數字...

說明：
在`waitThreeSeconds()`執行完成、全域的`console.log('finished execution');`也執行完成，點擊事件才執行其對應函式`clickHandler()`，因為JS會先把執行時期的同步程式都執行完，才處理事件。
也就是說**執行時間太長的同步程式可以干擾非同步事件的處理時間**，在開發時得注意這點。
　
　
　
## 小結
同步與非同步是JS中極為重要的一環，當初學習AJAX時，常在書上或聽人說JS會註冊非同步事件，向誰註冊呢?就是事件佇列`Event Queue`啦。
　
今天的筆記內容可以參照Udemy課程：[JavaScript 全攻略：克服JS 的奇怪部分](https://www.udemy.com/javascriptjs/)2-13、2-18

