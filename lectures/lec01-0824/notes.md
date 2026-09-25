# aug24 Lecture #1

## Topic/Event and Required Reading

> Class organization:   data, information, and knowledge
>    communication and computation
>    How to model information, knowledge, security, and privacy
>    Perfect Information Security vs Computational Security
>    P vs NP
>    Symmetric ciphers vs Public-Key Encryption
>    Indistinguishability and unpredictability
>    Randomization and Interactive Proofs
>    Languages, NLP, ML

> Chapters 1-3
> Search on the Web or with an AI system. For example:
>  "[google | chatgpt] P NP"
>  "[google | chatgpt] RSA"
>  "[google | chatgpt] Zero Knowledge Proof"
>  "[google | chatgpt] MAC"
>  "[google | chatgpt] Digital Signature"
>  "[google | chatgpt] Homomorphic Encryption"
>  "[google | chatgpt] Differential Privacy"
>  "[google | chatgpt] Network Security"
>  "[google | chatgpt] Connection between Cryptography and ML"

## P and NP

https://github.com/ffengc/NP-is-not-that-Hard

Complexity theory

复杂性理论主要关注输入规模增长时的趋势，而不是某台电脑跑几秒。

P 是这样一类问题：

> 存在一个确定性算法（deterministic algorithm），能够在多项式时间内求出答案。

严格来说，P 通常讨论判定问题（decision problem），也就是答案只有：YES or NO

**P = 容易求解，也容易验证。**

**NP（Nondeterministic Polynomial Time）**：不一定能快速找到答案，但如果别人把答案给你，你能够快速验证。

比如数独：

- 自己从头找到解可能很难；
- 别人给你一个填好的答案，你可以很快检查它对不对。

所以可以理解为：

> **NP = 答案可能难找，但容易验证。**

注意，NP 不是 **Non-Polynomial**，不是“不能在多项式时间解决”的意思。

**P vs NP 问题**

因为能快速求解的问题肯定也能快速验证，所以：

$P\subseteq NP$

现在不知道的是：

$P=NP\quad\text{还是}\quad P\neq NP$

也就是：

> 一个答案如果能够被快速验证，是否意味着它也能够被快速找到？

目前没人证明出来，但大多数研究者认为：

$P\neq NP$

**NP-complete**

**NP-complete** 是 NP 中最难的一类问题，例如 SAT、Hamiltonian Cycle、TSP 的判定版本。

如果任何一个 NP-complete 问题被发现存在多项式时间算法，那么：

$P=NP$

与密码学的关系

密码学依赖“某些问题容易正向计算，但很难反向求解”。

例如 RSA：

- 两个大质数相乘很容易；
- 从乘积重新分解出两个质数，被认为很难。

**如果 $P=NP$，许多“答案容易验证、但很难找到”的问题都会变得容易，现代密码学的大量安全基础可能因此崩溃。**

## RSA

### RSA 是什么？

**RSA** 是一种经典的公钥密码算法（public-key cryptography），名字来自三位作者：

- Rivest
- Shamir
- Adleman

它可以用于：

- ==公钥加密（public-key encryption）==
- ==数字签名（digital signature）==

RSA 的核心思想是：

> ==两个大质数相乘很容易，但只知道乘积，想把它重新分解成原来的两个质数很困难。==

------

#### 公钥和私钥

RSA 有两把钥匙：

- **Public key（公钥）**：可以公开，任何人都能用
- **Private key（私钥）**：只有接收者自己知道

用于加密时：

1. 别人使用你的公钥加密消息；
2. 只有你能使用私钥解密。

可以简单表示为：

$\text{Ciphertext}=\text{Encrypt}(\text{Public Key},\text{Message})$ 

$\text{Message}=\text{Decrypt}(\text{Private Key},\text{Ciphertext})$

------

#### RSA 怎么生成密钥？

**第一步：选择两个大质数**

选择：

$p,\ q$

现实中它们非常大。为了演示，我们使用：

$p=3,\quad q=11$

**第二步：计算 $n$**

