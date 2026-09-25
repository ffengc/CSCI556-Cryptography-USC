# Lecture6 - 0909

这个note是我过了一遍 agent_notes/ 里面的内容后，另外加上上课上讲的内容自己的一些note

[TOC]

## Vocabulary

- 设计密码叫 cryptography
- 拆密码叫 cryptanalysis
- 两者合起来叫 cryptology
- brute-force attack / exhaustive-search attack（穷举攻击）

## 这节课讲什么

上节课讲了 perfect secrecy, 可以做得到，但是代价很大，密钥一定要和mesg一样长。

这节课干三件事：

```
1. Cryptanalysis  —— 历史上的密码是怎么被破的，从中学到什么教训
2. Types of Attacks —— 把"攻击者有多大本事"分成四档，形式化威胁模型
3. 转向 computational security —— 放弃"绝对安全"，改要"算不动"
```

外加老师自己的一条线索：**Cryptography vs Machine Learning**。

## Cryptanalysis 密码分析

Cryptanalysis 就是「破解密码的学问」。 

- 设计密码叫 cryptography
- 拆密码叫 cryptanalysis
- 两者合起来叫 cryptology

### 教材 §1.3 部分

这部分教材讲了一些历史的密码，目的是告诉我们：

- 拍脑袋的设计是不行的
- 简单方法几乎不可能达到安全

**Caesar's cipher（凯撒密码）**

做法： 每个字母往后挪 3 位。

```
a→D,  b→E,  c→F,  ...,  x→A,  y→B,  z→C（绕回）

明文：begin the attack now
密文：EHJLQWKHDWWDFNQRZ
```

致命问题： 根本没有钥匙。 挪 3 位是写死的。谁知道了这个方法，就能直接解密。

> 现在网上论坛用的 **ROT-13**（挪 13 位）是它的变种，但没人指望它保密——只是让剧透内容不至于被一眼看到。

**Shift cipher（移位密码）+ 穷举攻击**

做法： 把「挪几位」变成钥匙 $k \in \{0,\ldots,25\}$。

$\mathrm{Enc}_k(m_1\cdots m_\ell) = c_1\cdots c_\ell$，其中 $c_i = [(m_i + k) \bmod 26]$

$\mathrm{Dec}_k(c_1\cdots c_\ell) = m_1\cdots m_\ell$，其中 $m_i = [(c_i - k) \bmod 26]$

这下有钥匙了，安全吗？大白话：还是不安全，因为钥匙只有 26 种。

‼️这种把所有钥匙试一遍的攻击叫 **brute-force attack / exhaustive-search attack（穷举攻击）**。

那么这里就提出了一个原则：到底需要多大，才能让穷举这个方法行不通？

 **Sufficient Key-Space Principle（充分密钥空间原则）**

Any secure encryption scheme must have a key space that is sufficiently large to make an exhaustive-search attack infeasible.

书本上说至少 $2^{70}$，长期安全还要更大。现在攻击者能用超算、几万台 PC、GPU 集群来加速穷举/

> [!note]
>
> 这只是必要条件，不是充分条件。 钥匙空间够大，方案仍可能因为别的原因被破——下面的单表替换密码就是例子。

**单表替换密码 + 频率分析**

做法：不再是"统一挪 k 位"，而是给 26 个字母任意指定一个置换。

```
a→X, b→Q, c→M, d→A, ... （任意一一对应）
```

钥匙空间： $26! \approx 2^{88}$，远超 $2^{70}$，穷举根本试不动。

但它照样被破了。 靠的是 frequency analysis（频率分析）：

```
英文里 e 出现最多（约 12.7%），然后是 t、a、o、i、n...
密文里出现最多的那个字母，多半就是 e
再结合双字母组合（th、he、in）等规律，逐步还原
```

教训：钥匙空间大 ≠ 安全。密文保留了明文的统计结构，这个结构就是漏洞。

**Vigenère cipher + 破解**

做法： 用一个长度为 $t$ 的钥匙串，循环使用。

```
钥匙 = "cafe"（长度 4）
第1个字母挪 c(2) 位，第2个挪 a(0) 位，第3个挪 f(5) 位，第4个挪 e(4) 位，
第5个又回到挪 2 位……如此循环
```

同一个明文字母，在不同位置会变成不同的密文字母，频率分析直接失效（密文频率被抹平了）。

Vigenère 曾长期被认为不可破（号称 *le chiffre indéchiffrable*）。

具体怎么破的，这里面有讲

**密文程度和攻击的关系：**

钥匙越长，攻击者需要的密文就越多。

