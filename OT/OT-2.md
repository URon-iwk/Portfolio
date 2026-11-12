---
marp: true
math: katex
style: |
  section {
    font-family: sans-serif, 'BIZ UDGothic';
    font-size: px;
  }
---

# 第2回 (2026-06-08)


## 前回
最適輸送: 2つの確率分布を比較する手法
確率を質量とみなし, "距離" を定め，コストが最小となる輸送方法を見つける
最適輸送コスト $\mathrm{OT}(\alpha, \beta, C)$ は，

$\mathcal{U} (a, b) := \{ \mathbf{P} \in \mathbb{R}^{n \times m} : \mathrm{P}_{ij} \geq 0, \mathbf{P} \mathbb{1}_m = \mathbf{a}, \mathbf{P}^{\mathrm{T}} \mathbb{1}_n = \mathbf{b} \}$ とすると，

$$\mathrm{OT}(\alpha, \beta, C) = \min_{\mathbf{P} \in \mathcal{U}(a, b)} \left\langle \mathbf{C}, \mathbf{P} \right\rangle$$

---

### 前回内容に関する追記

1. 目的関数の書き方

$$
L_P (\mathbf{P}) := \left\langle \mathbf{C}, \mathbf{P} \right\rangle
$$

2. $\mathcal{U} (a, b)$ の有界性

制約条件より, $\quad$ $0 \leq P_{ij} \leq a_i, \quad 0 \leq P_{ij} \leq b_j$
つまり, $\quad$ $P_{ij} \in [0, \min (a_i, b_j)]$

$\mathbf{P} \in [0, \min (a_1, b_1)] \times [0, \min (a_1, b_2)] \times \cdots \times [0, \min (a_n, b_m)]$
$\text{i.e. } \mathcal{U} (a, b) \subset \prod_{i=1}^{n} \prod_{j=1}^{m} [0, \min (a_i, b_j)]$

> Aが有界である: $\exists M > 0, \forall x \in A : d(x, 0) \leq M$

$\mathcal{U} (a, b)$ は非空有界閉集合, つまり コンパクト集合

---

## 点群比較

$\displaystyle \min_{\mathbf{P} \in \mathcal{U}(a, b)} \left\langle \mathbf{C}, \mathbf{P} \right\rangle$ の書き換え


$$
\begin{aligned}
\min_{\mathbf{P} \in \mathbb{R}^{n \times m}} & \sum_{i=1}^{n} \sum_{j=1}^{m} C(x_i, y_j) P_{ij} \\
\text{subject to} \quad & P_{ij} \geq 0  \quad \quad \quad  \quad \quad (i = 1, \ldots , n , \  j = 1, \ldots, m) \\
& \sum_{j=1}^{m} P_{ij} = a_i  \quad \quad \quad (i = 1, \ldots , n) \\
& \sum_{i=1}^{n} P_{ij} = b_j  \quad \quad \quad (j = 1, \ldots, m)
\end{aligned}
$$

---

### 点群比較の例 (P.26 例 2.1 より)

各点は当確率で,

$$
\begin{aligned}
\bm{x}_1 = \begin{pmatrix} 2.2 \\ 2.1 \end{pmatrix},
\bm{x}_2 = \begin{pmatrix} 3.2 \\ 5.3 \end{pmatrix},
\bm{x}_3 = \begin{pmatrix} 4.5 \\ 4.4 \end{pmatrix},
\bm{x}_4 = \begin{pmatrix} 3.1 \\ 3.8 \end{pmatrix} \\
\bm{y}_1 = \begin{pmatrix} 4.8 \\ 1.9 \end{pmatrix},
\bm{y}_2 = \begin{pmatrix} 4.1 \\ 3.3 \end{pmatrix},
\bm{y}_3 = \begin{pmatrix} 2.0 \\ 5.5 \end{pmatrix},
\bm{y}_4 = \begin{pmatrix} 3.4 \\ 2.5 \end{pmatrix}
\end{aligned}
$$

コスト関数は $C(\bm{x}, \bm{y}) = \| \bm{x} - \bm{y} \|_2^2$

> L2-ノルム: $\| \bm{x} \|_2 = \sqrt{x_1^2 + x_2^2}$
> ただし, $\bm{x} = \begin{pmatrix} x_1 & x_2  \end{pmatrix}^{\mathsf{T}}$

