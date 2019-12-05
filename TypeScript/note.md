# 変数
型定義でorが使える
複数の状態を取る型をUnion Type（ユニオン型）と呼ぶ
```
let birthYear: number | string;
birthYear = 1999; // ok
birthYear = '平成'; // ok
birthYear = null; // error
```

型だけじゃなく、値を限定する事もできる
```
let food: 'ラーメン' | 'おにぎり';
food = 'ラーメン'; // ok
food = 'ドーナッツ'; // error

// 数値もいける
type rate = 0 | 5 | 10;
let point: rate = 0; // ok
let point: rate = 12; // error
```

型と値を組み合わせることもできる
```
let birthYear: number | '昭和' | '平成';
birthYear = '昭和';
```

# プリミティブ型
- boolean 
- number
- string
- 配列
- オブジェクト
- 関数
- undefined
- null

このうち、それ以上分解できないシンプルなデータを「プリミティブ型」と呼びます。

## undefined と null
無効な値をとる可能性がある場合は、ユニオン型でnullを許可する。
```
let string: string | null = null;
```
tsconfigのcompilerOptions.strict: trueもしくはcompilerOptions.strictNullChecks: trueの場合は、null型の変数にundefinedを入れられない。

# 複合型
他のプリミティブ型、もしくは複合型自身を内部に含み、大きなデータを定義できるデータ型のこと。  
配列やオブジェクトなどがこれにあたる。  

## タプル
配列の要素ごとに型が違う「タプル型」。あんまし使わない
```
const movie: [string, number] = ['Gozilla', 1954];
movie[0] = 2019; // error
```

# 基本的な型付け
## 型に名前をつける
```
type Coin = 1 | 5 | 10 | 50 | 100 | 500;
type FoodMenu = '北極' | '冷やし味噌';

const coin: Coin = 500;

function orderFood(food: FoodMenu) {
}
```

typeでオブジェクトの属性の型定義もできる
```
type Person = {
  name: string;
  favoriteBank: string;
  favoriteFood?: string;
}

const person: Person = {
  name: 'Yuki',
  favoriteBank: 'Mizuho'
};
```

## 属性名が可変のオブジェクト
```
const postalCodes: { [key: string]: string } = {
  '602-0000': '京都市上京区',
  '602-0827': '京都市上京区相生町'
};
```

## AかつBでなければならない
A | B は「AもしくはB」のor。  
A & B で「AかつB」。  
型を合成する
```
type Twitter = {
  twitterId: string;
}

type Instagram = {
  instagramId: string;
}

// 両方のオブジェクトの属性がないとエラーになる
const yukiSNS: Twitter & Instagram = {
  twitterId: 'yuki',
  instagramId: 'yuki'
}

// 合成した方に名前をつけられる
type SNSAccount = Twitter & Instagram;
```

## 型ガード
ユニオン型の型を1つに絞る  
```
let userId: string | number = getUser();

if (typeof userId === 'string') {
  // このif文の中では、userIdは文字列型として扱われる
  userId.toUpperCase() // ok
} else {
  // このif文の中では、userIdは数値型として扱われる
}
```
型ガードで使えるのは、typeofやinstanceof、inや比較

## ユーザー定義の型ガード
Array.isArray()などのクラスメソッドは、型ガードには使えないが、ユーザー定義の型ガード関数を作れば使える。  
型ガードの関数を作成。
```
// arg is Array は 引数名 is 型 であり、実態は真偽値である。返り値がtrueの場合、引数の型を保証する
function isArray(arg: any): arg is Array {
  return Array.isArray(arg);
}
```

## 型アサーション
キャスト（asを後置）。  
実行分ではなく、あくまでもコンパイラの持つ型情報を上書きするもの。  
型ガードと異なり、実行時に情報を一切参照せずに変数の型だけが変わる。  
numberからstringなどの無理矢理のキャストはエラーになる。
```
const page: any = { name: 'profile' };
// any型からはasでどんな型にも変換できる
const name: string = page as string;
```

