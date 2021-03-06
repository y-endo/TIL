## PHP
### 変数と定数
変数は$を最初につける。  
定数は2パターンあり、constで宣言するものとdefineで宣言するものがある。  
constはPHP5.3以降でしか使えない。5.2以下はdefineしか使えない  
```
$name = 'Yuki';
const NAME = 'Yuki';
define('NAME', 'Yuki')
```

### 連想配列
```
$array2 = [
  'name' => '遠藤',
  'age' => 26
];
echo "名前は{$array2['name']}";
```

### 比較演算子の==と===
==は型を無視した一致、===は型と値も一致。  
なので===を使うようにする（JSと一緒）

### 比較の早見表
https://www.php.net/manual/ja/types.comparisons.php  

### switchはあまり使わない方がいいらしい
PHPのswitchは比較に == が使われているので、型の比較がされない。  
厳密比較にするには自分で === を書く。  
```
case 1: ×
case === 1: ○
```

### mb_*** 系
マルチバイト文字（日本語）に対して純粋な関数を使うと予期せぬ動作になる場合がある。  
```
strlen('あいうえお'); // 15になる
mb_strlen('あいうえお'); // 5になる
```

### var_dumpを見やすくする方法
手っ取り早く整形するにはpreタグで囲んでしまう。  
```
echo '<pre>';
echo var_dump(hoge);
echo '</pre>';
```

### POSTデータの受取
受け取ったデータは「スーパーグローバル」と呼ばれる変数に代入される。  
$_POST, $_GET $_SESSION, $_FILES などがある。  
```
<input type="text" name="user">
↓
$_POST['user']
```

### 受け取ったデータの存在を確認する empty関数
empty関数で変数の存在を確認する。  
```
if (empty($_POST['user'])) {
	// $_POST['user']は空
}
```

### str_replaceとpreg_replaceの違い
str_replace() 簡単な置き換え。  
preg_replace() 正規表現のような技巧的な置換え。  
https://cpoint-lab.co.jp/article/201806/3400/  

### ファイルの書き込み
fopen関数でファイルを開く。  
fwrite関数で書き込む。  
fopenの第一引数にファイルのパス、第二引数にモードを指定。  
モード

- r: 読み込みのみ
- w: 書き込み(ファイルをリセットしてから書き込み)
- a: 書き込み(ファイルの末端から追記する)

ファイルが存在しない場合は新規で作成される。  

```
if ($file_handle = fopen('ファイルパス', '[モード]')) {
	// 書き込み
	fwrite($file_handle, 'データ');
	// ファイルを閉じる
	fclose($file_handle);
}
```

### ファイルの読み込み
fgets関数を使う。  
データを1行ずつ取得する関数で、実行するたびにファイルポインターリソースの位置が更新されるので、繰り返し実行することで最後までデータを見に行ける。  
```
if ($file_handle = fopen('ファイルパス', 'r')) {
	// 1行ずつ読み込み
	while($data = $fgets($file_handle)) {
		// データ
	}
	// ファイルを閉じる
	fclose($file_handle);
}
```

### 配列の末尾に要素を追加する書き方
```
$arr = array();
$arr[] = 'hoge'; // この書き方は配列の末尾に追加される
```

### お手軽サニタイズ
htmlspecialchars関数。  
特殊文字をHTMLエンティティへ変換を行う。  
https://www.php.net/manual/ja/function.htmlspecialchars.php  
```
$clean = htmlspecialchars($string, $flags, 'UTF-8');
```

### データベースの接続
PDO（PHP Data Objects）を使えば簡単にMySQLなどに接続できる。  
MySQL以外のデータベースと接続するときも同じメソッドが使えて便利。  
https://qiita.com/mpyw/items/d52351bd1a8068344cc2  
https://qiita.com/mpyw/items/b00b72c5c95aac573b71  
https://qiita.com/sunnyG/items/dd981a5ee4487cedf02f  
prepareを使う場合、bindValueの第3引数で型をちゃんと渡す。  

