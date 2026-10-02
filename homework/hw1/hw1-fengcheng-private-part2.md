# Homework #1 — Practice Problems - Part2

> USC CSCI 556: Introduction to Cryptography, Fall 2026 \
> **Lecturer:** Prof. Shang-Hua Teng \
> **Student:** Fengcheng Yu \
> **USC ID:** `**********` \
> **Date:** Sep 06, 2026


---

## Problem 4: Private-Key Encryption and Cryptanalysis

### Reading

> **Reading:** Katz–Lindell Sections 1.2, 1.4.1, and 2.1.

### (a) Syntax and correctness

#### Part 1: Syntax

A private-key encryption scheme consists of three algorithms. Give the syntax of

$\mathrm{Gen}, \quad \mathrm{Enc}, \quad \mathrm{Dec},$

including the input and output of each algorithm and which algorithms may be randomized. Then state the exact correctness requirement formally, including all quantifiers and randomness.

> [!tip]
>
> 这里有两部分要回答：
>
> 第一句：Give the syntax of Gen, Enc, Dec, including the input and output of each algorithm and which algorithms may be randomized 
>
> 第二句：state the exact correctness requirement formally, including all quantifiers and randomness

**Answer:**

A private-key encryption scheme is a triple of probabilistic polynomial-time algorithms $(\mathrm{Gen}, \mathrm{Enc}, \mathrm{Dec})$ together with a specified finite message space $\mathcal{M}$ with $\mathcal{M} > 1$. (一个私钥加密方案 = 消息空间 $\mathcal{M}$ + 三个算法)

The key-generation algorithm $\mathrm{Gen}$ takes no input (in the concrete setting of Chapters 1-2; in the asymptotic setting it takes the security parameter $1^n$) and outputs a key $k$ chosen according to some distribution. **$\mathrm{Gen}$ is probabilistic.** We write $k \leftarrow \mathrm{Gen}$. The key space $\mathcal{K}$ is the set of all keys output by $\mathrm{Gen}$ with nonzero probability.

> 密钥生成算法 $\mathrm{Gen}$ 不接收输入（在第1-2章的具体设定中；在渐近设定中，它接收安全参数 $1^n$），并输出一个根据某种分布选取的密钥 $k$。$\mathrm{Gen}$ 是概率性的。我们记作 $k \leftarrow \mathrm{Gen}$。密钥空间 $\mathcal{K}$ 是 $\mathrm{Gen}$ 以非零概率输出的所有密钥构成的集合。

The encryption algorithm $\mathrm{Enc}$ takes as input a key $k \in \mathcal{K}$ and a message $m \in \mathcal{M}$, and outputs a ciphertext $c$. **$\mathrm{Enc}$ may be probabilistic:** running $\mathrm{Enc}_k(m)$ twice may yield different ciphertexts. We write $c \leftarrow \mathrm{Enc}_k(m)$, reserving $c := \mathrm{Enc}_k(m)$ for the case that $\mathrm{Enc}$ is deterministic. The ciphertext space $\mathcal{C}$ is the set out all $c$ output by $\mathrm{Enc}_k(m)$ over all $k \in \mathcal{K}$, all $m \in \mathcal M$, and all random choices of $\mathrm{Enc}$.

> 加密算法 $\mathrm{Enc}$ 接收密钥 $k \in \mathcal{K}$ 和消息 $m \in \mathcal{M}$ 作为输入，并输出密文 $c$。**$\mathrm{Enc}$ 可能是概率性的：** 对同一输入运行 $\mathrm{Enc}_k(m)$ 两次可能会产生不同的密文。我们记作 $c \leftarrow \mathrm{Enc}_k(m)$，而将 $c := \mathrm{Enc}_k(m)$ 这一记法保留用于 $\mathrm{Enc}$ 为确定性算法的情形。密文空间 $\mathcal{C}$ 是由所有 $k \in \mathcal{K}$、所有 $m \in \mathcal{M}$ 以及 $\mathrm{Enc}$ 的所有随机选择所产生的密文 $c$ 构成的集合。

The decryption algorithm $\mathrm{Dec}$ takes as input a key $k \in \mathcal{K}$ and a ciphertext $c \in \mathcal{C}$, and outputs a message $m \in \mathcal{M}$. Under perfect correctness $\mathrm{Dec}$ may be assumed deterministic without loss of generality, since $\mathrm{Dec}_k(c)$ must produce the same output every time it run. We therefore write $m := \mathrm{Dec}_k(c)$.

> 解密算法 $\mathrm{Dec}$ 接收密钥 $k \in \mathcal{K}$ 和密文 $c \in \mathcal{C}$ 作为输入，并输出消息 $m \in \mathcal{M}$。在完全正确性（perfect correctness）的条件下，不失一般性，可假设 $\mathrm{Dec}$ 是确定性的，因为 $\mathrm{Dec}_k(c)$ 每次运行时必然产生相同的输出。因此，我们记作 $m := \mathrm{Dec}_k(c)$。

|      算法      | 输入                                     | 输出                | 随机性     |
| :------------: | :--------------------------------------- | :------------------ | :--------- |
| $\mathrm{Gen}$ | 无（或 $1^n$）                           | $k \in \mathcal{K}$ | 必须随机   |
| $\mathrm{Enc}$ | $k \in \mathcal{K}$，$m \in \mathcal{M}$ | $c \in \mathcal{C}$ | 可以随机   |
| $\mathrm{Dec}$ | $k \in \mathcal{K}$，$c \in \mathcal{C}$ | $m \in \mathcal{M}$ | 不妨确定性 |

#### Part 2: Correctness requirement

(Perfect Correctness.) For every key $k \in \mathcal{K}$ and every message $m \in \mathcal{M}$,

​	$\mathrm{Pr}[\mathrm{Dec}(\mathrm{Enc}_k(m)) = m] = 1$
where the probability is taken over the random coins of $\mathrm{Enc}$ (and of $\mathrm{Dec}$, if it is randomized)

Equivalently, incorporating key generation: for every $m \in \mathcal{M}$,

​	$\mathrm{Pr}[k \leftarrow \mathrm{Gen}; c \leftarrow \mathrm{Enc}_k(m) : \mathrm{Dec}_k(c) = m] = 1$
the probability now being over the coins of $\mathrm{Gen}$ and $\mathrm{Enc}$.

