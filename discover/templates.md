# 模板

我們將採用「由外到內」的開發模式，也就是先開發「靜態」的 HTML/JavaScript 外殼，再一步步向內調整到整個程式能正常運作。

所以這一章我們只會集中在 `/client` 目錄，因為這邊的東西都只會在前端運作。

（由於我們要使用 Bootstrap，不過為了未來進一步開發的準備，不使用只單純引入 CSS 樣式的套件，而改裝 Nemo64:bootstrap 這個使用 LESS 且能夠自行挑選元件配置的進階套件。）

首先，在 `/client` 目錄下建立 `main.html`。記得，以 `main.*` 命名的檔案，都是最後才會載入的。

我們在 `main.html` 檔案中，擺放了兩組標籤：`<head>` 與 `<body>`。Meteor 實際上只支援三種模板類型，這是其中兩種，第三種就是之後最常會使用到的 `<template>`。

在 `<body>` 區塊裡，你會看到一組奇怪的標籤：`{{> postsList}}`，這是 Meteor 呼叫、載入模板的語法，這裏的標準解釋就是：「在這裡載入名為 **postsList** 的模板」。

所以接著我們就要來建立 `postsList` 模板，我們在 `/client/templates/posts` 目錄下建置 `posts_list.html` 的檔案。

Meteor 模板是這樣規範的：

1. 它就是 HTML 檔案。
2. 最上層的標籤使用 `<template>`，再賦予 `name="xxx"` 的屬性。
3. 其實擺在哪裡都可以，視自己的習慣與專案的需求彈性定義。
4. 真正重要只有模板「名稱」。

我們在 `postsList` 模板中使用了 `{{#each posts}}` 這樣的語法，裡面再呼叫另一個名為 `postItem` 的模板。

## Spacebars

Meteor 的預設模板語言叫做 **Spacebars**，是根據 **handlebars** 設計的。

> 更進一步探究的話，Meteor 的前端介面框架其實叫做 **Blaze**，Spacebars 只是一層便利使用的語法庫，也有可能使用其他現有的模板語言，至少目前已經有使用 **jade** 當模板語言的套件。

Spacebars 替正常的 HTML 加入了三種額外功能：

1. inclusions（"partials"）： 像是呼叫命名模板 `{{> postsList}}` 的語法就是一種「嵌入」。
2. expressions： 呼叫目前物件的屬性，例如 `{{title}}` 或 `{{url}}` 這樣的語法，就是標準的「表達式」。
3. block helpers： 使用 `{{#each}}...{{/each}}` 這樣的迭代，或是 `{{#if}}...{{/if}}` 這樣的條件判斷，替模板加入邏輯判斷處理能力。

## Template Helpers

雖然我們使用 Spacebars 語法寫出了類似這樣的模板：

```
<template name="postsList">
  <div class="posts">
    {{#each posts}}
      {{> postItem}}
    {{/each}}
  </div>
</template>
```

但實際上它還沒有任何作用，因為 Meteor 將模板與實際的邏輯拆解開來，維持彼此的獨立運作。

模板需要 `helpers`。這些 **helpers** 實際上像是「廚師」，他們把原始食材（資料庫裡的資料）烹煮成一道道的佳餚，模板是端去給客人時盛裝用的「**盤子**」（容器）。

換句話說，模板的角色僅限於呈現、遞迴取得變數，而 `helpers` 是實際做大多數苦工的角色，它得根據各種狀況（條件、參數），向資料庫取得資料，還得把正確的資料擺進每一組需要呈現的變數之中。

> Meteor 模板使用的 helpers，與一般 MVC 框架所說的 Controllers 不太一樣，最好還是不要混淆、誤用了。

因為 Meteor 會自己進行最後的大統整，正如剛才前面提到，只要你有設置模板名稱、從其他模板呼叫（嵌入）也使用了正確的名稱，模板本身寫在哪兒不重要。同樣的，只要在 JavaScript 中 `Template.postsList.helpers({})` 寫得正確，這段代碼擺在哪兒也都沒關係。因此可以看到 Meteor 應用程式都採用一種「功能關聯性」原則來組織檔案。

例如你在 `/client/templates/posts` 目錄擺放幾個模板檔案，就直接使用同名（副檔名改成 `.js`）的模式新增 JavaScript 檔案，與這個模板相關的 `helpers` 都集中擺放。

在這種「從外而內」的開發模式下，在 `helpers` 檔案中還可以先以靜態的資料陣列，模擬之後會向資料庫取出的東西樣貌。

> 注意這範例中如何設置 `domain` 的，書中稱之為 **JavaScript Magic**，因為你知道某些元素具有哪些屬性可以取用，這裡是指 `a` 標籤的 `hostname` 屬性，所以才會先用 JavaScript 建立一個虛擬的 `a` 標籤、將 `this.url` 設給 `a`，轉一手取出了要用的 `domain` 資料。