- 破 shift cipher：一个单词的密文可能就够
- 破单表替换：需要更长的密文
- 破 Vigenère：需要大约 $t$ 倍长的密文（每组都得足够长，频率才稳定）
- Vigenère 如果钥匙和消息是一样长的，就是安全的

### 结论

**总之教材这部分的结论就是：**

Vigenère 撑了几百年才被破
更复杂的方案也被造过
但所有历史方案最终都被攻破了
复杂 ≠ 安全

## Type of Attacks

四种威胁模型。

what is threat model:

> Now that we have fixed a security goal, it remains to specify a **threat model**. This specifies what "power" the attacker is assumed to have, but **does not place any restrictions on the adversary's strategy**.

大白话的意思就是：threat model 规定的是“攻击者能做什么动作”，不规定他打算怎么用这些动作

打个比方： 你评估保险箱，只能假设"小偷有一把电钻、能在屋里待 8 小时"，不能假设"小偷只会从正面钻"——他可能从底下钻。

四种攻击模型，按攻击者能力从弱到强：

1. Ciphertext-only attack（唯密文攻击）
2. Known-plaintext attack（已知明文攻击）
3. Chosen-plaintext attack（选择明文攻击，CPA）
4. Chosen-ciphertext attack（选择密文攻击，CCA）

### Ciphertext-only attack

严格说法：攻击者只观察到一个或多个密文，试图推断出对应明文的信息。

比如，攻击者在公开的信道上，窃听

### Known-plaintext attack

攻击者能拿到一对或多对「明文/密文」，它们由同一把钥匙产生。目标是推断出另一个密文（同一把钥匙加密的）的明文信息。

大白话：攻击者手上有一些「原文—密文」对照样本。 但他不能选这些原文是什么，只是碰巧知道了。

书上的一些例子：

```
例一：两人每次开始通信都先发一句 "hello"
      → 攻击者知道第一段密文对应的明文就是 hello

例二：用加密保护季度财报，直到发布日
      → 发布之后，任何窃听过密文的人就自动获得了对应明文
```

书上非常狠的一个评价：

All the classical encryption schemes we have seen are trivial to break using a known-plaintext attack.

前面那些历史密码，在已知明文攻击下全都不堪一击。

### Chosen-plaintext attack

攻击者可以自己挑选明文，然后拿到对应的密文。

攻击者手里有一台"加密机"，他想加密什么就加密什么，随便试。

和上面那个的区别：

```
② 已知明文：样本是天上掉下来的，攻击者只能被动接受
③ 选择明文：攻击者主动点菜，想要哪个明文的密文就要哪个
```

**例子：**

```
Eve 选定消息 m₁, m₂, m₃, ... 交给加密系统，拿回它们的密文。
```

现实中怎么可能？ 听起来很离谱，但很常见：

- 你给某个加密邮件系统发一封信，对方收到后会加密存档 → 你就"选择"了明文
- Web 服务把你提交的数据加密后存进 cookie
- 历史著名案例：二战中盟军故意让德军观察到特定事件（如在某处布雷），诱使德军发出含已知内容的加密电报。 （这个例子算是比较好理解的了）

> 详细讨论在 §3.4.2（Lec 7 会讲）。

### Chosen-ciphertext attack

攻击者除了②③的能力外，还能选定密文，获得（关于）其解密结果的信息——哪怕只是"解出来是不是一段合法英文"。

大白话：攻击者还有一台"解密机"，可以拿任意密文去问"这个解出来是什么/合不合法"。

当然，他不能直接把目标密文塞进去问（那就没意思了）。

例子：

```
攻击者把改造过的密文发给服务器，观察服务器的反应：
    返回"解密成功" vs "格式错误" vs 直接崩溃
哪怕只泄露一个比特的信息，也可能足以逐步还原明文。
```

这类真实攻击叫 padding-oracle attack（书上 §3.7.2）。

> 详细讨论在 §3.7。

### 四种模型对比

|          模型           | 攻击者能拿到                 | 主动性 | 强度 |
| :---------------------: | :--------------------------- | :----: | :--: |
|     Ciphertext-only     | 只有密文                     |  被动  | 最弱 |
|     Known-plaintext     | 一些（明文, 密文）对         |  被动  |  ↓   |
| Chosen-plaintext (CPA)  | 任意自选明文的密文           |  主动  |  ↓   |
| Chosen-ciphertext (CCA) | 再加上任意自选密文的解密信息 |  主动  | 最强 |

一个关键的结论：

None of these threat models is inherently better than any other; the right one to use depends on the environment in which an encryption scheme is deployed.

> 没有哪个模型"更好"，要看方案实际部署在什么环境里

