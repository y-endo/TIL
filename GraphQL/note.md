## GraphQLの入門記事
https://tatsuyashi.hatenablog.com/entry/2019/01/15/005225  
https://qiita.com/zonomaa/items/5de4b14dcd839db5f148  
https://cloudnweb.dev/2019/06/graphql-with-apollo-server-and-express-graphql-series-part-1/  
https://employment.en-japan.com/engineerhub/entry/2018/12/26/103000  

## APIの種類

- Query（データ取得）
- Mutation（データ更新）
- Subscription（変更検知）

これらはGraphQLスキーマの予約後になっている。  
```
type Query{}
type Mutaion{}
type Subscription{}
```

## ざっくりとした自分の中での理解
apollo-server-expressを使ってる。  

- スキーマの定義
	- typeDefsで型を定義する。定義した型は上記のQueryとかで使う
- リゾルバを定義
	- スキーマで定義したQueryとかに対応するリゾルバをresolversに定義する。

```
const users = [
	{ id: 1, name: 'Hoge' },
	{ id: 2, name: 'fuga' }
];

const typeDefs = gql`
	type User {
		id: Int!
		name: String!
	}
	
	type Query {
		users: [User]
	}
`;

const resolvers = {
	Query: {
		users: () => users
	}
};
```

## query
データを取得するqueryの発行  
取得するデータのフィールドを空にはできない。  
```
// bad
query {
	data
}

// good
query {
	data {
		hoge
		fuga
	}
}
```

## fragment
冗長な繰り返しを避けるために、プロパティをまとめる  
```
fragment allProps on prop {
	id
	name
	age
}

query {
	users {
		allProps
	}
}
```
Apollo Clientのfragmentの使い方  
https://www.apollographql.com/docs/react/data/fragments/  

## 入力値バリデーション
更新する際に、バリデーションを事前に定義することが可能です。$文字列の後に続くものが変数となり、:の後に型と必須チェックを記載しています。  
例えば、文字列型で必須にしたい場合は$str:String!、数値型で必須ではない場合は$num:Intとなります。  
```
mutation setStatus($id:ID! $status:LiftStatus!) {
  setLiftStatus(id:$id status: $status) {
    id
    name
    status
  }
}
```

Apollo Subscriptionの参考記事  
https://lambda4.fun/graphql/apollo-express/subscription/  

```
const express = require('express');
const { ApolloServer, PubSub } = require('apollo-server-express');
const http = require('http');
const typeDefs = require('./typedefs');
const resolvers = require('./resolvers');

const app = express();
const port = 3030;

const server = new ApolloServer({
  typeDefs,
  resolvers,
  context: { pubsub: new PubSub() }
});
server.applyMiddleware({ app, path: '/graphql' });

const httpServer = http.createServer(app);
server.installSubscriptionHandlers(httpServer);

httpServer.listen({ port }, () => {
  console.log(`Server ready at http://localhost:${port}${server.graphqlPath}`);
});

---

Mutation: {
  addUser: async (_, args, context) => {
    const user = new UserModel({ ...args });
    await user.save();
    context.pubsub.publish('userAdded', {
      userAdded: {
        id: user.id,
        name: user.name,
        age: user.age
      }
    });
    return user;
  }
},
Subscription: {
  userAdded: {
    subscribe: (_, __, context) => context.pubsub.asyncIterator('userAdded')
  }
}
```

## Typeのextend
https://www.apollographql.com/docs/graphql-tools/generate-schema/#extending-types  
```
type Hoge {
	hoge: String
}
extend type Hoge {
	fuga: String
}
```

## 引数の型をObject（定義した型）にしたい
typeで宣言したものを、引数の型に指定するとエラーになる。  
inputで宣言する。  
```
type Hoge {}
input HogeInput {}

Mutation {
	hoge(hoge: Hoge) {} # エラー
	hoge(hoge: HogeInput) {} # OK
}
```

## Apollo Client + React 入門
https://www.apollographql.com/docs/react/  
https://qiita.com/seya/items/e1d8e77352239c4c4897  

- apollo-boost
	- その名の通り「とりあえずApolloで動かしたいんや！！」というあなたをブーストしてくれるパッケージです。
	- apollo-cache-inmemory や apollo-link-http などApollo Clientを使う際に基本使うことになるであろうパッケージたちが一緒にインストールされます。詳しい中身を知りたい方はREADMEをどうぞ。
- react-apollo
	- ReactとGraphQLクエリの繋ぎこみをサポートしてくれるライブラリです。
- graphql-tag
	- GraphQLクエリをテンプレートリテラルで書いたものをパースしてくれる便利なライブラリです。
- graphql
	- graphql-tagのpeerDependencyとして必要です。他にも色々機能あるパッケージなんですが、今回入れる目的はそれ。

@apollo/clientだけで充分じゃない？  
公式チュートリアルをやった感じだとreact-apolloとかgraphql-tagは必要なくて、@apollo/clientに全て含まれてそう。

## apollo-boost はサンプルコード専用と思ったほうがよい
https://gfx.hatenablog.com/entry/2018/10/30/112054  
apollo-boostはサンプルとかで使って、プロダクションではapollo-clientおよび関連モジュールを直接使ったほうがよい。  

## graphql-codegen
TypeScriptの型定義ファイルを、スキーマから自動生成する。  
https://graphql-code-generator.com/  
https://qiita.com/mizchi/items/fb9f598cea94d2c8072d  