---
comments: true
---

# Mathjax学习笔记

## 基础

- displayed 公式： 分隔符` $$ ... $$`  或者 `\[...\]` 
- inline 公式 `$...$` 或者 `(...)` 

### 希腊字母

|  名称   |    大写    |   Tex    |    小写    |   Tex    |
| :-----: | :--------: | :------: | :--------: | :------: |
|  alpha  |    $A$     |    A     |  $\alpha$  |  \alpha  |
|  beta   |    $B$     |    B     |  $\beta$   |  \beta   |
|  gamma  |  $\Gamma$  |  \Gamma  |  $\gamma$  | \gammma  |
|  delta  |  $\Delta$  |  \Delta  |  $\delta$  |  \delta  |
| epslion |    $E$     |    E     | $\epsilon$ | \epsilon |
|  zeta   |    $Z$     |    Z     |  $\zeta$   |   zeta   |
|   eta   |    $H$     |    H     |   $\eta$   |   \eta   |
|  theta  |  $\Theta$  |  \Theta  |  $\theta$  |  \theta  |
|  iota   |    $I$     |    I     |  $\iota$   |  \itoa   |
|  kappa  |    $K$     |    K     |  $\kappa$  |  \kappa  |
| lambda  | $\Lambda$  | \Lambda  | $\lambda$  | \lambda  |
|   mu    |    $M$     |    M     |   $\mu$    |   \mu    |
|   nu    |    $N$     |    N     |   $\nu$    |   \nu    |
|   xi    |   $\Xi$    |   \Xi    |   $\xi$    |   \xi    |
| omicron |    $O$     |    O     | $\omicron$ | \omicron |
|   pi    |   $\Pi$    |   \Pi    |   $\pi$    |   \pi    |
|   rho   |    $P$     |    P     |   $\rho$   |   \rho   |
|  sigma  |  $\Sigma$  |  \Sigma  |  $\sigma$  |  \sigma  |
|   tau   |    $T$     |    T     |   $\tau$   |   \tau   |
| upsilon | $\Upsilon$ | \Upsilon | $\upsilon$ | \upsilon |
|   psi   |   $\Psi$   |   \Psi   |   $\psi$   |   \psi   |
|  omega  |  $\Omega$  |  \Omega  |  $\omega$  |  \omega  |

### 上标与下标

- 上标：^ 例如 `x_i^2` : $x_i^2$ 
- 下标：_ 例如 `x_j` $x_j$ 

注意： 一个组即单个字符或者使用{...}包裹起来的内容。他可以消除二义性

例如：`10^10`会得到 $10^10$	 

​	    `10^{10}` 会得到 $10^{10}$ 

### 括号

- 小括号和方括号：() []  $()$  $[]$ 
- 大括号：`\}  \{`   或者  `\lbrace`   `\rbrace`  
    - 例如：`\{a*b\}` $\{a*b\}$    
- 尖括号： `\langle` 和 `\rangle`   ：$\langle x\rangle$
- 上取整：`\lceil`和`\rceil` ：$\lceil x\rceil$
- 下取整：`\lfloor` 和 `\rfloor` ：$\lfloor x \rfloor$
- 不可见括号，用 . 表示

注意：原始符号不会随着公式大小缩放，可以使用\left(...\right)来自适应地调整括号大小。

### 求和与积分

- \sum 用来表示求和符号，其下标表示求和下限，上标表示上限。
    - `\sum_1^n`    $\sum_1^n$
- \int 用来表示积分符号
    - `\int_1^\infty` :   $\int_1^\infty$ 
- `\prod` ：$\prod$ 
- `\bigcup` : $\bigcup$ 
- `\bigcap` ：$\bigcup$
- `\iint` : $\iint$

### 分式和根式