Since $\mathrm{Dec}$ is deterministic, this is equivalent to the "support" formulation: for every $k \in \mathcal{K}$, every $m \in \mathcal{M}$, and every ciphertext $c$ in the support of $\mathrm{Enc}_k(m)$. (i.e., every $c$ that $\mathrm{Enc}_k(m)$) outputs with nonzero probability.

> （完美正确性）对于任意密钥 $k \in \mathcal{K}$ 和任意消息 $m \in \mathcal{M}$，
>
> $\mathrm{Pr}[\mathrm{Dec}(\mathrm{Enc}_k(m)) = m] = 1$
>
> 其中概率是针对 $\mathrm{Enc}$（以及 $\mathrm{Dec}$，若其为随机算法）的随机性（随机比特）而言的。
>
> 等价地，若纳入密钥生成过程：对于任意 $m \in \mathcal{M}$，
>
> $\mathrm{Pr}[k \leftarrow \mathrm{Gen}; c \leftarrow \mathrm{Enc}_k(m) : \mathrm{Dec}_k(c) = m] = 1$
>
> 此时概率针对的是 $\mathrm{Gen}$ 和 $\mathrm{Enc}$ 的随机性。
>
> 由于 $\mathrm{Dec}$ 是确定性算法，这等价于基于“支撑集”（support）的表述：对于任意 $k \in \mathcal{K}$、任意 $m \in \mathcal{M}$ 以及 $\mathrm{Enc}_k(m)$ 支撑集中的任意密文 $c$（即 $\mathrm{Enc}_k(m)$ 以非零概率输出的任意 $c$），均满足上述条件。

**一个容易漏的点**

「every ciphertext in the support」这个说法比「概率 1」更强调一件事：

```
不是"绝大多数随机币下能还原"
而是"==只要 Enc 可能吐出这个 c，Dec 就必须还原成功=="
```

K&L §2.1 原话就是这么写的：

> for all $k \in \mathcal{K}$, $m \in \mathcal{M}$, and **any ciphertext $c$ output by** $\mathrm{Enc}_k(m)$, it holds that $\mathrm{Dec}_k(c) = m$ with probability 1.

### (b) Types of attacks

Classify each situation as a **ciphertext-only**, **known-plaintext**, or **chosen-plaintext** attack, and briefly justify the classification.

1. Eve obtains several ciphertexts but no corresponding plaintexts.
2. Eve learns that a particular ciphertext $c$ encrypts a particular message $m$.
3. Eve selects messages $m_1, m_2, \ldots$ and obtains their encryptions.

#### 解答

**1. Ciphertext-only attack.** Eve observes only ciphertexts and has no corresponding plaintexts. This is the weakest threat model: it requires nothing more than passive eavesdropping on the public channel.

**2. Known-plaintext attack.** Eve obtains a plaintext/ciphertext pair $(m, c)$ generated under the key in use. She knows the pairing, but she did **not** choose $m$ — the pair was simply revealed to her. Her goal is to use it to learn about some *other* ciphertext encrypted under the same key.

**3. Chosen-plaintext attack.** Eve selects the messages $m_1, m_2, \ldots$ herself and obtains their encryptions. Unlike case 2 she is active rather than passive: she controls which plaintexts get encrypted.

> [!tip]
> 
>**初稿错在哪 —— 第 2 条的理由写反了**
> 
>原来写的是：
> 
>> Eve have some particular ciphertext $c$ encrypts to $m$, **but she is not able to determine what particular plaintext she have**.
> 
>==「她无法确定自己手上的明文是什么」与结论自相矛盾。== 题面说的是 Eve **已经得知** $c$ 加密的正是 $m$ —— 她恰恰**知道**这个明文。若真的不知道明文，那就退回成 ciphertext-only 了。
> 
>正确的理由是：她**知道配对**，但**没得挑** —— 这对 $(m,c)$ 是碰巧暴露给她的，不是她点的菜。==「知道 vs 选择」才是 known-plaintext 与 chosen-plaintext 的分界线。==
> 
>**另外，三条各一句话太简略。** 题目虽说 "briefly justify"，但理由至少要点出**与相邻模型的区别在哪**；第 2、3 条的差别只在"能不能自己挑"，不写这句等于没给理由。

### (c) Correctness does not imply security

Assume that the message space $\mathcal{M}$ contains at least two distinct messages, set $\mathcal{C} = \mathcal{M}$, and let $\mathrm{Gen}$ be any key-generation algorithm over a nonempty key space $\mathcal{K}$. Consider the scheme

$\mathrm{Enc}_k(m) = m, \qquad \mathrm{Dec}_k(c) = c,$

where $k$ is generated but never used.

1. Prove that the scheme is correct.
2. Show that the ciphertext determines the plaintext and prove that the scheme is not perfectly secret. Explain what this example shows about the distinction between correctness and security.

Answer:

> [!tip]
>
> scheme的正确定，永远都是之前那一套：Gen, Enc, Dec, 这一套东西。我们要证明这个正确性，是要证明，解密出来的东西和原来输入是一样的。而不是让你去证明  $\mathrm{Enc}_k(m) = m, \qquad \mathrm{Dec}_k(c) = c.$ 这个是条件。scheme永远都是上面那一套。

所以证明很简单。

(1), we need to prove that: $\mathrm{Dec}_k(\mathrm{Enc}_k(m)) = m$

$\mathrm{Dec}_k(\mathrm{Enc}_k(m)) = \mathrm{Dec}_k(m) = m$, therefore, the scheme is correct.

(2), need to prove two part:

1. ciphertext determines the plaintext
2. prove the scheme is not perfectly secret

> [!tip]
>
> 回忆定义（Definition 2.3）：
>
> ```
> 对每一个分布、每个 m、每个 Pr[C=c] > 0 的 c，都要有
>     Pr[M = m | C = c] = Pr[M = m]
> ```
>
> 要证「不是」完美保密，只需举出一个反例——找到一组具体的（分布、$m$、$c$）让等式不成立。
>
> 怎么举例子？因为 $\mathcal{M}$ 至少有两条消息，取其中两条 $m_0 \ne m_1$, 让消息各以 1/2 的概率取这两条。
>
> 所以此时先验：
>
> $\mathrm{Pr}[M = m_1] = 1/2$
>
> 现在攻击者看到密文 $c = m_0$
>
> 后验：$\mathrm{Pr}[M = m_0 | C = m_0] = 1$
>
> 因为密文就是明文，所以攻击者看到密文之后，知道了明文，所以先验 != 后验。

