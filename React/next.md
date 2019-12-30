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