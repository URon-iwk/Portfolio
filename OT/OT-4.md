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


# 第4回 (2026-10-19)


## 前回

ワッサースタイン距離が実際に距離であることを確認した.

> **p-ワッサースタイン距離:**
> $\quad$$\mathcal{X}$ 上の距離関数 $d: \mathcal{X} \times \mathcal{X} \to \R$ と $p \geq 1 \in \R$, 
> $\quad$コスト関数が $C(x, y)= d(x, y)^p$ であるとき,
> $\quad$$\mathcal{X}$ 上の確率分布 $\alpha, \ \beta$ について
> $$W_p (\alpha, \beta) := \text{OT}(\alpha, \beta, C)^{\frac{1}{p}}$$

---

## 双対問題とLagrange緩和

主問題: 総輸送コストを最小化する輸送方法を求める (第1回&第2回)
双対問題: 主問題の最適値の下界の最大化を目指す

双対問題に求められる性質:

- 双対問題の最大値が主問題の最適値以下であることが保証される.
- 主問題より簡単に解ける

双対問題の最大値 $m$ 総輸送コストの一つ $L_P (\mathbf{P})$ が近ければ, 代替の最適値とみなし, 主問題の解のよさを考えることができる

$$
m \leq \mathrm{OT} \leq L_P (\mathbf{P})
$$


---

### 点群比較の場合

主問題:

$$
\begin{align*}
& \min_{\mathbf{P} \in \mathbb{R}^{n \times m}} \sum_{i=1}^n \sum_{j=1}^m C_{ij} P_{ij} \\
& \text{subject to } P_{ij} \geq 0 , \sum_{j=1}^{m} P_{ij} = a_i , \sum_{i=1}^{n} P_{ij} = b_j
\end{align*}
$$

双対問題:

$$
\begin{align*}
& \max_{\boldsymbol{u} \in \mathbb{R}^n, \boldsymbol{v} \in \mathbb{R}^m} \sum_{i=1}^n u_i a_i + \sum_{j=1}^m v_j b_j \\
& \text{subject to } u_i + v_j \leq C_{ij}
\end{align*}
$$

---

#### 主問題と双対問題の対応

.|主問題|双対問題
-|-|-
目的 | 最小化 | 最大化
変数 | $P_{ij}$ | $u_i, \ v_j$
変数の制約条件 | $P_{ij} \geq 0$ | $u_i + v_j \leq C_{ij}$
$C_{ij}$ | 目的関数の係数 | 制約条件の右辺
$a_i$ | 制約条件の右辺 | 目的関数の係数
$b_j$ | 制約条件の右辺 | 目的関数の係数

---

#### 効果

1. 変数の数が $n \times m$ から $n + m$ に変わることで計算が簡単になる場合がある
   - ただし, 制約条件が $n \times m$ 本になるため, 一般には言えない
2. $a_i, \ b_j$ が係数になることで $a_i, b_j$ が輸送に与える影響を把握できる
3. 最適輸送を解くアルゴリズムに双対問題の構造を用いるものが存在する

---

### 一般の場合

主問題:

$$
\begin{align*}
& \min_{\pi \in  \mathcal{P}(\mathcal{X} \times \mathcal{X})}  \int_{\mathcal{X} \times \mathcal{X}} C(x, y) \ d \pi(x, y) \\
& \text{subject to } \pi(\mathcal{A} \times \mathcal{B}) \geq 0, \pi(\mathcal{A} \times \mathcal{X}) = \alpha(\mathcal{A}) , \pi(\mathcal{X} \times \mathcal{B}) = \beta(\mathcal{B})
\end{align*}
$$

双対問題: $\varphi , \psi : \mathcal{X} \to \mathbb{R}$

$$
\begin{align*}
& \max_{\varphi, \psi} \int_{\mathcal{X}} \varphi (x) \ d \alpha (x) + \int_{\mathcal{X}} \psi (y) \ d \beta(y) \\
& \text{subject to } \varphi (x) + \psi (y) \leq C(x, y)
\end{align*}
$$


