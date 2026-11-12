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


# 第5回 (2026-11-02)


## 双対問題の同値な書き換え

$$
\begin{align*}
& \max_{\boldsymbol{u} \in \mathbb{R}^n, \boldsymbol{v} \in \mathbb{R}^m} \sum_{i=1}^n u_i a_i + \sum_{j=1}^m v_j b_j \\
& \text{subject to } u_i + v_j \leq C_{ij}
\end{align*}
$$

$\tilde{v}_j = - v_j$ とすれば

$$
\begin{align*}
& \max_{\boldsymbol{u} \in \mathbb{R}^n, \boldsymbol{v} \in \mathbb{R}^m} \sum_{i=1}^n u_i a_i - \sum_{j=1}^m \tilde{v}_j b_j \\
& \text{subject to } u_i - \tilde{v}_j \leq C_{ij}
\end{align*}
$$

> 機械学習の分野では下の定式化を用いる場合も多い

---

## 自由度調整

> 問題: 解は複数存在する

#### 確認

ある実行可能解を $(\boldsymbol{u}, \boldsymbol{v})$ とする.
任意の $c \in \R$ について, $(\boldsymbol{u} + c \mathbb{1}_n, \boldsymbol{v}+ c \mathbb{1}_m)$ は,

$$
(u_i + c) - (v_j + c)  = u_i - v_j \leq C_{ij} \\
$$

より 実行可能であり,

---

$$
\begin{align*}
\sum_{i=1}^n (u_i + c) a_i - \sum_{j=1}^m (v_j + c) b_j & = \sum_{i=1}^n u_i a_i  + c \sum_{i=1}^n a_i - \sum_{j=1}^m v_j b_j - c \sum_{j=1}^m b_j \\
& = \sum_{i=1}^n u_i a_i - \sum_{j=1}^m v_j b_j + c - c \\
& = \sum_{i=1}^n u_i a_i- \sum_{j=1}^m v_j b_j
\end{align*}
$$

より $(\boldsymbol{u}, \boldsymbol{v})$ と $(\boldsymbol{u} + c \mathbb{1}_n, \boldsymbol{v}+ c \mathbb{1}_m)$ で目的関数の値は変化しない.

---

### 正規化の例

$c' = \frac{\mathbb{1}_n^{\transpose} \boldsymbol{u} + \mathbb{1}_m^{\transpose} \boldsymbol{v} }{n + m}$ を用いて,

$$
\left( \boldsymbol{u}' , \boldsymbol{v}' \right) = \left( \boldsymbol{u} - c' \mathbb{1}_n , \boldsymbol{v} - c' \mathbb{1}_m \right)
$$

と変形したものを正規形とすることがある.
これは, $\boldsymbol{u}, \boldsymbol{v}$ の全ての成分の和が $0$ になるように変形したものである.

> ただし, 定数の加減による違いしか防ぐことはできない

他にも $\boldsymbol{u}$ の成分の和が0 や ある成分を基準として0 とする正規化も考えられる

---

## 相補性条件

最適な輸送を $\mathbf{P}^*$ とすると,主問題の最適値は,

$$
\begin{align*}
p^* & = \sum_{i=1}^n \sum_{j=1}^m C_{ij} P^*_{ij} \\
& \stackrel{(1)}{=} \sum_{i=1}^n \sum_{j=1}^m C_{ij} P^*_{ij} + \sum_{i=1}^n u_i \left( a_i - \sum_{j=1}^m P^*_{ij} \right) + \sum_{j=1}^m v_j \left( b_j - \sum_{i=1}^n P^*_{ij}\right) \\
& = \sum_{i=1}^n \sum_{j=1}^m \left( C_{ij} -u_i -v_j \right) P^*_{ij} + \sum_{i=1}^n u_i a_i + \sum_{j=1}^m v_j b_j \\
\end{align*}
$$

> (1) は主問題の制約条件から. $u_i, \ v_j$ は任意の複素数で成り立つ

---

最適な双対変数を $\boldsymbol{u}^*,\  \boldsymbol{v}^*$, 双対変数の最適値を $d^*$ とすると,
強双対定理より $p^* = d^*$ であり,

$$
\begin{align*}
d^* & = \sum_{i=1}^n u^*_i a_i + \sum_{j=1}^m v^*_j b_j \\
& = p^* =  \sum_{i=1}^n \sum_{j=1}^m \left( C_{ij} - u^*_i - v^*_j \right) P^*_{ij} + \sum_{i=1}^n u^*_i a_i + \sum_{j=1}^m v^*_j b_j \\
\end{align*}
$$

よって $\sum_{i=1}^n \sum_{j=1}^m \left( C_{ij} - u^*_i - v^*_j \right) P^*_{ij} = 0$ であり,
主問題の制約条件 (1) $P_{ij} \geq 0$ と双対問題の制約条件 $u_i + v_J \leq C_{ij}$ より

$$
(C_{ij} - u^*_i - v^*_j ) P^*_{ij} = 0
$$

---

また, ある実行可能解 $\mathbf{P}, \boldsymbol{u}, \boldsymbol{v}$ が $(C_{ij} - u_i - v_j) P_{ij} = 0$ を満たすとすると

