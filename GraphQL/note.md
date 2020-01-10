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