---

コスト行列は,

$$
\mathbf{C} = \begin{pmatrix} 
6.80 & 5.05 & 11.60 & 1.60 \\
14.12 & 4.81 & 1.48 & 7.88 \\
6.34 & 1.37 & 7.46 & 4.82 \\
6.50 & 1.25 & 4.10 & 1.78
\end{pmatrix}
$$

最適解は,

$$
\mathbf{P} = \begin{pmatrix} 
0.00 & 0.00 & 0.00 & 0.25 \\
0.00 & 0.00 & 0.25 & 0.00 \\
0.25 & 0.00 & 0.00 & 0.00 \\
0.00 & 0.25 & 0.00 & 0.00
\end{pmatrix}
$$

---

### 点群比較のヒストグラムへの応用

$\\$

カテゴリ数: $n$
輸送: カテゴリ $i$ $\to$ カテゴリ $j$
質量: そのカテゴリをとる確率

$\\$
$\\$

> 点群比較では, 点の数が $n$, $m$ であり 点 $i$ $\to$ 点 $j$ の輸送を考えていた

---

#### 定式化

$$
\begin{aligned}
\min_{\mathbf{P} \in \mathbb{R}^{n \times n}} & \sum_{i=1}^{n} \sum_{j=1}^{n} C_{ij} P_{ij} \\
\text{subject to} \quad & P_{ij} \geq 0  \quad \quad \quad  \quad \quad (i, j = 1, \ldots , n) \\
& \sum_{j=1}^{n} P_{ij} = a_i  \quad \quad \quad (i = 1, \ldots , n) \\
& \sum_{i=1}^{n} P_{ij} = b_j  \quad \quad \quad (j = 1, \ldots, n)
\end{aligned}
$$

---

#### 例 (P.28)

カテゴリ: 犬, 猫, 虎, 鳥

分布は,

$$ \mathbf{a} = \begin{pmatrix} 0.2 \\ 0.5 \\ 0.2 \\ 0.1 \end{pmatrix}, \mathbf{b} = \begin{pmatrix} 0.3 \\ 0.3 \\ 0.4 \\ 0.0 \end{pmatrix}$$

---

コスト関数は,

$$ \mathbf{C} = 
\begin{pmatrix}
0 & 2 & 2 & 2 \\
2 & 0 & 1 & 2 \\
2 & 1 & 0 & 2 \\
2 & 2 & 2 & 0
\end{pmatrix} $$

> 猫と虎は似たカテゴリのため 輸送コストが低いとしている

最適輸送行列は,


$$
\mathbf{P} = \begin{pmatrix} 
0.2 & 0.0 & 0.0 & 0.0 \\
0.0 & 0.3 & 0.2 & 0.0 \\
0.0 & 0.0 & 0.2 & 0.0 \\
0.1 & 0.0 & 0.0 & 0.0
\end{pmatrix}
$$


---

## 連続分布に対する定式化

### 輸送の定め方

1. 点群比較の場合を考える

輸送行列 $\mathbf{P}$ は 確率ベクトル $\mathbf{a}, \mathbf{b}$ を周辺分布に持つ同時分布の表と解釈できる

> $\mathbf{P} \mathbb{1}_m = \mathbf{a}, \mathbf{P}^{\mathrm{T}} \mathbb{1}_n = \mathbf{b}$ より,
> $$
\begin{aligned}
\mathbf{P} = 
& \begin{pmatrix}
  P_{11} & \cdots & P_{1m} \\
  \vdots & \ddots & \vdots \\
  P_{n1} & \cdots & P_{nm}
\end{pmatrix}
\to 
\begin{pmatrix}
  a_1\\
  \vdots \\
  a_n
\end{pmatrix} \\
& \quad \qquad \ \ \downarrow
\\
& \ \begin{pmatrix}
\, b_1 & \ \ \cdots \ \ & b_m \,
\end{pmatrix}
\end{aligned}
$$

---

2. 逆に同時分布を輸送量とする

$\alpha$, $\beta$ を周辺分布に持つ同時分布 $\pi$ を考える.

$\alpha$, $\beta$ は $\mathcal{X}$ 上の分布であり, $\pi$ は $\mathcal{X} \times \mathcal{X}$ 上の分布

