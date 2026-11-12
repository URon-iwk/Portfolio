---
marp: true
math: mathjax
style: |
  section {
    font-family: sans-serif, 'BIZ UDGothic';
    font-size: 32px;
  }
---

$$
\DeclareMathOperator{\E}{\mathbb{E}}
\DeclareMathOperator{\R}{\mathbb{R}}
\DeclareMathOperator{\N}{\mathbb{N}}
\DeclareMathOperator{\P}{\mathbb{P}}
\DeclareMathOperator{\transpose}{\mathsf{T}}
\DeclareMathOperator{\Var}{Var}
\DeclareMathOperator{\Cov}{Cov}
\DeclareMathOperator{\Corr}{Corr}
\DeclareMathOperator{\plim}{plim}
\DeclareMathOperator{\rank}{rank}
\DeclareMathOperator*{\argmin}{arg\,min\ }
\DeclareMathOperator*{\argmax}{arg\,max\ }
$$

# 第3回 (2026-07-13)

## 前回

2つの一般の分布に対し, 距離を求めることができる式を求めた

$$
\begin{aligned}
\min_{\pi \in  \mathcal{P}(\mathcal{X} \times \mathcal{X})} & \int_{\mathcal{X} \times \mathcal{X}} C(x, y) \ d \pi(x, y) \\
\text{subject to} \quad 
& \pi(\mathcal{A} \times \mathcal{B}) \geq 0 \quad & (\forall \mathcal{A}, \mathcal{B} \in \mathcal{F}(\mathcal{X})) \\
& \pi(\mathcal{A} \times \mathcal{X}) = \alpha(\mathcal{A}) & (\forall \mathcal{A} \in \mathcal{F}(\mathcal{X})) \\
& \pi(\mathcal{X} \times \mathcal{B}) = \beta(\mathcal{B}) & (\forall \mathcal{B} \in \mathcal{F}(\mathcal{X})) \\
\end{aligned}
$$

---

## ワッサースタイン距離

距離の公理を満たすような分布同士の "距離"

$\mathcal{X}$ 上の距離関数 $d: \mathcal{X} \times \mathcal{X} \to \R$ と $p \geq 1 \in \R$, 
コスト関数が $C(x, y)= d(x, y)^p$ であるとき,
$\mathcal{X}$ 上の確率分布 $\alpha, \ \beta$ について

$$
W_p (\alpha, \beta) := \text{OT}(\alpha, \beta, C)^{\frac{1}{p}}
$$

を $p$-ワッサースタイン距離 という

---

### 距離の公理

1. 非負性

$$
d(x, y) \geq 0
$$

2. 同一性

$$
d(x, y) = 0 \Leftrightarrow x = y
$$

3. 対称性

$$
d(x, y) = d(y, x)
$$

4. 三角不等式

$$
d(x,y) \leq d(x, z) + d(z, y)
$$

---

##### 1の非負性は, 2, 3, 4から示すことが可能

$$
\begin{align*}
\\
0 & = d(x, x) &(\because 同一性) \\
& \leq d(x, y) + d(y, x) &(\because 三角不等式)\\
& = d(x, y) + d(x, y) &(\because 対称性)\\
& = 2 d(x, y)
\end{align*}
$$

よって, 
$$
0 \leq d(x, y)
$$

---

### ワッサースタイン距離は本当に距離なのか (点群で示す)

1. 非負性


$$
\begin{align*}
\text{OT} (\alpha, \beta, C) & =
\min_{\pi \in  \mathcal{P}(\mathcal{X} \times \mathcal{X})} \int_{\mathcal{X} \times \mathcal{X}} C(x, y) \ d \pi(x, y) \\
& = \min_{\pi \in \mathcal{P}(\mathcal{X} \times \mathcal{X})} \mathbb{E}_{\pi} [C(X, Y)]
\end{align*}
$$

ここで, $C(x, y) = d(x, y)^p$ であり $d$ は距離関数のため, $C(x, y) \geq 0$
非負の値のみを取る確率変数の期待値は非負のため, $\E_\pi [C(X, Y)] \geq 0$

よって, 

$$
W_p (\alpha, \beta) =  \left( \min_{\pi \in \mathcal{P}(\mathcal{X} \times \mathcal{X})} \mathbb{E}_{\pi} [C(X, Y)] \right)^\frac{1}{p} \geq 0
$$

---

2. 同一性


$$ W_p (\alpha, \beta)^p = \min_{\mathbf{P} \in \mathbb{R}^{n \times m}} \sum_{i=1}^{n} \sum_{j=1}^{m} C(x_i, y_j) P_{ij}$$


> $\alpha = \beta \Rightarrow W_p (\alpha , \beta) = 0$ を示す.

輸送行列 $\mathbf{P}$ を以下とする. ただし, 以前と同様 $\mathbf{a} = \begin{pmatrix} a_1 & \cdots & a_n \end{pmatrix}^{\transpose}$

$$
\mathbf{P} = \begin{pmatrix}
a_1 & 0 & \cdots  & 0 \\
0 & a_2 & \cdots & 0 \\
\vdots & \vdots & \ddots &  \vdots \\
0 & 0 & \cdots & a_n \\
\end{pmatrix}
$$

