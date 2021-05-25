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

## Hooks APIの良記事
https://programmagick.com/blogs/react_hooks_api  
https://sbfl.net/blog/2019/11/12/react-hooks-introduction/  

## React Routerのform(submit)対応
https://gist.github.com/elitan/5e4cab413dc201e0598ee05287ee4338  

## Functinal Componentから呼ぶ関数はどこで定義するべきか
https://stackoverflow.com/questions/46138145/functions-in-stateless-components  
コンポーネントの中に入れてしまうと、レンダリングする度に関数が再定義されるので、そとで定義するべき。  
もしくはuseCallbackを使う。  

## Functinal Componentのインスタンス変数
再レンダリングで保持したい変数（stateじゃない）はuseRefを使う。  
https://ja.reactjs.org/docs/hooks-faq.html#is-there-something-like-instance-variables  
> はい！ useRef() フックは DOM への参照を保持するためだけにあるのではありません。“ref” オブジェクトは汎用のコンテナであり、その current プロパティの値は変更可能かつどのような値でも保持することができますので、クラスのインスタンス変数と同様に利用できます。

ただし、TypeScriptで書く場合currentがreadOnlyになる場合がある。  
https://stackoverflow.com/questions/58017215/what-typescript-type-do-i-use-with-useref-hook-when-setting-current-manually  
```
const hoge = useRef<HTMLElement>(null); // ← hoge.current = read-only
const hoge = useRef<HTMLElement | null>(null); // OK
```

### アニメーション
#### react-transition-group
↓参考記事
https://qiita.com/koedamon/items/2665ea80f19589aa2f7d  

主に { Transition, CSSTransition, TransitionGroup } がある。  
Transitionはインラインスタイルで、コールバックとかもできる。  
CSSTransitionは各フェーズごとにクラスを付与する感じで、Vueのtransitionと同じような感じ。  
TransitionGroupはTransitionまたはCSSTransitionのリストを管理する為のコンポーネント  
コンポーネントのマウント、アンマウント時にenter、exitのスタイルが適用される。  
```
import { Transition, CSSTransition } from 'react-transition-group';

const style = {
  entering: {
    transition: 'opacity 0.5s ease',
    opacity: 1
  },
  entered: {
    transition: '',
    opacity: 1
  },
  exiting: {
    transition: 'opacity 0.5s ease',
    opacity: 0
  },
  exited: {
    transition: '',
    opacity: 0
  }
};
const ExampleTransition = () => {
  const [show, setShow] = useState(true);

  const onClick = () => {
    setShow(!show);
  };

  return (
    <div>
      <Transition in={show} timeout={500}>
        {state => <p style={style[state]}>ExampleTransition</p>}
      </Transition>
      <button onClick={onClick}>On / Off</button>
    </div>
  );
};

const ExampleCSSTransition = () => {
  const [show, setShow] = useState(true);

  const onClick = () => {
    setShow(!show);
  };

  return (
    <div>
      <CSSTransition in={show} classNames={'fade'} timeout={500}>
        <p>ExampleCSSTransition</p>
      </CSSTransition>
      <button onClick={onClick}>On / Off</button>
    </div>
  );
};
```

#### react-spring
↓ 参考記事  
https://qiita.com/uehaj/items/260f188851045cc091ac  
従来のアニメーションライブラリとはちょっと違い、ユニークなアニメーション設定ができる。  
> 従来のアニメーションライブラリだと、アニメーションのタイミングや移動速度などは、継続時間とベジエ曲線(イージング関数)で指定するのが普通でした。これに対してreact-springでは慣性、摩擦力、張力をもった物理的な性質(物性)でタイミングを指定

```
import { useSpring, animated } from 'react-spring';

const Example = () => {
  const [show, setShow] = useState(true);
  const spring = useSpring({
    opacity: show ? 1 : 0
  });

  return (
    <div>
      <animated.div style={spring}>Example</animated.div>
      <button
        onClick={() => {
          setShow(!show);
        }}
      >
        On / Off
      </button>
    </div>
  );
};
```

**単一コンポーネントにuseTransitionを使う マウント/アンマウントでアニメーション**  
https://alligator.io/react/advanced-react-spring/  
```
import { useTransition, animated } from 'react-spring';

const Index: React.FC = () => {
  const [isModalOpen, setIsModalOpen] = React.useState(false);
  const transition = useTransition(isModalOpen, null, {
    from: { opacity: 0 },
    enter: { opacity: 1 },
    leave: { opacity: 0 }
  });
  
  return <>
          {transition.map(
            ({ item, key, props }) =>
              item && (
                <animated.div style={props} key={key}>
                  <Modal />
                </animated.div>
              )
          )}
        </>;
};
```

