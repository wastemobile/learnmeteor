# 路由

我們需要添加一個超級重要的第三方套件：[Iron Router](https://github.com/EventedMind/iron-router)。它有三大功能：

1. 設定路由（網址列的路徑）。
2. 處理篩選（在網址列後方加入不同的參數或查詢）。
3. 管理訂閱（管控哪個路徑可以取得哪些資料）。

### 路由器的字彙知識

其實 Iron Router 參考了許多優良框架的設計，例如 Rails。許多詞彙都是通用的，下面這些詞彙可以建立對路由器的基礎瞭解：

1. 路由（Routes）：路由是整個路由機制運作的基礎單元。它是一組設定，告訴應用程式在遇到某一個網址時，要到哪裡去，以及要做什麼事。
2. 路徑（Paths）：路徑就是你的應用程式裡面的一個「網址（URL）」。可以是靜態的（`/terms_of_service`）或動態的（`/posts/xyz`），甚至還可以包含查詢參數（`/search?keyword=meteor`）。
3. 區段（Segments）：路徑的不同部分，根據 `/` 拆分的小單元。
4. 掛鉤（Hooks）：掛鉤是一些你打算進行的行動，可以發生在路由之前、之後、甚至是進行之中。一個標準的例子就是事前檢查用戶是否擁有瀏覽該頁面的權限。
5. 路由模板（Route Templates）：每一個路由都需要指向一個模板。若是你沒有指定，路由器預設會去尋找與路由同名的模板。
6. 排版檔（Layouts）：可以將排版檔視為內容的「外框」。排版檔包含目前模板外層所需要的所有 HTML 代碼，即使模板內容改變了，排版檔也維持不動。
7. 控制器（Controllers）：某些時候，你大多數的模板會重複使用相同的參數。為了不要一直重複撰寫相同的代碼，你可以讓這些路由去從一個單獨的「路由控制器」繼承那些通用的路由邏輯。

完整的路由器說明文件可以在[GitHub](https://github.com/EventedMind/iron-router)上看到。

## 路由：把網址對應到模板

Meteor 採用極簡、也最彈性的方式設計「模板（Template）」，只支援三種頂層標籤（`<head>`, `<body>` 以及 `<template>`），因此模板更像是 **partials**，有寫入 `<head>` 與 `<body>` 的則接近**排版檔（Layout）**。

安裝了 Iron Router 之後，這套件把 Layout + Template 的模式帶了進來。先寫好一個預設的排版檔，在 `<body>` 裡擺一個 `{{> yield}}` 特殊標籤，路由器會自動根據路由的設定、替換成對應的模板。

排版檔中可以擺放不只一個 `yield`，必須以不同的名稱區隔，像是 `{{> yield "aside"}}` 或 `{{> yield "footer"}}`，將主區塊、側邊欄或頁腳拆分開來。

![Layouts and Templates](https://book.discovermeteor.com/images/diagrams/router-diagram@2x.png)

一開始可以建立一個單獨的路由設定檔，頂端寫「預設設定」，後面則是不同的路由。

```
Router.configure({
	layoutTemplate: 'layout'
});

Router.route('/', {
	name: 'postsList'
});
```

## 命名路由

路由有一個預設尋找「對應模板」的機能，事實上如果你沒有賦予路由一個「明確的名稱」，它第一優先是根據設定的「路徑」去找（上面的範例就不適用，因為 `/` 沒有對應的模板存在）。因此主動替路由命名、並以路由名稱去設定相同名稱的模板，是個好習慣。

命名路由還可以使用 `{{pathFor}}` 這個 helper，會回傳路由的網址。

## 等候資料

```
Router.configure({
	layoutTemplate: 'layout',
	loadingTemplate: 'loading',
	waitOn: function() {
		return Meteor.subscribe('posts');
	}
});
```

路由的功能之一：掛鉤。這裏的 `waitOn` 就是個必定會用到的掛鉤：我們把之前寫在 `main.js` 裡面訂閱資料的代碼移到這裡，這個路由會確保訂閱的資料正確取到前端時才呈現出來。

Iron Router 還內建提供了「等候載入」的模板設定，只要寫入模板名稱，就會先載入等候模板、等到資料準備妥當，把組好的版替換上去。

```
<template name="loading">
	{{> spinner}}
</template>
```

> 安裝額外套件 `meteor add sacha:spin`，就只需要呼叫 `{{> spinner}}` 模板，一切搞定。

> 再次驗證「響應式編程」：我們讓路由在等候資料時，先顯示「載入中」的模板，但 Meteor 究竟如何判斷何時該把用戶導回正確的呈現模板？答案當然是「當資料準備妥當」，但我們卻根本沒有寫任何一行代碼來確定這件事，其實這就是「響應式編程」的概念（還記得試算表的例子嗎？）。

## 路由至特定貼文

你不可能替成千上百的內容分別指定路由，對動態資料來說，我們需要用到「動態路由」。

我們先建一個給單獨內容的模板 `postPage`，再設定路由：

```
Router.route('/posts/:_id', {
  name: 'postPage'
});
```

特殊的 `:_id` 語法告訴路由器兩件事：

1. 以 `/posts/xyz` 這樣的格式來滿足路由（任何資料都行）。
2. 從路由的 `params` 陣列中找出 `_id` 這個屬性來匹配。

注意：這裡使用 `_id` 實際上只是為了辨識方便，路由器根本不知道你是不是擺進了正確的 `_id`，或只是隨機的一組字串。

符合上面的路由格式，就會顯示出對應的模板了，但我們還少了一段東西：路由器知道擁有這個 `_id` 的貼文就是我們要呈現的資料，可是模板卻不知道啊！該如何填補這段空缺？

還好，路由器已經內建了解決方案：你可以指定一個模板的「**資料情境（data context）**」，你可以把資料情境想成一塊美味蛋糕的內餡，這塊蛋糕的外觀是由模板與排版檔組成的。

![The data context.](https://book.discovermeteor.com/images/diagrams/router-diagram-2@2x.png)

回到範例，我們根據從網址列取得的 `_id` 資料，去找出符合的 `Posts`。

```
Router.route('/posts/:_id', {
  name: 'postPage',
  data: function() { return Posts.findOne(this.params._id); }
});
```

記得 `fineOne()` 只會回傳一筆吻合的查詢，同時也只提供比對一種參數的能力：也就是 `id`，類似這樣 `{_id: id}` （`_id` 是指資料的唯一識別碼，`id` 是變數，你必須在前面的程式想辦法讓它拿到正確的資料 `_id`）。

在路由的 `data` 函數，`this` 對應著目前符合、被選取的路由，我們可以使用 `this.params` 拿到這個命名路由的各種資料。（？）

### 深入理解資料情境

[參考](https://www.discovermeteor.com/blog/a-guide-to-meteor-templates-data-contexts/)

## 使用動態命名路由的 helper

`{{pathFor 'postPage' someOtherPost}}`

## 404處理

1. 建立 404 模板。
2. 在 `Router.configure` 中添加 `notFoundTemplate:`。
3. 另建 `Router.onBeforeAction('dataNotFound', {})` 處理動態路由 404。


