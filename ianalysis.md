# Edit history
- 2023/11/15 create this page

# About this page

This page explains about *Interval Analysis* with its theory, my extesion and specification for my programs.

Please see Wikipedia for what *Interval Analysis* is

# Theory


Let `X=[minX,maxX]`, `Y=[minY,maxY]`, `minX,maxX,minY,maxY < infinity` be intervals on the real number field `R`.
## Theoretical Definitions
```
X + Y := {x+y in R : x in X, y in Y}
```

```
X - Y := {x-y in R : x in X, y in Y}
```

```
X * Y := {x*y in R : x in X, y in Y}
```

```
X / Y := {x/y : x in X, y in Y}
where Y does not contain 0
```

```
sqrt(X) := {sqrt(x) : x in X}
where sqrt means the positive root and every number of X is non-negative
```

## Theoretical Properties
```
X + Y = [minX+minY,maxX+maxY]
```
```
X - Y = [minX-maxY,maxX-minY]
```
```
X * Y = [minS,maxS]
where S:={minX*minY,minX*maxY,maxX*minY,maxX*maxY}
```
```
X / Y = [minS,maxS]
where S:={minX/minY,minX/maxY,maxX/minY,maxX\maxY} and maxY < 0 or 0 < minY
```
```
sqrt(X) = [sqrt(minX),sqrt(maxX)]
where 0 <= minX
```

## Theorems
Theorem.
```
[a,b] + (-infinity,d] = (-infinity,b+d]
```
Proof.
```
x in r.h.s.
=> x in [a+c,b+d] for small c
=> x = y + z, a<=y<=b and c<=z<=d
```
Theorem.
```
[a,b] + [c,infinity) = [a+c,infinity)
```
Proof.
```
x in r.h.s.
=> x in [a+c,b+d] for large d
=> x = y + z, a<=y<=b and c<=z<=d
```
Theorem.
```
(-infinity,a] + (-infinity,b] = (-infinity,a+b]
```
Proof.
```
x in r.h.s.
=> x in [c+d,a+b] for small c,d
=> x = y + z in [c,a]+[d,b]
```
Theorem.
```
(-infinity,a] + [b,infinity) = (-infinity,infinity)
```
Proof.
```
If a < b then
x in r.h.s.
=> take negative small c < a, x-c > b
=> x = c + d, c < a and b < d
```
Theorem.
```
(-infinity,infinity) + (-infinity,infinity) = (-infinity,infinity)
```
Proof.
```
-infinity< x,y < infinity
=> -infinity < x + y < infinity

-infinity < z < infinity
=> -infinity < z+0 < infinity
```
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