## MongoDBをbrewでMacにインストール
https://reffect.co.jp/windows/mac-mongodb-install  
設定ファイル  
/usr/local/etc/mongod.conf  
MongoDBのデフォルトのポートは、27017に設定されています。  

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