## Ruby lang

### hello world

`puts 'Hello ,world!'`

### commment

Use `#` to convert a line to comment.

### 文字列結合

`'hoge'+'fuga'` equals `'hogefuga'`.

### 変数の命名規則

Python と似てる
Use `_` for combination like `user_name`.

### テンプレートリテラル的なやつ

Use `#{}` to use variables in string.  
`"こんにちは、#{user_name}さん"`  
You must use `""` for template literal.  
`''` doesn't work.

### each 文が気持ち悪い感じ

```Ruby
配列.each do |変数|
  # 処理
end
```

### メソッド宣言

引数なし

```rb
def メソッド名
  # 処理
end
```

引数あり

```rb
def メソッド名(引数)
  # 処理
end
```

返り値あり

```rb
def メソッド名(引数)
  # 処理
  return 返り値
end
```

返り値が真偽値の場合（なにこれめんどい）

```rb
def メソッド名?(引数)
  # 処理
  return 返り値 # 比較演算
end
# 使用するときも?が必要（めんどい）
```

キーワード引数

```rb
def メソッド名(引数:) # :は引数の「後ろ」に付ける
  # 処理
end
```

### クラス

```rb
class クラス名
  attr_accessor :インスタンス変数名 # :付け忘れがち
  # コンストラクタ
  def initialize(引数:) # キーワード引数には:必要
    self.インスタンス変数 = 引数 # :不要
    # 処理
  end
  # クラスメソッド
  def クラス名.クラスメソッド名(引数:)
    # 処理
  end
end
# インスタンスの生成
インスタンス名 = クラス名.new
# その他はだいたいPythonな感じ
# クラス変数の命名はまだ知らん
```

継承

```rb
class サブクラス < 親クラス
  # 処理
  # オーバーライドは特にアノテーションとかはない
  def initialize(引数1:,引数2:)
    super(引数1:引数1)
  end
end
```

### ファイル分割

外部ファイルの読込

```rb
require './モジュール名'
```

## Rails

アプリの作成
`$ rails new アプリ名`
サーバー起動
`$ rails server`
ページの生成
`$ rails generate controller home top`
`localhost:3000/home/top`が生成される
