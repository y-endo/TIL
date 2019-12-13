## Jestの入門良記事
https://www.wakuwakubank.com/posts/525-javascript-jest/

## Jestインストール
```
yarn add -D jest
```

## Jestとは？
Facebook製のJavaScriptテストツール  
Reactと相性が良い。React専用という訳ではない  

## ドキュメント
https://jestjs.io/docs/ja/getting-started.html

## テストファイル
*.test.js、*.spec.js もしくは __tests__ ディレクトリ以下のファイルをテストファイルとみなす。  
testRegexオプションで変更できる。  

## test()関数
テストをする場合、test()関数を実行する。  
第一引数にテストの概要、第二引数にテスト内容を関数として記述する。  
テスト関数の中で、expect()関数を使ってテストする。  
expect()の返り値に対してマッチャーを使って値をチェックする。  
```
test('テストの概要をここに書く', () => {
  expect(hello('test')).toBe('Hello test');
})
```
test関数は、it関数でも同じ動作をする

## describe(name, fn)関数
describeは複数の関連したテストを1つの「テストスイート」としてグループ化したブロックを作成する。  
```
const myBeverage = {
  delicious: true,
  sour: false,
};

describe('my beverage', () => {
  test('is delicious', () => {
    expect(myBeverage.delicious).toBeTruthy();
  });

  test('is not sour', () => {
    expect(myBeverage.sour).toBeFalsy();
  });
});
```

## マッチャ―の種類
https://jestjs.io/docs/ja/expect.html#content  

よく使うマッチャ―  
```
// 等しい
expect().toBe()

// not（〜でない）
expect().not.toBe()

// toBeは厳密な等価性を判定するので、オブジェクトの場合はtoEqualを使う
expect({ a: 100 }).toEqual({ a: 100 })

// 大きい
expect(4).toBeGreaterThan(3)
expect(4).toBeLessThanOrEqual(4)

// null, true, false
expect(null).toBeNull()
expect(true).toBeTruthy()
expect(false).toByFalsy()

// 文字列
expect('hoge').toMatch(/ge/)
expect('hoge').not.toMatch(/ao/)

// 配列
expect([1, 2, 3]).toContain(2)

// 例外
const fuga = () => {
  throw new Error('error message fuga');
}

expect(fuga).toThrow('error message xxx');
```

## jest-dom
DOMの状態をテストするカスタムJestマッチャ―  
https://github.com/testing-library/jest-dom

## Vueの公式テストライブラリ
vue-test-utils  
https://vue-test-utils.vuejs.org/ja/