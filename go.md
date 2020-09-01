# Go 文法

## Hello, world!

```go
package main
func main(){
  // print'I'n ではなく print'l'n
  println("Hello, world!")

  // カンマ区切りも可
  println(1, 5)
}
```

## 変数宣言

静的型付けだが型推論が強力っぽい。

```go
// var 変数名 型名
var hoge string

// var 変数名 型名 = 値
var hoge string = "fuga"

// var 変数名 = 値 !初期値代入の場合は型推論される
var hoge = "fuga"

// 変数名 := 値 !上記の省略記法
hoge := "fuga"
```

インデントスタイルは、かなり厳密らしい。
あと、未使用変数があるとエラーを吐くらしい。

## 演算子

インクリメントの省略記法が Python と違ってサポートされている。

```go
i = i + 1
i += 1
i++  //これはOK

//これは無理（式でなく文扱い）
println(i++)

//これも無理（文扱いなので前置型はない）
++i
```

## if文関連

### 構文

```go
if hoge == fuga {
  println(true)
} else if hoge > fuga {
  println("すごく……大きいです……")
} else {
  println("小さすぎて見えませんでした")
}
```

## 比較演算子

`==`,`!=`：等価、非等価
`<`,`>`,`<=`,`>=`：大小
`&&`,`||`,`!`：論理積、論理和
