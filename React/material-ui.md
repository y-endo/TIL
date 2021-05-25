## Material-ui
https://material-ui.com/ja/

Reactで使えるUIパッケージ。  
マークアップをできる人がCSSフレームワークを使うのと同じ感覚で、Reactの基礎知識がある人ならスグに使えるようになると思う。  
便利なコンポーネントがたくさんあるので、配置するだけでちゃんとした画面が作れる。  
デザインを作り込むものでは使えないかもしれないが、お客さん向けに納品する管理画面とかで使うのはいいかも。

### フォントについて
Material-UIはRobotoフォントをベースに設計されているが、フォントの自動読み込みが組み込まれていないので、開発者がGoogleWebFonts等で読み込み処理を追加する必要がある。  
公式に載っているCDN読み込みタグ  
```
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap" />
```
npmからインストールしてエントリポイントでインポートする方法でもOK。  
バンドルサイズが大きくなるので注意。  
```
npm install fontsource-roboto
```
Material-UIが依存しているフォントウェイトは300、400、500、700のみ。それ以外を読み込んでも無駄。  

### CssBaseline
normalize.css的なのが用意されている。  
https://material-ui.com/ja/components/css-baseline/  
```
import CssBaseline from '@material-ui/core/CssBaseline';

export default function MyApp() {
  return (
    <React.Fragment>
      <CssBaseline />
      {/* The rest of your application */}
    </React.Fragment>
  );
}
```
CssBaseLineを既存の要素とかに影響させたくない場合、子要素にのみスコープを当てるScopedCssBaseLineもある。  
```
import React from 'react';
import ScopedCssBaseline from '@material-ui/core/ScopedCssBaseline';
import MyApp from './MyApp';

export default function MyApp() {
  return (
    <ScopedCssBaseline>
      {/* The rest of your application */}
      <MyApp />
    </ScopedCssBaseline>
  );
}
```

### createStyles
createStylesはTypeScriptのときのみ任意で使うもので、型拡大を防ぐ関数。  
makeStylesとセットで使う。  
```
const useStyles = makeStyles((theme: Theme) => {
	createStyles({...})
})
```
これがないと何が困るのか、どのような問題があるのかまでは調べてない。  
取り敢えず入れておいた方が無難そう。  
> createStylesは、単なるidentity関数です。実行時に「何でもする」するのではなく、コンパイル時に型推論をガイドするのに役立つだけです。


### TypeScriptで管理画面作成
https://dev.classmethod.jp/articles/react-material-ui/