$n=pq=3\times11=33$

$n$ 会同时出现在公钥和私钥中。

**第三步：计算 Euler’s totient**

Euler’s totient: 欧拉总计函数

$\phi(n)=(p-1)(q-1)$

所以：

$\phi(33)=2\times10=20$

**第四步：选择公钥指数 $e$**

要求 $e$ 与 $\phi(n)$ 互质。这里选择：

$e=3$

**第五步：计算私钥指数 $d$**

找到 $d$，使得：

$ed\equiv1\pmod{\phi(n)}$

这里：

$3\times7=21\equiv1\pmod{20}$

所以：

$d=7$

最终：

- 公钥（public key）：$(n,e)=(33,3)$
- 私钥（private key）：$(n,d)=(33,7)$

------

### RSA 如何加密？

假设消息转换成数字：

$m=4$

使用公钥加密：

$c=m^e\bmod n$

所以：

$c=4^3\bmod33=64\bmod33=31$

密文是：

$c=31$

------

### RSA 如何解密？

使用私钥：

$m=c^d\bmod n$

因此：

$m=31^7\bmod33=4$

成功恢复原始消息。

RSA 能正确解密，背后的数学基础主要是：

- Modular Arithmetic（模运算）
- Euler’s Theorem（欧拉定理）
- Chinese Remainder Theorem（中国剩余定理）

这些内容课程后面会具体讲。

------

### RSA 为什么被认为安全？

攻击者知道：

$n,\ e,\ c$

但不知道：

$p,\ q,\ d$

如果能从 $n$ 分解出 $p,q$，就能计算 $\phi(n)$，进而算出私钥 $d$。

因此 RSA 的安全性与 **Integer Factorization（大整数分解）**的困难性密切相关。

需要注意：

> ==大整数分解目前没有被证明一定很难，也没有被证明是 NP-complete；只是经典计算机目前没有已知的高效算法。==

------

### 一个重要问题：原始 RSA 并不安全

上面的公式叫 **Textbook RSA**：

$c=m^e\bmod n$

它是确定性的（deterministic）：相同消息使用相同公钥，每次都会产生相同密文。因此攻击者可以比较密文、猜测消息，还可能进行其他攻击。

现实中必须使用安全的编码或填充方案（padding scheme）：

- RSA-OAEP：用于加密
- RSA-PSS：用于签名

==所以不能自己直接使用原始 RSA 公式处理真实数据。==

一句话总结：

> **RSA 利用“乘法容易、分解困难”的不对称性，让公钥可以公开，而私钥仍然难以计算。**

## Zero Knowledge Proof 

**Zero-Knowledge Proof，ZKP（零知识证明）**是一种密码学协议：

> 证明者（Prover）可以向验证者（Verifier）证明“我知道某个秘密”或“某个陈述是真的”，但不泄露秘密本身。

==例如，你可以证明：==

> ==我知道某个账户的密码。==

==但整个过程中，你不需要把密码告诉对方。==

### 经典例子：洞穴问题

假设一个环形洞穴有左右两条路，中间有一道需要密码才能打开的门。

Peggy 是证明者，声称自己知道密码；Victor 是验证者。

过程如下：

1. Peggy 随机选择左路或右路进入洞穴，Victor看不到她选择了哪条路。
2. Victor站在入口，随机要求她从左边或右边出来。
3. 如果Peggy知道密码，她可以打开中间的门，因此无论Victor要求哪边，她都能从指定方向出来。
4. 如果她不知道密码，只有在Victor恰好选择她进入的那一边时，才能成功，单次蒙对概率是：

$\frac{1}{2}$

重复 $k$ 次后，骗子全部蒙对的概率是：

$\left(\frac{1}{2}\right)^k$

例如重复20次：

$\left(\frac{1}{2}\right)^{20}\approx0.000095\%$

因此，Victor可以非常确信Peggy知道密码，但始终没有看到密码本身。

### ZKP 必须满足三个性质

**Completeness（完备性）**

如果陈述是真的，并且证明者确实知道秘密，那么诚实验证者应该接受证明。

