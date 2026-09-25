# aug31 Lecture3

你们下一节课的核心，是把上一节 **Shamir 的门限秘密共享**推广成：

> ==不只是“任意 $k$ 个人可以恢复秘密”，而是可以指定更复杂的权限规则。==

## 1. 从 Shamir Scheme 到 General Scheme

Shamir Secret Sharing 只能直接表达：

$k\text{-out-of-}n$

例如公司有 5 个人，任意 3 个人合作就能恢复秘密。

但现实中的权限可能是：

> Alice 和 Bob 一起，或者 Carol 和 David 一起，才能恢复秘密。

写成逻辑公式：

==$(A\land B)\lor(C\land D)$==

它显然不是简单的“任意两个人都可以”：

- $A+B$：可以
- $C+D$：可以
- $A+C$：不可以
- $B+D$：不可以

所以单一的 threshold $k$ 表达不了，需要 **General Secret Sharing Scheme**。

------

## 2. Access Control / Access Structure

这里的 **Access Control** 是规定：

> 哪些参与者组合有权恢复秘密？

所有有权恢复秘密的集合，组成 **Access Structure（访问结构）**，通常写成：

$\Gamma$

例如：

$\Gamma=\{\{A,B\},\{C,D\},\{A,B,C\},\ldots\}$

因为 $A,B$ 已经可以恢复，那么加入更多人以后，$A,B,C$ 当然也应该可以恢复。

这叫 **Monotone（单调性）**：

$X\in\Gamma,\quad X\subseteq Y \Longrightarrow Y\in\Gamma$

直觉就是：

> ==一个有权限的集合加入更多人以后，不会突然失去权限。==

==因此秘密共享通常只能自然表达包含 **AND、OR** 的正向规则，不表达类似“有 Alice 但不能有 Bob”这种带 NOT 的非单调规则。==

------

## 3. Monotone Formula

把每个人看成一个 Boolean variable：

- $A=1$：Alice 参与
- $A=0$：Alice 不参与

然后用 AND 和 OR 写权限规则。例如：

$F=(A\land B)\lor(C\land D)$

如果当前参与者是 $A,B,C$，公式为：

$(1\land1)\lor(1\land0)=1$

所以可以恢复秘密。

如果参与者是 $A,C$：

$(1\land0)\lor(1\land0)=0$

所以不能恢复。

这就是 **Access Structure 与 Monotone Formula 的对应关系**。

------

## 4. Two Basic Primitives

老师说的两个基本 primitive，基本就是：

| 逻辑门 | 秘密怎么分                  | 实质       |
| ------ | --------------------------- | ---------- |
| OR     | 两边都得到秘密 $s$          | 1-out-of-2 |
| AND    | 将 $s$ 随机拆成 $s_1+s_2=s$ | 2-out-of-2 |

### OR：复制秘密

要实现：

$A\lor B$

直接让：

$\operatorname{share}_A=s,\qquad \operatorname{share}_B=s$

A 或 B 单独都能恢复。

### AND：随机拆分秘密

要实现：

$A\land B$

在模 $q$ 的运算下随机选择 $r$：

==$\operatorname{share}_A=r$$\operatorname{share}_B=s-r\pmod q$==

两个人把 share 相加：

$r+(s-r)=s\pmod q$

任何一个人单独看到的都是随机数，因此不知道 $s$。

最好记成：

> **OR 就复制；AND 就拆开。**

------

## ==‼️5. General Scheme 怎么构造？==这个例子挺重要的

对于任意 Monotone Formula，从最上面的逻辑门开始，递归地分配秘密：

- 遇到 OR：把当前秘密传给每个分支；
- 遇到 AND：把当前秘密随机拆给不同分支；
- 最后到达变量时，把得到的数交给对应的人。

例如：

$(A\land B)\lor(C\land D)$

假设秘密：

$s=10\pmod {17}$

最上面是 OR，所以左右两个分支都分配秘密 10。

左边 $A\land B$，随机拆成：

$A=4,\qquad B=6$

因为：

$4+6=10$

右边 $C\land D$，随机拆成：

$C=13,\qquad D=14$

因为：

$13+14=27\equiv10\pmod {17}$

因此：

- $A+B$：恢复 $10$
- $C+D$：恢复 $10$
- $A+C$：两个不相关的随机 share，不能恢复
- $B+D$：同样不能恢复

这就是整个 General Scheme 的核心。

------

## 6. 为什么还是 Perfect Information Security？

对于不满足权限公式的参与者集合，他们一定缺少某个 AND 分支中的随机 share。

缺少以后，秘密仍然可能是任意值，并不是“算起来很困难”，而是：

> 他们拥有的信息在概率分布上与秘密无关。

所以即使攻击者计算能力无限，也推断不出秘密，依然属于：

$\text{Perfect / Information-Theoretic Security}$

------

## 7. 分享的论文和这节课是什么关系？

你材料里的论文是 Shamir 1979 年的：

> **How to Share a Secret**

它解决的是 $(k,n)$ threshold scheme，是这节课的基础。[Shamir 原论文](https://www.cs.utexas.edu/~lam/395t/papers/shamir.pdf)

而你发的下一课标题——**Access Structure、Monotone Formula、Two Basic Primitives、General Scheme**——几乎直接对应另一篇经典论文：

> **Generalized Secret Sharing and Monotone Functions**
> Josh Benaloh and Jerry Leichter

这篇论文就是把访问结构写成 monotone formula，然后用：

- OR：复制秘密
- AND：随机加法拆分

构造任意单调访问规则。[Benaloh–Leichter 论文](https://archiv.infsec.ethz.ch/education/as09/secsem/papers/BenLei96.pdf)

所以两篇的关系是：

$\text{Shamir：门限规则} \quad\longrightarrow\quad \text{General Scheme：任意单调权限规则}$

你上课前重点记住四句话就够了：

1. **Access Structure**：哪些人组合起来有权恢复秘密。
2. **Monotone**：有权限的集合加入更多人后仍有权限。
3. **OR = 复制秘密；AND = 随机拆分秘密。**
4. 沿着 monotone formula 递归执行这两个操作，就得到 general secret-sharing scheme。