- `\frac ab`   ： $\frac ab$   (如果分子分母不是单个字符，使用{...}分组)
- `\over` ：`{a+1\over b+1}`: ${a+1\over b+1}$
- `\sqrt ` :   `\sqrt{\frac ab}` :  $\sqrt{\frac ab}$   `\sqrt[4]{a \over b}$` : $\sqrt[4]{a \over b}$

### 字体

- \mathbb 或 \Bbb 显示黑板粗体字，此字体经常用来表示实数，整数，有理数和负数：$\mathbb {CHNQRZ}$
- \mathbf 显示黑体字：$\mathbf  {BCDEFG}$
- \mathtt 显示打印机字体：$\mathtt  {BCDEFG}$
- \mathrm 显示罗马体 : $\mathrm {ABCD}$
- \mathscr 显示手写体 : $\mathscr {ABCD}$
- \mathfrak 显示Fraktur字母（一种德国字体）：$\mathfrak{ABCD}$

### 特殊函数和符号

- 常见三角函数 $sin(x)$  $arctan(x)$   $\lim_{1\rightarrow\infty}$
- 比较运算符 : \lt \gt \le \ge \neq : $\lt$  $\gt$  $\le$  $\ge$  $\neq$   。可以加上\not： $\not\lt$ 
- \times  \div \pm \mp : $\times$   $\div$  $\pm$  $\mp$ 
- \cdot 表示居中的点 : x \codt y : $x \cdot y$
- 集合关系与运算：
    - \cup $\cup$
    - \cap $\cap$
    - \setminus $\setminus$
    - \subset $\subset$
    - \subseteq $\subseteq$
    - \subsetneq $\subsetneq$
    - \supset $\supset$
    - \in $\in$
    - \notin $\notin$
    - \emptyset $\emptyset$
    - \varnothing  $\varnothing$
- 表示排列使用 \binom  {n+1}{2k}或{n+1\choose 2k}
    - $\binom {n+1}{2k}$   $ {n+1\choose 2k}$ 
- 箭头：\to \rightarrow \leftarrow \Rightarrow \Leftarrow \mapsto
    - $\to$ $\rightarrow$ $\leftarrow$ $\Rightarrow$ $\Leftarrow$ $\mapsto$
- 逻辑运算符：\land \lor \lnot \forall \exists \top \bot \vdash \vDash
    - $\land$  $\lor$ $\lnot$    $\forall$    $\exists$  
- \star \ast \oplus \circ \bullet 
    - $\star$ $\ast$ $\oplus$ $\circ$ $\bullet$ 
- \approx $\approx$   \sim $\sim$  \cong $\cong$  \equiv $\equiv$  \prec $\prec$ 
- \infty $\infty$ \aleph_o $\aleph_o$ \navla $\nabla$  \Im $\Im$  \Re $\Re$ 
- \pmode   $a\equiv b \pmod n$
- \ldots  \cdots  ldots 的位置稍低，cdots位置居中
    - $a_1 + a_2 +\cdots +a_n$   
    - $a_1,a_2,\ldots,a_n$

### 空间

`a \ b` ：$a \ b$ 

`a \; b` :  $a \; b$

`a\quad b` ：$a\quad b$

`a\qquad b` :  $a\qquad b$

### 顶部符号



加波浪线 输入 \widetilde   \sim

加一个点 \dot{要加点的字母}加两个点\ddot{要加点的字母}



### 声调/变音符号

```latex
 $\dot{a} \ddot{a} \acute{a} \grave{a}$ 
```

 $\dot{a} \quad \ddot{a} \quad\acute{a}\quad \grave{a}$ 

```latex
 $\check{a} \breve{a} \tilde{a} \bar{a}$ 
```

 $\check{a} \quad \breve{a} \quad \tilde{a} \quad \bar{a}$ 

```latex
 $\hat{a} \widehat{a} \vec{a} \overline{a}$ 
```

 $\hat{a}\quad \widehat{a} \quad\vec{a} \quad \overline{a}$

### 数学公式

#### 1. 界限

