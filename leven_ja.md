[インデックスに戻る](./index_ja.md)

# 修正履歴
- 2024/02/18 新規作成
- 2024/04/02 表示の不具合を修正

# このページについて
このページは**レーベンシュタイン距離**の理論、プログラムについて私なりに説明しています。

# 理論
レーベンシュタイン距離は２つの文字列の間の距離を与えるものだと認識しています。
ある文字列 $`a`$を以下の操作のみを使って別の文字列 $`b`$へと書き換える時、それらの一つ一つの手順を数えます。

- 文字の挿入（$`I`$）
- 文字の削除（$`D`$）
- 文字の置換（$`R`$）

挿入の例を見ましょう。$`x=`$"stein", $`y=`$"steain" という２つの文字列を考えます。この場合、"`a`" という文字を文字列 $`x`$ の文字 "e" と "i" の間に挿入すると文字列 $`y`$ を得ます。
<div style="text-align: center;">
st<b>ei</b>n => st<b>eai</b>n
</div>

削除の例を見ましょう。これは挿入の例の操作の方向を逆にすれば得られます。つまり２つの文字列 $`x=`$"steain", $`y=`$"stein" を考える時、$`x`$ の文字"e" と "i" の間の文字 "a" を削除します。
<div style="text-align: center;">
st<b>eai</b>n => st<b>ei</b>n
</div>

置換の例を見ましょう。２つの文字列 $`x=`$"stein", $`y=`$"stain" を考えます。この場合、文字 "e" が "a" への置換の対象となります。
<div style="text-align: center;">
st<b>e</b>in => st<b>a</b>in
</div>

挿入と置換が混在している例を見ましょう。文字列 $`x=`$"bnb" から $`y=`$"anana" へ書き換える時、いくつかの操作の経路が存在します。例えば
<div style="text-align: center;">
<b>b</b>nb =>r an<b>b</b> =>r ana =>i ana<b>n</b> =>i anan<b>a</b>
</div>
<div style="text-align: center;">
bnb =>i bnb<b>n</b> =>i <b>b</b>nbn<b>b</b> =>r an<b>b</b>nb =>r anan<b>b</b> =>r anana
</div>
ただし、=>i は挿入を表し =>r は置換を表します。挿入の場合は書き換え後に挿入した文字を太文字で表し、置換の場合は書き換え前に置換する文字を太文字で表しています。

一方の数は $`4`$ 、他方の数は $`5`$ であり数が異なります。

いくつかの操作の経路の中で操作の数が最小であるものが**レーベンシュタイン距離**を与えます。

## 数学的性質

以下、レーベンシュタイン距離の計算アルゴリズムのための数学的性質を見ていきましょう。端的にアルゴリズムを知りたいという方は読み飛ばしても良いと思います。


文字列 $`x`$ から $`y`$ へ書き換える時、レーベンシュタイン距離を $`D(x,y)`$ と表しましょう。
文字列の部分文字列を考える時、$`x[...i]`$ は 文字列 $`x`$ の$`1`$番目から$`i`$番目までの部分文字列、$`x[i]`$は$`i`$番目の文字を与えるとしましょう。ただし $`x[0]`$ は長さ0の文字を表すこととします。そして$`D(x[0],y[0]) = 0`$と規約しても不都合はないと考えられます。

**定理**

*$`D(x[...i],y[...j])`$ は次の値の内の最小値に一致する：*
- $`D(x[...(i-1)],y[...j]) + 1`$
- $`D(x[...i],y[...(j-1)]) + 1`$
- $`D(x[...(i-1)],y[...(j-1)]) + t(x[i],y[j])`$

*ただし、$`t(a,b)`$ は文字 $`a, b`$ が一致する時に $`0`$ 、そうでない時に $`1`$ を与えます。*

**例**

先の例 $`x=`$"bnb", $`y=`$"anana" を考えましょう。

次の文字列の組の図式を考えます：

$$
\begin{array}{ccccc}
 (x[0],y[0]) & \to & (x[0],y[...1]) & \to & (x[0],y[...2]) \\
 \downarrow & & \downarrow & & \downarrow \\
 (x[...1],y[0]) & \to & (x[...1],y[...1]) & \to & (x[...1],y[...2]) \\
 \downarrow & & \downarrow & & \downarrow \\
(x[...2],y[0]) & \to & (x[...2],y[...1]) & \to & (x[...2],y[...2]) \\
\end{array}
$$

右へ遷移すると部分文字列 $`y[...j]`$ のインデックス $`j`$ が増加し、下へ遷移すると部分文字列 $`x[...i]`$ のインデックス $`i`$ が増加します。

今回の例に沿って具体的に見ていきましょう：