大白话： 如果你的系统里攻击者根本没机会选明文，那用 CPA 安全的方案就是浪费；反之如果他能选，你用只防唯密文的方案就是找死。

## 从 Perfect Secrecy 转向 Computational Security (Ch 3)

### why and what

为什么要做这个转变：

```
Ch 2 的结论：完美保密做得到，但
    钥匙必须 ≥ 消息一样长（|K| ≥ |M|）
    而且只能用一次
现实中这没法用：要加密 1GB 文件就得先安全传 1GB 钥匙。
```

所以退一步：

> 不要求"绝对安全"，只要求"攻击者算不动"。

书上的原话很实在：

> 一个方案如果只对「投入 200 年超算算力」的攻击者泄露不超过 $2^{-60}$ 的信息，对任何现实应用来说已经足够了。

**严格说法**

两个 relaxations:

> Computational security incorporates two relaxations:
>
> 1. Security is only guaranteed against **efficient adversaries** that run for some feasible amount of time.
> 2. Adversaries can potentially succeed with some **very small probability**.

放宽一：只防"跑得完"的攻击者
        给他无限时间他确实能破，但破解需要的资源远超现实可得 → 就算安全

放宽二：允许极小的失败概率
        只要这个概率小到可以忽略 → 就不用管

注意：放宽的是"安全的标准"，不是"数学的严谨"。 定义和证明照样要写，只是定义变弱了。

|            | Information-theoretic（Ch 2） | Computational（Ch 3 起）   |
| ---------- | ----------------------------- | -------------------------- |
| 攻击者算力 | 无限                          | 有限（多项式时间）         |
| 失败概率   | 必须是 0                      | 允许可忽略的一点点         |
| 钥匙长度   | ≥ 消息长度                    | 可以很短（如 128 位）      |
| 能加密多少 | 一次                          | 可以加密 GB 级、很多条消息 |
| 依赖       | 无（纯数学）                  | 依赖某些数学难题           |

### 量化的方法

#### 第一种：The Concrete Approach

严格的定义：

A scheme is $(t, \varepsilon)$-secure if any adversary running for time at most $t$ succeeds in breaking the scheme with probability at most $\varepsilon$.

把话说死：「跑 $t$ 这么久，成功率不超过 $\varepsilon$」。

**书上的一个例子：**

假设穷举 $n$ 位钥匙需要 $2^n$ 次运算：

```
n = 60：
    4 GHz 台式机（每秒 4×10⁹ 次）→ 2⁶⁰/(4×10⁹) 秒 ≈ ==9 年==     对个人电脑够用
    但当时最快的超算（每秒 2×10¹⁶ 次）→ ==只要 1 分钟==

n = 80：
    那台超算也要跑 ==约 2 年==

n = 128（今天的推荐值）：
    比 2⁸⁰ 大 2⁴⁸ 倍
```

书上给了个的参照：宇宙大爆炸至今的秒数，大约是 $2^{58}$。

这个方式的局限性：

```
"跑 5 年破不了"—— 用什么电脑？台式机还是超算集群？
                   算上摩尔定律的算力增长了吗？
                   用现成算法还是专门优化过的？
而且它只说了 5 年，对"跑 2 年"和"跑 10 年"几乎没说什么。
```

#### The Asymptotic Approach（渐近方式）

这本书，采用的就是这个方法。

严格说法：

引入安全参数（security parameter） $n$（可以理解成钥匙长度）：

1. "高效攻击者" = 运行时间是 $n$ 的多项式的随机算法（记作 ppt = probabilistic polynomial-time）
2. "成功概率很小" = 比任何 $n$ 的多项式的倒数还小，这叫negligible（可忽略）

于是安全的一般形式是：

> A scheme is secure if any ppt adversary succeeds in breaking the scheme with at most negligible probability.

```
安全参数 n = 一个"旋钮"，你想多安全就调多大
   调大 → 更安全，但加解密也更慢、钥匙更长

"多项式时间" = 现实中跑得完
"可忽略概率" = 小到可以当成 0
```

为什么叫"渐近"？ 因为它描述的是 $n$ 足够大时的行为，对小的 $n$ 可能没有意义。

**例子（书上 example 3.2）**

某方案渐近安全，攻击者跑 $n^3$ 分钟能以 $2^{40} \cdot 2^{-n}$ 的概率攻破：

```
n = 40：跑 40³ 分钟（约 6 周）→ 成功概率 = 1     ← 完全不安全！
n = 50：跑 50³ 分钟（约 3 个月）→ 成功概率 ≈ 1/1000  ← 可能还不能接受
n = 500：跑 200 年 → 成功概率 ≈ 2⁻⁵⁰⁰            ← 稳了
```