> 参考: R. Cristoferi, *Optimal Transport Lecture Notes*, $\S$ 4.1, pp. 36-37.

---

### 点群比較の双対問題の構成 (Lagrange緩和)

一般形の最適化問題は以下のように書ける

$$
\begin{align*}
\min_{\boldsymbol{x} \in \mathbb{R}^d} f(\boldsymbol{x}) \\
\text{subject to } & g_i (\boldsymbol{x}) \leq 0 \quad (i = 1, \ldots, n) \\
& h_j (\boldsymbol{x}) = 0 \quad (j = 1, \ldots, m)
\end{align*}
$$

---

ここで, 関数 $\mathcal{I}_{\leq 0} (x), \mathcal{I}_{0} (x) : \mathbb{R} \to \mathbb{R} \cup \{\infty\}$ を以下とすると,

$$
\begin{align*}
\mathcal{I}_{\leq 0} (x) & := \begin{cases} 0, & x \leq 0, \\ \infty & x > 0 \end{cases} \\
\mathcal{I}_{0} (x) & := \begin{cases} 0, & x=0 \\ \infty, & x \neq 0 \end{cases}
\end{align*}
$$

最適化問題は以下のように書き換えられる

$$
\min_{\boldsymbol{x} \in \mathbb{R}^d} f(\boldsymbol{x}) + \sum_{i=1}^n \mathcal{I}_{\leq 0} (g_i (\boldsymbol{x})) + \sum_{j=1}^m \mathcal{I}_{0} (h_j (\boldsymbol{x}))
$$

> 制約がすべて満たされれば  $\mathcal{I}_{\leq 0} (g_i (\boldsymbol{x})), \mathcal{I}_{0} (h_j (\boldsymbol{x}))$ 共に $0$
> 一つでも違反するといずれかが無限大となる

---

任意の $\mu \geq 0, \ \nu \in \mathbb{R}$ を用いると,

$$\mu x \leq \mathcal{I}_{\leq 0} (x), \nu x  \leq \mathcal{I}_{0} (x)$$

より, 関数 $\mathcal{I}_{\leq 0} (x), \mathcal{I}_{0} (x)$ の下界を作り出せ,

$$
\min_{\boldsymbol{x} \in \mathbb{R}^d} f(\boldsymbol{x}) + \sum_{i=1}^n \mu_i g_i (\boldsymbol{x}) + \sum_{j=1}^m \nu_j h_j (\boldsymbol{x})
$$

は, $\displaystyle \min_{\boldsymbol{x} \in \mathbb{R}^d} f(\boldsymbol{x}) + \sum_{i=1}^n \mathcal{I}_{\leq 0} (g_i (\boldsymbol{x})) + \sum_{j=1}^m \mathcal{I}_{0} (h_j (\boldsymbol{x}))$ の最適値の下界となっている

これを Lagrange関数 $L (\boldsymbol{x}, \mu_1, \ldots, \mu_n, \nu_1, \ldots, \nu_m)$ という.

---

Lagrange関数の $\mu_1, \ldots, \mu_n, \nu_1, \ldots, \nu_m$ を変数とする関数 $G$ を
Lagrange双対関数といい, 以下として定義される

$$
G (\mu_1, \ldots, \mu_n, \nu_1, \ldots, \nu_m) := \min_{\boldsymbol{x} \in \mathbb{R}^d} L (\boldsymbol{x}, \mu_1, \ldots, \mu_n, \nu_1, \ldots, \nu_m)
$$


> Lagrange双対関数は $g$ で書かれることが標準的だが, 経済数学の教科書 (p. 33, 補遺) と記号を統一するため $g$ は制約条件に用いることとした.

Lagrange双対関数はその定義から,

$$
G (\mu_1, \ldots, \mu_n, \nu_1, \ldots, \nu_m) \leq \min_{\boldsymbol{x} \in \mathbb{R}^d} f(\boldsymbol{x})
$$

---

$G$ の最大化は, 下界をできるだけ主問題の最適値に近づけることに等しく,

