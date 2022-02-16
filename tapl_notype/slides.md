---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# TaPL ふりかえり

「型システム入門」への入門

---

# アジェンダ

今日のアジェンダ

- 📝 第３章 型なし算術式 の振り返り
- 🎨 第５章 型なしラムダ計算 の振り返り

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# モチベーション

- 第３章「型なし算術式」は第８章「型付き算術式」のベースになっている
- 第５章「型なしラムダ計算」は第９章「単純型付きラムダ計算」のベースになっている

→ 第二部に進む前に、それぞれの議論の流れをおさらいしておく

---

# 第３章 型なし算術式

## 主な内容

- 「算術式」に関するメタ議論（3.1、3.2、3.4、3.5）
- 証明技法の紹介（3.3）

---

# 証明技法について

メタな定理を証明するときは、項に関する帰納法をよく使う

本書では、以下の３種類が紹介されている

- サイズに関する帰納法
- 深さに関する帰納法
- 構造に関する帰納法

特に、構造に関する帰納法が強力

（3.3を参照）

あと「導出に関する帰納法」とかもある

---

# 算術式とは

> 型システムやその性質について厳密に述べるには、プログラミング言語のより基本的な側面を形式的に取り扱うことから始める必要がある（p.17）

算術式や、ラムダ計算は「より基本的な側面を形式的に」取り出したもの

---

# 型なし算術式の導入

「構文」と「意味論」をおさえればよい

<div grid="~ cols-2 gap-16">
<div>
<strong>構文</strong> とは

- プログラミング言語として有効な記号列とそうでないものを区別するための規則

</div>
<div>
<strong>意味論</strong> とは

- 項の意味を決定するための規則
</div>
</div>

---

# 構文の定義

構文は次のような定義があるが、いずれも等価である（命題3.2.6）

1. 帰納的な項の定義（定義3.2.1）

$$
\begin{split}
(1) &\{\text{true, false, }0\} \subseteq \mathfrak{T} \\
(2) & t_1 \in \mathfrak{T} \Rightarrow \{\text{succ }t_1,\text{pred }t_1,\text{iszero }t_1\}\subseteq \mathfrak{T} \\
(3) & t_1 \in \mathfrak{T} \text{ and } t_2 \in \mathfrak{T} \text{ and } t_3 \in \mathfrak{T} \Rightarrow \text{if } t_1 \text{ then } t_2 \text{ else } t_3 \subseteq \mathfrak{T}
\end{split}
$$

2. 推論規則による項の定義（定義3.2.2） （割愛。p.19を参照）
3. 具体的な項の定義（定義3.2.3）

$$
\begin{array}{l}
S_0 &=& \phi\\
S_{i+1} &=& \{\text{true},\text{false},0\}\\
&&\cup\{\text{succ }t_1,\text{pred }t_1, \text{iszero }t_1|t_1\in S_i\}\\
&&\cup\{\text{if }t_1\text{ then }t_2 \text{ else }t_3|t_1,t_2,t_3\in S_i\}\\
&&\\
S &=& \cup_i S_i
\end{array}\\
$$

---

# 意味論について

意味論とは、項がどう評価されるかを形式的に定めるものである。

意味論の形式化には次の３つのアプローチがある

- 操作的意味論
    - > 項tの意味は、tを初期状態として動き始めた機械が到達する最終状態と考えられる(p24)
- 表示的意味論
- 公理的意味論

この本では主に、操作的意味論を用いる

---

# 操作的意味論の形式的定義

ブール式だけの操作的意味論が図3-1

<div grid="~ cols-2 gap-4">
<div>

構文：
$$
\begin{split}
t ::=&\\
&\text{true}\\
&\text{false}\\
&\text{if }t\text{ else }t\text{ then }t\\
v ::=&\\
&\text{true}\\
&\text{false}
\end{split}
$$
</div>
<div>

評価：
$$
\begin{split}
\text{if true then }t_2\text{ else }t_3 \rightarrow t_2\quad\text{(E-IFTrue)}\\
\text{if false then }t_2\text{ else }t_3 \rightarrow t_3\quad\text{(E-IFFalse)}\\
\frac{t_1 \rightarrow t_1}
{\text{if }t'_1\text{ then }t_2\text{ else }t_3\rightarrow\text{if }t'_1\text{ else }t_2\text{ then }t_3}\quad&\text{(E-IF)}
\end{split}
$$
</div>
</div>

左側は項および値の定義である。値は項の部分集合で、評価の最終結果になりうるもの。

右側は評価関係とよばれる。

---

# メタな議論

ここまでに導入した構文と操作的意味論（推論規則）をもとに、この「言語」についていくつかのメタ定理を証明する。

- 1ステップ評価の決定性（定理3.5.4）
- 正規型と値の同値性（定理3.5.7と定理3.5.8）
- (多ステップ評価関係における)正規型の一意性（定理3.5.11）
- 評価の停止性（定理3.5.12）

---

# 1ステップ評価の決定性（定理3.5.4）

