

基于上传的 Tom Leinster《Basic Category Theory》PDF（目录及前72页详细内容），全书总结如下。

---

## 1. 全书核心研究问题与写作脉络

### 核心研究问题
本书的核心议题是数学中无处不在的**泛性质（universal property）**。作者在开篇即指出：泛性质是“对任意…存在唯一的…”这类命题的精确表述，它在一个范畴内通过映射关系来刻画对象，且在同构意义下唯一确定该对象。

### 写作脉络
全书采用“**一种核心思想，三种等价视角，最终统一**”的结构：

1. **引论（Introduction）**：通过单点集、整数环 $\mathbb{Z}$、自由向量空间、张量积、群同态的核、van Kampen 定理等初等例子，直观展示泛性质的“存在唯一映射”特征，并强调**泛性质在同构意义下唯一确定对象**。
2. **基础语言（Ch1）**：建立范畴、函子、自然变换的基本词汇，为严格讨论泛性质提供语法。
3. **三种严格视角展开泛性质**：
   - **Ch2 伴随函子（Adjoints）**：泛性质的第一种严格语言，通过 Hom 集自然同构、unit/counit、comma 范畴中的初始对象三种等价方式刻画。
   - **Ch4 可表函子（Representables）**：第二种语言，通过 Yoneda 引理将对象与其所表示的 Hom 函子等同。
   - **Ch5 极限（Limits）**：第三种语言，通过锥（cone）的终对象来统一各种数学构造（积、等化子、拉回等）。
4. **统一与深化（Ch6）**：证明三种视角的深刻联系——极限可用可表函子和伴随描述；伴随函子与极限/余极限有基本交互（右伴随保持极限，左伴随保持余极限）；并以**一般伴随函子定理（General Adjoint Functor Theorem）**收束全书。

---

## 2. 分章节核心概念、公理/定义、关键定理

### Introduction（引论）
- **核心概念**：泛性质（universal property）的直观。
- **关键思想**：
  - “存在唯一（there exists a unique）”是范畴论的核心句式。
  - 满足同一泛性质的两个对象未必相等，但必然**同构**（Lemma 0.3）。
- **典型例子**：终对象（单点集）、初始对象（$\mathbb{Z}$ 作为初始环）、自由向量空间（基的泛性质）、张量积、核的泛性质、van Kampen 定理。

---

### Chapter 1: Categories, Functors and Natural Transformations

#### 1.1 Categories（范畴）
- **定义 1.1.1（范畴的公理）**：一个范畴 $\mathscr{A}$ 由对象类 $\mathrm{ob}(\mathscr{A})$、态射集 $\mathscr{A}(A,B)$、复合运算 $\circ$ 及单位态射 $1_A$ 组成，满足：
  - **结合律**：$(h\circ g)\circ f = h\circ(g\circ f)$；
  - **单位律**：$f\circ 1_A = f = 1_B\circ f$。
- **核心概念**：交换图（commutative diagram）、离散范畴（discrete category，仅有恒等态射）、单对象范畴（与群/幺半群一一对应）、预序集（preordered set，视为范畴）。
- **典型例子**：$\mathbf{Set}$（集合与映射）、$\mathbf{Grp}$（群与群同态）、$\mathbf{Ring}$（环与环同态）、$\mathbf{Top}$（拓扑空间与连续映射）。

#### 1.2 Functors（函子）
- **定义 1.2.1（函子）**：函子 $F:\mathscr{A}\to\mathscr{B}$ 是对象与态射的映射，保持复合与单位元：$F(g\circ f)=F(g)\circ F(f)$，$F(1_A)=1_{F(A)}$。
- **核心概念与例子**：
  - **遗忘函子（forgetful functor）**：$\mathbf{Grp}\to\mathbf{Set}$ 等，遗忘结构。
  - **自由函子（free functor）**：$\mathbf{Set}\to\mathbf{Vect}_k$（自由向量空间）、$\mathbf{Set}\to\mathbf{Grp}$（自由群）。
  - **预层（presheaf）**：**定义 1.2.15**，函子 $\mathscr{A}^{\mathrm{op}}\to\mathbf{Set}$。
  - **忠实/满函子（faithful/full）**：**定义 1.2.16**，要求 $\mathscr{A}(A,A')\to\mathscr{B}(F(A),F(A'))$ 为单射/满射。
  - **子范畴（subcategory）**：**定义 1.2.18**；**满子范畴（full subcategory）**要求包含所有对象间的全部态射。

