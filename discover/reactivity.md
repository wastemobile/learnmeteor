# 響應

> 如果資料集合是 Meteor 的核心功能，那麼「響應性」就是讓這個核心能夠發揮作用的外殼。

（ps. 實在不太同意 Collections 是 Meteor 的核心功能的說法。）

探究 Meteor 如何偵測到資料集合的異動、又如何修改前端的用戶介面呈現，我們必須瞭解一下 `.observe()` 的作用。

```
Posts.find().observe({
  added: function(post) {
    // when 'added' callback fires, add HTML element
    $('ul').append('<li id="' + post._id + '">' + post.title + '</li>');
  },
  changed: function(post) {
    // when 'changed' callback fires, modify HTML element's text
    $('ul li#' + post._id).text(post.title);
  },
  removed: function(post) {
    // when 'removed' callback fires, remove HTML element
    $('ul li#' + post._id).remove();
  }
});
```

我們之前有提到 `find()` 實際只是個「指針」，現在我們用 `.observe` 去監控這個指針的異動，當指針回傳幾種不同的改變時，我們就手動去改變瀏覽器的 DOM。

這樣的代碼很快就會變得非常複雜，你得妥善改變貼文的每一種屬性，還要顧及 HTML 呈現的正確性；萬一你必須根據多個資料來源進行組裝與修改，事情就更複雜了。

> 有時我們不得不使用上面這種由 `observe()` 驅動的範式，特別是與第三方的小工具互動時。例如嵌入了外部的地圖，你就要使用 `observe()` 監測 Meteor collection 的 `added` 與 `removed` 回傳，接著呼叫地圖 API 的 `dropPin()` - 加上大頭針，或是 `removePin()` - 移除大頭針，進而即時改變地圖上的呈現。

## 宣告式

透過 Spacebars 的語法，搭配模板的 helpers，Meteor 將複雜的事務（包含 `observe()` 的監控處理）通通隱藏在底層，讓開發者能夠簡單、輕易的實現響應式編程。

## Computations

雖然 Meteor 強調是一個即時、響應式的框架，並不表示所有的代碼都隨時隨地處於響應的狀態，否則你的應用程式每一分每一秒都得不停地反覆運行。相反的，響應性只侷限在特定的代碼區塊，我們稱這些區域為「computations」（計算域）。

換句話說，一個計算域是一段代碼，根據它監控的響應式資料源的改變而改變。如果你有一個響應式資料源（例如，一個連線區段變數），而你希望時時因為資料源的異動而做出反應（處理），你就需要替它設置一個計算域。

你通常不需要主動宣告計算域，是因為 Meteor 已經賦予模板與 helper 之間一種特殊的計算域，所以只要你使用了正確的模板語法、設置正確的 helper，它就會處於響應式的狀態。

每一個響應式資料源都紀錄了正在使用它的計算域，一旦資料源改變了，就會立即通知這些計算域。技術上來看，資料源會呼叫這些計算域的 `invalidate()` 函數。

計算域一般都被設置為根據 `invalidation` 的狀態重新計算，這就是模板計算域的工作方式（當然模板還同時施展了其他魔法，例如讓頁面重排更有效率）。當你有特殊需求時，確實可以透過一些控制項來處理計算域的行為，但實務上你幾乎沒有機會這麼做。

## 設置一個計算域

你可以使用 `Tracker.autorun` 函數在計算域中包裹一段代碼，讓它成為響應狀態：

```
Meteor.startup(function() {
  Tracker.autorun(function() {
    console.log('There are ' + Posts.find().count() + ' posts');
  });
});
```

當你試著插入一筆資料後，主控台就會立刻跳出一行帶著總貼文數量的訊息了。

