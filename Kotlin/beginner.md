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

## 演算子
### 範囲演算子
範囲を表現できる。  
```
// m..n
val i = 10
println(i in 1..20) // true
```

### ifは結果を変数に格納できる
```
var num = 10;
var msg = if (num >= 5) {
	"numは5以上"
} else {
	"numは5未満"
}
println(msg) // numは5以上
```

### when式
switchみたいなもん。  
whenもifと同様に結果を返す。  
```
when(チェックする変数・式) {
	条件1 -> 条件1を満たしたときの処理
	条件2 -> 条件2を満たしたときの処理
	else -> {
		条件を満たさなかったときの処理
	}
}
```
引数がなければ、ifの代替として使える。  
```
val a = 10
when {
	a <= 5 -> println("aは5以下")
	a <= 10 -> println("aは10以下")
	else -> println("aは10よりも大きい")
}
```

### forループ
ラベル構文+breakで外側のループも終了できる。  
outer@のループをbreakするには、break@outer  
```
outer@ for (i in 1..3) {
	for (j in 1..3) {
		if (i * j > 5) break@outer
		print("${i * j}")
	}
	println()
}

結果
1 2 3
2 4
```

## 関数
### 構文  
```
fun 関数名 (引数: 引数の型, 引数2: 引数の型 = デフォルト値, ...): 戻り値の型 {
	// 処理
}

関数名(引数)
```
引数と戻り値の型宣言は必須。  
戻り値がない場合は、Unitを指定する。  

戻り値が単一の場合は構文を省略した書き方がができる。  
```
fun sayHello(): Unit = println("Hello")
sayHello()
```

### 名前付き引数
関数を呼び出すときに、名前付き引数を使えばどの引数にどの値を渡すか指定できる  
名前付き引数と通常の引数を混在させる場合は、名前付き引数を後方に記述する必要がある。  
```
fun sayHello(firstName: String = "Hoge", lastName: String = "Fuga"): Unit {
	println("Hello, ${firstName} ${lastName}")
}

sayHello(lastName = "Endo") // Hello Fuga Endo
sayHello("Yuki", lastName = "Endo") // Hello Yuki Endo
sayHello(firstName = "Yuki", "Endo") // エラー
```

### 可変長引数
引数にvarargを使うと可変長引数になる。  
複数の値を渡せる。  
内部的には配列と同じ扱いになる。  
```
fun allSum(vararg values: Int): Int {
	var result = 0
	for (value in values) {
		result += value
	}
	return result
}
allSum(1, 2, 3, 4, 5) // 15
```

### 高階関数
引数に関数を受け取ったり、戻り値で関数を返す関数。  
forEachなど。  
引数に関数を渡すには「::関数名」という形で渡す。  
```
fun prnt(n: Int) {
	print(n)
}

arrayOf(1, 2, 3, 4).forEach(::prnt)
```

### 匿名関数とラムダ式
匿名関数（無名関数）の書き方は、ラムダ式が主流  
```
// ラムダ式の書き方
{ 引数 -> 処理 }

// 省略形がたくさんある
最後の引数がラムダ式の場合は()の外に出せる
fun hoge(a: Int, b: () -> Unit){}
hoge(1) {
	// ラムダ式
}
```

ラムダ式でreturnすると直上の関数を抜けてしまう。  
```
fun hoge() {
	val arr = arrayOf(1,2,3)
	arr.foEach {
		return // このreturnはforEachではなく、hogeを抜ける
	}
}
```
それを防ぐためにラベルを使う。  
```
fun hoge() {
	val arr = arrayOf(1,2,3)
	arr.foEach loop@ {
		return@loop
	}
}
```
ラムダ式を引数にとる高階関数の名前をラベルに指定してもよい  
```
fun hoge() {
	val arr = arrayOf(1,2,3)
	arr.foEach {
		return@forEach
	}
}
```

## クラス
### プライマリコンストラクタ
Kotlinのコンストラクタ構文  
```
class クラス名 constructor(引数: 型) {}
```
コンストラクタで受け取った引数はクラス内のinitで扱われる。  
```
class Human constructor(name: String, age: Int) {
	var name: String
	var age: Int
	
	init {
		this.name = name
		this.age = age
	}
}
```
プライマリコンストラクタがアノテーションやアクセス修飾子を持たない場合はconstructorを省略可能。  
```
class Human(name: String, age: Int) {}
```

プライマリコンストラクタの引数にvar/valをつけることでプロパティの宣言と初期化を同時に行える。  
```
class Human(var name: String, var age: Int) {
	fun intro() {
		println("${this.name}、${this.age}")
	}
}
```

### クラスの継承
Kotlinでクラスの継承を行う場合、元となるクラスはopenと宣言する必要がある。  
継承するときは クラス名の右に :元のクラス名 と書く。  
この時に引数を渡すとsuper()的な感じで、元のクラスのコンストラクタを通すことができる。  
```
open class Human(var name: String) {
	open fun intro() {
		println("")
	}
}

class PerfectHuman(name: String, var place: String): Human(name) {
	override fun intro() {
		println("")
		super.intro()
	}
}
```

### データクラス
データを保持するだけの特殊なクラス。  
データクラスを定義する際のルール。  
- プライマリコンストラクタが1つ以上の引数を持つ
- コンストラクタの引数はすべてvar/valを付与してプロパティの宣言とする
- abstract / open / saled / innerにすることはできない

よく使うメソッドが予め用意されている。
- equals
- toString
- componentN
- copy
など

```
data class Member(val name: String, val age: Int)
```

### オブジェクト式
再利用を目的としないクラス。  
```
objet {クラス本体}
```

### オブジェクト宣言
ひとつのインスタンスしか持たないようなクラス（シングルトン）
```
object オブジェクト名 {オブジェクト本体}
object TanakaTaro {
	val name = "田中太郎"
	
	fun intro() {
		println("${name}です。")
	}
}
```

### コンパニオンオブジェクト
staticなクラス変数・メソッドの事
```
class MyClass {
	companion object {
		fun create(): MyClass = MyClass()
	}
}

val myClass = MyClass.create()
println(my::class) // class MyClass
```

### コンパイル時定数
コンパイル時に初期化される定数。  
const修飾子を付与する。  
const修飾子を利用するには以下の条件を満たす必要がある。  
- トップレベルかオブジェクト式、コンパニオンオブジェクトのメンバである
- 整数、浮動小数点数、真偽値、文字型、文字列型のいずれかで初期化されている
- カスタムのgetterを持たない
```
const val VERSION = 1
```

### ネストクラス
classの中に定義するclass  
親に強く依存していて、親からしか呼ばれないものはネストクラスを利用する。  
```
class Outer {
	private class Nested {
		fun hoge()
	}
}
```
ネストされたクラスから親クラスを参照したい場合は、inner修飾子を使う。  
```
class Outer(val name: String = "Outer") {
	inner class Nested {
		fun hoge() {
			println("${this@Outer.name}")
		}
	}
}
```

## 拡張関数
継承を用いずに既存のクラスにメソッドを追加できる。  
拡張関数を使えば、openではないクラスにたいしてもメソッドを追加できるようになる。  
```
fun 拡張するクラス名.追加メソッド名(引数: 型, ...): 型 {処理}
```

## オーバーロード
引数の方や数が異なっていれば同じ名前のメソッドを定義することができる。  
これをメソッドのオーバーロードという。  
基本的に演算子以外では使わない。