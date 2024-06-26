<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
<script type="text/x-mathjax-config">
 MathJax.Hub.Config({
 tex2jax: {
 inlineMath: [['$', '$'] ],
 displayMath: [ ['$$','$$'], ["\\[","\\]"] ]
 }
 });
</script>

[インデックスに戻る](./index_ja.md)

# 修正履歴
- 2024/04/14 新規作成

# このページについて
このページは**隣接行列によるグラフの連結性の判定**について説明しています。

グラフ理論におけるグラフの概要については*Wikipedia*を参考にしてください。

# 理論

簡単のため単純グラフを考えます。

グラフは隣接行列によって表現することができます。すなわち

$$
G = (g_{ij})_{ij}
$$

ただし、グラフのサイズはノードの数$n$、$g_{ij}$はノード$i$とノード$j$の間に辺があるとき$1$、そうでないときは$0$の値を取ります。単純グラフを考えているので必然的に$g_{ii}=0, g_{ij}=g_{ji}$となります。

一つ行を取り出してみます：$g_{i*} = (g_{ij})_j$
これはノード$i$につながっているノード$j$全体を表します。

同様に列を一つ取り出してみます：$g_{*j} = (g_{ij})_i$
同様に、これはノード$j$へ辺でつながるノード$i$全体を表します。

これらをベクトルとみて内積をとってみます：

$$
g_{i*}.g_{*k} = (g_{ij}.g_{jk})_{ik}
$$

これはノード$i$からノード$k$へと***辺を二つ通って***（重複してもよい）たどり着く道の数を表します。

実際、ノード$i$からノード$j$へ辺があり、ノード$j$からノード$k$へと辺があるとき、$g_{ij}.g_{jk}=1.1=1$となり、道$i,j,k$が確かに存在します。どちらかの辺がないときは$g_{ij}.g_{jk}=0$となるので、ノード$j$を経由しての道は存在しません。

これをノード$i$に隣接しているノード$j$を経由してノード$k$に至る道をすべて考えるので$g_{i*}.g_{*k}$はノード$i$から二つ辺を通ってノード$k$に至る道の数となります。

この事実の帰結として、
行列の$2$乗は、内積をすべてのノード間にわたって行うわけですから、隣接行列の$2$乗の$(i,j)$成分はノード$i$からノード$j$へ二つの辺を通ってできる道の総数を表します。

帰納的に隣接行列の$k$乗の$(i,j)$成分はノード$i$からノード$j$へ辺を$k$つ通って至る道の数となります。

この結果はグラフの連結性の判定に応用できます。
$n$をグラフのノード数とするとき、あるノード$i$からあるノード$j$へ至るには一般的に考えてせいぜい$n-1$つの辺を通ればよいことがわかります。ですので隣接行列の$n-1$までの各累乗を足し合わせた結果の$(i,j)$成分、つまりノード$i$からノード$j$へ高々$n-1$つ辺を通る道の数、のすべてが$0$でないならばグラフは連結であるといえます。そうでなければグラフは非連結です。

# 例

ノード$A,B,C,D$が四角形のようにつながっていて$A$と$C$、$B$と$D$が対角線にあるグラフを考えましょう。

隣接行列は

$$
G=
\begin{bmatrix}
0 & 1 & 0 & 1 \\
1 & 0 & 1 & 0 \\
0 & 1 & 0 & 1 \\
1 & 0 & 1 & 0
\end{bmatrix}
$$

となります。$g_{AC}=0$、$g_{BD}=0$です。

$2$乗すると

$$
G^2=
\begin{bmatrix}
2 & 0 & 2 & 0 \\
0 & 2 & 0 & 2 \\
2 & 0 & 2 & 0 \\
0 & 2 & 0 & 2
\end{bmatrix}
$$

これらを足すと

$$
G+G^2=
\begin{bmatrix}
2 & 1 & 2 & 1 \\
1 & 2 & 1 & 2 \\
2 & 1 & 2 & 1 \\
1 & 2 & 1 & 2
\end{bmatrix}
$$

となり連結となります。

[インデックスに戻る](./index_ja.md)