同一个"渐近安全"的方案，$n$ 取小了照样被秒。所以最终部署时还是得算具体数字。

上面这个例子，电脑变快了咋办。

```
方案：诚实方跑 10⁶·n² 周期，攻击者跑 10⁸·n⁴ 周期能以 ≤2^(−n/2) 成功

用 2 GHz 电脑，取 n = 80：
    诚实方耗时 3.2 秒
    攻击者要跑约 3 周，成功率 2⁻⁴⁰

换成 8 GHz 电脑，把 n 提到 160：
    诚实方还是 3.2 秒（没变慢）
    攻击者要跑 13 周以上，成功率降到 2⁻⁸⁰
```

电脑变快，反而让攻击者更难了——因为诚实方可以把安全参数调更大。

## Cryptography vs Machine Learning

教材没有这部分，这是老师自己的线索，Lec 1 的 topic 列表里也有（"Connection between Cryptography and ML"）。以下是这个主题的标准内容与本课的衔接，具体老师课上讲了什么以课堂为准。

大白话：

- 机器学习的目标：从样本里学出规律
- 密码学的目标：让人从样本里学不出任何规律

**攻击模型，学习范式，一一对应**

| 密码攻击模型            | 对应的机器学习范式                                           |
| ----------------------- | ------------------------------------------------------------ |
| Known-plaintext attack  | 监督学习（supervised learning） —— 给你一堆 (输入, 输出) 对，学出这个函数 |
| Chosen-plaintext attack | 主动学习 / membership query —— 学习者可以主动挑输入去查询    |
| 破解成功                | 学出了那个函数（或它的一个好的近似）                         |

> 「已知明文攻击」在 ML 眼里就是一个标准的监督学习问题：给你 $(m_1, c_1), (m_2, c_2), \ldots$，请学出 $m \mapsto c$ 这个映射。
>
> **一个加密方案安全，等价于说"这个函数族不可学习"。**

所以下一节课（Lec 7，09/14）要讲的 pseudorandom function，从 ML 角度看就是：看再多输入输出对，也学不出下一个输出是什么的函数。

**这个和理论上的深层联系：**

- 计算学习理论（PAC learning）与密码学互为表里：Kearns–Valiant 等人证明了——若某些密码学假设成立，则某些学习问题必然困难。学不动和破不了，是同一个数学事实的两种说法。
- P vs NP 是共同的地基：Lec 1 已经讲过——若 $P = NP$，大量"验证容易、求解难"的问题都会变简单，现代密码学的基础会崩塌。

**两个方向的实际交叉：**

用密码学保护 ML：

```
同态加密        →  在密文上直接做推理，服务器看不到你的数据
安全多方计算    →  多家医院联合训练模型，谁也不交出原始病例
差分隐私        →  发布模型/统计结果时，保证看不出任何单个个体
联邦学习        →  数据不出本地，只交换（加密的）梯度
```

> 这几项 Lec 1 都提过，后面 Lec 21–22（11/02、11/04）会讲 multiparty computation。

用 ML 做密码分析：

```
用神经网络辅助区分器（neural distinguisher）攻击轮减版分组密码
用 ML 做侧信道攻击（从功耗、电磁辐射、时序中提取密钥）
```

**注意：ML 攻不破有严格安全证明的方案**——因为"任何高效算法都攻不破"里，已经包含了所有 ML 算法。ML 的用武之地是攻击没有证明的、或实现有缺陷的系统。

## 课堂实况/版书

依据课堂照片整理。==板书比 syllabus 那一行宽得多==：老师把 Lec 4 的框架复习了一遍，然后直接跳到 PRNG 和计算复杂性——这两块本来排在 Lec 7、Lec 15–17。

> ① GEN/ENC/DEC 框架 + Eve 窃听        ← Lec 4 复习
> ② Single ciphertext attack           ← 即 ciphertext-only
> ③ Hyper-cube（1/2/3/4-cube）
> ④ Pseudo-Random Number Generation (PRNG)
> ⑤ One time pad + Shannon
> ⑥ Computational Complexity Theory（Ax=b、Linear program、Max-flow）
> ⑦ key-exchange、Public-key             ← 后续预告

![image-20260910200004972](./assets/image-20260910200004972.png)

### 1. 框架（复习 Lec4）

```
   GEN
    ↓ k
 [Sender] ──── c ────> [Receiver]
              ↑
             Eve

ENC(k, m) ⟹ c
DEC(k, c) ⟹ m
```

就是上节课的三算法。**「Eve」是密码学里窃听者的传统名字（eavesdropper 的谐音）**，对应 ciphertext-only attack——板书上写的 "Single ciphertext attack"，即只截获一个密文。

