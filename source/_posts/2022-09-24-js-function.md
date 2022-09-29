---
title: function函式
date: 2022-9-24
tags: 
   - 前端技術筆記
   - 網頁設計
   - Javascript
---

### function 函式

如果以實驗室的角度來看，函式可以說是化學反應的燒瓶，丟入原料，希望獲得產出。

來看看函式的組成。

``` bash
function 函式名稱(參數){
   執行的計算過程／行為;
}

//在你需要的地方執行該函式
console.log(函式名稱(參數))
```

來一個簡單的計算函式

``` bash
function multiplyMe(a){
   return a*5;
}

console.log(multiplyMe(5));
// 回傳25
```

** 參數可以是(a)/ (a,b)（待補充）

### Global Scope 全域

下列程式碼中 let myGlobal 跟 oopsGlobal 作用域都是全域。就算在幾千行後你看不到的地方呼叫他，他都會出現。

``` bash
let myGlobal = 10;
function fun1() {
oopsGlobal = 5;
}
```

### Local Scope 限定域（非正式翻譯，僅本人用法）

下列程式碼中const Fox作用域僅在function myTest內。

``` bash
function myTest() {
  const Fox = "S";
  console.log(Fox);
}

myTest();
// 回傳S
console.log(Fox);
// 報錯，因為Fox一離開該函式就隱形了。
```

因此，全域變數跟限定域變數可以撞名，不會報錯。

### return回傳結果

一個函式是可以不用寫return statement的，如果沒有把值回傳，函式一樣會運作，只是會回傳undefined。

另外，記得寫在return後面的東西是不會回傳的。

¶ 資料來源：

 (1)佇列：[[資料結構] 佇列 (Queue)](https://ithelp.ithome.com.tw/articles/10204226)