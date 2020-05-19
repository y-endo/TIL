## CSS
### CSSカスタムプロパティ(CSS変数)
CSSで使える変数。IE以外のモダンブラウザで使える。  
変数を定義するには--から始まる変数名を宣言する。  
```
--text-color: #ccc;
```
変数を呼び出すにはvar()関数を使う。  
```
p {
  color: var(--text-color);
}
```
CSSカスタムプロパティが使えない場合のフォールバック  
CSSカスタムプロパティが宣言されていなかったり無効になっている場合に代わりの値を適用させる。  
```
p {
  color: var(--text-color, #333);
}
```
基本的にはグローバルに変数を定義して使う事が多いらしい。  
グローバル変数として定義するには:rootの中で宣言する。  
```
:root {
  --text-color: #ccc;
}
```
別の要素で変数を上書きされた場合、ローカルスコープで新しく定義された値が適用される。  
```
:root {
  --text-color: #ccc;
}
p {
  color: var(--text-color);
}
.blue {
  --text-color: blue;
}

<p>テキストは#333</p>
<p class="blue">テキストはblue</p>
```
#### IE11などレガシーブラウザでも使う場合の対策
@supporsブロックでクロスブラウザ対応を行う。  
```
section {
  color: blue;
}
@supports(--css: variables) {
  section {
    --my-color: blue;
    color: var(--my-color, 'blue');
  }
}
```