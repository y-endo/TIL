## Vue + Jestのテスト
この辺のモジュールがテストに必要  
```
"devDependencies": {
  "@vue/test-utils": "^1.0.0-beta.30",
  "babel-jest": "^24.9.0",
  "jest": "^24.9.0",
}
```

test utilsのmount()メソッドを使って、コンポーネントのラッパを作成する。
```
import { mount } from '@vue/test-utils';
import ComponentHoge from './ComponentHoge';

// コンポーネントがマウントされ、ラッパが作成される
const wrapper = mount(ComponentHoge);

// wrapper.vmを介して、実際のVueインスタンスにアクセスできる
const vm = wrapper.vm
```

## ユーザーのインタラクションのシミュレート
wrapper.find('selector')で要素を取得できる。  
要素のメソッド「.trigger('eventName')」でシミュレートできる  
```
const button = wrapper.find('button');
button.trigger('click');
```