```latex
$\min x \max y \inf s \sup t$
$\lim u \liminf v \limsup w$
$\dim p \deg p \det m \ker\phi$
$\infty$
```

$\min x \quad \max y \quad \inf s \quad \sup t$
$\lim u \quad \liminf v \quad \limsup w$
$\dim p \quad \deg p \quad \det m \quad \ker\phi$

 $\infty$

#### 2. 微分以及导数

```latex
$dt \mathrm{d}t \partial t \nabla\psi$
```

$dt \quad \mathrm{d}t \quad \partial t \quad \nabla\psi$

```latex
$\prime \backprime f^\prime f' f'' f^{(3)} \dot{y} \ddot{y}$ 
```

$\prime \quad \backprime \quad f^\prime \quad f' \quad f'' \quad f^{(3)} \quad \dot{y} \quad \ddot{y}$ 

#### 3. 模算数

```latex
$a \equiv 1 \pmod {m}$
$a\%b$ 
```

$a \equiv 1 \pmod {m}$

 $a\%b$ 

```latex
$a \bmod b$
```

$a \bmod b$

```latex
 $\gcd(m,n) \operatorname{lcm}(m,n)$ 
```

  $\gcd(m,n) \quad \operatorname{lcm}(m,n)$ 

```latex
$\mid \nmid \shortmid \nshortmid$ 
 ps：\mid可以用|代替。
```

$\mid \quad \nmid \quad \shortmid \quad \nshortmid$  

#### 4. 根号

```latex
$\surd \sqrt{2} \sqrt[n]{} \sqrt[n]{x}$ 
```

$\surd \quad \sqrt{2} \quad \sqrt[n]{} \quad \sqrt[n]{x}$ 

#### 5. 运算符

```latex
 $+ - \pm \mp \dotplus$ 
 $\times \div \divideontimes / \backslash$ 
 $\cdot * \star \circ \bullet$ 
 $\oplus \ominus \otimes \oslash \odot$ 
 $\bigoplus \bigotimes \bigodot$ 
```

$+ \quad - \quad \pm \quad \mp \quad \dotplus$ 
 $\times \quad \div \quad \divideontimes \quad / \quad \backslash$ 
 $\cdot \quad * \quad \star \quad \circ \quad \bullet$ 

$\oplus\quad \ominus\quad \otimes\quad \oslash\quad \odot$ 

$\bigoplus \quad \bigotimes \quad \bigodot$ 

#### 6. 集合

```latex
 $\emptyset \varnothing$ 
```

 $\emptyset \quad \varnothing$ 

```latex
 $\in \notin \not\in \ni \not\ni$ 
 ps:\not是在下一个字符上画斜杠。
```

 $\in \quad \notin \quad \not\in \quad \ni \quad \not\ni$  

```latex
$\subset \supset$
```

$\subset\quad  \supset$

```latex
$\subseteq \nsubseteq \subsetneq \varsubsetneq \sqsubseteq$ 
$\supseteq \nsupseteq \supsetneq \varsupsetneq \sqsupseteq$ 
```

$\subseteq \quad \nsubseteq \quad \subsetneq \quad \varsubsetneq \quad \sqsubseteq$ 

$\supseteq \quad \nsupseteq \quad \supsetneq \quad \varsupsetneq \quad \sqsupseteq$ 

```latex
$\cap \bigcap \cup \bigcup$
```

$\cap \quad \bigcap \quad \cup \quad \bigcup$

#### 关系符号

```latex
 $= \ne \neq \equiv \not\equiv$ 
```

 $= \quad \ne \quad \neq \quad \equiv \quad \not\equiv$ 

```latex
$\sim \nsim \backsim \thicksim \simeq \backsimeq \eqsim \cong \ncong$ 
```

$\sim \quad \nsim \quad \backsim \quad \thicksim \quad \simeq \quad \backsimeq\quad  \eqsim \quad \cong\quad  \ncong$ 