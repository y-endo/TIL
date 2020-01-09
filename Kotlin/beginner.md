## Kotlin入門
Udemyの入門講座を購入したのでやっていく。  
https://www.udemy.com/course/kotlin-beginner/  

## 基本構文
### 変数
TypeScriptのlet/constがvarになった印象。  
型推論があるから、初期値があるなら型の省略できる。  
```
var 変数名: 変数の型 = 初期値
```

型サフィックスについて  
文字や値にサフィックスを付けることで、型を定義できる。  
```
var a = 10L // Long型になる
var b = 10F // Float型になる
```

### リテラル
trimMarginを使うと、|が行頭になる。  
```
println("改行する\n改行した")
println("""改行する
					|改行した""").trimMargin()
```

### 配列とコレクション
配列と代表的なコレクション（List・Set・Map）  
配列は宣言の段階でサイズを決める必要があるので、柔軟性に欠ける。  
```
var array = arrayOf(1, 2, 3) // [1, 2, 3]
var intArray = intArrayOf(1, 2, 3) // [1, 2, 3]
var strArray: Array<String?> = arrayOfNulls(3) // [null, null, null]

var list = listOf("a", "b", "c")
var set = setOf("A", "B", "A", "C")
var map = mapOf("First" to 1)
```

## 定数
定数は val で宣言する。  
```
val a = 10
```