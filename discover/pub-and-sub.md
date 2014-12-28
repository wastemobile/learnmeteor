# 資料發行與訂閱機制

本章詳細解釋了 Meteor Publication 與 Subscribe 的原理，以及基礎的使用方法。

```
Meteor.publish('allPosts', function(){
  return Posts.find({'author':'Tom'}, {fields: {
    date: false
  }});
});
```

不僅可以根據欄位篩選資料（只發行 `author=Tom` 的資料），還可以篩選欄位（上面就省略了 `date` 欄位）。

```
// on the server
Meteor.publish('posts', function(author) {
  return Posts.find({flagged: false, author: author});
});

// on the client
Meteor.subscribe('posts', 'bob-smith');
```

發行還可以根據訂閱送進來的「參數」，動態決定要發行哪些資料（通常可以用在只發行登入用戶的個人資料）。
