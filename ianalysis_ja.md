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

# 修正履歴
- 2023/11/15 新規作成

# このページについて
このページは**区間解析**の理論、私なりの拡張およびプログラムの仕様について説明しています。

区間解析の概要については*Wikipedia*を参考にしてください。

# 理論

**区間解析**は数値計算の**精度保証**のための理論として現れたと私は認識しています。

通常の計算で数を引数として扱っている代わりに、この理論では区間を引数として扱っています。演算の単位が点ではなく区間となります。

私の利用法からすると区間解析は数学関数の値の精度に主に関わると思っています。つまり、関数のテイラー展開が有限個の項で収まらないものに対して区間解析はその値の精度を保証するものと考えています。
逆に言うと、四則演算においてはわざわざ区間を計算単位として扱う必要は無いように見えます。

区間解析では基本的な演算として四則演算の他に**平方根**を取り入れています。テイラー展開の剰余項の計算に伴い、これらの五則演算が用いられます。

以下、理論的な定義と性質について見ていきましょう。
## 定義
以下、$X$, $Y$はそれぞれ実直線$\bm{R}$上の有限もしくは無限区間であるとします。
この時、区間に対する５則演算は以下のように定義されます。

足し算：
$$
X + Y := \{x+y \in\bm{R} : x\in X, y\in Y \}
$$

引き算：
$$
X - Y := \{x-y\in\bm{R} : x\in X, y\in Y\}
$$

掛け算：
$$
X * Y := \{x*y \in\bm{R} : x\in X,y\in Y\}
$$

割り算：
$$
X / Y := \{x/y \in\bm{R} : x\in X, y\in Y\}
$$
ただし、$Y$は$0$を含まないとします。

平方根：
$$
\sqrt{X} := \{\sqrt{x}:x\in X\}
$$
ただし、$X$は負の数を含まないとします。

## 基本的性質
ここでは五則演算の基本的性質をみていきましょう。

有限区間$X=[\min X,\max X]$、$Y=[\min Y,\max Y]$に対して
$$
X + Y = [\min X+\min Y,\max X + \max Y].
$$
$$
X - Y = [\min X-\max Y,\max X - \min Y].
$$
$$
X * Y = [\min S,\max S],
$$
ただし$S = \{ \min X * \min Y,\min X * \max Y,\max X * \min Y, \max X * \max Y \}$.
$$
X / Y = [\min S,\max S],
$$
ただし$S=\{\min X/\min Y,\min X/\max Y,\max X/\min Y,\max X/\max Y\}$かつ$\max Y < 0$もしくは$0 < \min Y$.
$$
\sqrt{X} = [\sqrt{\min X},\sqrt{\max X}],
$$
ただし$\min X \geq 0$.

私が調査した文献の理論では有限区間に対してのみ演算が定義されており、また割り算に対しては$0$を含まないという条件が仮定されていました。しかし割り算に対して$0$を含まないという仮定は、区間が大きいときに演算を停止させるというデメリットがあります。$0$を含む区間の場合にも割り算を正当化するには複数の無限区間を要します。

そこで無限区間と複数の区間を許容した五則演算に拡張したいと思います。

## 拡張

$$
[a,b] + (-\infty,d] = (-\infty,b+d]
$$
$$
[a,b] + [c,\infty) = [a+c,\infty)
$$
$$
(-\infty,b] + (-\infty,d] = (-\infty,b+d]
$$
$$
(-\infty,b] + [c,\infty) = (-\infty,\infty)
$$
$$
(-\infty,b] + [c,\infty) = (-\infty,\infty)
$$
$$
(-\infty,\infty) + (-\infty,\infty) = (-\infty,\infty)
$$
証明. 

無限区間を有限区間の和として表す：減少する数列$c$に対して
$$(-\infty,d] = \bigcup_{c}[c,d],$$
$$(-\infty,d] = \bigcup_{c}[a+c,b+d],$$
そして次の性質を使う：
$$[a,b]+[c+d] = [a+c,b+d].$$
QED.

$$
-(-\infty,b] = [-b,\infty)
$$
$$
-[a,\infty) = (-\infty,-a]
$$
$$
-(-\infty,\infty) = (-\infty,\infty)
$$

$$
[a,b] * (-infinity,d] = (-infinity,maxS] or [minS,infinity) or (-infinity,infinity) 
$$

Theorem.
```
[a,b] * (-infinity,d] = (-infinity,maxS] or [minS,infinity) or (-infinity,infinity) 
where S={-sign(a)*infinity,-sign(b)*infinity,a*d,b*d}
```
```
[a,b] * [c,infinity) = (-infinity,maxS] or [minS,infinity) or (-infinity,infinity) 
where S={sign(a)*infinity,sign(b)*infinity,a*c,b*c}
```
```
[a,b] * (-infinity,infinity) = (-infinity,infinity)
```
```
(-infinity,b] * (-infinity,d] = [b*d,infinity) or (-infinity,infinity)
```
```
(-infinity,b] * [c,infinity) = (-infinity,b*c] or (-infinity,infinity)
```
```
(-infinity,b] * (-infinity,infinity) = (-infinity,infinity)
```
```
[a,infinity) * [c,infinity) = [a*c,infinity) or (-infinity,infinity)
```
```
[a,infinity) * (-infinity,infinity) = (-infinity,infinity)
```
```
(-infinity,infinity) * (-infinity,infinity) = (-infinity,infinity)
```