---

この $\mathbf{P}$ は, $a_i \geq 0 \ (i = 1, \ldots, n)$ より $P_{ij} \geq 0$

$\mathbf{P} \mathbb{1}_m = \begin{pmatrix} a_1 & \cdots & a_n \end{pmatrix}^{\transpose} = \mathbf{a}$
$\mathbf{P}^{\transpose} \mathbb{1}_n = \mathbf{a} = \mathbf{b} \ \ (\because \alpha = \beta)$

より制約条件を満たす

$\alpha = \beta$ より, $n = m, \  x_i = y_i$ であり,
輸送行列 $P$ は非対角成分が 0 であるため, 

$$\sum_{i=1}^{n} \sum_{j=1}^{m} C(x_i, y_j) P_{ij} = \sum_{i=1}^{n} C(x_i, x_i) P_{ii}$$

---

$C(x, y) = d(x, y)^p$ であったので, 距離の公理の同一性より,

$$
C(x_i , x_i) = d(x_i, x_i)^p = 0^p = 0
$$

となり,

$$
\sum_{i=1}^{n} C(x_i, x_i) P_{ii} = \sum_{i=1}^{n} 0 P_{ii} = 0
$$

非負性より, 

$$
0  = \min_{\mathbf{P} \in \mathbb{R}^{n \times m}} \sum_{i=1}^{n} \sum_{j=1}^{m} C(x_i, y_j) P_{ij}
= W_p (\alpha, \beta)^p 
= W_p (\alpha, \beta)
$$



---

> $W_p (\alpha , \beta) = 0 \Rightarrow \alpha = \beta$ を示す.

$W_p (\alpha, \beta)= 0$ より, $\min \sum_{i=1}^{n} \sum_{j=1}^{m} C(x_i, y_j) P_{ij} = 0$

このとき, $\mathbf{P} \in \R^{n \times n}$, 分布 $\alpha, \ \beta$ がとりうる点は共に $x_1, \ldots, x_n \in \mathcal{X}$ とする.

> 前回のカテゴリの例と同じく, 確率 0 の点も含んでいる

---

$P_{ij} > 0$ となる $(i, j)$ で $C(x_i, x_j) = 0$ でなければならず,

$C(x, y) = d(x, y)^p$ であったので, $x_i = x_j$ つまり $i=j$

輸送行列の非対角成分が 0 であり, 制約条件を満たす $\mathbf{P}$ は,
制約条件2 $\mathbf{P} \mathbb{1}_n = \begin{pmatrix} a_1 & \cdots & a_n \end{pmatrix}^{\transpose} = \mathbf{a}$ から


$$
\mathbf{P} = \begin{pmatrix}
a_1 & 0 & \cdots  & 0 \\
0 & a_2 & \cdots & 0 \\
\vdots & \vdots & \ddots &  \vdots \\
0 & 0 & \cdots & a_n \\
\end{pmatrix}
$$

---

この輸送行列 $\mathbf{P}$ は, 制約条件3 $\mathbf{P}^{\transpose} \mathbb{1}_n = \mathbf{b}$ も満たさないといけないため

$\mathbf{P}^{\transpose} \mathbb{1}_n =\begin{pmatrix} a_1 & \cdots & a_n \end{pmatrix}^{\transpose} =  \mathbf{a} = \mathbf{b}$

よって, $\alpha = \beta$ となる

---

3. 対称性

> $W_p (\alpha, \beta) = W_p (\beta, \alpha)$ を示す.

$\hat{\mathbf{P}} = \mathbf{P}^{\transpose}$ とすると, $\hat{\mathbf{P}} \in \mathcal{U}$ であり, $\hat{P}_{ji} = P_{ij}$

$C(x, y) = d(x, y)^p$ より, $C(x_i, y_j) = C(y_j, x_i)$ のため,

$$
\sum_{j=1}^{m} \sum_{i=1}^{n} C(y_j, x_i) \hat{P}_{ji} =  \sum_{i=1}^{n} \sum_{j=1}^{m} C(x_i, y_j) P_{ij}
$$

左辺は $\beta$ から $\alpha$ への総輸送コストであるため, $W_p(\alpha , \beta)= W_p (\beta , \alpha)$

---

4. 三角不等式

> $W_p (\alpha, \gamma) \leq W_p (\alpha, \beta) + W_p (\beta , \gamma)$ を示す.

分布 $\alpha, \beta, \gamma$ がとりうる点は共に $x_1, \ldots, x_n \in \mathcal{X}$ とし,
$\alpha$ から $\beta$ への最適輸送行列を $\mathbf{P}^*$, $\beta$ から $\gamma$ への最適輸送行列を $\mathbf{Q}^*$ とする.

$\mathbf{R} = \mathbf{P}^* 
\begin{pmatrix}
\frac{1}{b_1} & \cdots & 0 \\
\vdots & \ddots & \vdots \\
0 & \cdots & \frac{1}{b_n}
\end{pmatrix} \mathbf{Q}^* = \mathbf{P}^* \mathrm{Diag}(\frac{1}{\mathbf{b}}) \mathbf{Q}^*$ と定める

