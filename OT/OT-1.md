---
marp: true
math: KaTeX
style: |
  section {
    font-family: sans-serif, 'BIZ UDGothic';
    font-size: 32px;
  }
---

# 第1回 (2026-05-18)

## 最適輸送 (Optimal Transport)

2つの確率分布を比較する手法の一つ

$\\$

**機械学習**: モデルが出力した分布と教師/訓練データの分布を比較  
$\quad$ $\quad$ $\quad$ $\to$ モデルの評価に利用できる  
**統計**: 2つの標本が同じ母集団を持つかの検定などに利用できる

---

## ヒストグラム

カテゴリ分け
例: { 犬: 0.2, 猫: 0.5, 虎: 0.2, 鳥: 0.1 } という確率分布

### 比較方法

確率ベクトルの比較 

> カテゴリ1, 2, ... , nを取る確率 : $a_1, a_2, \ldots, a_n$
> 確率ベクトル $\mathbf{a} := [a_1, a_2, \ldots, a_n]^{\mathrm{T}} \in \mathbb{R}^n$

## 点群

取りうる点とその点を取る確率を結びつけたもの
多くの場合 全ての点の確率が等しいとしてプロットを作る

---

## 連続分布

サンプルを取り出して 点群として比較する

経験分布: i.i.dサンプリング(各標本がi.i.d) で各点が等確率

## 輸送の理解

各点をもう一つの分布と等しくなる様に運び, その運搬距離 (コスト) を最小にしたい

### 点群の数が異なる場合

各点の確率を「質量」とみなし分割して複数の点に運ぶ と考える

---

## 最適輸送法の利点

1. 距離を要素に持つ
    - 単に輸送元と輸送先の差 (輸送量) だけで比較するのではなく，  
2点の距離 (輸送コスト) も考え 比較できる
    - カテゴリ分けのヒストグラムでは，2つのカテゴリの距離を定めれば良い
2. 適切な条件下で距離とできる
    - ワッサースタイン距離 と呼ぶ (詳しくは後述)
    - 距離の公理 (特に三角不等式) を満たす

---

### 距離の公理

一般の距離空間 $(\mathcal{X}, d)$ で，$\forall x, y, z \in \mathcal{X}$ について，
> $\mathcal{X}$ : 集合, $\mathcal{X}$ 上の距離 $d : \mathcal{X} \times \mathcal{X} \to \mathbb{R}$

1. 非負性

$$
\begin{aligned}
d(x, y) &\geq 0 \\
d(x, y) &= 0 \Leftrightarrow x = y
\end{aligned}
$$

2. 対称性

$$
d(x, y) = d(y, x)
$$

3. 三角不等式

$$
d(x, y) \leq d(x, z) + d(z, y)
$$

---

## 線形計画問題としての定式化 (点群の場合)

$\\$

$$
\begin{align}
& \min_{\mathbf{P} \in \mathbb{R}^{n \times m}} \sum_{i=1}^{n} \sum_{j=1}^{m} C(x_i, y_j) \mathbf{P}_{ij} \\
\text{subject to} \quad & \mathbf{P}_{ij} \geq 0  \quad \quad \quad  \quad \quad (i = 1, \ldots , n , \  j = 1, \ldots, m) \\
& \sum_{j=1}^{m} \mathbf{P}_{ij} = \mathbf{a}_i  \quad \quad \quad (i = 1, \ldots , n) \\
& \sum_{i=1}^{n} \mathbf{P}_{ij} = \mathbf{b}_j  \quad \quad \quad (j = 1, \ldots, m)
\end{align}
$$

---

### 目的関数 : $\displaystyle \min_{\mathbf{P} \in \mathbb{R}^{n \times m}} \sum_{i=1}^{n} \sum_{j=1}^{m} C(x_i, y_j) \mathbf{P}_{ij}$

$\\$
空間 $\mathcal{X}$ 上の2つの重み付き点群 $\displaystyle \quad \alpha = \sum_{i=1}^{n} a_i \delta_{x_i} ,\ \beta = \sum_{j=1}^{m} b_j \delta_{y_j}$

> $\alpha$ : 点 $x_i$ を 確率 $a_i$ でとる **確率分布**
> $\delta_{x_i}$ : ディラック測度, 点 $x_i$ を確率1でとる確率分布

---

コスト $C(x_i, y_j) \in \mathbb{R}$ を コスト関数 $C : \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ で定める

$\quad$ 例 : $C(x, y) = \| x-y \|_2$

$\\$

$\mathbf{P}_{ij}$ : 点 $x_i$ から 点 $y_j$ への輸送量

$\\$

$\displaystyle \sum_{i=1}^{n} \sum_{j=1}^{m} C(x_i, y_j) \mathbf{P}_{ij}$ : 総輸送コスト

---


### 条件 1 : $\mathbf{P}_{ij} \geq 0 \quad(i = 1, \ldots , n  , \  j = 1, \ldots, m)$

$\\$
輸送行列 $\mathbf{P} \in \mathbb{R}^{n \times m}$