「1ステップ評価」と「決定性」について

### 1ステップ評価（定義3.5.3）
<br>

> 1ステップ評価関係とは、図３−１の３つの規則を満たす、項に関する最小の二項関係である

平たく言うと、$t\rightarrow t'$は項tを図３−１のいずれかの規則でt'に変換できるということ

### 1ステップ評価の決定性

$$
t\rightarrow t'\text{ かつ }t\rightarrow t'' \Rightarrow t'=t''
$$

１ステップ評価した結果は一通りに定まるということ

証明は導出に関する帰納法を用いる

---

# 正規型と値の同値性（定理3.5.7と定理3.5.8）

正規型について


### 正規型の定義（定義3.5.6）
<br>

> 項tが<strong>正規型</strong>であるとは、当てはまる評価規則がない、つまり$t\rightarrow t'$となる$t'$が存在しないということである

<br>

### 定理3.5.7と定理3.5.8
<br>

- すべての値は正規型である（定理3.5.7）
- $t$が正規型であるならば$t$は値である（定理3.5.8）

証明は構造的帰納法を用いる

定理3.5.8は一般の体系では成り立たない（実行時エラー）

---

# 多ステップ評価関係における正規型の一意性（定理3.5.11）

### 多ステップ評価関係の定義（定義3.5.9）

多ステップ評価関係$\rightarrow^{\ast}$とは１ステップ評価の反射的推移的閉包である

(1) $t\rightarrow t'$ ならば $t\rightarrow^{\ast}t'$

(2) すべての$t$について$t\rightarrow^{\ast}t$

(3) $t\rightarrow^{\ast}t'$かつ$t'\rightarrow^{\ast}t''$ならば$t\rightarrow^{\ast}t''$

### 正規型の一意性（定理3.5.11）
<br>

> $t\rightarrow^{\ast}u$かつ$t\rightarrow^{\ast}u'$で、$u$と$u'$が両方正規型ならば、$u=u'$が成り立つ

---

# 評価の停止性（定理3.5.12）

定理3.5.12

> すべての項$t$に対して、ある正規型$t'$が存在し、$t\rightarrow^{\ast}t'$を満たす

証明には項のサイズに関する帰納法を用いる

これも一般に成り立つわけではない（無限ループとか）

---

# メタな議論(再掲)

以下のようなメタ定理を確認しました

- 1ステップ評価の決定性（定理3.5.4）
- 正規型と値の同値性（定理3.5.7と定理3.5.8）
- (多ステップ評価関係における)正規型の一意性（定理3.5.11）
- 評価の停止性（定理3.5.12）

ここまではboolのみの体系だったが、ここから算術式を含む体系に拡張する

---

# 型なし算術式

図3-2

<div grid="~ cols-2 gap-4">
<div>
新しい構文形式

$$
\begin{split}
t ::=& ...\\
&0\\
&\text{succ }t\\
&\text{pred }t\\
&\text{iszero }t\\
v ::=&...\\
&\text{nv}\\
nv ::=&...\\
&0\\
&\text{succ nv}
\end{split}
$$
</div>
<div>
新しい評価規則

$$
\begin{split}
&\frac{t_1\rightarrow t'_1}{\text{succ }t_1\rightarrow\text{succ }t'_1}\quad\text{(E-Succ)}\\
&\text{pred }0 \rightarrow 0\quad\text{(E-Zero)}\\
&\text{pred}(\text{succ nv}_1) \rightarrow\text{nv}_1\quad\text{(E-PredSucc)}\\
&\frac{t_1\rightarrow t'_1}{\text{pred }t_1\rightarrow \text{pred }t'_1}\quad\text{(E-Pred)}\\
&\text{iszero }0\rightarrow \text{true}\quad\text{(E-IsZeroZero)}\\
&\text{iszero}(\text{succ nv})\rightarrow \text{false}\quad\text{(E-IsZeroSucc)}\\
&\frac{t_1\rightarrow t'_1}{\text{iszero }t_1\rightarrow\text{iszero }t'_1}\quad\text{(E-IsZero)}
\end{split}
$$
</div>
</div>

これは「新しく追加される規則」

---

# 各種メタ定理について

boolのみの体系で成り立っていた定理が算術式に拡張した時に成り立つかどうか

### 対応表

| | |
| --- | --- |
| 1ステップ評価の決定性 | 成り立つ |
| 値と正規型の同値性 | 成り立たない |
| 多ステップ評価における、正規型の一意性 | 成り立つ |
| 評価の停止性 | 成り立つ |

---

# 行き詰まり状態

行き詰まり状態とは

「値と正規型の同値性」は算術式に拡張した体系では成り立たない。

これは例えば$\text{succ true}$のように、正規型であるが値ではない項が存在するからである。これを<strong>行き詰まり状態</strong>と呼ぶ（定理3.5.15）

行き詰まり状態は大事↓