$$
\begin{array}{ccccc}
 (\text{"}\text{"},\text{"}\text{"}) & \to & (\text{"}\text{"},\text{"}a\text{"}) & \to & (\text{"}\text{"},\text{"}an\text{"}) \\
 \downarrow & & \downarrow & & \downarrow \\
 (\text{"}b\text{"},\text{"}\text{"}) & \to & (\text{"}b\text{"},\text{"}a\text{"}) & \to & (\text{"}b\text{"},\text{"}an\text{"}) \\
 \downarrow & & \downarrow & & \downarrow \\
(\text{"}bn\text{"},\text{"}\text{"}) & \to & (\text{"}bn\text{"},\text{"}a\text{"}) & \to & (\text{"}bn\text{"},\text{"}an\text{"}) \\
\end{array}
$$

まず $`(\text{"}b\text{"},\text{"}an\text{"})`$ のレーベンシュタイン距離 $`D(\text{"}b\text{"},\text{"}an\text{"})`$ を考えます。
計算のアルゴリズムを考えるため、愚直に考えるより図式のステップを考えていったほうがアルゴリズムを見つけやすいと思います。

最初に直上の前ステップ $`(\text{"}\text{"},\text{"}an\text{"})`$ を使ってレーベンシュタイン距離を求めることはできないか考えましょう。この場合、文字列 "b" を一旦削除すれば帰着できることが分かります。つまり $`D(\text{"}b\text{"},\text{"}an\text{"}) \leq D(\text{"}\text{"},\text{"}an\text{"}) + \#\{ D \} = D(\text{"}\text{"},\text{"}an\text{"}) + 1`$ を得ることができます。ここで不等号が成り立つことはレーベンシュタイン距離が最小の手数を考えることに由来します。（この場合、一旦削除することは最小の手数を与えないように思えます。）

次に左のステップ $`(\text{"}b\text{"},\text{"}a\text{"})`$ を考えましょう。これは "a" に挿入を行い "an" を得る操作を加えれば良いと思われます。つまり $`D(\text{"}b\text{"},\text{"}an\text{"}) \leq D(\text{"}b\text{"},\text{"}a\text{"}) + \#\{ I \} = D(\text{"}b\text{"},\text{"}a\text{"}) + 1`$ を得ます。

最後に対角線上のステップ $`(\text{"}\text{"},\text{"}a\text{"})`$ を考えます。これは "b" を一旦削除し $`D(\text{"}\text{"},\text{"}a\text{"})`$ の操作の経路を経て "a" に "n" を挿入すれば、"b" から "an" を得ます。ここで一旦 "b" を削除し "n" を挿入することは操作の最初に "b" を "n" へ置換することと同等です。よって $`D(\text{"}b\text{"},\text{"}an\text{"}) \leq D(\text{"}\text{"},\text{"}a\text{"}) + \#\{ R\} = D(\text{"}\text{"},\text{"}a\text{"}) + 1`$ を得ます。

今までの帰着の方法において前ステップの操作の前後に新たな操作を加えることを考えました。
任意の操作の経路は前ステップのどれかの操作の経路を通らなければならないでしょう。実際、 "b" から "an" を得るにあたって、"b" を削除するのであれば直上のステップ $`(\text{"}\text{"},\text{"}an\text{"})`$ の操作の経路を経るし、そうしないのであれば最後の操作として "a" に文字を挿入する（ $`(\text{"}b\text{"},\text{"}a\text{"})`$ ）か "b" を置換する（$`(\text{"}\text{"},\text{"}a\text{"})`$）はずです。
そして最短の経路の部分経路は最短であるべきです。

よって $`D(\text{"}b\text{"},\text{"}an\text{"})`$ は $`D(\text{"}\text{"},\text{"}an\text{"}) + 1`$、$`D(\text{"}b\text{"},\text{"}a\text{"}) + 1`$もしくは $`D(\text{"}\text{"},\text{"}a\text{"}) + 1`$と一致しなければなりません。
ただし、対角線上のステップからレーベンシュタイン距離を得る場合は置換が加わらないことがあります。実際、$`D(\text{"}bn\text{"},\text{"}an\text{"}) = D(\text{"}b\text{"},\text{"}a\text{"})`$です。

**証明**
省略します。

## 計算アルゴリズム

上記の定理より、レーベンシュタイン距離 $`D(x,y), x=x[...m], y=y[...n]`$を得るにはの直前のステップ $`D(x[...(m-1)],y[...n]), D(x[...m],y[...(n-1)]), D(x[...(m-1)],y[...(n-1)])`$を考えれば良いことが分かりました。それらのレーベンシュタイン距離を得るにはさらにインデックスを戻らなければなりません。よって、この解法のトリガーとして次のレーベンシュタイン距離を最初に考えなければなりません：

$$
D(x[0],y[...j]), D(x[...i],y[0]) (i=0,1,2,...,m; j=0,1,2,...,n).
$$

これらの値を与えることは容易に思います。
計算アルゴリズムのためのメモとして、定理においても見たように、次のマトリクスを用意したほうが見通しが良くなります：

