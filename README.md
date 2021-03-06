Learn Meteor
============

（測試使用 Prose.io 線上編輯器，與中文相容度不錯。）

Meteor 是一個正在崛起的新興平台、框架、應用程式伺服器，以及一個整合性的開發平台。他替網站開發與網站應用程式帶來嶄新的視野，與過去的開發模式截然不同，說是「革命」也不為過。當然，擁抱它也並不是全無障礙，但跨過這倒不算太高的柵欄之後，風景怡人啊！

首先，從技術底層來看，Meteor 是「同構（Isomorphic）」模式的應用程式，依賴著把 JavaScript 引擎搬到主機端運作的 Node.js，讓你同時開發前端與後端，只使用一種語言：JavaScript。

對很多想要掌握新世代網路與資訊風潮的人來說，往往一直卡在學習程式語言這道關卡，在傳統的環境，你必須了解前端的 HTML, CSS 以及簡單的 JavaScript（通常是 jQuery），然後需要學習一種後端語言，PHP, Ruby, Python 或 Haskell 等等，以及用來儲存資料的資料庫（通常是 MySQL）。

慢慢的也開始流行一種「單頁式應用程式（One Page Application）」，把大部分操作的邏輯從後端搬到前端，真正的後端成了提供資料、接收資料，並在背後處理資料的隱性伺服器，你的前端應用多半與一個或多個 RESTful APIs 溝通。提供這種雲端服務的廠商也多了起來。

但這並不完整。舉例來說，你做了自己的書評網站（從 Amazon 的 API 拿回書目資料），還模擬臉書讓讀者能夠對你的書評按個讚、或給個五星評等，但你希望呈現統計後的評分結果，像是「根據64個人的評價，獲得4.25的平均分數」。資料庫記錄下了每個人對一篇書評的星等評價、儲存到後端主機（或類似 Firebase 這樣的雲端服務），但平均分數卻只能每次取回資料時在前端即時運算並呈現。比較理想的狀況，應該是後端有個程式在每次有人送進評等時，就計算並寫下平均數值，前端就只需要取回這個計算後的數值而已。結果就是你還是得想辦法在後端跑自己想要的邏輯。

Meteor 就是一個讓你能同時掌握、設計、開發前端與後端的架構。

只使用 JavaScript 是一種正確的選擇嗎？這應該是許多人的疑問。

不管你的網站與應用程式採用什麼架構開發，JavaScript 都是必學的技術，畢竟它是所有瀏覽器、以及使用網頁組版引擎的應用程式，標準的官方語言（甚至可說是唯一的一種語言）。而目前許多人理解的 JavaScript，有一大半都在處理瀏覽器內很細微的事務：修改 DOM、擷取與計算點擊位置、處理動畫與轉場特效...。但若是把 JavaScript 擺進後端、實際用它處理原來其他程式語言運作的事，就接近它另一個面向：千變萬化的函數式編程特質。對完全沒有什麼程式基礎的人來說，其實比前端那種追求細緻控制的 JavaScript 更容易入手。

## Meteor 的優點解析

1. 讓你全盤掌控網站與應用程式的每一部分邏輯。
2. 提供完整的網站開發工具。如果你使用 Mac，就只要準備一個順手的編輯器（建議 Sublime Text 或 GitHub Atom），懂得打開終端機、使用一些指令操作，其他什麼都不需要。
3. 簡單入手。Meteor 非常精萃的將過去各種語言的開發框架之優點匯集起來，再搭配一些社群開發的套件，幾小時之內就能摸到訣竅。

第四點，也應該是 Meteor 最大的亮點，就是所謂的「響應式編程邏輯（Reactive Programming）」。

這一點也不嚇人，其實你早就懂了，就像是 Excel 這樣的試算表。當你將格子 C 設定成等於 A+B，也就是寫了一段超簡單的程式：C = Sum(A,B)，之後任何時候你改變了 A 或 B 的數值，C 也就自動跟著變了；即使另一個格子 D 設定成 Cx3，當 A 改變時，C 與 D 都自動改變，你完全不用多做任何事。這就是響應式編程，Meteor 的關鍵核心就在這地方：當你設置好程式依賴的**響應式資料源**之後，焦點就只要擺在商業邏輯怎麼處理，不用擔心資料變化後的擷取、監控、傳遞，而這恰好是過去程式開發最麻煩的地方。

## Meteor 的缺點

1. 它目前只支援一種資料庫：NoSQL 陣營的 MongoDB。
2. 你無法與過去的環境整合並存，例如它不能成為你傳統 PHP 網站架構中的一部分，你必須單獨替它配置主機來跑。
3. 目前的 Meteor 還不支援 SSR - Server Side Render，因此不太適合做以靜態資料為主的服務。