> 真的东西能够被成功证明。

**Soundness（可靠性）**

如果陈述是假的，作弊的证明者不应该能够轻易欺骗验证者。

> 不知道秘密的人，通过验证的概率应该极低。

严格来说，通常不能保证概率绝对为零，但可以通过重复协议，使作弊概率变得可以忽略（negligible）。

**Zero-Knowledge（零知识性）**

验证者除了知道“这个陈述是真的”之外，不应得到其他有用信息。

> 验证者知道你确实掌握秘密，但不知道秘密是什么。

### 它和 NP 有什么关系？

NP 问题的特点是：给出一个 witness 后，可以快速验证。

传统证明中，证明者直接把 witness 交给验证者。但在 Zero-Knowledge Proof 中：

> Prover证明自己拥有一个有效的 witness，却不把 witness 本身交出来。

例如，证明：

$\text{“我知道 }x\text{，使得 }f(x)=y\text{。”}$

验证者最终相信你知道 $x$，但无法得知 $x$。

### Interactive 和 Non-interactive

传统的 ZKP 是交互式的（interactive）：

1. Prover发送信息；
2. Verifier随机提出 challenge；
3. Prover作出 response；
4. 双方重复多轮。

现代系统也常用 **Non-Interactive Zero-Knowledge Proof，NIZK**，证明者生成一份证明，验证者可以独立检查，不需要来回交流。

常见技术包括：

- zk-SNARK
- zk-STARK
- Bulletproofs

### 有什么用途？

- 身份验证：证明知道密码，但不发送密码
- Blockchain：证明交易合法，但隐藏金额或参与者
- 匿名认证：证明“我满足某个条件”，但不暴露身份
- 隐私计算：证明计算结果正确，但不公开输入数据
- Multi-party Computation：证明参与者正确执行了协议

一句话总结：

> **Zero-Knowledge Proof 让你证明“我知道”，但不需要告诉对方“我知道的具体是什么”。**

## MAC

### MAC 是什么？

这里的 **MAC** 指：

> **Message Authentication Code（消息认证码）**

不是网络里的 Media Access Control。

MAC 用来保证两件事：

1. **Integrity（完整性）**：消息没有被篡改。
2. **Authenticity（真实性）**：消息确实来自持有密钥的人。

它**不负责加密消息**，所以不保证 Confidentiality（机密性）。

### MAC 怎么工作？

通信双方 Alice 和 Bob 事先共享一个秘密密钥：

$k$

Alice 要发送消息 $m$，先计算：

$t=\operatorname{MAC}_k(m)$

这里的 $t$ 叫：

- Tag
- Authentication Tag
- MAC Tag

Alice 把消息和 Tag 一起发送：

$(m,t)$

Bob 收到后，用同一个密钥重新计算：

$t'=\operatorname{MAC}_k(m)$

然后检查：

$t'\stackrel{?}{=}t$

- 相等：接受消息
- 不相等：消息被篡改，或者发送者没有正确密钥

**简单例子**

Alice 发送：

> Transfer $100 to Bob

同时计算对应的 MAC Tag。

攻击者把消息修改为：

> Transfer $10,000 to Bob

但攻击者不知道秘密密钥 $k$，所以无法为新消息生成正确的 Tag。

Bob重新验证时，计算结果不一致，于是拒绝消息。

### MAC 和普通 Hash 的区别

普通 Hash：

$h=H(m)$

任何人都能计算。如果攻击者修改消息，也可以重新计算一个新的 Hash，所以不能证明消息来自谁。

MAC 使用秘密密钥：

$t=\operatorname{MAC}_k(m)$

攻击者不知道密钥，因此即使修改消息，也无法生成合法 Tag。

| 方法 | 使用密钥     | 检测篡改 | 验证发送者 |
| ---- | ------------ | -------- | ---------- |
| Hash | 否           | 有限     | 否         |
| MAC  | 是，共享密钥 | 是       | 是         |

### 常见的 MAC：HMAC

最常见的是：

> **HMAC（Hash-based Message Authentication Code）**