$$
\max_{\substack{\mu_1, \ldots \mu_n \geq 0 \\ \nu_1, \ldots \nu_m \in \mathbb{R}}} G (\mu_1, \ldots, \mu_n, \nu_1, \ldots, \nu_m)
$$

これを Lagrange双対問題といい, 
$\mu_1, \ldots, \mu_n, \nu_1, \ldots, \nu_m$ をLagrange乗数 または 双対変数という.

---

この双対問題を点群比較に当てはめると, 制約条件は以下となり,

$$
\begin{align*}
P_{ij}  \geq 0 , \quad a_i - \sum_{j=1}^{m} P_{ij} = 0, \quad b_j - \sum_{i=1}^{n} P_{ij} = 0
\end{align*}
$$

Lagrange関数は, 双対変数 $\boldsymbol{u} \in \mathbb{R}^n , \boldsymbol{v} \in \mathbb{R}^m$ を用いて

$$
\begin{align*}
L(\mathbf{P}, \boldsymbol{u}, \boldsymbol{v}) &= \sum_{i=1}^n \sum_{j=1}^m C_{ij} P_{ij} + \sum_{i=1}^n u_i \left(a_i - \sum_{j=1}^{m} P_{ij}\right) + \sum_{j=1}^m v_j \left(b_j - \sum_{i=1}^{n} P_{ij}\right) \\
&= \sum_{i=1}^n \sum_{j=1}^m (C_{ij} - u_i - v_j) P_{ij} + \sum_{i=1}^n u_i a_i + \sum_{j=1}^m v_j b_j
\end{align*}
$$

> $P_{ij} \geq 0$ はLagrange関数には組み込まず制約条件のままにしておく

---

このLagrange関数を用いて主問題は, 制約条件 $P_{ij} \geq 0$ の下で

$$
\min_{\mathbf{P} \in \mathbb{R}^{n \times m}} \sum_{i=1}^n \sum_{j=1}^m (C_{ij} - u_i - v_j) P_{ij} + \sum_{i=1}^n u_i a_i + \sum_{j=1}^m v_j b_j
$$

であり, 第一項は, $C_{ij} - u_i - v_j \geq 0$ のとき $P_{ij} = 0$ が最小化を満たし,
$C_{ij} - u_i - v_j < 0$ のときは $P_{ij} \to \infty$ で $L(\mathbf{P}, \boldsymbol{u}, \boldsymbol{v}) \to -\infty$ となる.

Lagrange双対関数は,

$$
\begin{align*}
G(\boldsymbol{u}, \boldsymbol{v}) = \begin{cases} \sum_{i=1}^n u_i a_i + \sum_{j=1}^m v_j b_j, & C_{ij} - u_i - v_j \geq 0 \\  -\infty , & C_{ij} - u_i - v_j < 0\end{cases}
\end{align*}
$$

---

双対問題で求めたいのは, 主問題の最適値の下界の最大化,
$\displaystyle \max_{\boldsymbol{u} \in \mathbb{R}^n , \boldsymbol{v} \in \mathbb{R}^m} G(\boldsymbol{u}, \boldsymbol{v})$ であるので, 上の場合のみ考えれば良く,
これを書き直すと, 初めに示した式が得られる

> $$
\begin{align*}
& \max_{\boldsymbol{u} \in \mathbb{R}^n, \boldsymbol{v} \in \mathbb{R}^m} \sum_{i=1}^n u_i a_i + \sum_{j=1}^m v_j b_j \\
& \text{subject to } u_i + v_j \leq C_{ij}
\end{align*}
$$

また, 主問題が線形計画であるため, 強双対定理より以下が成り立ち,

$$
\max_{\boldsymbol{u} \in \R^n. \boldsymbol{v} \in \R^m} G(\boldsymbol{u}, \boldsymbol{v}) = \min_{\mathbf{P} \in \R^{n \times m}} L_P (\mathbf{P})
$$

主問題の代わりに双対問題を解くことで最適値を求めることができる

---

### 強双対定理の証明


#### 弱双対定理