### 2. Hyper-cube

把 $\{0,1\}^n$ 画出来

这是为了让你"看见"密钥空间有多大。

$n$ 位 0/1 串的全体 $\{0,1\}^n$，可以画成一个 $n$ 维超立方体，每个顶点就是一个串：

|   维数   | 图形                             |   顶点数   |
| :------: | :------------------------------- | :--------: |
|  1-cube  | 一条线段：`0 —— 1`               | $2^1 = 2$  |
|  2-cube  | 正方形：`00, 01, 10, 11`         | $2^2 = 4$  |
|  3-cube  | 立方体                           | $2^3 = 8$  |
|  4-cube  | 超立方体（画成两个立方体连起来） | $2^4 = 16$ |
| $n$-cube | ——                               |   $2^n$    |

为什么老师要画这个：

```
密钥空间 K = {0,1}^n  →  超立方体的所有顶点
"均匀选一把钥匙"      →  ==在所有顶点里等概率挑一个==
穷举攻击              →  把所有顶点走一遍 → 要走 2^n 步
```

这就是「$n$ 每加 1，难度翻倍」的几何图像。 $n=128$ 时顶点数 $2^{128}$，比宇宙年龄的秒数（约 $2^{58}$）还多得多。

### 3. PRNG：Pseudo-Random Number Generation

这个很重要，是重点！

**板书上的核心式子：**

$\phi: \{0,1\}^n \longrightarrow \{0,1\}^L, \qquad \text{where } L > n$

大白话：拿一小把真随机的种子，"拉长"成一大串看起来也很随机的东西。

```
输入：n 位真随机（种子 seed）      比如 128 位
输出：L 位伪随机（L 比 n 大得多）   比如 1 GB
```

"伪随机"是什么意思：它不是真随机（$2^L$ 种可能里只能产生 $2^n$ 种，绝大多数串根本产生不出来），但任何高效算法都区分不出它和真随机的差别。

**为什么老师把 PRNG 和 One-time pad 画在一起**

这是本节最关键的一条线索。 回忆上节课的死结：

```
OTP 完美保密，但钥匙必须和消息一样长（Shannon: |K| ≥ |M|）
要加密 1GB，就得先安全传 1GB 钥匙 → 不实用
```

**PRNG 就是破局的办法：**

```
只安全共享 128 位的种子 s
双方各自用 φ 把 s 拉成 1GB 的伪随机串
再拿这个串当"一次性密码本"去异或
```

这样实际共享的钥匙只有 128 位，却加密了 1GB。

但这跳出 Shannon 定理了吗？ 没有——它不再是完美保密了。真钥匙空间只有 $2^{128}$，理论上攻击者穷举种子就能破。只不过 $2^{128}$ 次运算算不动。

```
完美保密（Ch 2）→ 无条件安全，但 |K| ≥ |M|
计算安全（Ch 3）→ 用 PRNG 绕开，代价是"只对高效攻击者安全"
```

### 4. One time pad + Shannon (复习 Lec4)

板书：

```
M = {0,1}^L        消息空间
K = {0,1}^L        密钥空间   ← 和消息一样长
C = {0,1}^L        密文空间

GEN:  在 K 上均匀取
Enc(k, m) = k ⊕ m
Dec(k, c) = k ⊕ c
```

「Shannon」指的就是 Shannon 定理 / $|\mathcal{K}| \ge |\mathcal{M}|$（K&L Theorem 2.10，书上 §2.4 还有更完整的 Shannon's Theorem）。

### 5. Computational Complexity Theory

板书列了三个例子：

```
Ax = b              解线性方程组
Linear program      线性规划：max cᵀx  s.t. Ax ≤ b
Max-flow            最大流
```

为什么讲这些： 这三个都是 P 里的问题——有多项式时间算法，"算得动"。

老师大概率是在铺垫这个对比：

|                | 例子                           | 性质                     |
| -------------- | ------------------------------ | ------------------------ |
| 算得动（P）    | 解线性方程组、线性规划、最大流 | 多项式时间               |
| 算不动（相信） | 大整数分解、离散对数           | 密码学的安全就建在这上面 |

> 呼应 Lec 1 的 P vs NP，也呼应本节第三部分的 "efficient adversary = 多项式时间"。「高效」这个词的精确含义，就是复杂性理论给的。

### 6. 预告: key-exchange、Public-key

板书最右边写了这两个词，是后面的内容：

```
Lec 12 (09/30)  Diffie-Hellman Key Exchange
Lec 8  (09/16)  Public-key encryption
```

解决的正是"钥匙怎么先共享"这个一直被搁置的问题。