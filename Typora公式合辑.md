# Typora公式合辑：

![image-20201111103951436](C:\Users\WH\AppData\Roaming\Typora\typora-user-images\image-20201111103951436.png)

### 文章目录

- - - [0.添加公式](https://blog.csdn.net/Ernest_YN/article/details/84064233#0_2)
    - [1.算式](https://blog.csdn.net/Ernest_YN/article/details/84064233#1_26)
    - - - [1.1 分式](https://blog.csdn.net/Ernest_YN/article/details/84064233#11__28)
        - [1.2 根式](https://blog.csdn.net/Ernest_YN/article/details/84064233#12__43)
        - [1.3 上下标](https://blog.csdn.net/Ernest_YN/article/details/84064233#13__58)
        - [1.4 向量](https://blog.csdn.net/Ernest_YN/article/details/84064233#14__73)
        - [1.5 均值](https://blog.csdn.net/Ernest_YN/article/details/84064233#15__88)
        - [1.6 极限](https://blog.csdn.net/Ernest_YN/article/details/84064233#16__103)
        - [1.7 积分](https://blog.csdn.net/Ernest_YN/article/details/84064233#17__118)
        - [1.8 累加](https://blog.csdn.net/Ernest_YN/article/details/84064233#18__133)
        - [1.9 累乘](https://blog.csdn.net/Ernest_YN/article/details/84064233#19__148)
    - [2.运算符](https://blog.csdn.net/Ernest_YN/article/details/84064233#2_166)
    - [3.函数](https://blog.csdn.net/Ernest_YN/article/details/84064233#3_193)
    - - - [3.1 三角函数和对数函数](https://blog.csdn.net/Ernest_YN/article/details/84064233#31__195)
        - [3.2 分段函数](https://blog.csdn.net/Ernest_YN/article/details/84064233#32__206)
    - [4.希腊字母](https://blog.csdn.net/Ernest_YN/article/details/84064233#4_232)







### 0.添加公式

语法：

```
$$
（这里输入要添加的公式）
$$
123
```

效果：
（ 这 里 输 入 要 添 加 的 公 式 ） （这里输入要添加的公式）（这里输入要添加的公式）
注：

如果要在同一行输入多个公式，只需要在公式之间加上\(空格)或者\quad

如果要输入多行公式，只需要在上一行公式的末尾加上\\





### 1.算式

##### 1.1 分式

语法：

```
$$
\frac{a}{b}
$$
123
```

效果：
a b \frac{a}{b}*b**a*​

##### 1.2 根式

语法：

```
$$
\sqrt{x} or \sqrt[n]{x}
$$
123
```

效果：
x   o r  x n \sqrt{x}\ \ or \ \sqrt[n]{x}*x*​ *o**r* *n**x*​

##### 1.3 上下标

语法：

```
$$
x^n or x_n
$$
123
```

效果：
x n  o r  x n x^n\ or\ x_n*x**n* *o**r* *x**n*​

##### 1.4 向量

语法：

```
$$
\vec{a}\cdot\vec{b}=0
$$
123
```

效果：
a ⃗ ⋅ b ⃗ = 0 \vec{a}\cdot\vec{b}=0*a*⋅*b*=0

##### 1.5 均值

语法：

```
$$
\overline{x}
$$
123
```

效果：
x ‾ \overline{x}*x*

##### 1.6 极限

语法：

```
$$
\lim_{n\to+\infty} n or \lim n
$$
123
```

效果：
lim ⁡ n → + ∞ n   o r  lim ⁡ n \lim_{n\to+\infty} n\ \ or\ \lim n*n*→+∞lim​*n* *o**r* lim*n*

##### 1.7 积分

语法：

```
$$
\int_0^n f(x)dx or \int f(x)dx
$$
123
```

效果：
∫ 0 n f ( x ) d x   o r  ∫ f ( x ) d x \int_0^n f(x)dx\ \ or\ \int f(x)dx∫0*n*​*f*(*x*)*d**x* *o**r* ∫*f*(*x*)*d**x*

##### 1.8 累加

语法：

```
$$
\sum_{i=1}^n a_i or \sum a_i
$$
123
```

效果：
∑ i = 1 n a i   o r  ∑ a i \sum_{i=1}^n a_i\ \ or\ \sum a_i*i*=1∑*n*​*a**i*​ *o**r* ∑*a**i*​

##### 1.9 累乘

语法：

```
$$
\prod_{i=1}^n x_i or \prod x_i
$$
123
```

效果：
∏ i = 1 n x i   o r   ∏ x i \prod_{i=1}^n x_i \ \ or\ \ \prod x_i*i*=1∏*n*​*x**i*​ *o**r* ∏*x**i*​





### 2.运算符

| 运算符 | markdown        |
| ------ | --------------- |
| ±      | \pm             |
| ×      | \times          |
| ÷      | \div            |
| ≤      | \leq            |
| ≥      | \geq            |
| ≠      | \neq            |
| ⇒      | \Rightarrow     |
| ⇔      | \Leftrightarrow |
| ⊂      | \subset         |
| ⊆      | \subseteq       |
| ∈      | \in             |
| ∉      | \notin          |
| ∪      | \cup            |
| ∩      | \cap            |
| R      | \mathbb{R}      |
| Q      | \mathbb{Q}      |
| Z      | \mathbb{Z}      |
| N      | \mathbb{N}      |





### 3.函数

##### 3.1 三角函数和对数函数

| 函数   | markdown |
| ------ | -------- |
| sin(x) | \sin(x)  |
| cos(x) | \cos(x)  |
| tan(x) | \tan(x)  |
| log2 x | \log_2 x |
| lnx    | \ln x    |
| lgx    | \lg x    |

##### 3.2 分段函数

语法：

```
$$
f(x)=
\begin{cases}
1 & x\geq 0\\
0 & x<0
\end{cases}
$$
1234567
```

效果：
f ( x ) = { 1 x ≥ 0 0 x &lt; 0 f(x)=

{10amp;x≥0amp;xlt;0{1amp;x≥00amp;xlt;0

*f*(*x*)={10*x*≥0*x*<0







### 4.希腊字母

| 大写 | markdown      | 小写 | markdown    |
| ---- | ------------- | ---- | ----------- |
| A    | A or \Alpha   | α    | \alpha      |
| B    | B or \Beta    | β    | \beta       |
| Γ    | \Gamma        | γ    | \gamma      |
| Δ    | \Delta        | δ    | \delta      |
| E    | E or \Epsilon | ϵ    | \epsilon    |
|      |               | ε    | \varepsilon |
| Z    | Z or \Zeta    | ζ    | \zeta       |
| H    | H or \Eta     | η    | \eta        |
| Θ    | \Theta        | θ    | \theta      |
| I    | I or \Iota    | ι    | \iota       |
| K    | K or \Kappa   | κ    | \kappa      |
| Λ    | \Lambda       | λ    | \lambda     |
| M    | M or \Mu      | μ    | \mu         |
| N    | N or \Nu      | ν    | \nu         |
| Ξ    | \Xi           | ξ    | \xi         |
| O    | O or \Omicron | ο    | \omicron    |
| Π    | \Pi           | π    | \pi         |
| P    | P or \Rho     | ρ    | \rho        |
| Σ    | \Sigma        | σ    | \sigma      |
| T    | T or \Tau     | τ    | \tau        |
| Υ    | Y or \Upsilon | υ    | \upsilon    |
| Φ    | \Phi          | ϕ    | \phi        |
|      |               | φ    | \varphi     |
| X    | X or \Chi     | χ    | \chi        |
| Ψ    | \Psi          | ψ    | \psi        |
| Ω    | \Omega        | ω    | \omega      |