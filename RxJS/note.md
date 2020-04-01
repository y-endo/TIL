## RxJS
非同期とイベントのためのObserverパターンを使ったライブラリ。  
イベントで渡ってきたデータを自由に加工することができるので、公式サイトでは「イベント用のLodash」と紹介されている。  

### 入門記事
https://qiita.com/katsunory/items/75651919f6786864c2b5  
https://qiita.com/katsunory/items/6635a243abd7c5fb74ea  


### 基本概念
- Observable: イベントや値をRxJSで受け取れる形にする
- Operators: 受け取ったイベントや値を加工する
- Subject: Observableを同時にいろんなところで受け取れるようにする
- Subscription: subscribeの解除を行う  

### subscribeメソッドの引数
本来はnext, error, completeの連想配列で引数を渡す形になっているが、無名関数を順番に渡すだけでも大丈夫。  
```
observable.subscribe({
  next: x => console.log('got value ' + x),
  error: err => console.error('something wrong occurred: ' + err),
  complete: () => console.log('done')
});
// ↓
observable.subscribe(
  x => console.log('got value ' + x),
  err => console.error('something wrong occurred: ' + err),
  () => console.log('done')
);
```