***

> [!tip]
>
> scheme的正确性，只需要 $\mathrm{Dec}_k(\mathrm{Enc}_k(m)) = m$，不一定需要保密
>
> 要证明不是完美保密，就要找到  `Pr[M = m | C = c] != Pr[M = m]` 的例子

---

## Problem 5: Perfect Secrecy

### Reading

> **Reading:** Katz–Lindell Chapter 2.

### (a) Definition

State the definition of perfect secrecy using conditional probabilities of the form

$\Pr[M = m \mid C = c].$

State the probability experiment, all required quantifiers, and the condition under which the conditional probability is defined.

> State the definition of perfect secrecy using conditional probabilities
> of the form Pr[M = m | C = c].          ← ①定义本身
>
> State the probability experiment,        ← ②概率实验
>       all required quantifiers,          ← ③全部量词
>       and the condition under which
>       the conditional probability is defined.   ← ④条件概率何时有定义

**Probability experiment.** Fix a private-key encryption scheme $(\mathrm{Gen}, \mathrm{Enc}, \mathrm{Dec})$ with message space $\mathcal{M}$, key space $\mathcal{K}$, and ciphertext space $\mathcal{C}$, and fix an arbitrary probability distribution over $\mathcal{M}$. The experiment is:

1. A message $m$ is drawn from $\mathcal{M}$ according to that distribution;
2. a key $k \leftarrow \mathrm{Gen}$ is generated, **independently of $m$**;
3. the ciphertext $c \leftarrow \mathrm{Enc}_k(m)$ is computed.

Let $M$, $K$, $C$ denote the random variables giving the message, the key, and the ciphertext in this experiment. The distribution of $K$ is determined by the scheme itself; the distribution of $M$ is determined by the context in which the scheme is used; $M$ and $K$ are independent.

**Definition (perfect secrecy).** The scheme is **perfectly secret** if for every distribution over $\mathcal{M}$, every $m \in \mathcal{M}$, and every $c \in \mathcal{C}$ with $\Pr[C = c] > 0$,

$\Pr[M = m \mid C = c] = \Pr[M = m].$

The requirement $\Pr[C = c] > 0$ is needed so that the conditional probability is defined — it prevents conditioning on a zero-probability event.

### (b) A small one-time pad

Let

$\mathcal{M} = \mathcal{K} = \mathcal{C} = \mathbb{Z}_3.$

The key $K$ is uniform over $\mathbb{Z}_3$ and independent of $M$, and

$\mathrm{Enc}_k(m) = m + k \pmod 3,$

$\mathrm{Dec}_k(c) = c - k \pmod 3.$

Prove correctness and perfect secrecy. The secrecy proof must hold for every distribution on $\mathcal{M}$.

**Answer:**

**Prove the correctness:** 

$\mathrm{Dec}_k(\mathrm{Enc}_k(m)) = c - k \pmod 3 = m + k - k \pmod 3 = m$, therefore the scheme is correct.

> [!caution]
>
> 这样写肯定是无法得到满分的。
>
> $c$ 是个没交代的符号。
>
> $\mathrm{Dec}_k(\mathrm{Enc}_k(m)) = \mathrm{Dec}_k(m + k \bmod 3) = (m + k) - k \equiv m \pmod 3$
>
> ```
> 第一步：把 Enc_k(m) = m+k 代进去   ← ==明确说是代入==
> 第二步：套 Dec 的定义 c − k，此时 c 就是 m+k
> 第三步：k − k = 0，得 m
> ```
>
> **还缺两样**
>
> 一、量词
>
> 你直接写了式子，没说"对哪些 $k$、哪些 $m$"。==正确性是全称命题：==
>
> > for every $k \in \mathcal{K}$ and every $m \in \mathcal{M}$
>
> 这正是 Problem 4(a) 里强调的 "including all quantifiers"，==同一份作业里前后要一致==。
>
> 二、结果要落回 $\mathbb{Z}_3$
>
> $m \in \mathbb{Z}_3$，而 $(m+k)-k$ 在整数里等于 $m$，模 3 之后仍是 $m$ ——==因为 $m$ 本来就在 ${0,1,2}$ 里==。这句可以一笔带过，但写上更完整。

**Correctness.** For every $k \in \mathbb{Z}_3$ and every $m \in \mathbb{Z}_3$,

$\mathrm{Dec}_k(\mathrm{Enc}_k(m)) = \mathrm{Dec}_k\bigl([m + k \bmod 3]\bigr) = [(m + k) - k \bmod 3] = [m \bmod 3] = m,$

where the last equality holds because $m \in \mathbb{Z}_3$. Since both $\mathrm{Enc}$ and $\mathrm{Dec}$ are deterministic, this holds with probability 1. Hence the scheme is perfectly correct.

**Perfect secrecy.** Fix an arbitrary distribution over $\mathcal{M} = \mathbb{Z}_3$, an arbitrary $m \in \mathbb{Z}_3$, and an arbitrary $c \in \mathbb{Z}_3$.

**Step 1.** For every $m' \in \mathbb{Z}_3$,

