## MongoDBをbrewでMacにインストール
https://reffect.co.jp/windows/mac-mongodb-install  
設定ファイル  
/usr/local/etc/mongod.conf  
MongoDBのデフォルトのポートは、27017に設定されています。  

## コマンド
```
mongo
show dbs
db
db.createCollection('collectionName')

db.colleciton.insert()

db.collection.find()
db.collection.find(query)

第2引数で取得結果を絞れる、0 or 1 で取得するフィールドを制御
db.collection.find({}, {_id: 0, name: 1})

db.collection.update({検索条件}, {更新内容})
e.g. db.collection.update({ name: 'AAA' }, { age: 30 })
注意）updateはごそっと上書きなので、上記の例だと { age: 30 } だけになる。
特定のフィールドだけ更新したい場合は、$set修飾子を使う
db.collection.update({ name: 'AAA' }, { $set: { age: 30 } })

db.collection.remove()
db.collection.remove(query)
```

## NodeとMongoDBをつなげるやつ
https://www.npmjs.com/package/mongodb  
```
const mongodb = require('mongodb');
const MongoClient = mongodb.MongoClient;

MongoClient.connect('mongodb://localhost:27017', (error, client) => {
  if (error) throw error;
  console.log('Connected successfully to server');

  const database = client.db('databaseName');
  database
    .collection('collectionName')
    .find(filter)
    .toArray((error, response) => {
      if (error) throw error;
      
      console.log(response);

      client.close();
    });
});
```

## MongoDBでasync/await
https://qiita.com/junjis0203/items/3fd47eee226acc193a1c  
https://qiita.com/sl2/items/681d40e4ba018ea8b808  

## MongoDBのGUI（管理ツール）
Compass（公式）  
https://www.mongodb.com/download-center/compass  

## GraphQLとMongoDBをつなげる
mongoose  
https://mongoosejs.com/

mongooseのSchemaにコレクションを指定する。  
https://stackoverflow.com/questions/7486528/mongoose-force-collection-name  
```
new mongoose.Schema({}, { collection: 'collectionName' });
```

mongooseでの保存操作
```
const hoge = new HogeModel({ name: 'fuga' });
hoge.save()

// 大量のドキュメントを挿入する場合
HogeModel.insertMany([{ name: 'fuga' }, { name: 'hoge' }]);
```

mongoose経由でデータを追加すると、versionKeyが追加される。  
フィールド名を変えることができて、デフォルトは__v  
https://intellipaat.com/community/27868/what-is-the-v-field-in-mongoose