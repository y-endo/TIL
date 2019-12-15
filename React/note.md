# stateについて
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

# Propsについて
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

### public class fields syntaxでbindの記載を省略できる
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