$$
d^* = \max_{\substack{\mu_1, \ldots \mu_n \geq 0 \\ \nu_1, \ldots \nu_m \in \mathbb{R}}} G (\mu_1, \ldots, \mu_n, \nu_1, \ldots, \nu_m) \leq \min_{\boldsymbol{x} \in \mathbb{R}^d} f(\boldsymbol{x}) = p^*
$$

pf. これはLagrange緩和の構成から自明


---

#### Farkasの補題

> $\mathbf{A} \in \mathbb{R}^{n \times m}, \mathbf{b} \in \mathbb{R}^n$ とし, 以下はどちらか一方のみが成り立つ
> (1) $\mathbf{A} \boldsymbol{x} = \mathbf{b}, \ \boldsymbol{x} \geq 0$ を満たす $\boldsymbol{x} \in \mathbb{R}^m$ が存在する.
> (2) $\mathbf{A}^{\transpose} \boldsymbol{y} \geq 0, \mathbf{b}^{\transpose} \boldsymbol{y} < 0$ を満たす $\boldsymbol{y} \in \mathbb{R}^n$ が存在する.

pf. (1), (2) は同時に成立しない ( (1) $\Rightarrow \neg$ (2), (2) $\Rightarrow \neg$ (1) )

(1), (2) が同時に成立すると仮定すると,

$$0 > \mathbf{b}^{\transpose} \boldsymbol{y} = (\mathbf{A} \boldsymbol{x})^{\transpose} \boldsymbol{y} = \boldsymbol{x}^{\transpose} (\mathbf{A}^{\transpose} \boldsymbol{y})$$

これは, $\boldsymbol{x} \geq 0, \mathbf{A}^{\transpose} \boldsymbol{y} \geq 0$ より $\boldsymbol{x}^{\transpose} (\mathbf{A}^{\transpose} \boldsymbol{y}) \geq 0$ となり矛盾.

---

pf. (1) が成立しないとき, (2) は成立する

$K = \{ \mathbf{A} \boldsymbol{x} | \boldsymbol{x} \geq 0 \}$ とすると, $K$ は

$$
\begin{align*}
1. & \forall \boldsymbol{z} \in K, \exists \boldsymbol{x} \geq 0, \forall \alpha \in \mathbb{R}_{\geq 0} : \alpha \boldsymbol{z} = \mathbf{A} (\alpha \boldsymbol{x}) \in K \\
2. & \forall \boldsymbol{z}_1, \boldsymbol{z}_2 \in K, \exists \boldsymbol{x}_1, \boldsymbol{x}_2 \geq 0, \forall \lambda \in [0, 1] : \\
& \qquad \lambda \boldsymbol{z}_1 + (1- \lambda) \boldsymbol{z}_2 = \mathbf{A} (\lambda \boldsymbol{x}_1 + (1- \lambda) \boldsymbol{x}_2) \in K
\end{align*}
$$

より, 非空閉凸錐である. (閉性については略)

---

(1) が成立しないという仮定から, $\mathbf{b} \notin K$
凸集合の分離定理より,

$$
\begin{align*}
& \exists \mathbf{y} \in \mathbb{R}^n, \mathbf{y} \neq 0, \exists \alpha \in \mathbb{R}, \forall \boldsymbol{z} \in K: \\
& \qquad \mathbf{y}^{\transpose} \mathbf{b} < \alpha \leq \mathbf{y}^{\transpose} \boldsymbol{z}
\end{align*}
$$

$\boldsymbol{z} \in K$ より 任意の $t \geq 0$ で $t \boldsymbol{z} \in K$ となる.
$\mathbf{y}^{\transpose} \boldsymbol{z} < 0$ と仮定すると, $t \to \infty$ で $\mathbf{y}^{\transpose} (t \boldsymbol{z}) \to - \infty$ となり, 
$\alpha \leq \mathbf{y}^{\transpose} (t \boldsymbol{z})$ に矛盾するため, $\mathbf{y}^{\transpose} \boldsymbol{z} \geq 0$

---

ここで, $A_{j} \in \mathbb{R}^n$ は $e_j$ を j 行目のみ1, それ以外に 0 を成分としてもつベクトルとすると,
$A_{j} = A e_j \in K$

