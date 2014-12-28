# 開始

只需要一行指令就能快速安裝好 Meteor：

```bash
curl https://install.meteor.com | sh
```

建立一個新的 Meteor 專案，則輸入下列指令：

```
meteor create projectName
```

## 安裝套件

安裝 Meteor 套件使用下面的指令：

```
meteor add underscore
```

使用 `meteor list` 會列出目前專案已安裝、使用中的套件。

### 瞭解 Meteor 套件的種類

1. Meteor platform packages：每個 Meteor 都內建的核心套件，一般不會管它。不過更深入之後，或是要區別實際上安裝給網站或行動裝置使用時，有可能會摸到。
2. Meteor 官方套件通常被稱為 **isopacks**。
3. 其他第三方開發、但上傳到 Meteor Package server 上，可供所有人安裝的套件。
4. 本地套件：存在你自己的 `/pacakges` 目錄中，自己建立的套件。
5. **NPM 套件**：雖然一般的 npm 套件無法讓 Meteor 直接取用，但其他套件有可能需要某些 npm 套件才能運作。

## 檔案執行規則

1. `/server` 此目錄中的代碼只在後端運行。
2. `/client` 此目錄中的代碼只在前端運行。
3. 其他的代碼，若是沒有指定，前、後端都可以運行。
4. `/public` 此目錄是用來擺放一些靜態素材，例如字型、圖片等，使用根目錄模式就可以。

### 代碼執行順序

1. `/lib` 此目錄中的程式會優先執行，通常會擺一些基礎的庫。
2. 以 `main.*` 命名的檔案會最後才載入。
3. **越下層的檔案，會優先載入。**
4. 其他檔案都會以名稱的字母序載入。
