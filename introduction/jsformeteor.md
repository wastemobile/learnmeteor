# JavaScript for Meteor

一個在十天之內被創造出來，然後被用了二十年之後，不僅沒有過時，還愈來愈受到重視的語言，根本就是個傳奇。這個傳奇真實的存在著，它就是 JavaScript。

我們可以這樣推測，這語言必定有些特殊之處；它的基礎語法可能很簡單，卻得以讓人們進行很有趣的「內部再創造」，進而完成很複雜的事。仔細想想就跟基礎數學很像，數學的運算符並不是太多，但可以寫出根本就像是外星文的複雜算式，即使你看得懂每一個符號，卻永遠摸不透它到底在做什麼。

Meteor 是個使用 JavaScript 同時開發前端與後端的架構，它已經做完了很多超乎想像的複雜底層工作，讓開發變得很簡單，但完全不會 JavaScript 還是不可能學習它，因此有人特別寫了這篇 [JavaScript for Meteor](https://www.discovermeteor.com/blog/javascript-for-meteor/) 來引領初學者，至少理解 JavaScript 最重要的20％，可以開始使用 Meteor。

## 變數 Variables

在 JavaScript 中宣告一個變數的方法：

```javascript
var a = 12;
```

變數存在的「場域」（又稱為情境、上下文）很重要，如果你在一個函數中使用 `var` 宣告變數，這變數就只有這個函數才能看到、才能夠使用；如果你省略了 `var` ，改以類似像 `a=12` 這樣的語法宣告變數，這個 `a` 就成了**全域變數(global variable)**，你的整個 Meteor 程式都看得到它，有時這是對的，但很多時候會造成不必要的麻煩，試著想想若你的程式寫作習慣不好，常常使用像上面這種單一字母快速建立變數，幾個檔案、幾十個函數寫完，可能程式一跑起來會遇到好幾個都叫做 `a` 的全域變數，最後得到什麼結果就不是那麼容易預測了...。

謹記一個原則：**「除非你十分確定自己需要的，是讓所有程式都一體適用的全域變數，否則養成習慣始終使用 `var` 來宣告變數**。

## 函數 Functions

函數式編程近年來好像愈來愈受到重視，某些人說 JavaScript 實際上是全世界被使用最多的一種函數式語言，由此可見函數對 JavaScript 重要性。

函數是這樣宣告的：

```javascript
var myAwesomeFunction = function(myArgument) {
	//do something
}
```

然後你使用這樣的方法去呼叫函數：

```javascript
myAwesomeFunction(something);
```

在 JavaScript 裏，函數也是一種變數！看看下面的代碼：

```javascript
square = function(a){
	return a*a;
}
applyOperation = function(f, a){
	return f(a);
}
applyOperation (square, 10);
```

很酷吧！

## 回傳 Return

敘述語法 `return` 讓一個函數運行之後返回一些東西，然後你根據這個結果去做其他的事，如果你不去處理，基本上它就沒有什麼顯性的結果呈現出來。

注意一件事：當函數已經寫出了 `return`，基本上就算是運行結束了，在 `return` 後面的代碼是不會被繼續執行的。

```javascript
myFunction = function(a){
	return a * 3;
	explodeComputer(); //這段代碼不會執行
}
```

函數都可視為一個個獨立的程式運作單元，它可以接受一些輸入：參數（arguments），然後得到一些結果：回傳（return）。如果回傳的是**值（values）**，就很可能是等著讓其他小程式（函數）去使用；當然也可以讓一個函數的結果，直接驅動、呼叫另外的小程式（還是函數）。

## 條件敘述

程式的邏輯運算，大部分都接近「如果xxx，就執行yyy，否則就zzz」。

```javascript
if (foo) {
	return bar;
}
```

上面的代碼也有一種簡縮的寫法：

```javascript
if (foo)
	return bar;
```

「如果」的條件判斷還可以加上「否則」這樣的情況（如果 `foo` 條件為真，就會執行 `function1()`，否則就執行 `function2()`：

```javascript
if (foo) {
	function1();
} else {
	function2();
}
```

同樣也有一種簡縮的寫法：

```javascript
foo ? function1() : function2();
```

宣告與指定變數時也能夠使用這樣的特殊語法：

```javascript
var n = foo ? 1 : 2;
```

變數 n 的值根據條件 foo 的真假判斷而有不同，foo 為真時 n 會等於 1，否則就等於 2。

更複雜一點的條件會是這樣：

```javascript
if (foo) {
	function1();
} else if (bar) {
	function2();
} else {
	function3();
}
```

## JavaScript Arrays 資料陣列

資料陣列是一次儲存了許多資料的抽屜或檔案櫃，你可以使用特定的編號取出資料來。

```javascript
// 資料陣列
a = [123, 456, 789];
// 取出資料
a[1]; // 456
```

只要記得第一個資料的編號是 `0` 就行了。

## JavaScript Objects 物件（對象）

定義一個 JavaScript 物件的方法如下面的範例：

```javascript
myProfile = {
  name: "John Smith",
  email: "johnsmith@gmail.com",
  city: "San Francisco",
  points: 1234,
  isInvited: true,
  friends: [
    {
      name: "John Doe",
      email: "johndoe@gmail.com"
    },
    {
      name: "Jane Doe",
      email: "janedoe@gmail.com"
    }
  ]
}
```

物件包含一大堆使用逗點 `,` 分隔的資料，每一組資料都是 `key（鍵）` 與 `value（值）` 的組合，值可以是單純的字串、數字、真假，還可以是變數、陣列或另一個物件（巢狀），甚至是**函數**。

取用物件的屬性（值）很容易，記得 `點語法（dot notation）` 就對了：

```javascript
myProfile.name; // John Smith
myProfile.friends[1].name; // Jane Doe
```

你幾乎可以在 JavaScript 裏隨處都遇到物件，特別是在調用函數時。舉例來說，在 Meteor 程式裡對資料庫查詢某一篇特定的文章，使用這樣的語法：

```javascript
Posts.findOne({ title: 'My First Post'});
```

這個 `{ title: 'My First Post'}` 參數是個匿名的 JavaScript 物件。在 JavaScript 的世界，你會發現大多數情況下，你並不需要替物件指定名稱（對函數也常常如此），就可以直接使用（因為沒有名稱，因此才被稱呼為**匿名（anonymous）**。

## 匿名函數 Anonymous Functions

前面我們看到過這個範例：

```javascript
// 先定義了一個計算平方的函數
square = function(a){
	return a * a;
}
// 定義另一個函數，接受以參數型態傳入的另一個函數
applyOperation = function(f,a){
	return f(a);
}
// 傳入計算平方的函數，我們就可以計算平方
// 如果存在另一個計算邊長的函數，我們還可以傳入它來計算邊長
applyOperation(square, 10);
```

但這裡要看一個 JavaScript 的特殊寫法，這裡常常是學習 JavaScript 的一個門檻：

```javascript
applyOperation = function (f, a) {
	return f(a);
}
applyOperation(
	function(a){
		return a * a;
	},
	10
) //100
```

相較原來的代碼，沒有先定義一個名為 `square` 的函數、再傳入它當成參數使用，而是直接在**參數呼叫**中直接定義，這就是「匿名函數」的使用，也是最常見的 JavaScript 範式之一。

（感覺這裡解釋得不太清楚，或是這個範例不太貼切）

## 串接 Chaining

先理解另一個「點語法」的使用，在對陣列或字串的處理上我們會常常遇到：

```javascript
var myArray = [123, 456];
myArray.push(789) // 123, 456, 789
```

```javascript
var myString = "abcdef";
myString.replace("a", "z"); // "zbcdef"
```

上面這個點語法的意義是：「對 `myString` 呼叫 `replace` 函數，使用了 "a" 與 "z" 兩個參數，並回傳處理後的結果」。

點語法最棒的就是可以「串接」使用，看起來像是這樣 `something.function1().function2().function3()` 的結構，例如：

```javascript
n = 5;
n.double().square(); //100
```

## This

最簡單的一個字 `this`，卻可能是精通 JavaScript 最困難的概念之一。

基本上，`this` 關鍵字讓你存取你正在工作的物件：它像是個變色龍，`this` 根據所處的環境持續變化著。

既然如此，乾脆跳過直接去解釋 `this` 的意義，我們提供兩種工具讓你自己來摸清楚。

第一個工具就是 `console.log()`，這個指令會在瀏覽器的主控台（終端機）顯示任何你想要呈現的物件。所以在一個函數起始時印出 `console.log(this)` 就可以清楚知道它現在到底是什麼：

```javascript
myFunction = function(a,b) {
	console.log(this);
	// do something
}
```

第二種範式則是把 `this` 指定給另一個變數：

```javascript
myFunction = function (a, b) {
	var myObject = this;
	// do something
}
```

看起來好像有點疊床架屋，但在你的代碼中使用 `myObject` 變數是很安全的，因為它不像 `this` 一樣會隨著情境變幻莫測。

## 運算子 Operators

JavaScript 中的 `===` 代表「全等」，也就是不僅「值」要相同，連「型態」也要一樣。

## 怪異的隨機字母

你常常會遇到這樣的代碼：

```javascript
$('body').addClass('loaded');
```

或是這樣：

```javascript
_.shuffle([1, 2, 3, 4, 5, 6]);
```

其實 `$` 與 `_` 都不是什麼特殊的 JavaScript 運算符號，它們實際上只是**使用者自訂的變數名稱**而已！

`$()` 是 jQuery 函數設定的一個別名（從 jQuery 函式庫引入），而 `_` 則是 Underscore 函式庫的主要物件名稱。

## 語法格式 Style

有一些慣例與規則，可以讓你的代碼更容易辨識：

* 使用「駝峰命名法（camelCase）」，也就是寫成 `myRandomVariable` 而不要寫成 `my_random_variable` 。
* 敘述句尾總是加上 `;`，即使在某些可以省略的狀況也一樣。
* 關鍵字之間總是加上「空格」，例如 `a = b + 1` 不要寫成 `a=b+1`。

更多的指引可以參考 [Meteor Style Guide](https://github.com/meteor/meteor/wiki/Meteor-Style-Guide)。

## 整合在一起

上面已經介紹了最基本的 JavaScript 語法，接下來就需要理解把它們通通組裝在一起的寫法，例如下面這段取自 `Microscope` 程式的代碼：


```javascript
Template.postEdit.events({
  'submit form': function(event) {
  	// 上面這兩行是 Meteor 撰寫模板事件的特殊語法
    event.preventDefault();
    // 這一行是用來取消瀏覽器預設、送出後的行為
    var currentPostId = this._id;
    // 定義一個變數，使用目前的貼文 ID
    var postProperties = {
      url: $(event.target).find('[name=url]').val(),
      title: $(event.target).find('[name=title]').val()
    }
		// 把用戶輸入到表單中的值（網址與標題）擺進一個變數中
		// 接著就進行對資料庫的更新
    Posts.update(currentPostId, {$set: postProperties}, function(error) {
      if (error) {
        // display the error to the user
        throwError(error.reason);
      } else {
        Router.go('postPage', {_id: currentPostId});
      }
    });
    // 遇到錯誤，就回應錯誤處理；
    // 正確存入後，就轉址回用戶貼文頁面
  }
});
```

`Template.postEdit.events({})` ：我們進入 `Template` 物件，取用它的 `postEdit` 屬性（你必須撰寫一個名為 `postEdit` 的模板），然後使用點語法呼叫 `events()` 函數（它是 `postEdit` 的一個屬性），而呼叫的方法是以一個「匿名的 JavaScript 物件」模式。

`submit form` ：第一個（也是唯一的） JavaScript 物件鍵值配對，因為鍵中包含了空格（`submit form`），所以被程式解讀成「引用」，實際的值是一個以 `event` 參數呼叫的匿名函數。

`event.preventDefault()` ：我們呼叫了 `event` 物件提供的 `.preventDefault()` 方法，用來停止表單的 `submit` 預設驅動的行為（送出、重載頁面...）。

`var currentPostId = this._id;` ：在這個情境中， `this` 代表正在編輯的這則貼文。我們宣告一個本地變數，讓它的值等同於貼文的 `_id` 屬性。

`var postProperties = {}` ：我們宣告另一個變數，然後使用了 **jQuery** 提供的便利功能，從用戶輸入資料的表單中取得 `url` 與 `title` 的值。

`Posts.update(currentPostId, {$set: postProperties}, function(error){})` ：我們傳送三件事到 `update()` 函數，要更新的貼文 ID，一個包含了更新資料的 JavaScript 物件，最後則是一個用以回傳的匿名函數，負責處理更新之後的結果。

## 下一步

上面這些最簡單的 JavaScript 知識，無法讓你寫出炫目的程式碼，也無法取代你該認真學習 JavaScript 的所有課程，但已經可以讓你看懂《Discover Meteor》一書中的範例代碼，至少能夠理解代碼大概是在做什麼事。

如果「從做中學」是你習慣的模式，那麼就趕緊開始閱讀《Discover Meteor》、跟著書中的講解，開始建立一個 Meteor 應用程式吧！

## 學習 JavaScript 的資源

下面是推薦學習 JavaScript 的幾個起點：

1. [Codecademy](http://www.codecademy.com/)
2. [JavaScript is Sexy](http://javascriptissexy.com/)
3. [SuperHero.js](http://superherojs.com/)