ただし, $b_i = 0$ のとき, $\frac{1}{b_i} = 0$ と置く

---

このとき, $\mathbf{R}$ は,

$$
\begin{align*}
\mathbf{R} \mathbb{1}_n &= \mathbf{P}^* \mathrm{Diag}(\frac{1}{\mathbf{b}}) \mathbf{Q}^* \mathbb{1}_n \\
& = \mathbf{P}^* \mathrm{Diag}(\frac{1}{\mathbf{b}}) \mathbf{b} \\
& = \mathbf{P}^* \mathbb{1}_{\{b_i > 0\}}
\end{align*}
$$

$\mathbf{P}^{\transpose} \mathbb{1}_n = \mathbf{b}$ より, $b_j = 0$ である $j$ で, 任意の $i$ について $P_{ij} = 0$


$$
\begin{align*}
\mathbf{P}^* \mathbb{1}_{\{b_i > 0\}} & = \mathbf{P}^* \mathbb{1}_n \\
& = \mathbf{a}
\end{align*}
$$

---

同様に

$$
\begin{align*}
\mathbf{R}^{\transpose} \mathbb{1}_n &= {\mathbf{Q}^*}^{\transpose} \mathrm{Diag}(\frac{1}{\mathbf{b}}) {\mathbf{P}^*}^{\transpose} \mathbb{1}_n \\
& = {\mathbf{Q}^*}^{\transpose} \mathrm{Diag}(\frac{1}{\mathbf{b}}) \mathbf{b} \\
& = {\mathbf{Q}^*}^{\transpose} \mathbb{1}_{\{b_i > 0\}} \\
& = {\mathbf{Q}^*}^{\transpose} \mathbb{1}_n \\
& = \mathbf{c}
\end{align*}
$$

$\mathbf{R} \geq 0$ であるため, 制約条件をすべて満たし, $\mathbf{R} \in \mathcal{U}$

---

$$
\begin{align*}
W_p(\alpha, \gamma) & = \min_{\mathbf{P} \in \mathcal{U}(\mathbf{a}, \mathbf{c})} \left( \sum_{i=1}^n \sum_{k=1}^n d(x_i, x_k)^p P_{ik} \right)^{\frac{1}{p}} \\
& \leq \left( \sum_{i=1}^n \sum_{k=1}^n d(x_i, x_k)^p R_{ik} \right)^{\frac{1}{p}} \\
& = \left( \sum_{i=1}^n \sum_{k=1}^n d(x_i, x_k)^p \sum_{j=1}^n P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}} \\
&= \left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n d(x_i, x_k)^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}}
\end{align*}
$$

---

距離関数の三角不等式 $d(x, y) \leq d(x, z) + d(z, y)$ より,

$$
\begin{align*}
\left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n d(x_i, x_k)^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}} & \leq \left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n (d(x_i, x_j) + d(x_j, x_k))^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}}
\end{align*}
$$


また, ミンコフスキーの不等式 $\left(\sum_{k=1}^n |x_k + y_k|^p \right)^{\frac{1}{p}} \leq \left(\sum_{k=1}^n |x_k|^p \right)^{\frac{1}{p}} + \left(\sum_{k=1}^n |y_k|^p \right)^{\frac{1}{p}}$ より,

$$
\begin{align*}
\left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n (d(x_i, x_j) + d(x_j, x_k))^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}}
\leq & \left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n d(x_i, x_j)^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}} \\
& + \left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n d(x_j, x_k)^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}}
\end{align*}
$$

---

$$
\begin{align*}
\left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n d(x_i, x_j)^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}} & = \left( \sum_{i=1}^n \sum_{j=1}^n d(x_i, x_j)^p P^*_{ij} \frac{1}{b_j}  \sum_{k=1}^n Q^*_{jk} \right)^{\frac{1}{p}} \\
& = \left( \sum_{i=1}^n \sum_{j=1}^n d(x_i, x_j)^p P^*_{ij} \frac{1}{b_j}  b_j \right)^{\frac{1}{p}} \\
& = \left( \sum_{i=1}^n \sum_{j=1}^n d(x_i, x_j)^p P^*_{ij} \right)^{\frac{1}{p}} = W_p(\alpha, \beta)
\end{align*}
$$

同様に, 
$$\left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n d(x_j, x_k)^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}} = \left( \sum_{j=1}^n \sum_{k=1}^n d(x_j, x_k)^p Q^*_{jk} \right)^{\frac{1}{p}} = W_p (\beta, \gamma)$$

---


$$
\begin{align*}
W_p(\alpha, \gamma) \leq & \left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n (d(x_i, x_j) + d(x_j, x_k))^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}} \\
& \leq \left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n d(x_i, x_j)^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}} + \left( \sum_{i=1}^n \sum_{j=1}^n \sum_{k=1}^n d(x_j, x_k)^p P^*_{ij} \frac{1}{b_j} Q^*_{jk} \right)^{\frac{1}{p}} \\
& = W_p(\alpha, \beta) + W_p(\beta , \gamma)
\end{align*}
$$

