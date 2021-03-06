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
いまいるディレクトリでプロジェクトを作りたい場合  
```
ng new ProjectName --directory ./
```
テストファイルの生成をスキップしたい場合
```
ng new ProjectName --skip-tests
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
app-routing.module.tsでRouterModuleをエクスポートし、アプリ全体で使えるようにする。  
テンプレートファイルにrouter-outletタグを追加することで、ルーティングされたビューを表示する箇所をルーターに教える。  
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

// URLによって表示されるコンポーネントが変わる
<router-outlet></router-outlet>
```
RouterModuleの forRoot はルートでした使えない。(app-routing.module.ts)  
他のルーティングで使う場合は、RouterModule.forChild を使う。  

#### リダイレクト設定
pathに訪問した際、redirectToにリダイレクトさせる。  
空のpathからリダイレクトさせる場合、pathMatchは'full'にする。  
pathMatchは'prefix'と'full'があり、'prefix'がデフォルト値。  
```
{ path: '', redirectTo: '/dashboard', pathMatch: 'full' }
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

### AppModule
Angularでは、アプリケーションの部品がどのように合わさるかや、必要なファイルやライブラリを知る必要がある。  
この情報をメタデータという。  
HttpClientModuleやFormsModuleをAppModuleの@NgModuleデコレータに定義しないと使えない。 

### HttpClientを使う
Angularで用意されているhttp通信用のモジュール。  
AppModuleでHttpClientModuleを使うために登録する必要がある。  
```
import { HttpClientModule } from '@angular/common/http';

@NgModule({
	imports: [
		HttpClientModule
	]
})
---
import { HttpClient } from '@angular/common/http';

@Component()
export class HogeWithHttpClient {
	constructor(
		private http: HttpClient
	){}
	