它利用 Hash Function 和秘密密钥生成 Tag，例如：

- HMAC-SHA256
- HMAC-SHA512

不要简单地自己设计成：

$H(k\|m)$

因为某些 Hash Function 可能受到 length-extension attack。应该直接使用标准 HMAC。

### MAC 的局限性

#### 1. 不提供加密

消息本身仍然可以被别人看到：

$(m,\operatorname{MAC}_k(m))$

所以实际系统经常同时使用：

- Encryption：隐藏消息内容
- MAC：防止消息被篡改

现代系统通常使用 **Authenticated Encryption**，例如 AES-GCM，一次同时提供机密性和完整性。

#### 2. 双方共享同一把密钥

Alice 和 Bob 都知道 $k$，因此 Bob 也能生成一个合法 MAC。

所以 MAC 不能向第三方证明“这条消息一定是 Alice 发的”。它不提供：

> **Non-repudiation（不可否认性）**

数字签名可以解决这个问题。

#### 3. MAC 本身不能防止 Replay Attack

攻击者虽然不能修改消息，却可能把一条旧消息原样重新发送。

因此通常还要在消息中加入：

- Nonce
- Timestamp
- Sequence Number

一句话总结：

> **MAC 使用双方共享的秘密密钥，为消息生成一个 Tag，用来验证消息没有被修改，并且来自持有密钥的一方。**

## Digital Signature

### Digital Signature 是什么？

**Digital Signature（数字签名）**用于证明：

1. 消息确实来自某个发送者——**Authenticity**
2. 消息没有被修改——**Integrity**
3. 发送者事后难以否认——**Non-repudiation**

它使用公钥密码体系：

- 发送者用 **Private Key（私钥）**签名
- 其他人用 **Public Key（公钥）**验证

过程可以简单表示为：

$\sigma=\operatorname{Sign}_{sk}(m)$

其中 $\sigma$ 是 signature。接收者检查：

$\operatorname{Verify}_{pk}(m,\sigma)$

如果消息被修改，验证就会失败。

实际中通常不是直接签整个消息，而是先计算 Hash：

$h=H(m)$

然后对 Hash 签名，这样效率更高。

### 和 MAC 的区别

| MAC                       | Digital Signature        |
| ------------------------- | ------------------------ |
| 双方共享同一把 secret key | 私钥签名，公钥验证       |
| 只有共享密钥的人能验证    | 任何拥有公钥的人都能验证 |
| 不提供不可否认性          | 可以提供不可否认性       |

常见数字签名算法包括：

- ==RSA-PSS==
- ==ECDSA==
- ==Ed25519==

一句话记忆：

> **Digital Signature 就是发送者用私钥“盖章”，其他人用公钥检查这个章是不是真的。**

## Homomorphic Encryption

### Homomorphic Encryption 是什么？

**Homomorphic Encryption（同态加密）**允许别人：

> ==**直接在密文（ciphertext）上进行计算，而不需要先解密。**==

计算完成后，数据拥有者解密结果，会得到与明文计算相同的答案。

简单表示：

$\operatorname{Decrypt}( \operatorname{Compute}(\operatorname{Encrypt}(x)) ) = \operatorname{Compute}(x)$

### 简单例子

Alice 有两个秘密数字：

$x=10,\quad y=20$

她把它们加密后发送给云服务器。服务器不知道 $x,y$ 的具体数值，但可以直接对密文做加法。

服务器返回计算后的密文，Alice 解密后得到：

$x+y=30$

==整个过程中，服务器只接触密文，没有看到原始数据。==

### 三种类型

- ==**Partially Homomorphic Encryption（PHE）**：只支持一种运算，例如加法或乘法。==
- ==**Somewhat Homomorphic Encryption（SHE）**：支持有限次数的加法和乘法。==
- ==**Fully Homomorphic Encryption（FHE）**：理论上可以在密文上执行任意计算。==

### 与机器学习的关系

例如医院可以加密患者数据，再让云服务器运行 ML model：

