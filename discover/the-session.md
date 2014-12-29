# 連線區段 Session

Meteor 的連線區段（Session）是一個特殊的響應式資料源，是一個全域的獨立物件：只有一個 session，因此你的應用程式各部位都可以透過它來彼此溝通，有點像是一個暫時的、用來溝通的中央交換機。

連線區段主要使用在前端，你可以從主控台這樣寫：

```
Session.set('pageTitle', 'A different title');
```

基本概念就是使用 `Session.set()` 設定一個命名屬性，接著使用 `Session.get()` 讀取資料，當然為了能讓模板獲得資料，你必須使用模板 helper。

## Autorun

響應式資料源，並不代表所有的反應都會是響應式的，例如：

```
helloWorld = function() {
  alert(Session.get('message'));
}
```

即使 `Session` 內的資料改變了、響應了，但這個跳出警示的 JavaScript 代碼還是傳統的（情境非響應式），它並不會因為內部的資料改變了就再度跳出。

Meteor 提供了 `Autorun` 來滿足這樣的需求。

如果從主控台這樣輸入：

```
Tracker.autorun(function() {
		console.log('Value is: ' + Session.get('pageTitle'));
});
```

當你再次改變 Session pageTitle 的值時，主控台就會再次呈現一次改變。

## HCR - Hot Code Reload

連線區段會在用戶重新載入一次頁面時重置，暫存的資料也都會清空、歸零。

當你在開發階段（不關閉終端機後台），Meteor 偵測到你修改了代碼，會驅動應用程式重載，這是一種內部的 HCR，此時的連線區段資料會被存入 Local Storage 中，因此可以持續使用下去。但只要你手動重載頁面，就等於是模擬用戶的主動行為，暫時資料就同樣會清空。

