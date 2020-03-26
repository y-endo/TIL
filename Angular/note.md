## Angular
### インストール
angular/CLIを使うのが一般的っぽい。  
```
npm i -g @angular/cli
```
ngコマンドが使えるようになるので、プロジェクトを作る  
```
ng new ProjectName
```
プロジェクトをフォルダ内で以下のコマンドを打つとlocalhost:4200でserveできる。
```
ng serve
```

### ファイル構成
フォルダ内にHTML、CSS、TSをまとめて1つのコンポーネントになっている。  
hoge
　- hoge.component.css
　- hoge.component.html
　- hoge.component.ts


### ダブルカーリーブラケット
{{}}のこと。Vueと一緒。

### *ngFor
AngularのHTMLに対するfor処理。  
```
<p *ngFor="let item of items">{{ item }}</p>
```

### *ngIf
AngularのHTMLに対するif処理
```
<p *ngIf="isEnable">Enable</p>
```

### プロパティバインディング
[プロパティ]="value"  
プロパティバインディングがバインドの対象とするのはプロパティであって、属性ではない。  
なので、属性としては存在するけど、プロパティとして存在しないものは正しく動作しない。  
カスタムデータ属性などはできないということ。
```
<a href="#" [title]="data.title">アンカー</a>
```

### 属性バインディング
属性バインディングは、属性にバインドできる。  
カスタムデータ属性なんかはこちらを使う。  
使い方はプロパティバインディングの接頭語に「attr.」をつける。  
```
<a [attr.data-href]="hoge">アンカー</a>
```

### イベントバインディング
(eventName)="method"  
HTMLタグにこの形で指定する。  
```
<button (click)="handleClick">ボタン</button>
```

### モジュール
複数のコンポーネントをまとめるためのもの。  

### Componentにプロパティデータを渡す方法
ようはReactのpropsみたいなもの。  
@angular/coreのInputを使って、htmlのプロパティバインディングしたデータを取得する。  
静的なデータ以外にも、もちろんオブジェクト(変数)とかも渡せる。  
```
<app-hoge [fuga]="fuga"></app-hoge>

import { Input } from '@angular/core';

export class Hoge {
	@Input() fuga; // 'fuga'
}
```