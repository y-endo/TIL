## Next.js
日本語ドキュメント  
https://nextjs-docs-ja.netlify.com/docs  

使い方  
```
yarn add next react react-dom
```

- (webpack と babel を利用した) 自動的なトランスパイルとビルド
- コードのホットリローディング
- ./pages 配下のファイルの構造化とサーバーサイドレンダリング
- 静的ファイルの配布./public/ は / にマッピングされます (プロジェクトに ./static/ ディレクトリを作成する必要があります。)

## cssに関して
https://qiita.com/tetsutaroendo/items/8e3351bc4bfbb419f662  
Next.js的にはstyled-jsxが推奨？  
ちょっと嫌だな… なんかキモい  

## dynamic routing
[hoge]で命名したファイルorディレクトリを作成すると反応する。  
```
/hoge/:fuga
hoge [fuga]/index.js
or
hoge/[fuga].js
```

## Nextのチュートリアルで出てきたfetchのポリフィル？
isomorphic-unfetch  
https://github.com/developit/unfetch/tree/master/packages/isomorphic-unfetch  
クライアント・サーバーどちらでも使えるfetchっぽい。  

## getInitialProps
Next.jsに用意された機能で、propsの初期値をここで入れられる。  

## タイトルの変更
https://stackoverflow.com/questions/52170634/how-to-set-documents-title-per-page  

## styled-jsxでsassを使う  
styled-jsx-plugin-sass  
https://github.com/giuseppeg/styled-jsx-plugin-sass  

babel.config.jsの追記例  
https://github.com/zeit/styled-jsx/issues/569  

## getInitialPropsでuseRouterを使いたい。
普通に実装するとエラーがでる。  
https://stackoverflow.com/questions/58407074/userouter-not-working-inside-getinitialprops  
getInitialPropsはサーバー側で動いているから当然。consoleもターミナル側に表示される。

## dynamic routingとLink組み合わせ
```
/task/[id].tsx

// これでも動作するけどハードリフレッシュになる
<Link href="/task/1">
<a>~~~</a>
</Link>

// こうする
<Link href="/task/[id]" as="/task/1">
<a>~~~</a>
</Link>
```

## APIルートにGraphQL
https://qiita.com/NanimonoDaemon/items/a0ed3d3b8a93b306c88c

## mongoose（MongoDB）がhot reloadでバグるときの対策 
https://www.hoangvvo.com/blog/migrate-from-express-js-to-next-js-api-routes/  
既にコンパイルされたmongoose.modelを上書きできない。  
```
const Usert = mongoose.models.User || mongoose.model('User', UserSchema);
```

## ディレクトリ構成 参考
https://sergiodxa.com/articles/next-file-structure/  
Next.js開発者のZeitがこうしてる？  

## APIルートでSubscription
無理っぽい。  
別途サーバーを用意しないとできない？多分。  
queryのキャッシュを無効にして、更新する度にquery発行で凌ぐ

## APIルートでfsをつかう
__dirnameが正常に動かないので、fs.readFileがうまくいかない。  
https://github.com/zeit/next.js/issues/8251  
next.config.jsにプロジェクトのルートパスをもたせる。