よって, $\boldsymbol{y}^{\transpose} A_j \geq 0$
これはすべての $j=1, \ldots, m$ で成立するため, $\boldsymbol{y}^{\transpose} A = A^{\transpose} \boldsymbol{y} \geq 0$

また, $0 \in K$ であるので, $\mathbf{y}^{\transpose} \mathbf{b} < \alpha \leq 0$ より, (2) が成立する

> 参考文献: 数理経済学的 SPECIAL PROJECT, "Farkasの補題の証明をわかりやすく解説" (https://www.met-sp.jp/proof-of-farkas-lemma)

--- 

#### 強双対定理

主問題を

$$\min c^{\transpose} x \ \text{subject to } Ax = b, \ x \geq 0$$

双対問題を

$$\max b^{\transpose} y  \ \text{subject to } A^{\transpose} y \leq c$$

とし, それぞれの実行可能な場合の最適値を $p^*, d^*$ とする.

弱双対定理より $d^* \leq p^*$

---

主問題が実行可能とすると, 有限の最適値 $p^*$ が存在する.

ここで $r < p^*$ となる任意の $r \in \R$ を取ると,

$$Ax = b, \ x \geq 0, \ c^{\transpose} x \leq r$$

を同時に満たす $x$ は存在しない.

このことは, ある実数 $s \geq 0$ が $c^{\transpose} x + s = r$ を満たすとすると,

$$
\begin{pmatrix} A & 0 \\ c^{\transpose} & 1 \end{pmatrix} \begin{pmatrix} x \\ s \end{pmatrix} = \begin{pmatrix} b \\ r \end{pmatrix}, \quad \begin{pmatrix} x \\ s \end{pmatrix} \geq 0
$$

に解が存在しないことに等しい

---

$z = \begin{pmatrix} y \\ t \end{pmatrix}, \ y \in \R^m, t \in \R$ としてFarkasの補題を適用すると

$$
\begin{align*}
\begin{pmatrix} A^{\transpose} & c \\ 0^{\transpose} & 1 \end{pmatrix} \begin{pmatrix} y \\ t \end{pmatrix} \geq 0 \\
\begin{pmatrix} b^{\transpose} & r \end{pmatrix} \begin{pmatrix} y \\ t \end{pmatrix} < 0
\end{align*}
$$

---

$t = 0$ とすると, $A^{\transpose} y \geq 0, \ b^{\transpose} y <0$

しかし, ある主問題の解 $x_0 \geq 0$ を考えると, $A x_0 = b$ であり,

$$
\begin{align*}
b^{\transpose} y &= (A x_0)^{\transpose} y \\
&= x_0^{\transpose} A^{\transpose} y \geq 0 \qquad(\because x_0 \geq 0, \ A^{\transpose} y \geq 0)
\end{align*}
$$

よって矛盾し, $t > 0$

---

Farkasの補題から得られた不等式を再度まとめると

$$
\begin{align*}
t > 0 \\
A^{\transpose} y + c t \geq 0  \tag{1} \\
b^{\transpose} y + r t < 0 \tag{2}
\end{align*}
$$

$t > 0$ より, (1), (2)はそれぞれ以下に変形できる

$$
\begin{align*}
- A^{\transpose} \frac{y}{t} \leq c \\
- b^{\transpose} \frac{y}{t} > r
\end{align*}
$$

---

$\lambda = - \frac{y}{t}$ とすれば,

$$
\begin{align*}
A^{\transpose} \lambda \leq c \tag{3} \\
b^{\transpose} \lambda > r \tag{4}
\end{align*}
$$

(3) は双対問題の制約条件に一致するため, $\lambda$ は双対問題の実行可能解とわかり,
(4) より, 双対問題の最適値 $d^*$ が $d^* > r$ を満たすことがわかる.

弱双対定理と合わせると $r < d^* \leq p^*$ であり, rは任意の $r<p^*$ となる実数であったので

$$d^* = p^*$$



> 参考文献: 梅谷俊治, "しっかり学ぶ数理最適化　モデルからアルゴリズムまで", 講談社出版, ISBN: 9784065212707
