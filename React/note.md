## JSXのHTML属性
React DOMの属性はキャメルケースで使用する。  
classはJSの予約後なので、classNameになる。  
```
const element = <h1 className="title">タイトル</h1>
```

## stateについて
Reactでstateを変更する場合は、React.Componentクラスを継承して、setStateメソッドを使う
```
class Hoge extends React.Component {
  constructor() {
    super();
    this.state = { name: 'yuki' };
  }

  render() {
    setTimeout(() => {
      // 直接stateを変更しても効かない
      // this.state.name = 'endo';

      this.setState({ name: "endo" })
    }, 1000);
    return (<div>{ this.state.name }</div>)
  }
}
```

## Propsについて
ReactでPropsを渡す時は属性={値}で渡す  
this.propsでデータにアクセスできる
```
const name = 'Yuki';

<Hoge name={name}>
```
また、コンポーネントを複数作成して、異なるpropsを渡せば、それぞれ異なるpropsを持ったコンポーネントができる  
```
<Hoge name={'Endo'}>
<Hoge name={'Yuki'}>
```

## Propsでメソッドも渡せる
メソッドをpropsで渡すことで、他のコンポーネントから渡したメソッドを実行できる  
```
fugaMethod() {
  console.log('fuga');
}
<Hoge changeText={this.fugaMethod.bind(this)}>

// Hoge
this.props.fugaMethod(); // fuga
```

## public class fields syntaxでbindの記載を省略できる
```
fugaMethod = () => {
  console.log('fuga');
}
<Hoge changeText={this.fugaMethod}>

// Hoge
this.props.fugaMethod(); // fuga
```

## Routerについて
```
import { BrowserRouter as Router, Route } from 'react-router-dom';

ReactDOM.render(
  <Router>
    <Router exact path="/" component={Top}>
  </Router>,
  app
)
```
/ にアクセスした時にTopコンポーネントが表示される。  
/hoge にアクセスした場合もTopコンポーネントが表示されてしまうので、それを防ぐためにexactを追加する。  
exactがあると厳密にパスが合っていないと表示されない。

## フラグメント
React でよくあるパターンの 1 つに、コンポーネントが複数の要素を返すというものがあります。フラグメント (fragment) を使うことで、DOM に余分なノードを追加することなく子要素をまとめることができるようになります。
```
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}

// 短い記法
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  );
}
```

## constructorでsuper(props)をやる理由
https://qiita.com/hand-dot/items/61a4b808f110b12e4281  
super()だけでもthis.propsが使えるけど、super(props)って書いたほうが良さそう

## ライフサイクルの詳しい解説
qiitaの良記事  
https://qiita.com/Julia0709/items/3c3fc8d29fd2e56ed7a9

## フェッチやタイマーをセットするタイミング
componentDidMount()で行う。  
単純に静的なデータを書き換えを行うと、ムダに2回レンダリングされる。  
直接のDOM操作は原則避ける

## アニメーションやタイマーを削除するタイミング
componentWillUnmount()で削除する。  
コンポーネントを破棄する直前に呼ばれるメソッド。

## データフローに影響しないデータについて
this.props は React 自体によって設定され、また this.state は特別な意味を持っていますが、何かデータフローに影響しないデータ（タイマー ID のようなもの）を保存したい場合に、追加のフィールドを手動でクラスに追加することは自由です。

## コンポーネントのrenderで何も表示させたくない場合
nullを返す。  
nullを返してもコンポーネントのライフサイクルに影響しない。  

## ループのkeyについて
keyにindexを設定するのは最終手段であり、とくに並び替えが発生する場合はユニークなidを用意するべき。  
keyにindexを設定した場合の悪影響について  
https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318

## フォームの本格的なソリューション
https://jaredpalmer.com/formik  
入力値のバリデーション、訪問済みフィールドの追跡やフォーム送信を含む完全なソリューションをお探しの場合は、Formik が人気のある選択肢のひとつです。

## コンポーネントの子要素
props.childrenで取得できる。  
```
<HogeComponent>
  <p>hogehoge</p>
</HogeComponent>

// Hoge
this.props.children
// ↓
<p>hogehoge</p>
```

## propsにコンポーネントを直接渡すこともできる
```
<Hoge child={<Fuga />} />
```

## Router v4でルーティング先にpropsを渡す方法
http://ngzm.hateblo.jp/entry/2017/06/23/001352  
```
<Router>
  <Route path="/" render={() => <Hoge hoge={'fuga'} />}>
</Router>
```

## DOMに直接アクセスする方法
Vueのref的なのがReactにもある。  
https://the2g.com/2796  
```
// constructorの中で定義
this.input = React.createRef();

<input type="text" ref={this.input}>
```

## hookとは
https://ja.reactjs.org/docs/hooks-intro.html

ファンクショナルコンポーネントでstateを使えるようにするもの。
```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}

// こう書いていたものが、↓のようにかける

import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

## useEffectとは
componentDidMount と componentDidUpdate と componentWillUnmount がまとまったもの。