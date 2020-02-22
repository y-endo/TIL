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