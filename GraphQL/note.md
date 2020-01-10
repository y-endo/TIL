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