$$
\begin{align*}
\sum_{i=1}^n u_i a_i + \sum_{j=1}^m v_j b_j & = \sum_{i=1}^n \sum_{j=1}^m \left( C_{ij} - u_i - v_j \right) P_{ij} + \sum_{i=1}^n u_i a_i + \sum_{j=1}^m v_j b_j \\
& = \sum_{i=1}^n \sum_{j=1}^m C_{ij} P_{ij} + \sum_{i=1}^n u_i \left( a_i - \sum_{j=1}^m P_{ij} \right) + \sum_{j=1}^m v_j \left( b_j - \sum_{i=1}^n P_{ij}\right) \\
& = \sum_{i=1}^n \sum_{j=1}^m C_{ij} P_{ij} \\
\end{align*}
$$

より, 最適解であると分かる.

---

以上より, 主問題, 双対問題それぞれの実行可能解 $\mathbf{P}, \boldsymbol{u}, \boldsymbol{v}$ について,

1. $(C_{ij} - u_i - v_j) P_{ij} = 0 \qquad (i=1, \ldots, n; j=1, \ldots, m)$
2. $\mathbf{P}, \boldsymbol{u}, \boldsymbol{v}$ が最適解

1, 2 は同値

---

## Slaterの条件

> 凸集合 S 上で定義された凸関数 $\boldsymbol{g}(x)$ について,
> $g_i(b) < 0$ を満たす点 $b \in S$ が存在する

凸関数 $f, \boldsymbol{g}$ に対して, 以下の最適化問題するとする.

$$
\begin{align*}
\min \ & f(x) \\
\text{subject to } & g_i (x) \leq 0 \quad (i = 1, \ldots, n) \\
& Ax = b
\end{align*}
$$

ある $x$ で $g_i (x) < 0 \quad (i=1, \ldots , n)$ なら強双対定理が成立する (十分条件)

> 証明略 (日本語での参考文献を見つけられなかったため)

---

### 最適輸送の式では

輸送多面体の非空性を示した際と同様に $P_{ij} = a_i b_j$ とすると, 
$a_i > 0, b_j > 0$ であれば, $P_{ij} > 0$ と不等式制約を厳密に満たし,
これは輸送多面体に含まれるため Slaterの条件を満たす.

---

## KKT条件 (Karush-Kuhn-Tucker 条件)

$f(x)$ が $\R^n$ の開集合 $U$ 上で定義される $C^1$-級実数値関数,
$\boldsymbol{g}(x) :U \to \R^m$, $\boldsymbol{h}(x): U \to \R^k$ が$C^1$-級関数とする.

$f(x)$ が 点 $\boldsymbol{a}$ で 条件 $\boldsymbol{g}(x) \leq 0, \boldsymbol{h}(x) = 0$ の下で極小値をとるとき $\nabla g_i(\boldsymbol{a}), \nabla h_j(\boldsymbol{a})$ が一次独立なら以下が成立する

$$
\begin{align*}
(1) \ & \nabla f(\boldsymbol{a}) + \sum_{i=1}^m \mu_i \nabla g_i (\boldsymbol{a}) + \sum_{j=1}^k \nu_j \nabla h_j (\boldsymbol{a}) = 0 \\
(2) \ & \mu_i \geq 0 \\
(3) \ & \mu_i g_i(\boldsymbol{a}) = 0 \qquad (i=1, \ldots , m) \\
(4) \ & \begin{pmatrix} \mu_1 & \cdots & \mu_m & \nu_1 & \cdots & \nu_k \end{pmatrix} \neq 0
\end{align*}
$$

---

最適輸送では, 
$$
\begin{align*}
f(\mathbf{P}) & = \sum_{i=1}^n \sum_{j=1}^m C_{ij} P_{ij} \\
g_{ij} (\mathbf{P}) & = - P_{ij} \leq 0 \\
h_{1i} (\mathbf{P}) & = a_i - \sum_{j=1}^m P_{ij} = 0 \\
h_{2j} (\mathbf{P}) & = b_j - \sum_{i=1}^n P_{ij} = 0 \\
\end{align*}
$$

---


前回の作ったLagrange双対関数では不等式制約 $P_{ij} \geq 0$ を関数に入れず
制約条件として残していたが関数に含めると,

$$
\begin{align*}
& \sum_{i=1}^n \sum_{j=1}^m C_{ij} P_{ij} + \sum_{i=1}^n \sum_{j=1}^m \left( C_{ij} - u_i - v_j \right) \left(-P_{ij}\right) + \sum_{i=1}^n u_i \left( a_i - \sum_{j=1}^m P_{ij} \right) + \sum_{j=1}^m v_j \left( b_j - \sum_{i=1}^n P^*_{ij}\right) \\
& = \sum_{i=1}^n u_i a_i + \sum_{j=1}^m v_j b_j 
\end{align*}
$$

より, (3) は相補性条件と等しくなる.

また, $f(x)$ が凸関数であるとき, 点 $a$ で最小値をとる十分条件となる. (証明略)
