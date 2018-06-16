## ScalaマンのためのElm

![](./elm-logo.png)

---

## Elmってどんな言語

- 信頼性の高いWebアプリ(フロントエンド)のためのめっちゃ楽しい言語
    - A delightful language for reliable webapps.(公式ページに書いてある)
- Haskellに似ているけど、数万倍敷居の低い言語

---

## お気持ち

### Scala出来る人がElmできないはずがない！！！
### 文法であれば十分でマスター！

---

## Elm入れ方

### はい今日からあなたもElmマン！

```
$ npm install -g elm
$ elm -v
0.18.0
```

---

## Elmのインストールで入るもの

- elm-make
    - Elmのコードを直接コンパイルするコンパイラ
- elm-repl
    - elmの式をその場で評価して確認できるツール
- elm-reactor
    - ブラウザで素早く確認できるビルド・実行環境
- elm-package
    - Elmのパッケージ管理ツール

---

## Elmに無いもの

### 確実にScala, Haskellより楽ちんです。

- 型クラス(Implicit)
- コンパイル時計算(型レベルプログラミング)
- モナ・・・ do(for)式

---

## リテラル: Bool, number

```elm
> True
True : Bool
> False
False : Bool
> 42
42 : number -- IntもしくはFloatである型
> 3.14
3.14 : Float
```

+++

## リテラル: Char, String

```elm
> 'a'
'a' : Char
> "abc"
"abc" : String
> """\
| hello\
| world\
| """
"\n  hello\n  world\n  " : String
```

+++

## リテラル: List

```elm
-- Seq(1, 2, 3, 4, 5)
> [1, 2, 3, 4, 5]
[1,2,3,4,5] : List number
> [1, "a"]
      ^^^
The 1st entry has this type:
    number
But the 2nd is:
    String
> []
[] : List a
```

+++

## リテラル: タプル

```elm
> (1, 2, 3, 4, 5)
(1,2,3,4,5) : ( number, number1, number2, number3, number4 )
> (1, "1", True, [(1, 2), (3, 4)], [["a", "b"], ["c", "d"]])
(1,"1",True,[(1,2),(3,4)],[["a","b"],["c","d"]])
    : ( number
      , String
      , Bool
      , List ( number1, number2 )
      , List (List String)
      )
> ()
() : ()
```

+++

## リテラル: レコード

```elm
-- case class Record(x: Int, y: Int, z: Int)
-- val record = Record(1, 2, 3)
> record = {x = 1, y = 2, z = 3}
{ x = 1, y = 2, z = 3 } : { x : number, y : number1, z : number2 }
> record.x
1 : number
> .y record
2 : number
-- record.copy(x = 10, z = record.y + 20)
> { record | x = 10, z = record.y + 20 }
{ x = 10, y = 2, z = 22 } : { y : number1, x : number, z : number1 }
```

+++

## 関数

```elm
> square n = n^2
<function> : number -> number
> square 5
> 25
```

+++

## 関数

```elm
-- type Name = String
> type alias Name = String
> type alias Age = Int
> type alias User = { name: Name, age: Age }
-- getName : User -> Name
> getName user = user.name
<function> : { b | name : a } -> a
getName (User "john" 15)
"john" : Repl.Name
```

+++

## 関数

```elm
-- ADT(sealed trait, case class)
> type Name = Name String
> type Age = Age Int
> type alias User = { name: Name, age: Age }
> User (Name "john") (Age 15)
{ name = Name "john", age = Age 15 } : Repl.User
> User "john"
Function `User` is expecting the 1st argument to be:

    Name

But it is:

    String
```

+++

## 関数

```elm
-- case class Container[T](v: T)
> type Container a = Container a
> Container 1
Container 1 : Repl.Container number
> Container "abc"
Container "abc" : Repl.Container String
> Container (1, "abc")
Container (1,"abc") : Repl.Container ( number, String )
```

+++

## 関数

```elm
> stdBMI = 22.0
> bmi h = (h ^ 2) * stdBMI
<function> : Float -> Float
> bmi 1.75
(1.75 ^ 2) * stdBMI
3.0625 * 22.0
67.35
```

+++

## 関数

```elm
-- ((h:Double) => (h ^ 2) * stdBMI)(1.75)
> (\h -> (h ^ 2) * stdBMI) 1.75
67.375 : Float
> bmi = (\h -> (h ^ 2) * stdBMI)
```

+++

## 関数

```elm
> 10 + 5 -- 二項演算子は中置記法が使える関数です。
15 : number
-- 演算子すべてメソッド・・・あれ？Scalaかな？
> (+) 10 5 -- 丸括弧で括ることで、通常の関数のように前置記法で記述できます。
15 : number
> True && False
False : Bool
> 1 < 3
True : Bool
> 1 == 2
False : Bool
> 1 /= 2
True : Bool
```

+++

## 関数