$$
\begin{array}{ccccc}
 D(x[0],y[0]) & \to &D(x[0],y[...1]) & \to &D(x[0],y[...2]) & \to & ... & \to & D(x[0],y[...n]) \\
 \downarrow & & \downarrow & & \downarrow & &  & & \downarrow  \\
 D(x[...1],y[0]) & \to &D(x[...1],y[...1]) & \to &D(x[...1],y[...2]) & \to & ... & \to & D(x[...1],y[...n]) \\
 \downarrow & & \downarrow & & \downarrow  &  &  & & \downarrow \\
 D(x[...2],y[0]) & \to &D(x[...2],y[...1]) & \to &D(x[...2],y[...2]) & \to & ... & \to & D(x[...2],y[...n]) \\
 \downarrow & & \downarrow & & \downarrow  &  &  & & \downarrow \\
 \vdots & & \vdots & & \vdots & & & & \vdots \\
  D(x[...m],y[0]) & \to &D(x[...m],y[...1]) & \to &D(x[...m],y[...2]) & \to & ... & \to & D(x[...m],y[...n]) \\
\end{array}
$$

すでにわかっている値を書くと

$$
\begin{array}{ccccc}
 0 & \to &1 & \to &2  & \to & ... & \to & n \\
 \downarrow & & \downarrow & & \downarrow & &  & & \downarrow  \\
 1 & \to &D(x[...1],y[...1]) & \to &D(x[...1],y[...2]) & \to & ... & \to & D(x[...1],y[...n]) \\
 \downarrow & & \downarrow & & \downarrow  &  &  & & \downarrow \\
 2 & \to &D(x[...2],y[...1]) & \to &D(x[...2],y[...2]) & \to & ... & \to & D(x[...2],y[...n]) \\
 \downarrow & & \downarrow & & \downarrow  &  &  & & \downarrow \\
 \vdots & & \vdots & & \vdots & & & & \vdots \\
 m & \to &D(x[...m],y[...1]) & \to &D(x[...m],y[...2]) & \to & ... & \to & D(x[...m],y[...n]) \\
\end{array}
$$

となります。
この表において最初に考えるべきは $`D(x[...1],y[...1])`$で、これ以外にないと思います。この値を得た後には、一貫した方向として、上から下への方向を優先するか左から右への方向を優先するかが選べます。
よって次のアルゴリズムを得ます。

```
文字列 x と y のレーベンシュタイン距離：
    変数：
        m: 文字列 x の長さ
        n: 文字列 y の長さ
        table: 表
    前処理： // 自明なレーベンシュタイン距離を与える
        // 最初の列のレーベンシュタイン距離を与える
        $table の最初の列 = [0,1,...,m]
        // 最初の行のレーベンシュタイン距離を与える
        $table の最初の行 = [0,1,...,n]
    主処理： // レーベンシュタイン距離を計算する
        繰り返し処理：
            変数：
                i=1,...,m: 縦に動くインデックス
                j=1,...,n: 横に動くインデックス
            変数：
                left_value: $table の第($i-1,$j)成分 + 1 // 左のステップからの計算
                above_value: $table の第($i,$j-1)成分 + 1 // 上のステップから計算
                diagonal_value: $table の第($i-1,$j-1)成分 // 対角線のステップのレーベンシュタイン距離
            // 文字 x[i] と y[j] が異なれば置換の分の手数を増分する
            条件： x の第$i文字と y の第$j文字が異なる
                $diagonal_value = $diagonal_value + 1
            変数：
                maximum_value: $left_value,$above_value,$diagonal_value の内の最大値
            $table の第($i,$j)成分 = $maximum_value // 文字列 x[...i] と y[...j] のレーベンシュタイン距離
    返却： $table の第($m,$n)成分 // 文字列 x と y のレーベンシュタイン距離
```

## プログラム
以下にpythonコードで実装したものを記載します：
```
// レーベンシュタイン距離を計算する
def levensteinDistance(x:str,y:str)->int:
    m = len(x)
    n = len(y)
    table = generateEmptyTable(m,n)
    for i in range(0,m+1):
        table[i][0] = i
    for j in range(1,n+1):
        table[0][j] = j
    for i in range(1,m+1):
        for j in range(1,n+1):
            left_value = table[i-1][j] + 1
            above_value = table[i][j-1] + 1
            diagonal_value = table[i-1][j-1]
            if(x[i] != y[j]):
                diagonal_value += 1
            maximum_value = maximum([left_value,above_value,diagonal_value])
            table[i][j] = maximum_value
    return table[m][n]

// 空のテーブルを生成
def generateEmptyTable(m:int,n:int)->list:
    table = []
    for i in range(0,m+1):
        row = []
        for j in range(0,n+1):
            row.append(None)
        table.append(row)
    return table

// 最大値を計算
def maximum(values:list)->int:
    l = len(values)
    if(l == 0):
        raise
    maximum_value = values[0]
    for i in range(1,l):
        value = values[i]
        if(value > maximum_value):
            maximum_value = value
    return maximum_value
```

[インデックスに戻る](./index_ja.md)