制約条件は点群のときと同様 3式で,

$$
\begin{align}
& \pi(\mathcal{A} \times \mathcal{B}) \geq 0 \quad & (\forall \mathcal{A}, \mathcal{B} \in \mathcal{F}(\mathcal{X})) & \qquad \\
& \pi(\mathcal{A} \times \mathcal{X}) = \alpha(\mathcal{A}) & (\forall \mathcal{A} \in \mathcal{F}(\mathcal{X})) & \quad \tag{2} \\
& \pi(\mathcal{X} \times \mathcal{B}) = \beta(\mathcal{B}) & (\forall \mathcal{B} \in \mathcal{F}(\mathcal{X})) & \quad \tag{3} \\
\end{align}
$$

> $\mathcal{F}$ は 標本空間上の $\sigma$-加法族

---

#### (1) $\pi(\mathcal{A} \times \mathcal{B}) \geq 0 \qquad (\forall \mathcal{A}, \mathcal{B} \in \mathcal{F}(\mathcal{X}))$

$\mathcal{A}$ から $\mathcal{B}$ へと運ばれる質量が非負である

> 点群比較では: $P_{ij} \geq 0 \quad (i, j = 1, \ldots , n)$

$\\$
#### (2) $\pi(\mathcal{A} \times \mathcal{X}) = \alpha(\mathcal{A})$, (3) $\pi(\mathcal{X} \times \mathcal{B}) = \beta(\mathcal{B})$

> 点群比較では: $\sum_{j=1}^{m} P_{ij} = a_i , \quad \sum_{i=1}^{n} P_{ij} = b_j$


---

このときコストは

$$
\mathbb{E}_{(X, Y) \sim \pi(X, Y)} [C(X, Y)]
$$

と, "$(X, Y) \sim \pi(X, Y)$ となるときのコストの期待値" として書ける


輸送コストの最小化は, $\mathcal{P}(\mathcal{X} \times \mathcal{X})$ を $\mathcal{X} \times \mathcal{X}$ 上の確率分布全体とすると,

$$
\min_{\pi \in \mathcal{P}(\mathcal{X} \times \mathcal{X})} \mathbb{E}_{(X, Y) \sim \pi(X, Y)} [C(X, Y)]
$$

---

### 期待値の書き換え

> 目標: $\displaystyle \mathbb{E}_{(X, Y) \sim \pi(X, Y)} [C(X, Y)] = \int_{\mathcal{X} \times \mathcal{X}} C(x, y) \ d \pi(x, y)$

$\pi$ の PDF を $f_{XY}$ とすれば

$$
\begin{align*}
\pi (\mathcal{A} \times \mathcal{B}) & = \int_{\mathcal{A} \times \mathcal{B}} f_{XY} (x, y) \ dx dy \\
& = \int_{\mathcal{A}} \int_{\mathcal{B}} f_{XY} (x, y) \ dx dy
\end{align*}
$$

---

このときの期待値は,

$$
\mathbb{E}[C(X, Y)] = \int_{- \infty}^{\infty} \int_{- \infty}^{\infty} C(x, y) f_{XY} (x, y) \ dx dy
$$

ラドンニコディム微分は,

$$
d \pi(x, y) = f_{XY} (x, y) \ dx dy
$$

と表され,

$$
\mathbb{E}[C(X, Y)] = \int_{\mathcal{X} \times \mathcal{X}} C(x, y) \ d \pi(x, y)
$$


---

### 完成形


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

### この定式化における離散分布の扱い

離散分布 $\alpha = \sum_{i=1}^{n} a_i \delta_{x_i}$ は, 関数 $\alpha : \mathcal{F}(\mathcal{X}) \to \mathbb{R}$ を

$$
\alpha (\mathcal{A}) = \sum_{x_i \in \mathcal{A}} a_i
$$

とすると, 確率測度 $\alpha$ によって表現できる

## 実際には

一般には 同時確率分布 $\pi$ は無限サイズであるため, そのまま計算するのは難しいため,

1. 経験分布を用いた点群比較

> 連続分布に対する定式化は 連続分布と離散分布を区別せず扱えるため 有用である

2. ニューラルネットワークでモデリングして解く