### PDOトランザクション
トランザクション = 複数の処理をまとめて行う  
beginTransaction 処理の開始
commit 処理の実行
rollback 処理のキャンセル
```
$pdo -> beginTransaction();

try {
	$stmt = $pdo->prepare(sql);
	$stmt->execute();
	
	$pdo->commit();
} catch(PDOException $e) {
	$pdo->rollback();
}
```

### Dockerで環境構築した時にPDOでMySQLに接続できなかったら
ドライバがない場合と、接続先が間違っている場合がある。  
https://qiita.com/tarch710/items/1236a23f7ffde4c512f2  
https://chariosan.com/2019/04/14/lamp_docker_pdoerror/  

### PHPで文字列リテラルに式展開
https://qiita.com/tadsan/items/e4796449c736cfb5c9bd  
普通の変数展開は次のようにできる。  
```
$hello = "hello";
$world = "world";

echo "{$hello}, {$world}!", PHP_EOL;
#=> "hello, world!"
```
ただし、式展開や定数は上記のやり方ではできない。  
こうするとできる。  
```
const HOGE = 'hoge';
$i = function ($v) { return $v; };
echo "{$i(HOGE)}, {$i(mb_strtoupper($world))}!";
```
> $iはidentity function、恒等関数(引数と同じものをそのまま返す函数)です。

### セッション
PHPのセッションの使い方。  
session_start関数を実行することでセッションの使用を開始する。  
session_startよりも前にHTMLの出力などしていると警告がでる。  
$_SESSIONに値を入れる。  
```
session_start();
$_SESSION['hoge'] = 'hoge';
```
セッションを削除する場合はunset関数を使う。  
```
unset($_SESSION['hoge']);
```
セッションに空の配列をいれると初期化、session_destroy関数でセッションを破棄。  
```
$_SESSION = [];
session_destroy();
```
> 注意: 通常のコードでは、session_destroy() を呼ぶ必要はありません。 セッションデータを破棄するよりも、 $_SESSION 配列をクリーンアップしてください。

### nl2br() 改行文字の前にHTMLの改行タグを挿入する関数
https://www.php.net/manual/ja/function.nl2br.php  

### PHPのコーディング規約
PSRという有名なコーディング規約があるらしい。  
またfixするツールもあるので、VSCodeに導入する。  
https://tech.glatchdesign.com/php-cs-fixer-for-vscode  

### ファイルの読み込み
require()、require_once()、include()、include_once()
パスの指定で同階層パスの指定を入れると動かないっぽい。  
```
require 'hoge.php';
require_once 'hoge.php';
include 'hoge.php';
include_once 'hoge.php';
```

### 現在のディレクトリ、ファイルのパス
```
__DIR__
__FILE__
```

### 設定(php.ini)はPHPのプログラムからも変更できる。
```
ini_set('prop', 'value');
```

### フォームのセキュリティ
XSS(Cross-Site Scripting)
クリックジャッキング
CSRF(Cross-Site Request Forgeries)
SQLインジェクション

XSS -> サニタイズで対処
クリックジャッキング -> X-FRAME-OPTIONS: DENYで対処 or htaccess
```
// .htaccessの場合
Header set X-FRAME-OPTIONS "DENY"
// PHPの場合
header('X-FRAME-OPTIONS:DENY');
```
CSRF -> sessionによるトークンで対処
```
if (!isset($_SESSION['token'])) {
	$_SESSION['token'] = bin2hex(random_bytes(32));
}
```
filter_var()関数を使うとバリデーションができる。  
https://www.php.net/manual/ja/function.filter-var.php  

### DB操作の基本 CRUD
Create 新規作成 insert
Read 表示 select
Update 更新 update(上書き)
Delete 削除 delete

### 厳密な型検査モード
```
declare(strict_types=1);
```
これを先頭に記載することで、厳密な型検査モードになる。  

### 関数について
#### 引数のデフォルト値 
```
function hoge($string = 'hoge') {}
```

