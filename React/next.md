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