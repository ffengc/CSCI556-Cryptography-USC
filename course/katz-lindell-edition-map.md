# Katz & Lindell 第1版 ↔ 第2版 章节对照

> 课程 outline 里所有 "Chapter N" 用的是**第 2 版**编号。
> 第 1 版：© 2008，553 页。
> 下表把 syllabus 引用的章节翻译成第 1 版编号。

## 第 1 版的章节结构（摘自第 1 版目录）

| Ch | 标题 |
| --- | --- |
| 1 | Introduction |
| 2 | Perfectly Secret Encryption |
| 3 | Private-Key Encryption and Pseudorandomness |
| 4 | Message Authentication Codes and Collision-Resistant Hash Functions |
| 5 | Practical Constructions of Pseudorandom Permutations (Block Ciphers) |
| 6 | *Theoretical Constructions of Pseudorandom Objects |
| 7 | Number Theory and Cryptographic Hardness Assumptions |
| 8 | *Factoring and Computing Discrete Logarithms |
| 9 | Private-Key Management and the Public-Key Revolution |
| 10 | Public-Key Encryption |
| 11 | *Additional Public-Key Encryption Schemes |
| 12 | Digital Signature Schemes |
| 13 | Public-Key Cryptosystems in the Random Oracle Model |

## syllabus 引用 → 第 1 版对应位置

| Lec | syllabus 写的（第2版） | 主题 | 第 1 版去读 |
| --- | --- | --- | --- |
| 1 | Ch 1–3 | 概览 | Ch 1–3（基本一致） |
| 2 | **Ch 13.3** | Shamir Secret Sharing | ==第 1 版没有这一节== —— 全书 "secret sharing" 零次出现，只能用 Shamir 原论文 |
| 2, 8–10, 15, 17 | **Ch 8** | Number Theory Basics | **Ch 7**（Number Theory and Cryptographic Hardness Assumptions） |
| 4 | Ch 1.2, 1.4, Ch 2 | Private-Key 框架、Perfect Secrecy | 同号 ✓ |
| 6 | Ch 3 | Cryptanalysis、攻击类型 | 同号 ✓ |
| 7 | Ch 3.1–3.3 | 计算安全、Pseudorandomness | 同号 ✓ |
| 8 | Ch 10.1, 10.2 | PKE 概览、CPA 定义 | 同号 ✓（1版 10.1 overview / 10.2 definitions、10.2.1 CPA） |
| 9 | **Ch 11.1** | PKE 概览 | **Ch 10.1** |
| 9, 10 | **Ch 11.5** | RSA | **Ch 10.4**（RSA Encryption，含 Textbook RSA 不安全性、Padded RSA） |
| 9, 10 | Ch 10.4 | Diffie–Hellman（作背景） | **Ch 9.3 附近**（The Public-Key Revolution） |
| 11 | **Ch 13.5** | Rabin Encryption | **Ch 11.2**（11.2.3 The Rabin Encryption Scheme） |
| 11 | **Ch 13.4** | Probabilistic Encryption | **Ch 11.1**（Goldwasser–Micali） |
| 12 | Ch 10 | Diffie–Hellman Key Exchange | **Ch 9**（Private-Key Management and the Public-Key Revolution） |
| 14 | **Ch 11.4.1** | El Gamal | **Ch 10.5**（The El Gamal Encryption Scheme） |
| 15, 17 | **Ch 7.4–7.8** | Blum–Micali、BBS、Indistinguishability | **Ch 6.4–6.8**（整体减 1：6.4 构造 PRG、6.5 PRF、6.6 PRP、6.7 必要假设、6.8 Computational Indistinguishability） |
| 18 | **Ch 7.1–7.3** | One-way / trapdoor functions、hardcore bits | **Ch 6.1–6.3**（6.3 A Hard-Core Predicate for Any One-Way Function） |
| 15, 17 | Fun reading Ch 5 | — | **Ch 4.6**（Collision-Resistant Hash Functions）；第2版 Ch5 是 Hash Functions，第1版并入 Ch4 |

**记忆口诀：**
- 第2版 **Ch 7（理论构造/OWF/PRG）** = 第1版 **Ch 6**
- 第2版 **Ch 8（数论）** = 第1版 **Ch 7**
- 第2版 **Ch 9（算法数论）** = 第1版 **Ch 8**
- 第2版 **Ch 10（密钥管理/DH）** = 第1版 **Ch 9**
- 第2版 **Ch 11（PKE：El Gamal、RSA）** = 第1版 **Ch 10**
- 第2版 **Ch 13.3/13.4/13.5** = 第1版 **无 / 11.1 / 11.2**
- Ch 1–4、Ch 12 两版基本同号