## keyof と Mapped Type: オブジェクトのキーの文字列のみを許容する動的な型宣言
```
type Park = {
  name: string;
  hasTako: boolean;
}

// Parkのキー（'name' | 'hasTako'）が割り当てられる
type Key = keyof Park;
const key: Key = 'name'; // ok
const key: Key = 'hoge'; // ng
// 1行でも書ける
const key: keyof Park = 'hasTako';
```

## インターフェースを使った型定義
オブジェクトの型をつける方法は、typeを使う以外に、インターフェース定義を使う方法もある。
```
type Person {
  name: string;
  age: number;
}

interface Person {
  name: string;
  age: number;
}

// インターフェースの場合、型合成は継承で表現できる
interface SNSAccount extends Twitter, Instagram{
}
```

# InterfaceとTypeの比較の重要ポイント
同名のInterfaceは全てマージされる。これを利用してInterfaceを拡張できる。
```
interface IPoint2D {
  x: number;
  y: number;
}
interface IPoint2D {
  name: string;
}
const ok: IPoint2D = {x: 1, y: 1, name: 'p1'}; // OK
const ng: IPoint2D = {x: 1, y: 1}; // コンパイルエラー
```
一方で、Typeは複数宣言すると単純にエラーになる。
```
type TPoint2D = {
  x: number;
  y: number;
}
type TPoint2D = { // コンパイルエラー
  name: string;
}
```
これは結構な違いで、Interfaceを使っていないライブラリにちょっと追加したいときに苦労するかもしれない。基本的になるべくInterfaceを使えというのはこれが理由だと思う。

# 関数
## 関数の返り値の型定義
compilerOptions.noImplicitAny オプションが true の場合には : void を書かないとエラーになりますので、忘れずに書くようにしましょう。

## デフォルト引数
```
function f(name='小動物', favorite='小豆餅') {
}

// 分割代入を使えば、デフォルト値つきのオブジェクト引数も楽
function f({ name='小動物', favorite='小豆餅' } = {}) {
}
```

## 関数を含むオブジェクトの定義方法
クラスを作るまでもない場合は、オブジェクトを作って関数をメンバーとしていれることがある。  
それがES2015以降は簡単に書ける。  
setter / getterの宣言も簡単に行えるようになった。
```
// 旧: オブジェクトの関数
var smallAnimal = {
   getName: function() {
     return "小動物";
   }
};
// 旧: setter/getter追加
Object.defineProperty(smallAnimal, "favorite", {
  get: function() {
    return this._favorite;
  },
  set: function(favorite) {
    this._favorite = favorite;
  }
});

// 新: オブジェクトの関数
//     functionを省略
//     setter/getterも簡単に
const smallAnimal = {
  getName() {
    return "小動物"
  },
  _favorite: "小笠原",
  get favorite() {
    return this._favorite;
  },
  set favorite(favorite) {
    this._favorite = favorite;
  }
};
```

# ジェネリクス
使われるまで型がきまらないような色々な型の値を受け入れられる。  
<T>のTはTypeの頭文字  

# 全イベント名がまとまった辞書
https://microsoft.github.io/PowerBI-JavaScript/interfaces/_node_modules_typedoc_node_modules_typescript_lib_lib_dom_d_.globaleventhandlerseventmap.html#click

https://microsoft.github.io/PowerBI-JavaScript/modules/_node_modules_typedoc_node_modules_typescript_lib_lib_dom_d_.html
このページでDOM系の型を確認できる、ヒエラルキーとか見ると分かりやすい
Event
 UIEvent
  MouseEvent
  TouchEvent
  〜〜〜

# Function.bind(this)は非推奨ってはなし
https://qiita.com/taqm/items/914a2b179e5676a09d35
```
class MyClass {
  private run = () => {
    alert(this);
  }
}
```

## デコレーター
公式の翻訳版  
https://mae.chab.in/archives/59845  
