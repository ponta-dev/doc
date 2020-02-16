# ドキュメント操作
* 1件登録
```javascript
db.コレクション名.insertOne(
    { 属性:バリュー,... }
)
```
* 複数登録
```javascript
db.コレクション名.insertMany([
    { 属性:バリュー,... },
    { 属性:バリュー,... },
    { 属性:バリュー,... }
])
```
* 全検索
```javascript
db.コレクション名.find()
```
## フィールドを指定した検索
```javascript
db.コレクション名.find(
    { 検索条件 },
    { key1: 0, key2: 1, key3: 1,...}
)
```
初めの{}が検索条件  
2番目の{}が表示するデータを示す（0が非表示、1が表示)  
例えばname: 1 はnameだけ表示する  
配列も属性を1にする  
オブジェクト内の属性は"オブジェクト.オブジェクト内の属性": 1  
""で囲う必要がある
## 検索条件
* 完全一致
```javascript
db.コレクション名.find(
    { key: value }
)
//例　IDの完全一致検索
db.test.find({_id: ObjectId("5e15e41ea1a009381072696d") })
```
* 部分一致
```javascript
db.コレクション名.find(
    { key: /value/ }
)
```
配列でもいける  
指定の仕方はフィールド指定同様（Objectも一緒）
```javascript
> db.test.find({ array : /bb/ })
{ "_id" : ObjectId("5e15e41ea1a009381072696d"), "array" : [ "aaaa", "bbb", "ccc" ], "obj" : { "name" : "bbb" } }
```
* 前方一致
```javascript
db.コレクション名.find(
    { key: /^value/ }
)
```
* 後方一致
```javascript
db.コレクション名.find(
    { key: /value$/ }
)
```

## 登録可能な属性
* string
* double
* int
* long
* decimal
* date
* boolean
* null
* array
* object