	getJson() {
		this.http.get('/assets/data.json');
	}
}
```

### VScodeでHTMLに警告がでる
テンプレートにHTMLファイルを使っている場合、VSCodeにHTMLHintを入れていると警告がでる。  
.htmlhintrc ファイルでルールを緩和して対策する。  
```
{
  "attr-lowercase": false, // 属性名が小文字か
  "doctype-first": false // DOCTYPE宣言が先頭にあるか
}
```

### Angular CLIを使ったコンポーネントの作成
html,css(scss),specなどを作ってくれる。  
しかもapp.componentにimportや登録まで済ませてくれる。  
```
ng generate component componentName
```

### ビルトインパイプ
補完バインディングの中でパイプ演算子(|)の直後で書式設定ができる。  
文字列、通貨金額、日付、その他の表示データを書式設定するのに向いている。  
また、独自のパイプを作ることもできる。  
```
// 文字列を大文字にするuppercase
{{ 'hogehoge' | uppercase }}
// HOGEHOGE
```

### 双方向データバインディング
[(ngModel)] が双方向データバインディング構文になる。  
```
<input [(ngModel)]="name" />
```
テンプレート駆動フォームで双方向データバインディングを行う場合、formを含める事ができない。  
inputタグをformタグの中に入れた状態でngModelを指定してもエラーがでる。  

### サービス
親子関係にないコンポーネント同士のデータを共有したりするのに使う。  
サービスのファイルもAngularのコマンドで生成可能。  
```
ng generate service serviceName
```
サービスクラスは@injectable()デコレータで注釈される。  
依存関係注入システムであることを表す。  
@InjectableデコレータのprovidedInでサービスの提供先を指定する。  
CLIで作成した場合、rootがデフォルトになっており、アプリケーション全体で使用することができる。  
providedInで得手のモジュールにのみ適用するようにしておけば、サービスが注入されない場合にツリーシェイキングの対象にできる。  

### その他
**文字列の前に+をつけてるやつ**  
JSの仕様で+を文字列の前におくと数値に暗黙の変換を行う。  

### AsyncPipe
*ngForでObservableの反復処理を行う場合は、リスト変数名の末尾に$をつける。  
Observableをそのままforofでは回せないので、パイプ演算子でasyncを使用する。  
```
<li *ngFor="let item of items$ | async"></li>
```
Asyncパイプでオブジェクトの子要素を出力したい時  
```
<div>{{ (user$ | async)?.name }}</div>
```

基本的な方針としてAyncパイプで解決できるケースはAsyncパイプを利用し、subscribe等の明示的な購読は最低限に留める。明示的な購読をした場合購読解除について考える必要がある。  
Asyncパイプを使えば、コンポーネントのライフサイクルに合わせて自動的に購読・購読解除がおこなわれる。  

### グローバルファイルの読み込み
cssやscriptをアプリケーション全体に反映させる(グローバル読み込み)には、angular.jsonを使うと簡単にできる。  
projects->projectName->architect->build,test
のstyleやscriptsの配列にangular.jsonからの相対パスでファイルパスを記述すると読み込まれる。  
上から順番に読み込まれていく。  

### CommonModule、BrowserModule
ngIfやngForなどの汎用的な機能を使うためにCommonModuleをimportsする必要がある。  
BrowserModuleにはCommonModuleが含まれており、app.module.tsでのみ使えば良い。  
子ModuleではCommonModuleを使う。  

### プロキシでHttpClientのCORSを回避(開発)
例えば、angularアプリはport: 4200で動いていて、APIサーバが:3000で動いていた場合、CORSになってしまい通信できない。  
それを回避する方法を以下の記事で紹介している。  
npm scriptsでやる方法を紹介しているけど、angular.jsonに書くほうがスマートっぽい。  
https://qiita.com/ksh-fthr/items/a462a96de7080092b73c  
https://angular.jp/guide/build  

### ディレクトリ構成
参考にした記事。  
https://itnext.io/choosing-a-highly-scalable-folder-structure-in-angular-d987de65ec7  

### サービスをpublicとprivateどちらで使うべきか
privateで宣言した場合、テンプレートファイル内で参照できない。  
なので、privateで宣言して、クラスのメソッドからサービスのメソッドを実行する形にするのが良いらしい。  
https://stackoverflow.com/questions/46596399/typescript-dependency-injection-public-vs-private

### サービスでSubjectを使用してコンポーネント間のデータを共有する
https://qiita.com/ksh-fthr/items/e43dd37bff2e51e95a59  
親子は@Input / @Outputでやり取りできるが、この方法なら親子関係じゃなくても大丈夫

### Angular After Tutorial
Angularチュートリアルを終えた人向けのコンテンツ。  
https://gitbook.lacolaco.net/angular-after-tutorial/  

### ChangeDetectionStrategy.OnPush
Angularのパフォーマンス改善で指定するもの。  
https://qiita.com/masaks/items/61150907ce95b509fcaa  
OnPushに指定されたComponentはアタッチ時に初回のチェックを行い、あとは外部の変更によるトリガーでチェックされることがない。  

### ng-container
https://qiita.com/shibukawa/items/c8c7fd22c1054348db3a  
ReactのFragment的なもの。  
```
<ng-container>
  <p>A</p>
</ng-container>
↓
<p>A</p>
```

### ng-template
表示されない。templateタグ的なもの。  
#で名前をつけて、ローディング中に出す別の要素として使える。  
```
<div *ngIf="loaded; else loading"></div>
<div *ngIf="loaded; then loaded; else loading"></div>
<ng-template #loaded>
	loaded
</ng-template>
<ng-template #loading>
	loading...
</ng-template>
```

### ng-content
Reactのchild的なもの。  
```
// outer.component.html
<div class="outer">
	<ng-content></ng-content>
</div>

<app-outer>
	<p>テキスト</p>
</app-outer>
↓
<div class="outer">
	<p>テキスト</p>
</div>
```

### ngIfによるテンプレートのブロック化
Asyncパイプを使う時に同じObservableに複数回Asyncパイプを適用できるが、同じObservableを2度購読することになるので非推奨。  
### ngIf-as
ngIf-as構文を使うとスマートに実装できる。  
```
{{ user$ | async }}
{{ user$ | async }}
<ng-container *ngIf="user$ | async as user">
	{{ user.name }}
	{{ user.name }}
