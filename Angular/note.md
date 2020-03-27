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
#### *ngForディレクティブで利用できる特殊変数
- index: 要素のインデックス
- first: 最初の要素か
- last: 最後の要素か
- even: インデックスが偶数か
- odd: インデックスが奇数か
```
// Idの中にindexが入る。
<li *ngFor="let item of items; index as Id"></li>
<li *ngFor="let item of items; let Id = index;"></li>
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

### Componentにプロパティデータを渡す方法(親コンポーネントから子コンポーネントに値を渡す)
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

### ComponentからEventを受け取る方法(子コンポーネントから親コンポーネントに値を渡す)
https://blog.officekoma.co.jp/2019/05/angulareventemitteroutputemit.html  
@angular/coreのOutputとEventEmitterを使い、メソッドを通じて渡す。  
```
<app-alert (notify)="handleNotify()"></app-alert>

export class AlertComponent implements OnInit {
  @Output() notify = new EventEmitter();
 }
 <button (click)="notify.emit()">Notify</button>
```

### Routingの使い方
ルーティングはRouterModule.forRootの中に定義する。  
```
// app.module.ts
@NgModule({
	imports: [
		RouterModule.forRoot([
			{ path: '', component: RootComponent },
			{ path: 'login', component: LoginComponent },
			{ path: 'product/:productId', component: ProductDetail }
		])
	]
})
export class AppModule {}

<a [routerLink]="['/detail']">Login</a>
<a [routerLink]="['/product/1']">Product</a>
```
#### URLからパラメータを取得する方法
@angular/routerの ActivatedRoute を使う。  
```
import { ActivatedRoute } from '@angular/router';

constructor(private route: ActivatedRoute) { }

ngOnInit() {
  this.route.paramMap.subscribe(params => {
    console.log(params);
  })
}
```