- 云服务器看不到患者数据；
- 模型直接处理密文；
- 医院解密得到预测结果。

==主要缺点是计算和存储开销很大，通常比明文计算慢很多。==

一句话记忆：

> ==**Homomorphic Encryption = 数据一直保持加密，但仍然可以被计算。**==

## Differential Privacy

### Differential Privacy 是什么？

**Differential Privacy（差分隐私，DP）**是一种保护个人数据的数学方法。

核心目标是：

> 无论某个人的数据是否包含在数据集中，最终统计结果都应该非常接近。

因此，攻击者看到结果后，很难判断某个具体的人是否参与了数据集，也很难推断他的个人信息。

### 简单例子

学校想公布学生的平均工资，但又不想泄露某个学生的信息。

系统不会直接公布精确结果，而是加入少量随机噪声（random noise）：

$\text{Published Result} = \text{True Result}+\text{Noise}$

例如真实平均工资是：

$\$80{,}000$

系统可能公布：

$\$80{,}137$

==加入噪声后，整体统计仍然有价值，但很难从结果反推出某个具体人的工资。==

### 隐私参数 $\epsilon$

Differential Privacy 经常使用：

$\epsilon\quad\text{(epsilon)}$

表示隐私强度：

- $\epsilon$ 越小：隐私越强，但噪声越大
- $\epsilon$ 越大：结果越准确，但隐私越弱

这体现了：

> **Privacy–Utility Tradeoff（隐私与数据可用性的权衡）**

### 与机器学习的关系

训练模型时，可以给：

- Training data
- Gradient
- Model parameters

加入控制过的噪声，避免模型记住并泄露某个训练样本，这类方法常叫 **Differentially Private Machine Learning**，例如 DP-SGD。

它和加密不同：

- Encryption：隐藏数据内容
- Differential Privacy：防止从统计结果或模型中推断单个人的信息

一句话记忆：

> **Differential Privacy 通过加入受控随机性，让结果保留整体规律，同时隐藏单个个体的影响。**

## Network Security

### Network Security 是什么？

**Network Security（网络安全）**是保护计算机、服务器和网络通信不被攻击、窃听、篡改或破坏。

主要保护三个目标，合称 **CIA Triad**：

- **Confidentiality（机密性）**：别人看不到数据
- **Integrity（完整性）**：数据没有被篡改
- **Availability（可用性）**：合法用户可以正常使用系统

通常还包括 **Authentication（身份验证）**：确认通信对象是谁。

### 常见网络攻击

- **Eavesdropping**：窃听通信内容
- **Man-in-the-Middle Attack, MITM**：攻击者夹在通信双方中间
- **Spoofing**：伪造身份或地址
- **Replay Attack**：重复发送以前截获的合法消息
- **Denial-of-Service, DoS**：发送大量请求，让服务无法使用
- **Malware**：病毒、木马、勒索软件等

### 常见防护方式

- **TLS/HTTPS**：加密浏览器与服务器之间的通信
- **Firewall**：过滤不安全的网络流量
- **VPN**：建立加密通信通道
- **Authentication**：密码、多因素认证、证书
- **IDS/IPS**：检测或阻止异常网络行为
- **Digital Certificate**：验证网站或服务器的身份

### 和 Cryptography 的关系

Cryptography 是 Network Security 的重要工具：

- Encryption 保护 confidentiality
- MAC 和 digital signature 保护 integrity、authenticity
- Key exchange 帮助双方建立秘密密钥

但 Network Security 范围更大，还包括系统漏洞、软件安全、权限管理、网络配置和 DoS 防御。

一句话记忆：

> **Cryptography 研究如何用数学保护信息；Network Security 研究如何保护整个网络通信和系统。**

##  Connection between Cryptography and ML

### Cryptography 和 ML 有什么联系？

Cryptography 和 Machine Learning 的联系主要有两个方向：

### 1. 用 Cryptography 保护 ML

机器学习经常需要处理敏感数据，例如：

- 医疗记录
- 人脸和身份信息
- 用户行为
- 金融数据

