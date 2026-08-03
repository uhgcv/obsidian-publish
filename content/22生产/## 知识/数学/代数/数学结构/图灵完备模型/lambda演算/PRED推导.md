
推导 `PRED`（前驱函数，即 $n - 1$）是 $\lambda$ 演算中最著名的“硬核”部分。克莱尼的滑动窗口思路虽然逻辑清晰，但展开成纯 $\lambda$ 项后非常庞大。
为了保证可读性，我们先**用宏（辅助函数）一步步推导**，最后再给出它的纯 $\lambda$ 表达式。

---
### 第一步：准备积木（丘奇序对与提取函数）
因为我们需要在迭代过程中记录“前一个值”和“当前值”，我们需要一个容器（序对）。
*   **构造序对**：$\text{PAIR} = \lambda a.\lambda b.\lambda f. f\ a\ b$ （把 $a$ 和 $b$ 存起来）
*   **取第一个元素**：$\text{FST} = \lambda p. p\ (\lambda a.\lambda b. a)$ （从序对中取 $a$）
*   **取第二个元素**：$\text{SND} = \lambda p. p\ (\lambda a.\lambda b. b)$ （从序对中取 $b$）
---
### 第二步：定义初始状态与步进函数
我们要走 $n$ 步，初始状态是第 0 步。
*   **初始状态 INIT**：我们让“前一个值”和“当前值”都是 0。
    $$ \text{INIT} = \text{PAIR}\ 0\ 0 $$
    *(表示序对 $(0, 0)$)*
*   **步进函数 NEXT**：每走一步，旧的“当前值”变成新的“前一个值”；旧的“当前值”加 1 变成新的“当前值”。
    $$ \text{NEXT} = \lambda p. \text{PAIR}\ (\text{SND}\ p)\ (\text{SUCC}\ (\text{SND}\ p)) $$
    *解释：读入旧序对 $p$，构造新序对。新序对的第一个元素是 $\text{SND}\ p$（旧当前值），第二个元素是 $\text{SUCC}\ (\text{SND}\ p)$（旧当前值+1）。*
---
### 第三步：组合成 PRED
丘奇数 $n$ 的本质是“把一个函数应用 $n$ 次”。
所以，我们把 $\text{NEXT}$ 这个步进函数，对 $\text{INIT}$ 应用 $n$ 次。走完 $n$ 步后，取出序对的第一个元素（前一个值）。
$$ \text{PRED} = \lambda n. \text{FST}\ (n\ \text{NEXT}\ \text{INIT}) $$
---
### 第四步：推导与执行（以 PRED 2 = 1 为例）
我们来看看 $\text{PRED}\ 2$ 是如何一步步计算的。
1. **代入 2 的定义**：
   $\text{PRED}\ 2 = \text{FST}\ (2\ \text{NEXT}\ \text{INIT})$
   *丘奇数 $2 = \lambda f.\lambda x. f\ (f\ x)$，意味着把 $f$ 应用 2 次。*
   $\to \text{FST}\ (\text{NEXT}\ (\text{NEXT}\ \text{INIT}))$
2. **计算第一次 NEXT**（最内层）：
   $\text{NEXT}\ \text{INIT}$
   $= \text{NEXT}\ (\text{PAIR}\ 0\ 0)$
   $= \text{PAIR}\ (\text{SND}\ (\text{PAIR}\ 0\ 0))\ (\text{SUCC}\ (\text{SND}\ (\text{PAIR}\ 0\ 0)))$
   *(提取出 $\text{SND}$ 是 0)*
   $\to \text{PAIR}\ 0\ (\text{SUCC}\ 0)$
   $\to \text{PAIR}\ 0\ 1$
   *(走完第 1 步，状态变成 $(0, 1)$)*
3. **计算第二次 NEXT**（外层）：
   把上一步的结果代入：
   $\text{NEXT}\ (\text{PAIR}\ 0\ 1)$
   $= \text{PAIR}\ (\text{SND}\ (\text{PAIR}\ 0\ 1))\ (\text{SUCC}\ (\text{SND}\ (\text{PAIR}\ 0\ 1)))$
   *(提取出 $\text{SND}$ 是 1)*
   $\to \text{PAIR}\ 1\ (\text{SUCC}\ 1)$
   $\to \text{PAIR}\ 1\ 2$
   *(走完第 2 步，状态变成 $(1, 2)$)*
4. **提取结果**：
   现在执行最外层的 $\text{FST}$：
   $\text{FST}\ (\text{PAIR}\ 1\ 2)$
   $\to 1$