$$
\mathbf{P} := 
\begin{pmatrix}
  \mathbf{P}_{11} & \cdots & \mathbf{P}_{1m} \\
  \vdots & \ddots & \vdots \\
  \mathbf{P}_{n1} & \cdots & \mathbf{P}_{nm}
\end{pmatrix}
$$

---

### 条件 2 : $\sum_{j=1}^{m} \mathbf{P}_{ij} = \mathbf{a}_i \quad (i = 1, \ldots , n )$


- 点 $x_i$ から輸送される総量が $x_i$ をとる確率 $a_i$ と一致

$\\$

### 条件 3 : $\sum_{i=1}^{n} \mathbf{P}_{ij} = \mathbf{b}_j \quad (j = 1, \ldots, m)$
- 点 $y_j$ に輸送される総量が $y_j$ をとる確率 $b_j$ と一致

$\\$

> 条件 2, 3 は 質量保存制約 と呼ばれる

---

### 質量保存制約の書き換え

- 条件 2 : $\mathbf{P} \mathbb{1}_m = \mathbf{a}$
- 条件 3 : $\mathbf{P}^{\mathrm{T}} \mathbb{1}_n = \mathbf{b}$

$$
\begin{aligned}
\mathbf{P} \mathbb{1}_m &= 
\begin{pmatrix}
  \mathbf{P}_{11} & \cdots & \mathbf{P}_{1m} \\
  \vdots & \ddots & \vdots \\
  \mathbf{P}_{n1} & \cdots & \mathbf{P}_{nm}
\end{pmatrix}
\begin{pmatrix}
  1 \\
  \vdots \\
  1
\end{pmatrix}
\\
&= 
\begin{pmatrix}
  \mathbf{P}_{11} + \cdots + \mathbf{P}_{1m} \\
  \vdots \\
  \mathbf{P}_{n1} + \cdots + \mathbf{P}_{nm}
\end{pmatrix}
=
\begin{pmatrix}
  \mathbf{a}_1 \\
  \vdots \\
  \mathbf{a}_n
\end{pmatrix}


\end{aligned}
$$


---

最適輸送行列 : 目的関数を最小化する輸送行列 $\mathbf{P}$

最適輸送コスト $\mathrm{OT}(\alpha, \beta, C)$ : 最適輸送行列における 総輸送コスト

$\\$
$\\$

輸送多面体 $\mathcal{U} (a, b)$ : 制約条件を満たす行列全体の集合

$$
\mathcal{U} (a, b) := \{ \mathbf{P} \in \mathbb{R}^{n \times m} : \mathbf{P}_{ij} \geq 0, \mathbf{P} \mathbb{1}_m = \mathbf{a}, \mathbf{P}^{\mathrm{T}} \mathbb{1}_n = \mathbf{b} \}
$$

---

#### (輸送多面体は非空である証明)

$\\$

$\tilde{\mathbf{P}} = \mathbf{ab}^{\mathrm{T}} \in \mathbb{R}^{n \times m}$ とする．
$\mathbf{a}_i, \mathbf{b}_j$ は確率で非負値のため， $\tilde{\mathbf{P}}_{ij} = \mathbf{a}_i \mathbf{b}_j \geq 0 \quad$ (条件 1)
$\tilde{\mathbf{P}} \mathbb{1}_m = \mathbf{a} \mathbf{b}^{\mathrm{T}} \mathbb{1}_m \stackrel{(*)}{=} \mathbf{a}, \quad \tilde{\mathbf{P}}^{\mathrm{T}} \mathbb{1}_n = \mathbf{b} \mathbf{a}^{\mathrm{T}} \mathbb{1}_n \stackrel{(*)}{=} \mathbf{b}$ より条件2, 3も満たす

$\mathcal{U}$ は，$\tilde{\mathbf{P}}$ を必ず要素に含むため，常に非空

> $(*)$ :
> $$
\begin{aligned}
\mathbf{b}^{\mathrm{T}} \mathbb{1}_m &= 
\begin{pmatrix}
  b_1, \cdots, b_m
\end{pmatrix}
\begin{pmatrix}
  1 \\
  \vdots \\
  1
\end{pmatrix}
= b_1 + \ldots + b_m
= 1
\end{aligned}
$$

---

#### 目的関数の書き換え

コスト行列 $\mathbf{C} \in \mathbb{R}^{n \times m}$ : コスト関数の点群上の値を $\mathbf{C}_{ij} = C(x_i, y_j)$ と表したもの

内積を用いると，
$$
\left\langle \mathbf{C}, \mathbf{P} \right\rangle = \sum_{i=1}^{n} \sum_{j=1}^{m} C(x_i, y_j) \mathbf{P}_{ij}
$$
と表せるため，最適輸送コストは，

$$\mathrm{OT}(\alpha, \beta, C) = \min_{\mathbf{P} \in \mathcal{U}(a, b)} \left\langle \mathbf{C}, \mathbf{P} \right\rangle$$