#### react-transition-groupとstyled-jsxのcssmoduleを組み合わせる方法
```
import transition from "~/styles/components/CSSTransition.scss";
import { CSSTransition } from "react-transition-group";

<CSSTransition
  key={todo.id}
  classNames={{
    enter: transition["fade-enter"],
    enterActive: transition["fade-enter-active"],
    enterDone: transition["fade-enter-done"],
    exit: transition["fade-exit"],
    exitActive: transition["fade-exit-active"],
    exitDone: transition["fade-exit-done"]
  }}
  timeout={500}
>
  <ToDoListItem todo={todo} />
</CSSTransition>;
```

### プロファイラ
アプリケーションのレンダー頻度やコストを計測できる。  
メモ化などの最適化が有効な可能性のある部位を特定する手助け。  
https://gist.github.com/bvaughn/60a883af01716a03a1b3285a1029be0c  
https://ja.reactjs.org/docs/profiler.html  

### 再レンダリングの検出
why-did-you-renderを使うと、再レンダリングを検出できる。  
https://github.com/welldone-software/why-did-you-render  
classベースのコンポーネントならwhy-did-you-updateの方が良さげ  
ちなみに、useStateやuseRefを使っている場合は最初の初期化処理でコンポーネントを再実行している。（再レンダリングではない

### useCallbackとReact.memo + useMemo
https://qiita.com/Climber22/items/2c6103b4e1ef7a1f2f7c  
https://qiita.com/k_7016/items/d1e6a5eb934aaf667739  
**userCallback**
第二引数が変わらない限り、再レンダリングで関数の再定義を防ぐ。  
子コンポーネントに関数を渡していて、再レンダリングで関数が再定義されると、内容に変化がないのにムダに子コンポーネントを再レンダリングしてしまうの防ぐために使う。  
その為、子コンポーネントに渡さない関数をuseCallbackでラップする必要はない。  

**React.memo**
第一引数にコンポーネントを返す関数を渡し、第二引数にPropsが同一であるかを判断する関数を渡します。  
第二引数がtrueを返すばあい再レンダリングをしない。  
第二引数に何も渡さなかった場合はPureComponent(shallowEqual)と同等の比較  

**useMemo**
重い処理などをメモ化するために使う。  
コストの高い処理を再レンダリング時に毎回行うと重くなってしまうので、何度実行しても結果が変わらない場合はメモ化する。  
第二引数に渡した値が変更されない限り実行されない。  


### コード分割
https://ja.reactjs.org/docs/code-splitting.html  
dynamic importに対応しているし、React.lazyを使えばもっと簡単にコンポーネントの遅延読み込みができる。  
ただし、React.lazyはSSRには使用できない。  

#### React.lazy
```
import OtherComponent from './OtherComponent';
↓
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

#### Suspense
遅延コンポーネントは、Suspenseコンポーネント内でレンダーされる必要がある。  
これによって、遅延コンポーネントのローディング待機中にフォールバック用のコンテンツを表示できる。  
```
<div>
	<Suspense fallback={<div>Loading...</div>}>
		<OtherComponent />
	</Suspense>
</div>
```

### @apollo/react-hooks
#### useQueryのrefetch
useQueryを再レンダリングとかではなく、もう一度データを取得させたい場合はrefetchを使う。  
```
import { useQuery } from '@apollo/react-hooks';

const { refetch } = useQuery();

refetch();
```

なぜかは分かっていないが、useQueryを使ったページから別のページに遷移して、refetchを行うと、strictmodeの場合警告が出る。（アンマウントしたコンポーネントに対するステート変更）  
useLazyQueryに置き換えると治ったので、一旦置き換えで対処する。  

### CSSModule
#### scssのimportについて
変数やmixinはimportを行ったscssをcssmoduleとして読み込んだときに扱える。  
また、他のscssで定義したクラスをimportすると、それも扱える。  
```
// elements.scss
.button {}

// componentA.scss
import 'elements.scss';

// componentA.jsx
import css from 'componentA.scss';

<button className={css['button']}></button>
```

### clsx
Reactで動的にクラス名を設定する場合に便利なライブラリ。  
https://qiita.com/taqm/items/c38855d8158cdd9d5a3e  
classnamesというのが主流だったが、こちらが上位互換らしくmaterial-uiでも使われている。  
classnames: https://www.yoheim.net/blog.php?q=20180701  