$\Pr[C = c \mid M = m'] = \Pr[m' + K \equiv c \pmod 3] = \Pr[K = [c - m' \bmod 3]] = \tfrac{1}{3},$

using that $K$ is uniform on $\mathbb{Z}_3$ and independent of $M$, and that $[c - m' \bmod 3]$ is a single element of $\mathbb{Z}_3$. Note this value does not depend on $m'$.

**Step 2.** By the law of total probability,

$\Pr[C = c] = \sum_{m' \in \mathbb{Z}*3} \Pr[C = c \mid M = m'] \cdot \Pr[M = m'] = \tfrac{1}{3} \sum*{m'} \Pr[M = m'] = \tfrac{1}{3} > 0,$

so the conditional probability below is always defined.

**Step 3.** By Bayes' theorem,

$\Pr[M = m \mid C = c] = \frac{\Pr[C = c \mid M = m] \cdot \Pr[M = m]}{\Pr[C = c]} = \frac{\tfrac{1}{3} \cdot \Pr[M = m]}{\tfrac{1}{3}} = \Pr[M = m].$

The distribution, $m$, and $c$ were arbitrary, so the scheme is perfectly secret. $\blacksquare$

**要注意的点**

1. 命脉在 Step 1 —— $1/3$ 里不含 $m'$

整个证明只有这一步有内容，后面两步是机械推导。「条件概率是个与 $m'$ 无关的常数」就是完美保密的全部

2. 「$K$ 均匀」和「$K$ 与 $M$ 独立」两个条件都要点名

```
均匀  →  才有 Pr[K = 某个特定值] = 1/3
独立  →  才能把 Pr[· | M=m'] 里的条件扔掉，直接算 K 的概率
```

漏掉「独立」是常见失分点。

3. 为什么这个证明对任何分布成立

Step 2 里 $\sum_{m'} \Pr[M = m'] = 1$ ——不管什么分布，概率和总是 1。所以全程没用到 $M$ 的任何具体信息。题目那句 "must hold for every distribution" 就靠这一步兑现。

4. 顺手把 $\Pr[C=c] > 0$ 解决了

Step 2 算出来恰好是 $1/3 > 0$，所以条件概率永远有定义，不用额外讨论。写一句"so the conditional probability is always defined"能接上 (a) 里那个技术条件。

**5. 结尾必须点明"三者任意"**

`The distribution, m, and c were arbitrary` ——这句话把三个量词一次性兑现。开头说 "Fix an arbitrary..."、结尾说 "were arbitrary"，是标准的全称证明写法。

### (c) A biased key

Keep the scheme from part (b), but let the independent key satisfy

$\Pr[K = 0] = \tfrac{1}{2}, \qquad \Pr[K = 1] = \Pr[K = 2] = \tfrac{1}{4}.$

Assume $M$ is uniform over $\mathbb{Z}_3$. Compute

$\Pr[M = 0 \mid C = 0],$

compare it with $\Pr[M = 0]$, and determine whether the scheme is perfectly secret.

**Answer:**

Since $K$ is independent of $M$, for every $m' \in \mathbb{Z}_3$

$\Pr[C = 0 \mid M = m'] = \Pr[m' + K \equiv 0 \pmod 3] = \Pr\bigl[K = [-m' \bmod 3]\bigr],$

so

$\Pr[C = 0 \mid M = 0] = \Pr[K=0] = \tfrac12, \quad \Pr[C = 0 \mid M = 1] = \Pr[K=2] = \tfrac14, \quad \Pr[C = 0 \mid M = 2] = \Pr[K=1] = \tfrac14.$

With $M$ uniform, the law of total probability gives ($P(A)=\sum_{i=1}^{n} P\left(A \mid B_{i}\right) P\left(B_{i}\right)$)

$\Pr[C = 0] = \tfrac13\left(\tfrac12 + \tfrac14 + \tfrac14\right) = \tfrac13 > 0,$

so the conditional probability is defined. By Bayes' theorem,

$\Pr[M = 0 \mid C = 0] = \frac{\Pr[C=0 \mid M=0] \cdot \Pr[M=0]}{\Pr[C=0]} = \frac{\tfrac12 \cdot \tfrac13}{\tfrac13} = \tfrac12.$

Comparing with the prior $\Pr[M = 0] = \tfrac13$:

$\Pr[M = 0 \mid C = 0] = \tfrac12 ;\ne; \tfrac13 = \Pr[M = 0].$

This exhibits a distribution over $\mathcal{M}$, a message, and a ciphertext of positive probability for which the defining equality of perfect secrecy fails. **Hence the scheme is not perfectly secret.** $\blacksquare$

The failure traces to Step 1 of part (b): there $\Pr[C=c \mid M=m']$ was the constant $\tfrac13$, independent of $m'$. Here it takes the value $\tfrac12$ when $m' = 0$ and $\tfrac14$ otherwise, so the ciphertext distribution does depend on the plaintext.

### (d) A lower bound on the key space

Let $\Pi = (\mathrm{Gen}, \mathrm{Enc}, \mathrm{Dec})$ be a perfectly correct and perfectly secret encryption scheme with finite, nonempty message and key spaces $\mathcal{M}$ and $\mathcal{K}$. Assume decryption is deterministic. Prove that

$|\mathcal{K}| \ge |\mathcal{M}|.$

**Answer:**

> **Proof (by contradiction).** Suppose $|\mathcal{K}| < |\mathcal{M}|$.
>
> Consider the **uniform** distribution over $\mathcal{M}$, and fix any $c \in \mathcal{C}$ with $\Pr[C = c] > 0$ (such a $c$ exists, since some ciphertext occurs with positive probability). Define
>
> $\mathcal{M}(c) \;=\; \{\, m \in \mathcal{M} \;:\; m = \mathrm{Dec}_k(c) \text{ for some } k \in \mathcal{K} \,\}.$
>
> Because $\mathrm{Dec}$ is deterministic, each key $k$ contributes at most one message to $\mathcal{M}(c)$, so
>
> $|\mathcal{M}(c)| \;\le\; |\mathcal{K}| \;<\; |\mathcal{M}|.$
>
> Hence there exists $m_0 \in \mathcal{M}$ with $m_0 \notin \mathcal{M}(c)$.
>
> We claim $\Pr[M = m_0 \mid C = c] = 0$. Indeed, suppose the events $M = m_0$ and $C = c$ both occurred, and let $k$ be the key actually generated. By perfect correctness $\mathrm{Dec}_k(c) = m_0$, so $m_0 \in \mathcal{M}(c)$ — a contradiction. Therefore $\Pr[M = m_0 \wedge C = c] = 0$, and so $\Pr[M = m_0 \mid C = c] = 0$.
>
> On the other hand, under the uniform distribution $\Pr[M = m_0] = 1/|\mathcal{M}| > 0$. Thus
>
> $\Pr[M = m_0 \mid C = c] \;=\; 0 \;\ne\; \frac{1}{|\mathcal{M}|} \;=\; \Pr[M = m_0],$
>
> contradicting perfect secrecy. Hence $|\mathcal{K}| \ge |\mathcal{M}|$. $\blacksquare$

**中文骨架**

```
反设 |K| < |M|
  ↓
取 M 上均匀分布，取一个出现概率为正的密文 c
  ↓
M(c) = 用所有钥匙去解 c，能解出的消息集合
Dec 确定性 ⟹ 一把钥匙至多解出一个 ⟹ |M(c)| ≤ |K| < |M|
  ↓
==必有某条消息 m₀ 解不出来==
  ↓
Pr[M = m₀ | C = c] = 0，但先验 Pr[M = m₀] = 1/|M| > 0
  ↓
后验 ≠ 先验，违反完美保密 ⟹ 矛盾
```

**大白话：** 钥匙比消息少，那对任何一个密文，能解出来的消息种类也少 —— ==总有某条消息「怎么解都解不出」，攻击者立刻就能把它排除掉==，这就泄露了信息。

> [!tips]
> **四个采分点**
>
> 1. ==必须自己挑「均匀分布」==。完美保密要求对**每一个**分布成立，所以推翻它只要找到**一个**分布出问题。均匀分布保证了 $\Pr[M=m_0] > 0$，反例才立得住。
> 2. ==「$\mathrm{Dec}$ 确定性」是 $|\mathcal{M}(c)| \le |\mathcal{K}|$ 的唯一依据==。若 $\mathrm{Dec}$ 可随机，一把钥匙就能解出多条消息，这个不等式立刻失效 —— 这也是题面特意写 "Assume decryption is deterministic" 的原因。
> 3. ==$\Pr[M=m_0 \mid C=c] = 0$ 要给理由==，不能直接断言。理由是 perfect correctness：真实用的那把 $k$ 必须满足 $\mathrm{Dec}_k(c) = m_0$，于是 $m_0$ 就落进 $\mathcal{M}(c)$ 了，与选取矛盾。
> 4. 取 $c$ 时要说明 $\Pr[C=c] > 0$，否则条件概率没有定义 —— 呼应 (a) 里那个技术条件。
>
> **这条定理的含义：** 若消息是 $\ell$ 位串，则 $|\mathcal{M}| = 2^\ell$，故 $|\mathcal{K}| \ge 2^\ell$，即==钥匙至少和消息一样长==。所以 one-time pad 的钥匙长度是**最优的**，这不是 OTP 的缺陷，而是完美保密的固有代价。

---

## Problem 6: Attack—Reusing a One-Time Pad

### Reading

> **Reading:** Katz–Lindell Section 2.2.

2.2其实就是在讲了一件事：

```
① 构造        M = K = C = {0,1}^ℓ
              Gen：均匀抽一把 ℓ 位钥匙
              Enc_k(m) = k ⊕ m
              Dec_k(c) = k ⊕ c

② 正确性      k ⊕ (k ⊕ m) = m       ← 异或两次抵消

③ 完美保密    Theorem 2.9
              不管明文是什么，Pr[C=c | M=m'] 恒为 2^(−ℓ)
              Bayes 上下约掉 → 后验 = 先验

④ 两个缺陷    钥匙必须和消息一样长
              钥匙只能用一次           ← Problem 6 考这个
```

### 题面设定

Let $M_1, M_2 \in \{0,1\}^{64}$ be plaintext byte strings, each representing exactly eight ASCII characters, with one 8-bit byte per character. A key $K \xleftarrow{\$} \{0,1\}^{64}$ is sampled uniformly once. The messages are encrypted bytewise as $C_i = M_i \oplus K$, but the same key $K$ is **incorrectly reused** for both messages. The corresponding ciphertexts, written as hexadecimal bytes, are

$C_1 = \texttt{E7 55 1A 76 80 31 27 CC}$

and

$C_2 = \texttt{E7 55 1A 76 80 30 36 CC}.$

You also know that

$M_1 = \texttt{MEET@8PM}.$

### (a) Information leakage

Prove that reusing an OTP key gives

$C_1 \oplus C_2 = M_1 \oplus M_2.$

Prove:

$C_1 \oplus C_2 = (M_1 \oplus K) \oplus (M_2 \oplus K) = M_1 \oplus M_2 \oplus K \oplus K = M_1 \oplus M_2$

> [!warning]
>
> $a \oplus 0 = a$, $a \oplus a = 0$

### (b) Recover the key

Convert $M_1$ to ASCII hexadecimal and recover all eight key bytes. Show every bytewise XOR.

#### 解答

> **这里的十六进制怎么去算：**
>
> 用两条 ASCII 规律就够：
>
> ```
> 大写字母：'A' = 0x41，往后依次 +1
> 数  字：  '0' = 0x30，往后依次 +1
> '@' = 0x40  （正好在 'A' 前面一个）
> ```

先把 $M_1$ 转化为十六进制：

```
'M'  第13个字母  → 0x41 + 12 = 0x4D
'E'  第5个字母   → 0x41 + 4  = 0x45
'E'                          = 0x45
'T'  第20个字母  → 0x41 + 19 = 0x54
'@'                          = 0x40
'8'              → 0x30 + 8  = 0x38
'P'  第16个字母  → 0x41 + 15 = 0x50
'M'                          = 0x4D
```

$\therefore$ $M_1$ = 4D 45 45 54 40 38 50 4D

> 十六进制如何异或：
>
> 0 = 0000      4 = 0100      8 = 1000      C = 1100
> 1 = 0001      5 = 0101      9 = 1001      D = 1101
> 2 = 0010      6 = 0110      A = 1010      E = 1110
> 3 = 0011      7 = 0111      B = 1011      F = 1111

$\therefore$ $M_1$ = 01001101 01000101 01000101 01010100 01000000 00111000 01010000 01001101 (其实不用把这个写出来，这样很容易出错)

直接算我们需要的就可以了。

$C_1 = \texttt{E7 55 1A 76 80 31 27 CC}$

$M_1$ = 4D 45 45 54 40 38 50 4D

$K = M_1 \oplus C_1$

<img src="assets/笔记%202025年1月13日%20copy.png" style="width:55%;" />

逐字节异或（$\oplus$ 逐位：相同得 0，不同得 1）：

```
 i │  C₁    M₁   │            二进制异或             │  K
───┼─────────────┼──────────────────────────────────┼─────
 1 │  E7    4D   │  1110 0111 ⊕ 0100 1101 = 1010 1010 │  AA
 2 │  55    45   │  0101 0101 ⊕ 0100 0101 = 0001 0000 │  10
 3 │  1A    45   │  0001 1010 ⊕ 0100 0101 = 0101 1111 │  5F
 4 │  76    54   │  0111 0110 ⊕ 0101 0100 = 0010 0010 │  22
 5 │  80    40   │  1000 0000 ⊕ 0100 0000 = 1100 0000 │  C0
 6 │  31    38   │  0011 0001 ⊕ 0011 1000 = 0000 1001 │  09
 7 │  27    50   │  0010 0111 ⊕ 0101 0000 = 0111 0111 │  77
 8 │  CC    4D   │  1100 1100 ⊕ 0100 1101 = 1000 0001 │  81
```

$\therefore\ K = \texttt{AA 10 5F 22 C0 09 77 81}$

**验算**（$C_1 = M_1 \oplus K$ 应当还原出 $C_1$）：

$\texttt{4D} \oplus \texttt{AA} = \texttt{E7}$，$\texttt{45} \oplus \texttt{10} = \texttt{55}$，$\texttt{45} \oplus \texttt{5F} = \texttt{1A}$，$\texttt{54} \oplus \texttt{22} = \texttt{76}$，$\texttt{40} \oplus \texttt{C0} = \texttt{80}$，$\texttt{38} \oplus \texttt{09} = \texttt{31}$，$\texttt{50} \oplus \texttt{77} = \texttt{27}$，$\texttt{4D} \oplus \texttt{81} = \texttt{CC}$ ✓

### (c) Recover the second message

Recover all eight bytes of $M_2$, convert them to ASCII, and explain why this attack does not contradict the perfect-secrecy theorem for the one-time pad.

#### 解答

由 (b) 已得

$K = \texttt{AA 10 5F 22 C0 09 77 81}$

而

$C_2 = \texttt{E7 55 1A 76 80 30 36 CC}$

由 $C_2 = M_2 \oplus K$ 得 $M_2 = C_2 \oplus K$，逐字节计算：

```
 i │  C₂    K    │            二进制异或             │  M₂  │ ASCII
───┼─────────────┼──────────────────────────────────┼──────┼───────
 1 │  E7    AA   │  1110 0111 ⊕ 1010 1010 = 0100 1101 │  4D  │  'M'
 2 │  55    10   │  0101 0101 ⊕ 0001 0000 = 0100 0101 │  45  │  'E'
 3 │  1A    5F   │  0001 1010 ⊕ 0101 1111 = 0100 0101 │  45  │  'E'
 4 │  76    22   │  0111 0110 ⊕ 0010 0010 = 0101 0100 │  54  │  'T'
 5 │  80    C0   │  1000 0000 ⊕ 1100 0000 = 0100 0000 │  40  │  '@'
 6 │  30    09   │  0011 0000 ⊕ 0000 1001 = 0011 1001 │  39  │  '9'
 7 │  36    77   │  0011 0110 ⊕ 0111 0111 = 0100 0001 │  41  │  'A'
 8 │  CC    81   │  1100 1100 ⊕ 1000 0001 = 0100 1101 │  4D  │  'M'
```

$\therefore\ M_2 = \texttt{4D 45 45 54 40 39 41 4D}$

转成 ASCII：

$M_2 = \texttt{MEET@9AM}$

**验算**（$M_2 \oplus K$ 应还原出 $C_2$）：$\texttt{4D} \oplus \texttt{AA} = \texttt{E7}$，$\texttt{39} \oplus \texttt{09} = \texttt{30}$，$\texttt{41} \oplus \texttt{77} = \texttt{36}$，其余字节与 (b) 相同 ✓

**观察：** $M_1 = \texttt{MEET@8PM}$、$M_2 = \texttt{MEET@9AM}$，==两条消息只在第 6、7 字节不同==——这与 (a) 算出的 $C_1 \oplus C_2 = \texttt{00 00 00 00 00 01 11 00}$ 完全吻合（只有第 6、7 字节非零）。

**Explanation（为什么这不与 one-time pad 的完美保密定理矛盾）：**

**不是定理错了，而是定理的前提被违反了。** 分三点：

**① 完美保密只针对单个密文。** Definition 2.3 的实验是「一条消息 → 一把钥匙 → **一个**密文」，Theorem 2.9 证的就是这个。本题是两个密文共用一把钥匙，==不在定理的管辖范围内==。

**② 这次泄露是必然的，不只是"没被覆盖"。** 把"一把钥匙加密两条消息"当成独立方案看：

$|\mathcal{M}| = 2^{64} \times 2^{64} = 2^{128}, \qquad |\mathcal{K}| = 2^{64}$

$|\mathcal{K}| < |\mathcal{M}|$，故由 Theorem 2.10（即 Problem 5(d)）==它不可能完美保密==。64 位钥匙保护不了 128 位信息。

**③ 定理的结论其实仍然成立。** 单看任一个密文，完美保密没被破坏：

$\Pr[M_1 = m \mid C_1 = c_1] = \Pr[M_1 = m]$

因为对单个密文而言 $K$ 仍是均匀且未知的。==泄露发生在**联合**分布上==——$(C_1, C_2)$ 合起来暴露了 $M_1 \oplus M_2$。

> 边缘分布无泄露，联合分布有泄露 —— 与 Problem 3(d) 强调 "complete **joint** distribution" 是同一件事。

首先：pertect secrecy：$\Pr[M = m \mid C = c]$




---

## Q7: Perfect Indistinguishability and a Random-Period Vigenère Cipher

### Reading

> **Reading:** Katz–Lindell Sections 1.3 and 2.1, especially Example 2.7 (page 32).

### 题面设定

Identify the alphabet with $\mathbb{Z}_{26}$ using

$\texttt{a} = 0,\ \texttt{b} = 1,\ \ldots,\ \texttt{z} = 25,$

and perform all letter arithmetic modulo 26. The message and ciphertext spaces are the **four-letter strings** over this alphabet.

The key-generation algorithm proceeds in the following order:

1. Choose the period $T$ according to

   $\Pr[T = 1] = \tfrac{1}{3}, \qquad \Pr[T = 2] = \tfrac{2}{3}.$

2. Conditional on $T = t$, sample $K_1, \ldots, K_t$ independently and uniformly from $\mathbb{Z}_{26}$. The key is $(T, K_1, \ldots, K_t)$.

For $M = M_1M_2M_3M_4$, encryption produces $C = C_1C_2C_3C_4$, where

$C_j = M_j + K_{1 + ((j-1) \bmod T)} \pmod{26} \qquad \text{for } j \in \{1,2,3,4\}.$

Decryption subtracts the same repeated key character in each position, modulo 26. Thus a period-one key uses $K_1$ in all four positions, while a period-two key alternates $K_1, K_2, K_1, K_2$.

In the perfect-indistinguishability experiment, an adversary outputs two messages $m_0, m_1$. The challenger independently generates a key as above and samples $b \xleftarrow{\$} \{0,1\}$, then returns $C = \mathrm{Enc}_K(m_b)$. The adversary outputs $b' \in \{0,1\}$ and succeeds when $b' = b$. A scheme is **perfectly indistinguishable** if every adversary, even a computationally unbounded one, succeeds with probability exactly $1/2$.

Analyze the following deterministic adversary $A$:

1. It chooses

   $m_0 = \texttt{math}, \qquad m_1 = \texttt{test}.$

2. For any four-letter string $x = x_1x_2x_3x_4$, define

   $\Delta(x) = x_1 - x_2 + x_3 - x_4 \pmod{26}.$

   After receiving $C$, the adversary outputs $b' = 0$ if $\Delta(C) = \Delta(m_0)$, and outputs $b' = 1$ otherwise.

### Before Solving

**首先，字母变成数字：**

```
a b c d e f g h i j k  l  m  n  o  p  q  r  s  t  u  v  w  x  y  z
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
```

**第二段：密钥如何生成（两层随机）**

分成两步抽密钥：

```
第一步：先掷一个不公平的骰子，决定"周期"是 1 还是 2
        T = 1  的概率 1/3
        T = 2  的概率 2/3      ← 注意不均匀！

第二步：根据 T 抽出对应个数的字母
        T = 1  →  只抽 1 个字母 K₁
        T = 2  →  抽 2 个字母 K₁, K₂（各自均匀、互相独立）
```

所以密钥不是一个数，而是一整套 $(T, K_1, \ldots, K_T)$。

**第 3 段：怎么加密**

$C_j = M_j + K_{1 + ((j-1) \bmod T)} \pmod{26}$

Thus a period-one key uses $K_1$ in all four positions, while a period-two key alternates $K_1, K_2, K_1, K_2$.

那个下标公式看着吓人，但题目下一句已经把答案告诉你了：

```
T = 1：四个位置都用 K₁
        位置:  1   2   3   4
        密钥:  K₁  K₁  K₁  K₁
T = 2：K₁ 和 K₂ 交替
        位置:  1   2   3   4
        密钥:  K₁  K₂  K₁  K₂
```

**一个例子：**

```
明文 math = 12, 0, 19, 7
假设 T=2, K₁=3, K₂=5
C₁ = 12 + 3 = 15  → p
C₂ =  0 + 5 =  5  → f
C₃ = 19 + 3 = 22  → w
C₄ =  7 + 5 = 12  → m
密文 = pfwm
```

**第 4 段：不可区分实验（游戏规则）**

**大白话：这是一个猜谜游戏。**

```
① 攻击者交出两条消息 m₀ 和 m₁         （他自己挑的，两条都公开）
② 系统私下抛硬币得到 b ∈ {0,1}         （50/50）
③ 系统加密其中一条：C = Enc(m_b)       （攻击者不知道加的是哪条）
④ 系统把密文 C 交给攻击者
⑤ 攻击者猜 b' —— 猜"你加密的是第 0 条还是第 1 条"
⑥ 猜对（b' = b）就算他赢
```

**第 5 段：这个攻击者怎么设计的**

$\Delta$ 是什么：把 4 个字母按「加减加减」交替算出一个数。

```
Δ(x) = x₁ − x₂ + x₃ − x₄
        +    −    +    −      ← ==正负交替，这是机关所在==
```

攻击者的策略：

```
拿到密文 C，算 Δ(C)
    若 Δ(C) 等于 Δ(math)  →  猜 b' = 0（我认为加密的是 math）
    否则                   →  猜 b' = 1
```

为什么这招可能有效？—— 关键直觉

把加密式子代进 $\Delta$ 看看密钥会怎样：

```
T = 1（四个位置都是 K₁）：

  Δ(C) = (M₁+K₁) − (M₂+K₁) + (M₃+K₁) − (M₄+K₁)
       = Δ(M) + (K₁ − K₁ + K₁ − K₁)
       = Δ(M) + 0
       = ==Δ(M)==            ← 密钥被完全消掉了！
```

$T=1$ 时，$\Delta(C)$ 直接等于 $\Delta(M)$ —— 密钥毫无遮蔽作用，攻击者一算就知道原文是哪条。

```
T = 2（交替 K₁ K₂ K₁ K₂）：

  Δ(C) = (M₁+K₁) − (M₂+K₂) + (M₃+K₁) − (M₄+K₂)
       = Δ(M) + 2K₁ − 2K₂     ← 还有随机项，没那么好破
```

**这就是整道题的骨架：$T=1$ 那条分支（概率 1/3）是个后门，攻击者在那里稳赢；$T=2$ 分支（概率 2/3）他只能部分蒙对。两者加权平均，总胜率必然超过 1/2。**

### (a)

Compute $\Delta(m_0)$ and $\Delta(m_1)$. Derive $\Delta(C)$ in terms of $\Delta(M)$ and the key characters separately for $T = 1$ and $T = 2$.

计算 $\Delta(m_0)$ 和 $\Delta(m_1)$。分别针对 $T = 1$ 和 $T = 2$ 的情况，推导 $\Delta(C)$ 关于 $\Delta(M)$ 及密钥字符的表达式。

**首先，我们要先计算两个 $\Delta$ 的值：**

先把字母变成数字：

```
math：  m = 12,  a = 0,   t = 19,  h = 7
test：  t = 19,  e = 4,   s = 18,  t = 19
```

代入 $\Delta(x) = x_1 - x_2 + x_3 - x_4 \pmod{26}$

```
Δ(math) = 12 − 0 + 19 − 7
        = 12 + 19 − 7
        = 24

Δ(test) = 19 − 4 + 18 − 19
        = 19 − 4 + 18 − 19
        = 14
```

刚好，都不用 mod26.

**第二部分：推 $\Delta(C)$ 的公式 **

核心一步：把 $\Delta(C)$ 拆成 消息部分+密钥部分

先写出通式。设第 $j$ 位用的密钥是 $K_{\sigma(j)}$，即 $C_j = M_j + K_{\sigma(j)}$。代进 $\Delta$:

```
Δ(C) = C₁ − C₂ + C₃ − C₄
     = (M₁ + K_σ(1)) − (M₂ + K_σ(2)) + (M₃ + K_σ(3)) − (M₄ + K_σ(4))
```

重新分组, 把所有 $M$ 归一堆，所有 $K$ 归一堆：

```
     = (M₁ − M₂ + M₃ − M₄) + (K_σ(1) − K_σ(2) + K_σ(3) − K_σ(4))
       └──────  Δ(M)  ─────┘   └────────  密钥项  ────────────┘
```

$$\Delta(C) = \Delta(M) + \bigl(K_{\sigma(1)} - K_{\sigma(2)} + K_{\sigma(3)} - K_{\sigma(4)}\bigr) \pmod{26}$$

这一步是整道题的关键。剩下只需分别代入两种周期，算那个密钥项。

**情形 $T = 1$**

四个位置都用 $K_1$，所以 $\sigma = (1,1,1,1)$：

```
密钥项 = K₁ − K₁ + K₁ − K₁ = 0
```

$$\boxed{\Delta(C) = \Delta(M)}$$

密钥被完全抵消了。

**情形 $T = 2$**

交替使用，$\sigma = (1,2,1,2)$：

```
密钥项 = K₁ − K₂ + K₁ − K₂ = 2K₁ − 2K₂ = 2(K₁ − K₂)
```

$$\boxed{\Delta(C) = \Delta(M) + 2(K_1 - K_2) \pmod{26}}$$

所以这一问答案汇总：

```
Δ(m₀) = Δ(math) = 24
Δ(m₁) = Δ(test) = 14

T = 1：  Δ(C) = Δ(M)                        ← 密钥完全消失
T = 2：  Δ(C) = Δ(M) + 2(K₁ − K₂)  mod 26
```

### (b)

Conditioned on $b = 0$, calculate the exact probability that $A$ succeeds. Analyze the cases $T = 1$ and $T = 2$ separately.

**第 0 步：先明确 $b = 0$ 意味着什么**

```
b = 0  →  系统加密的是 m₀ = math
       →  所以 Δ(M) = Δ(math) = 24
```

攻击者什么时候算赢？ 他的规则是：

```
若 Δ(C) = Δ(m₀) = 24  →  输出 b' = 0
否则                    →  输出 b' = 1
```

现在真实的 $b = 0$，所以他要赢就必须输出 0，也就是必须 $\Delta(C) = 24$。

**第 1 步：$T = 1$ 的情况**

由 (a)：$T=1$ 时 $\Delta(C) = \Delta(M)$。

Δ(C) = Δ(M) = 24        ← 恒等于 24，跟密钥 K₁ 是什么完全无关

条件 $\Delta(C) = 24$ **必然满足**，所以：

$$\Pr[\text{成功} \mid b=0, T=1] = 1$$

攻击者百分之百赢。== 这就是 (a) 里发现的那个后门。

**第 2 步：$T = 2$ 的情况**

由 (a)：$\Delta(C) = \Delta(M) + 2(K_1 - K_2) = 24 + 2(K_1 - K_2) \pmod{26}$。

要成功就要 $\Delta(C) = 24$，即

```
24 + 2(K₁ − K₂) ≡ 24   (mod 26)
       2(K₁ − K₂) ≡ 0   (mod 26)
```

记 $d = K_1 - K_2 \bmod 26$，问题变成：**求 $2d \equiv 0 \pmod{26}$ 的解。**

**直觉会说"两边除以 2，得 $d \equiv 0$"——==这是错的==。**

因为 $\gcd(2, 26) = 2 \ne 1$，==所以 $2$ 在模 26 下**不可逆**，不能两边同除==。（就是 Problem 1 里学的：只有 $\gcd(b,N)=1$ 才可逆。）

**正确做法 —— 回到整除的定义：**

```
2d ≡ 0 (mod 26)
  ⟺  26 整除 2d
  ⟺  13 整除 d          （两边约掉公因数 2）
  ⟺  d = 0  或  d = 13   （在 0~25 范围内）
```

==**有两个解，不是一个。**== 这正是题目末尾那条 counting note 在警告的事。

```
d = 0  →  2×0  = 0  ≡ 0   ✓
d = 13 →  2×13 = 26 ≡ 0   ✓   ← 容易漏掉这个
```

**note by fengcheng: 其实这个肉眼就能看出来**

**第 3 步：算 $d$ 落在 ${0, 13}$ 的概率**

$K_1, K_2$ 各自均匀、互相独立，所以 $d = K_1 - K_2 \bmod 26$ ==也是均匀分布在 $\mathbb{Z}_{26}$ 上==（固定 $K_2$ 时，$K_1 - K_2$ 跟着 $K_1$ 跑遍所有值）。

```
Pr[d = 任一特定值] = 1/26
Pr[d ∈ {0, 13}]    = 2/26 = 1/13
```

$$\Pr[\text{成功} \mid b=0,, T=2] = \frac{1}{13}$$

然后用全概率公式算就行了

**第 4 步：合并两种周期**

```
Pr[成功 | b=0] = Pr[T=1]·Pr[成功|b=0,T=1] + Pr[T=2]·Pr[成功|b=0,T=2]
               =  1/3  ×  1     +   2/3  ×  1/13
               =  1/3           +   2/39
```

所以这一问的答案汇总：

```
T = 1：  Pr[成功 | b=0, T=1] = 1          ← 密钥抵消，必胜
T = 2：  Pr[成功 | b=0, T=2] = 1/13       ← 要 2(K₁−K₂) ≡ 0，有 2 个解
合并：   Pr[成功 | b=0]      = 5/13
```

### (c)

Conditioned on $b = 1$, calculate the exact probability that $A$ succeeds. Again analyze both possible periods separately.

和b一样思路，等标准答案下来之后，自己重新做一次。

但这里要注意的，b=1, 如果攻击者算出 b'=1 才算赢。 b' = 1 的意思是：$\Delta(C) \ne \Delta(m_0)$, 所以这里是反过来了。要注意。

### (d)

Consider the four cases

$(b, T) \in \{(0,1), (0,2), (1,1), (1,2)\}.$

For a case $(b, T) = (\beta, t)$, state the condition under which $A$ is correct and compute $\Pr[b' = b \mid b = \beta, T = t]$. Multiply by $\Pr[b = \beta, T = t]$ to obtain the branch contribution $\Pr[b = \beta, T = t, b' = b]$. Present all four cases in a probability tree or table, sum their contributions to calculate $\Pr[b' = b]$ exactly as a fraction, compare the result with $1/2$, and conclude whether the scheme is perfectly indistinguishable.

> **Important counting note:** Conditioned on $T = 2$, any one specified ordered pair $(K_1, K_2) = (u, v)$ has probability $1/26^2$. An equation relating $K_1$ and $K_2$ may be satisfied by many ordered pairs, so its probability must account for all satisfying pairs.