**推导成功！$\text{PRED}\ 2 = 1$。**
---
### 第五步：纯 $\lambda$ 表达式
如果你把上面所有的宏（$\text{FST}$, $\text{SND}$, $\text{PAIR}$, $\text{SUCC}$, $\text{INIT}$, $\text{NEXT}$）全部代入并展开，你会得到一个非常巨大的表达式。
经过逻辑学家的化简，有一个等价的、更紧凑的纯 $\lambda$ 表达式版本（它利用了一个不需要显式构造 PAIR 的巧妙技巧，但逻辑仍然是滑动窗口）：
$$ \text{PRED} = \lambda n.\lambda f.\lambda x.\ n\ (\lambda g.\lambda h.\ h\ (g\ f))\ (\lambda u. x)\ (\lambda u. u) $$
**简要解析这个紧凑版：**
*   它依然是通过 $n$ 次迭代来工作。
*   $\lambda u. x$ 充当了初始的“前一个值”（恒为 $x$，即 0 次应用）。
*   $\lambda g.\lambda h.\ h\ (g\ f)$ 充当了步进函数。它巧妙地把 $f$（代表 +1 的动作）不断往后传递，最后用一个恒等函数 $\lambda u. u$ 截断，提取出少套了一层 $f$ 的结果。
这就是 $\lambda$ 演算中减法的基础。有了 $\text{PRED}$，减法 $\text{SUB}$ 就很简单了：
$$ \text{SUB} = \lambda m.\lambda n.\ n\ \text{PRED}\ m $$
*(把 $\text{PRED}$ 对 $m$ 应用 $n$ 次，就是 $m - n$)*。


---
---
在纯 $\lambda$ 演算中，要写出 `-1` 的表达式，我们必须使用之前讨论过的**整数表示法（序对表示法）**。因为标准丘奇数只有自然数 $\mathbb{N}$，没有 `-1` 的容身之地。
在序对表示法中，整数 $z$ 用一个丘奇序对 $(a, b)$ 表示，代表 $z = a - b$。
因此，**`-1` 就是 $0 - 1$，即 `PAIR 0 1`**。
下面我们一步步把它展开为纯 $\lambda$ 表达式。
### 1. 宏定义形式
*   **0** = $\lambda f.\lambda x. x$
*   **1** = $\lambda f.\lambda x. f\ x$
*   **PAIR** = $\lambda a.\lambda b.\lambda c. c\ a\ b$
所以，`-1` 的宏定义是：
$$ -1 \equiv \text{PAIR}\ 0\ 1 $$
### 2. 展开与 $\beta$-归约（推导纯表达式）
我们将 0、1 和 PAIR 的定义代入：
$$ -1 \equiv (\lambda a.\lambda b.\lambda c. c\ a\ b)\ (\lambda f.\lambda x. x)\ (\lambda f.\lambda x. f\ x) $$
**第一步归约**（把 $a$ 替换为 `0`）：
$$ \to (\lambda b.\lambda c. c\ (\lambda f.\lambda x. x)\ b)\ (\lambda f.\lambda x. f\ x) $$
**第二步归约**（把 $b$ 替换为 `1`）：
$$ \to \lambda c. c\ (\lambda f.\lambda x. x)\ (\lambda f.\lambda x. f\ x) $$
### 3. 最终的纯 $\lambda$ 表达式
为了避免变量名冲突（捕获），我们给内部两个丘奇数的变量换个名字，最终的 `-1` 纯 $\lambda$ 表达式为：
$$ -1 \equiv \lambda c.\ c\ (\lambda g.\lambda y. y)\ (\lambda h.\lambda z.\ h\ z) $$
### 4. 验证它的代数意义
在这个系统中，整数 `0` 表示为 `PAIR 0 0`：
$$ 0_{int} \equiv \lambda c.\ c\ (\lambda g.\lambda y. y)\ (\lambda g.\lambda y. y) $$
整数加法 `ADD_int` 是对序对的两个分量分别做丘奇加法。当我们用 `ADD_int` 把上面构造的 `-1` 和 `1` 相加时：
$$ \text{ADD}_{int}\ (-1)\ 1 = \text{PAIR}\ (\text{PLUS}\ 0\ 1)\ (\text{PLUS}\ 1\ 0) = \text{PAIR}\ 1\ 1 $$
而 `PAIR 1 1` 正好是 $1 - 1 = 0$，也就是整数 `0`。
所以，这个纯 $\lambda$ 项完美地扮演了 `-1` 的角色，使得 `-1 + 1 = 0` 成立。