密码学可以让模型在不直接暴露数据的情况下训练或预测。

常见方法包括：

- **Homomorphic Encryption**：模型直接处理加密数据
- **Secure Multi-Party Computation, MPC**：多方共同训练模型，但不公开各自的数据
- **Secure Aggregation**：服务器只能看到多个用户更新后的总和
- **Zero-Knowledge Proof**：证明模型正确执行了计算，但不公开输入或模型
- **Differential Privacy**：减少模型泄露单个训练样本的风险

例如，医院把加密后的患者数据交给云服务器。服务器运行 ML model，但看不到患者原始信息，医院最后解密预测结果。

------

### 2. 用 ML 攻击或分析 Cryptographic Systems

机器学习也可以用于安全分析，例如：

- 识别网络攻击
- 检测恶意软件
- 分析加密流量
- 发现实现中的异常
- 进行 **Side-Channel Attack（侧信道攻击）**

例如，攻击者可能用 ML 分析设备的运行时间、功耗或电磁信号，从而推测密码学密钥。

这里通常不是直接破解数学算法，而是攻击它的具体实现。

------

### 3. 两者共享的理论概念

Cryptography 和 ML 都关心：

- **Randomness（随机性）**
- **Indistinguishability（不可区分性）**
- **Adversary（对手）**
- **Information Leakage（信息泄露）**
- **Computational Hardness（计算困难性）**

例如密码学会问：

> 攻击者能否区分真正的随机数和伪随机数？

机器学习会问：

> 模型能否区分来自不同分布的数据？

某些密码学假设还可以用来证明：

> 某些学习问题在计算上很难，或者不存在高效的学习算法。

一句话总结：

> **Cryptography 可以保护 ML 的数据、模型和计算；ML 也可以分析或攻击密码系统；两者在随机性、不可区分性和计算困难性方面有很深的理论联系。**

## Shamir’s CACM paper

> **How to Share a Secret**
>  作者：Adi Shamir
>  发表：1979 年，*Communications of the ACM (CACM)*

Adi Shamir 就是 **RSA 中的“S”**。这篇论文只有两页，但提出了非常经典的 **Shamir’s Secret Sharing（沙米尔秘密共享）**。[MIT 提供的论文 PDF](https://web.mit.edu/6.857/OldStuff/Fall03/ref/Shamir-HowToShareASecret.pdf?utm_source=chatgpt.com)

### 论文解决什么问题？

假设有一个秘密 $D$，例如公司的私钥。希望把它分成 $n$ 份：

- 任意 $k$ 份放在一起，可以恢复秘密；
- 只有 $k-1$ 份时，得不到任何关于秘密的信息。

这叫：

$(k,n)\text{-threshold scheme}$

例如 $(3,5)$：

- 秘密被分给5个人；
- 任意3个人合作可以恢复；
- 只有1个或2个人，无法获得秘密。

### 核心方法

Shamir把秘密放进一个多项式：

$q(x)=D+a_1x+\cdots+a_{k-1}x^{k-1}$

其中秘密就是：

$q(0)=D$

然后把多项式上的不同点分给不同的人：

$(1,q(1)),\ (2,q(2)),\ldots,(n,q(n))$

一个 $k-1$ 次多项式需要 $k$ 个点才能唯一确定。因此：

- 拿到 $k$ 个点：通过 **Polynomial Interpolation（多项式插值）**恢复 $q(x)$，进而得到 $q(0)=D$；
- 少于 $k$ 个点：存在很多条可能的多项式，无法确定秘密。

### 为什么叫 Perfect Security？

因为少于 $k$ 份时，不是“计算起来太难”，而是：

> 从数学上没有足够信息确定秘密，每个可能的秘密仍然同样可能。

这属于 **Information-Theoretic Security（信息论安全）**，不依赖攻击者的电脑有多强。

所以明天这节课会用到：

- Polynomial Interpolation
- Modular Arithmetic
- Finite Field
- 一点 Linear Algebra 和 Number Theory

这篇论文就是你们 8 月 26 日这节课的核心内容。