> 行き詰まり状態の項は無意味または間違ったプログラムに対応する。したがって、項を実際に評価せずに、その項の評価が決して行き詰まり状態になら<strong>ない</strong>ことをいいたい。そのために、数値に評価される項（pred、succ、iszeroの引数にできるのはこれだけ）と、ブール値に評価される項（条件式の条件部分となれるのはこれだけ）とを区別できる必要がある。項をこのように分類するために、二つの<strong>型</strong><strong>Nat</strong>と<strong>Bool</strong>を導入する。（p69）

算術式は以上

---

# 第５章　型なしラムダ計算

主な内容

- ラムダ計算の導入（5.1、5.3）
    - 構文
    - 操作的意味論
        - 簡約の規則
        - 代入の扱い
- ラムダ計算に慣れる（5.2）

---

# ラムダ計算

ラムダ計算とは

関数定義と関数適用を抽象化したもの

ラムダ計算の構文は以下の通り


$$
\begin{array}{l}
t ::=&&\\
&x\quad\\
&\lambda x.t\\
&t\quad t&
\end{array}
$$


シンプルな定義だが、これだけで３章の算術式と同等以上の表現能力を持つ

---

# ラムダ計算の操作的意味論

構文の次は操作的意味論を導入する

ラムダ計算の操作的意味論は

1. 評価順序（どの項をどの順番で適用するか）
1. 代入規則（適用時に項をどう置き換えるか）

について考える必要がある

---

# ラムダ計算の評価順序（評価戦略）

> ラムダ計算のための評価戦略には数種類あり、長年にわたってプログラミング言語の設計者や理論家が研究してきた。（p42）

- 完全ベータ簡約
    - 任意の簡約基をいつでも簡約してよい
- 非正格な評価（遅延評価）
    - 正規順序
        - 最も左かつもっとも外側の簡約基を最初に簡約する
    - 名前呼び
        - 抽象の内部で簡約しない。関数適用における被適用項も簡約しない（多分）
    - 必要呼び
        - 名前呼びに加え、一度簡約した結果をメモ化しておき、利用するというもの
        - Haskell、Rで採用されている
- 正格な評価
    - 値呼び
        - ほとんどの言語で採用されている

---

# ラムダ計算における代入の形式的な定義

代入は難しい。単純に置き換えればいいかというと、そうでもない。

1. 束縛変数を誤って置換してしまうパターン

$$
[x\mapsto y](\lambda x.x) = \lambda x.y \quad\text{まちがい}
$$

2. 変数捕獲が起こるパターン

$$
[x\mapsto z](\lambda z.x) = \lambda z.x \quad\text{これもまちがい}
$$

---

# ラムダ計算における代入の形式的な定義

つづき

要するに、$[x\mapsto s]t$という代入を行う際は

1. xがtの束縛変数かどうか
2. sの自由変数がtの束縛変数かどうか

をチェックする必要がある

特に、sの自由変数がtの束縛変数だった場合は項中の変数名を一貫して置き換えて（アルファ変換）変数束縛が起こらないようにする。

> 慣習5.3.4 束縛変数の名前のみが異なる項は、任意の文脈で置き換え可能である


---

# ラムダ計算における代入の形式的な定義

つづき

これらを踏まえた代入の定義が以下

定義5.3.5

$$
\begin{split}
&[x\mapsto s]x = s\\
&[x\mapsto s]y = y\quad y\neq x\text{の場合}\\
&[x\mapsto s](\lambda y.t_1)=\lambda y.[x\mapsto s]t_1\quad y\neq x\text{かつ}y\notin FV(s)\text{の場合}\\
&[x\mapsto s](t_1\quad t_2)=([x\mapsto s]t_1)([x\mapsto s]t_2)
\end{split}
$$

---

# ラムダ計算における操作的意味論

個々までの議論を踏まえて

<div grid="~ cols-2 gap-4">
<div>
構文：

$$
\begin{split}
t ::=&\\
&x\\
&\lambda x.t\\
&t\quad t\\
v ::=&\\
&\lambda x.t
\end{split}
$$
</div>
<div>
評価：

$$
\begin{split}
\frac{t_1\rightarrow t'_1}{t_1\quad t_2\rightarrow t'_1\quad t_2}&\quad\text{(E-App1)}\\
\frac{t_2\rightarrow t'_2}{v_1\quad t_2\rightarrow v_1\quad t'_2}&\quad\text{(E-App2)}\\
(\lambda x.t_{12})v_2\rightarrow [x\mapsto v_2]t_{12}&\quad\text{(E-AppAbs)}
\end{split}
$$
</div>
</div>

E-App1、E-App2によって、値呼びが表現されている

---

# ラムダ計算の表現能力

- Church数
- Churchブール
- $\lambda NB$
- 再帰

詳しいことは割愛（5.2を見てください）

算術式と同等以上の計算能力があるということ。

---
layout: center
class: text-center
---

このスライドはslidevを使って作成した

https://sli.dev

---
layout: image
image: ./picture/sleeping_cat.jpg
---

# つかれた