</ng-container>
```
user$がnullの時*ngIfの評価がfalseになるので内側のブロック全体が表示されなくなる。  
*ngIfのelse構文で対処できる。  
```
<ng-container *ngIf="user$ | async as user; else userIsNull">
	{{ user.name }}
</ng-container>
<ng-template #userIsNull>
	User is Null
</ng-template>
```
### as-snapshotパターン
else構文でカバーできるがテンプレートが肥大化しがち。  
そもそもの原因がObservableがnullを流した時に*ngIfがfalseと評価してしまうから。  
なので常にtrueになるような評価式にしておくと良い。  
```
<ng-container *ngIf="{ user: user$ | async } as snapshot">
	{{ snapshot.user?.name }}
</ng-container>
```
as-snapshotパターンの注意点  
**複数の無関係なAsyncパイプを内包させてはいけない。**  
```
<ng-container *ngIf="{ foo: foo$ | async, bar: bar$ | async } as snapshot">
  <div>Foo: {{ snapshot.foo }}</div>
  <div>Bar: {{ snapshot.bar }}</div>
</ng-container>
```
この場合foo$かbar$のどちらかが更新されると内部全体が再評価されてしまう。  
このような場合は冗長になるがちゃんと分けるべき。  
```
<ng-container *ngIf="{ foo: foo$ | async } as snapshot">
  <div>Foo: {{ snapshot.foo }}</div>
</ng-container>
<ng-container *ngIf="{ bar: bar$ | async } as snapshot">
  <div>Bar: {{ snapshot.bar }}</div>
</ng-container>
```

### Form
https://angular.jp/guide/forms-overview  
https://angular.jp/guide/reactive-forms  
主にリアクティブフォームを使いことになりそう？  
FormBuilderなどを使うには、AppModuleにFormsModuleとReactiveFormsModuleをimportする。  
#### [formGroup]とformGroupNameの違い。 [formControl]とformControlName の違い。
formGroupNameはネスト用かな？[formGroup]の中に入れ子のグループがある場合、そいつはformGroupName指定にする。  

formControlも考え方は同じだけど、逆。  
[formControl]が入れ子ようで、formControlNameは[formGroup]の子。  
https://stackoverflow.com/questions/40171914/what-is-the-difference-between-formcontrolname-and-formcontrol  

#### リアクティブフォームバリデーション
リアクティブフォームのバリデーションはテンプレートの属性を使わずに、バリデータ関数をコンポーネントクラスのフォームコントロールモデルに直接追加する。  
**ブラウザのバリデーションを有効にするには**  
formタグに「ngNativeValidate」属性を追加する。これがないとrequired属性を設定していてもブラウザのアラートが表示されない。  
```
<form ngNativeValidate>
<input required>
</form>
```

##### バリデータ関数
バリデータ関数には同期バリデータと非同期バリデータの2種類がある。  
同期バリデータはFormControlインスタンス時の第2引数、非同期バリデータは第3引数に渡す。  

##### 組み込みバリデータ
requiredやminlengthなど、テンプレート駆動形フォームの属性として使用できる組み込みバリデータは、@angular/formsのValidatorsクラスの関数として使用できる。  
https://angular.jp/api/forms/Validators  
また独自のバリデータ関数を記述することも可能。  
https://angular.jp/guide/form-validation#custom-validators  
複数のバリデータを設定したい場合は、配列で指定する。  
```
'title': new FormControl('title', Validators.required)
'title': new FormControl('title', [Validators.required, Validators.minlength(5)])
```
formGroup.get('プロパティ名').コントロールステータス で状態を知ることができる。  
```
this.fb.get('name').valid
this.fb.get('name').dirty
...etc
```
バリデーションにエラーがある場合はerrorsでアクセスできる。  
```
this.fb.get('name').errors
```

##### コントロールステータスCSSクラス
フォームの状態に応じてクラスが付与される。  
- ng-valid: 値が妥当な場合
- ng-invalid: 値が不正な場合
- ng-pristine: 値が変更されていない場合
- ng-dirty: 値が変更された場合
- ng-pending: バリデータの検証作業待ち
- ng-untouched: 要素にフォーカスが当たったことがある場合
- ng-touched: 要素にフォーカスが当たったことがある場合