```elm
-- (/) : Float -> Float -> Float
> 5 / 2
2.5 : Float
— (//) : Int -> Int -> Int
> 5 // 2
2 : Int
-- 2はFloatとみなされる
> 5.5 / 2
2.75 : Float
> 5 // 2.5 -- TYPE MISMATCH
```

+++

## let式

```elm
-- お行儀の良いブロック式 { val a = 10; a + 5 }
> let a = 10 in a + 5
15 : number
> let px x = toString x ++ "px" in (px 10, px 5)
("10px","5px") : ( String, String )
```

+++

## let式

```elm
circle r =
    let
        pi =
            3.14

        circumference =
            2 * pi * r

        area =
            pi * r ^ 2
    in
        "Circle(circumference = " ++ (toString circumference) ++ ", area = " ++ (toString area) ++ ")"

<function> : Float -> String
> circle 3
"Circle(circumference = 18.84, area = 28.26)" : String
```
実際には、let式は関数中でこのように使われます。

---

## if式

```elm
> powerLevel = 10000
10000 : number
-- if (powerLevel > 9000) "OVER 9000!!!" else "meh"
> if powerLevel > 9000 then "OVER 9000!!!" else "meh"
"OVER 9000!!!" : String
> String.length (if powerLevel > 9000 then "OVER 9000!!!" else "meh")
12 : Int
```

+++

## if式

```elm
pushKey key n =
    if key == 40 then
        n + 1
    else if key == 38 then
        n - 1
    else
        n

> pushKey 40 5
6 : number
```

---

## パターンマッチ

```elm
fib n =
    case n of
        0 ->
            1

        1 ->
            1

        _ ->
            fib (n - 1) + fib (n - 2)

> fib 10
89 : number
```

+++

## パターンマッチ

```elm
> tuple = (8, 4)
(8, 4) : ( number, number1 )
> case tuple of
|     ( x, y ) ->\
|         x + y
12 : numbner

record = { x = 7, y = 8 }
> case record of\
|     { x, y } ->\
|         x + y
15 : number
```

+++

## パターンマッチ

```elm
> plusPair (x, y) = x + y
<function> : ( number, number ) -> number
> plusPair (4, 4)
8 : number
> (\(x, y) -> x + y) (4, 4)
8 : number
> let {x, y} = { x = 4, y = 4 } in x + y
8 : number
> let ({x, y, z} as record) = { x = 1, y = 2, z = 3 } in { record | y = x * y, z = x + z }
{ x = 1, y = 2, z = 4 } : { x : number2, y : number, z : number1 }
```

---

## Union Types

```
> type Money = Dollar | EURO | JPY
> Dollar
Dollar : Money
> EURO
EURO : Money
> JPY
JPY : Money
> JPY == JPY -- Union Typesは等価性を備えています。
True : Bool
```

+++

## Union Types

```
toJPYRate : Money -> Float
toJPYRate money =
    case money of
        Dollar ->
            109.134

        EURO ->
            129.462

        JPY ->
            1
```

+++

## Union Types

```elm
> type Foo = Bar Int
> getBarNum (Bar num) = num
<function> : Repl.Foo -> Int
> getBarNum (Bar 5)
5 : Int
```

+++

## Union Types

値があるか無いかの分岐に使う[Maybe](http://package.elm-lang.org/packages/elm-lang/core/5.1.1/Maybe)型です。

```elm
type Maybe a  -- Container a のように型変数を持ちます。
    = Just a
    | Nothing
```

+++

## Union Types

```elm
-- getNumOrZero : Maybe Int -> Int
> getNumOrZero mNum =
    case mNum of
        Just n ->
            n

        Nothing ->
            0

> let f x = if x > 5 then Just x else Nothing in getNumOrZero(f 10)
10 : number
> let f x = if x > 5 then Just x else Nothing in getNumOrZero(f 3)
0 : number
>
```

+++

## Union Types

[Result](http://package.elm-lang.org/packages/elm-lang/core/5.1.1/Result)型は、Maybeと似た型ですが、失敗したときの理由を任意の型で受け取ることができます。

```elm
> safeSqrt n =
    if n >= 0 then
        Ok n
    else
        Err "n must be a positive number."

> safeSqrt 1
Ok 1 : Result.Result String number
> safeSqrt -1
Err "n must be a positive number." : Result.Result String number
```

---

## 高階関数

```elm
-- (1 to 100).filter(_ % 2 == 0).map(_ * 2)
> List.range 1 100 |> List.filter (\x -> x % 2 == 0) |> List.map (\x -> x * 2)
[4,8,12,16,20,24,28,32,36,40,44,48,52,56,60,64,68,72,76,80,84,88,92,96,100,104,108,112,116,120,124,128,132,136,140,144,148,152,156,160,164,168,172,176,180,184,188,192,196,200]
    : List Int
```

---

## デモ

### 最近業務時間で作ったもの