#### 1.3 Natural Transformations（自然变换）
- **定义 1.3.1（自然变换）**：函子 $F,G:\mathscr{A}\rightrightarrows\mathscr{B}$ 之间的自然变换 $\alpha:F\to G$ 是一族态射 $\alpha_A:F(A)\to G(A)$，使得对所有 $f:A\to A'$，naturality square 交换：
  $$
  \begin{array}{ccc}
  F(A) & \xrightarrow{F(f)} & F(A') \\
  \downarrow{\alpha_A} & & \downarrow{\alpha_{A'}} \\
  G(A) & \xrightarrow{G(f)} & G(A')
  \end{array}
  $$
- **构造 1.3.6**：自然变换的**垂直复合**；由此形成**函子范畴** $[\mathscr{A},\mathscr{B}]$（或 $\mathscr{B}^{\mathscr{A}}$）。
- **定义 1.3.10（自然同构）**：函子范畴中的同构。
- **引理 1.3.11**：$\alpha$ 是自然同构 $\iff$ 每个分量 $\alpha_A:F(A)\to G(A)$ 都是同构。
- **定义 1.3.15（范畴等价）**：存在函子 $F:\mathscr{A}\leftrightarrows\mathscr{B}:G$ 及自然同构 $1_{\mathscr{A}}\cong G\circ F$、$F\circ G\cong 1_{\mathscr{B}}$。
- **定义 1.3.17（本质满）**：函子 $F$ 是 **essentially surjective on objects**，若 $\forall B\in\mathscr{B}$，$\exists A\in\mathscr{A}$ 使 $F(A)\cong B$。
- **命题 1.3.18（等价的实用判定）**：$F$ 是等价 $\iff$ $F$ 是 **full**、**faithful** 且 **essentially surjective on objects**。

---

### Chapter 2: Adjoints（伴随函子）

#### 2.1 Definition and examples
- **定义 2.1.1（伴随）**：函子 $F:\mathscr{A}\leftrightarrows\mathscr{B}:G$ 构成伴随，记作 $F\dashv G$，若存在自然同构
  $$\mathscr{B}(F(A),B) \cong \mathscr{A}(A,G(B))$$
  对 $A\in\mathscr{A}, B\in\mathscr{B}$ 自然成立。
- **典型例子**：
  - **自由–遗忘**：$\mathbf{Set}\rightleftarrows\mathbf{Vect}_k$、$\mathbf{Set}\rightleftarrows\mathbf{Grp}$；
  - **Abel 化**：$\mathbf{Grp}\rightleftarrows\mathbf{Ab}$（左伴随为商去换位子群）；
  - **离散/余离散拓扑**：$\mathbf{Set}\rightleftarrows\mathbf{Top}$；
  - **积与指数**：$(-\times B)\dashv (-)^B$ 在 $\mathbf{Set}$ 中。
- **定义 2.1.7（初始/终对象）**：对象 $I$ 是**初始的（initial）**，若对任意 $A$ 存在唯一态射 $I\to A$；对象 $T$ 是**终的（terminal）**，若对任意 $A$ 存在唯一态射 $A\to T$。
- **引理 2.1.8**：任意两个初始（或终）对象之间存在唯一的同构。
- **例子 2.1.9**：初始/终对象可视为与平凡范畴 $\mathbf{1}$ 之间的伴随函子。

#### 2.2 Adjunctions via units and counits
- **引理 2.2.2（三角恒等式）**：给定伴随 $F\dashv G$，unit $\eta:1\to GF$ 与 counit $\varepsilon:FG\to 1$ 满足：
  $$(\varepsilon F)\circ(F\eta) = 1_F,\qquad (G\varepsilon)\circ(\eta G) = 1_G$$
- **定理 2.2.5（伴随的 unit–counit 刻画）**：给定函子 $F,G$，以下二者存在**一一对应**：
  1. 伴随 $F\dashv G$；
  2. 自然变换对 $(\eta,\varepsilon)$ 满足三角恒等式。

#### 2.3 Adjunctions via initial objects
- **定义 2.3.1（逗号范畴 comma category）**：对函子 $P:\mathscr{A}\to\mathscr{C}, Q:\mathscr{B}\to\mathscr{C}$，逗号范畴 $(P\Rightarrow Q)$（或 $(P\downarrow Q)$）的对象为三元组 $(A,h,B)$，其中 $h:P(A)\to Q(B)$。
- **例子 2.3.3**：**切片范畴（slice category）** $\mathscr{A}/A$ 与**余切片范畴（coslice category）** $A/\mathscr{A}$ 是逗号范畴的特例。
- **引理 2.3.5**：在伴随 $F\dashv G$ 中，unit 映射 $\eta_A:A\to GF(A)$ 使得 $(F(A),\eta_A)$ 成为逗号范畴 $(A\Rightarrow G)$ 中的**初始对象**。
- **定理 2.3.6（伴随的三种刻画等价）**：以下等价：
  1. $F\dashv G$（Hom 自然同构）；
  2. 存在 unit/counit 满足三角恒等式；
  3. 对每个 $A$，comma 范畴 $(A\Rightarrow G)$ 有初始对象（由 $\eta_A$ 给出）。

---

### Chapter 3: Interlude on sets（集合论插曲）
- **核心内容**：集合论中的基本构造（如等化子、积等），作为后续极限概念的特例。
- **核心概念**：**小范畴（small category）**与**大范畴（large category）**的区分，为处理“所有范畴的范畴”等集合论问题提供基础。
- **历史注记**：范畴论与集合论、逻辑学发展的关系。

---

### Chapter 4: Representables（可表函子）
- **核心概念**：
  - **可表函子（representable functor）**：形如 $H^A=\mathscr{A}(A,-)$（协变）或 $H_A=\mathscr{A}(-,A)$（反变）的 Hom 函子，以及与之自然同构的函子。
  - **泛元（universal element）**。
- **关键定理**：
  - **Yoneda 引理（The Yoneda Lemma）**：对任意函子 $X:\mathscr{A}^{\mathrm{op}}\to\mathbf{Set}$，有自然同构
    $$[\mathscr{A}^{\mathrm{op}},\mathbf{Set}](H_A, X) \cong X(A)$$
    特别地，$H_A\cong H_B \iff A\cong B$。
- **推论**：可表函子在同构意义下唯一；Yoneda 引理是连接“对象”与“函子”的桥梁。

---

### Chapter 5: Limits（极限）
- **核心概念**：
  - **锥（cone）**与**余锥（cocone）**。
  - **极限（limit）**：锥范畴中的**终对象**；**余极限（colimit）**：余锥范畴中的**始对象**。
  - **典型例子**：积（product）、等化子（equalizer）、拉回（pullback）及其对偶（余积、余等化子、推出/pushout）。
- **关键定理**：
  - 极限可用 Hom 集刻画：$\mathscr{A}(A, \lim D) \cong \lim \mathscr{A}(A, D(-))$。
  - **Ch5.3**：函子与极限的交互——某些函子“保持”极限或余极限。

---

### Chapter 6: Adjoints, representables and limits（统一章）
- **核心内容**：将伴随、可表函子、极限三种视角统一。
- **关键结果**：
  - **极限的可表/伴随描述**：极限既可用 Hom 函子的可表性表述，也可视为对角函子的右伴随（当范畴完备时）。
  - **Presheaves 的极限（Ch6.2）**：预层范畴 $[\mathscr{A}^{\mathrm{op}},\mathbf{Set}]$ 中的极限可逐点（pointwise）计算。
  - **伴随与极限的交互（Ch6.3）**：
    - **右伴随保持极限**（$G$ 保持 $\lim$）；
    - **左伴随保持余极限**（$F$ 保持 $\mathrm{colim}$）。
  - **一般伴随函子定理（General Adjoint Functor Theorem, GAFT）**：在完备范畴中，函子 $G$ 有左伴随的充分必要条件（保持极限并满足解集条件）。

---

### Appendix（附录）
- **内容**：**一般伴随函子定理（GAFT）**的完整证明，作为全书技术性最强的收尾。