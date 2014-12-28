# 部署 Deployment

Meteor 提供了最快速的部屬環境，只要輸入 `meteor deploy myapp.meteor.com` 即可（前提是你必須先註冊好 Meteor.com 的帳號，應用程式的名稱也尚未被其他人使用）。

你確實可以註冊、對應自己的網域名稱到這個部署環境，但它顯然不太合適作為正式的服務環境。（因為不太確定究竟可以支撐怎樣的負荷，沒有很明確的數據告知。）建議還是將這個環境當作學習與測試之用。

[Modulus](https://modulus.io/) 是一個專門提供 Node.js 快速部署的 PaaS 平台。這樣的平台的優點是升級部署很方便，缺點就是貴了一些。

[Meteor Up（簡稱 mup）](https://github.com/arunoda/meteor-up) 是專門設計來部署 Meteor 的程式，適合搭配 Digital Ocean 或 AWS 這類 VPS 使用。雖然多了一些配置的麻煩，但 mup + Digital Ocean 是一開始學習 Meteor 部署的最佳方案，因為你既可以學到完整的環境配置，又不致於過於昂貴。

## 使用 Meteor Up

使用 `npm install -g mup` 安裝 Meteor Up（當然你必須預先配置好自己環境中的 Node 與 NPM）。

Meteor Up 的標準模式是獨立建置一個「部署目錄」，例如原先你的專案在 `meteor/microscope` 目錄，就應該建另一個 `meteor/microscope-deployment` 目錄。

前往該目錄之後，輸入 `mup init` 初始化部署環境。

你會發現目錄下多了 `mup.json` 與 `settings.json` 兩個檔案。

`mup.json` 是與部署相關的設定；而 `settings.json` 則是所有與應用程式相關的設定，例如 OAuth 令牌、分析程式需要的認證資訊等。

```
{
  //遠端主機需要的認證資訊
  "servers": [{
    "host": "hostname",
    "username": "root",
    "password": "password"
    //or pem file (ssh based authentication)
    //"pem": "~/.ssh/id_rsa"
    //使用 Digital Ocean 通常會設定金鑰，就可以省略登入
  }],

  //install MongoDB in the server
  "setupMongo": true,

  //location of app (local directory)
  "app": "/path/to/the/app",

  //configure environmental
  "env": {
    "ROOT_URL": "http://supersite.com"
  }
}
```

### MongoDB 設置

建議使用類似 [Compose](https://www.compose.io) 這類的雲端 MongoDB 資料庫服務，Compose 的正規環境是從每月每GB資料收取$18元開始，會自動向上升級調整。

使用 Compose 時，就需要將 `setupMongo` 設定為 `false`，並在下方的 `env` 環境變數中添加一筆 `MONGO_URL`。

### 設置其他環境變數

諸如 `ROOT_URL`, `MAIL_URL`, `MONGO_URL` 等都需要事先填妥。

## 環境配置

在開始部署前，需要先準備好主機，輸入：

```
mup setup
```

上面這行指令會運行一陣子，因為它將根據上面設置好的主機認證資訊，從本地遠端連接那台主機，進行各種準備工作，包含了在主機上安裝軟體（Node, MongoDB 等等）。

一旦主機安裝完成，你就可以開始部署：

```
mup deploy
```

它會打包並準備好你的應用程式，等後上傳完成，就可以打開瀏覽器測試了。