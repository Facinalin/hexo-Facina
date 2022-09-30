---
title: Shortcircuit Evaluation短路求值
date: 2022-9-29
tags: 
   - 前端技術筆記
   - 網頁設計
   - Javascript
---

### Shortcircuit Evaluation 短路求值-高級寫法（？）

來了，短路求值。聽起來就很快。（？）

現在我們有個唱片行的資料，老闆為了進銷存每個唱片都會編號。實際情境需求：因應唱片會有售出情形，編號可以重複使用，但裡面的資料已經不同，故需新增資料或更新唱片資料。不過因為曲目方面的清單小助手還在整理，所以有些編號的專輯不會有收錄曲目。

示範物件：

``` bash
const recordCollection = {
  2548: {
    albumTitle: 'Slippery When Wet',
    artist: 'Bon Jovi',
    tracks: ['Let It Rock', 'You Give Love a Bad Name']
  },
  2468: {
    albumTitle: '1999',
    artist: 'Prince',
    tracks: ['1999', 'Little Red Corvette']
  },
  1245: {
    artist: 'Robert Palmer',
    tracks: []
  },
  5439: {
    albumTitle: 'ABBA Gold'
  }
};
```
** 第一種寫法：（If/ Else if 基本款）

``` bash
function updateRecords(records現有的物件資料組, id資料單位的編號, prop屬性如artist/tracks和albumTitle, value屬性資料組的值) {
  if(prop!=="tracks" && value !==""){
    records[id][prop]= value;

    //如果輸入的prop不是"tracks"（而是artist或albumTitle），且值不等於空字串時，用輸入的value覆蓋掉資料原本的值。//
  }

  else if(prop=== "tracks" && records[id].hasOwnProperty("tracks")=== false){
    records[id][prop]= [value];

    //如果輸入的prop是"tracks"，且該資料組尚未含有tracks這個prop時，為其新增一個名為tracks的陣列並把輸入的value送進去。//
  }

  else if(prop=== "tracks" && value !==""){
    records[id][prop].push(value);

     //如果輸入的prop是"tracks"，且其值不為空字串時（代表該資料組已有tracks這個prop），從陣列的最尾端推輸入的value進去。//
  }

  else{
    delete records[id][prop]

    //如果其輸入value為空字串時，直接刪除該屬性。（順便整理資料）//
  }
  return records;

  //最後回傳更新過的物件。//
}

updateRecords(recordCollection, 5439, 'artist', 'ABBA');
```

** 第二種寫法：（短路求值款）

剛開始還沒回過神來，看不懂第二區的短路求值在幹嘛，後來用了一個理解方式，發現超具象化，分享給各位。若以人的視角來說，先請二號小幫手看過一遍整體哪些專輯有track的紀錄，哪些沒有，有的話就先放著，沒有的話要多幫他用奇異筆畫上去一個空陣列[ ]。（當然這時候已經過濾掉沒有track資料的那些資料了）全部掃完一遍之後，便開始增加新曲目進去這個陣列（甚至請個三號小幫手來新增也沒差），理論上只要有畫陣列的都會有資料，不會忘記新增進去。計畫通：）

有種家庭代工的既視感，一個人先掃完一部分，另一個人再負責填資料，比起兩個人個別一個一個填資料快多了。

``` bash
function updateRecords(records, id, prop, value) {
  if (value === '') {
    delete records[id][prop];
  } else if (prop === 'tracks') {
    records[id][prop] = records[id][prop] || []; 
    records[id][prop].push(value);

    //如果已有該prop的資料組，就回傳該陣列，無的話便為他新增一個空陣列。確認完後，整體一起新增值到陣列。
  } else {
    records[id][prop] = value;
  }
  return records;
}

```

** 第三種寫法：（變數款）

變數真的是一個超級靈活的工具。

``` bash
function updateRecords(records, id, prop, value) {

  const album = records[id];

  if (value === "") {
    delete album[prop];
  }

  else if (prop !== "tracks") {
    album[prop] = value;
  }
  else {
    album["tracks"] = album["tracks"] || [];
    album["tracks"].push(value);
  }

  return records;
}
```