#### 型宣言(タイプヒンティング)
引数の型を指定できる。  
PHP5ではタイプヒンティングと呼ぶが、PHP7から型宣言という呼び名に。  
PHP5はクラス名/インターフェース名、self、array、callableしか使えない。  
```
function hoge(string $string) {};
```
型の種類  
https://www.php.net/manual/ja/functions.arguments.php#functions.arguments.type-declaration  

#### 可変引数
引数の前に...を付与する。  
```
// paramsは配列で、全ての引数が含まれる。
function hoge(...$params){}
```

#### コールバック関数
関数の引数に関数を渡すことができる。  
```
function fuga(){}
function hoge(callable $func)
{
	$func();
}
hoge('fuga');
```

#### メソッドチェーン
->でメソッドをつなげる。  
```
$price = $cart->getItem()->getPrice();
```

### クラス
アクセス修飾子  
- private
- protected
- public

コンストラクタは function __construct() と書く。  
```
class Product
{
    private $products = [];
    public const HOGE = '定数';

    public function __construct(array $products)
    {
        $this->products = $products;
    }

    public function getProducts()
    {
        return $this->products;
    }

    public function addProduct($product)
    {
        $this->products[] = $product;
    }

    public static function getClassName()
    {
        return 'Product';
    }
}
```
#### staticの呼び方
クラス名::定数orメソッド
```
Product::HOGE;
Product::getClassName();
```

#### クラスの継承
extendsで継承。  
親クラスの呼び出しはparent::でおこなう。  
```
public function __construct() {
	parent::__construct();
}
```

#### 抽象クラス(abstract)
継承した子クラスにメソッドの定義を強制できる。  
class名やメソッド名の先頭にabstractを宣言する。  
また抽象メソッドの中身は書くことができない。  
```
abstract class HogeAbstract {
	abstract public function sayHoge();
}
```

#### インターフェース(interface)
インターフェースも子クラスにメソッドの定義を強制できる。  
抽象クラスとの違いは、インターフェース内には強制させるメソッドしかかけない。  
また、インターフェースは複数継承することができて、implementsを使って継承する。  
```
interface Hoge {
	public function hogeMethod();
}
interface Fuga {
	public function fugaMethod();
}
class HogeFuga implements Hoge,Fuga {
	public function hogeMethod(){};
	public function fugaMethod(){};
}
```

#### トレイト(trait)
トレイトは複数継承できる。  
メソッドをそのまま継承するので、使い回しができて便利。  
先頭にtraitを宣言する。overrideも可能。  
クラスの中で use トレイト名 と宣言して使う。  
```
trait ProductTrait
{
    public function getProduct()
    {
        echo $this->className;
        echo 'プロダクト';
    }
}

trait NewsTrait
{
    public function getNews()
    {
        echo 'ニュース';
    }
}

class Product
{
    use ProductTrait;
    use NewsTrait;

    public $className = 'Product';

    public function getInformation()
    {
        echo 'クラスです。';
    }

    public function getNews()
    {
        echo 'override getNews()';
    }
}
```

### 名前空間(namespace)、オートロード(autoload)
名前空間の解説記事  
https://qiita.com/7968/items/1e5c61128fa495358c1f  
最初に名前空間を宣言すると、ファイル内に書かれたメソッドは宣言した名前空間のものになる。  
```
namespace hoge;
function sayHoge(){};

echo hoge\sayHoge();
```
useを使えば名前空間をインポートしたりエイリアスを作成できる。  

オートロードはComposerを使えば簡単にできる。  
requireでいちいちファイルを読み込む記述をしなくてすむようになる。  
```
// composer.json
{
    "autoload": {
        "psr-4": {
            "App\\": "src"
        }
    }
}
```
composer install でいろいろダウンロードされる。  
```
require_once __DIR__ . '/vendor/autoload.php';

use App\Hoge\FugaClass;
echo FugaClass
```

### Composer
PHP版のライブラリ管理ツールてきな感じ？っぽい。  
Composerのインストール方法。  
https://qiita.com/tomk79/items/e6e1db94ea8b661b1e86  
composer経由でライブラリをインストールする。  
```
composer require ライブラリ名
```
composerのオートロードでインストールしたライブラリはuseで読み込める。  