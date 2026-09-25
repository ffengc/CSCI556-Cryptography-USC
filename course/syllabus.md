# CSCI 556 课程信息（Fall 2026）

> 来源：https://viterbi-web.usc.edu/~shanghua/teaching/Fall2026-556/index.html
> 最后同步：2026-09-06。老师说明 outline **subject to changes**，每次开工前跑 `bash course/sync.sh`。

## 基本信息

| 项 | 内容 |
| --- | --- |
| 课程 | CSCI 556, Introduction to Cryptography |
| 老师 | Professor Shang-Hua Teng，shanghua[@]usc.edu |
| Office Hours | ==每周一== 12:15–1:45 PM，GCS SB9（B2），或预约 |
| TA | TBA（网页上仍未公布） |
| 上课 | Mon & Wed 10:00–11:50 AM，VPD 105 |
| 先修 | CSCI 270 或老师许可 |

## 成绩构成

==注意：网页上的 Homeworks 一栏是空的，成绩里没有作业分数。== 分数全部压在参与、Quiz 和 project 上。

| 权重 | 项目 |
| --- | --- |
| 15% | Class Participation |
| 20% | Quiz（唯一一次，**10/05**） |
| 20% | Theoretical Thinking Assignment：2 页写作 + 5 分钟课堂展示 |
| 20% | Project 展示：30 分钟，**三人一组** |
| 25% | Term Report：5 页，**个人**独立写作 |

三个大件（Theoretical Thinking、Project、Term Report）占 65%，且网页未给 due date —— 需要从课堂/Brightspace 确认。

## 教材

- **必读**：*Introduction to Modern Cryptography*, 2nd Edition — Jonathan Katz & Yehuda Lindell。课程表里所有 "Chapter X" 都指这本。
- 补充：[A Graduate Course in Applied Cryptography](https://toc.cryptobook.us/) — Boneh & Shoup（本地已有 `readings/textbooks/Boneh-Shoup-v0.6.pdf`）
- 另外会用研究论文作 handout。

## Course Outline

| Lec | 日期 | 主题 | Required Reading | Fun Reading |
| --- | --- | --- | --- | --- |
| 1 | 08/24 | Class organization；data/information/knowledge；建模 security & privacy；Perfect vs Computational Security；P vs NP；对称 vs 公钥；Indistinguishability & unpredictability；Randomization & Interactive Proofs；Languages, NLP, ML | Ch 1–3 | Ch 1.3 |
| 2 | 08/26 | Shamir's Secret Sharing：perfect security 的例子；线性代数与数论基础 | Handout（Shamir CACM）, Ch 13.3, Ch 8 | Ch 1.1 |
| 3 | 08/31 | Secret Sharing II：General Scheme；Access Control；Monotone Formula；Two Basic Primitives | Handout: Benaloh–Leichter | Quantum Secret Sharing（[1](https://arxiv.org/pdf/quant-ph/0405179.pdf), [2](https://arxiv.org/pdf/quant-ph/9806063.pdf)） |
| 4 | 09/02 | Private-Key Encryption 的经典框架：Key generation、Encryption/Decryption、Perfectly-Secret Private-Key Encryption、Information and Probability | Ch 1.2, 1.4, Ch 2 | |
| 5 | 09/07 | **Labor Day，停课** | | |
| 6 | 09/09 | Cryptography vs Machine Learning；Cryptanalysis；Types of Attacks | Ch 3 | |
| 7 | 09/14 | Computational Approach to Security；复杂性理论基础；Randomness vs Pseudorandomness | Ch 3.1–3.3 | |
| 8 | 09/16 | 现代安全通信框架：Public-key encryption；Chosen Plaintext Attacks；数论基础 | Ch 10.1, 10.2, Ch 7, Ch 8 | |
| 9 | 09/21 | RSA；中国剩余定理 | Ch 10.4, Ch 7, Ch 11.1, Ch 11.5, Ch 8 | |
| 10 | 09/23 | RSA（续）；数论基础 | Ch 10.4, Ch 8, Ch 11.5 | |
| 11 | 09/28 | Rabin Encryption Scheme；Probabilistic Encryption | Ch 13.5, Ch 13.4 | |
| 12 | 09/30 | Diffie–Hellman Key Exchange | Ch 10 | |
| 13 | 10/05 | **Quiz** | | |
| 14 | 10/07 | Discrete Logarithms；El Gamal（Fall Recess 前一天） | Ch 11.4.1 | |
| 15 | 10/12 | Pseudorandom Generation：Blum–Micali；数论基础 | Ch 7.4–7.8, Ch 8 | Ch 5 |
| 16 | 10/14 | Pseudorandom number generation | | |
| 17 | 10/19 | Indistinguishability and unpredictability；Blum–Blum–Shub PRG | Ch 7.4–7.8, Ch 8 | Ch 5 |
| 18 | 10/21 | One-way and trapdoor functions；hardcore bits | Ch 7.1–7.3 | |
| 19 | 10/26 | Interactive and Zero Knowledge Proofs | Handout | Handout |
| 20 | 10/28 | Zero Knowledge Proofs | Handout | |
| 21 | 11/02 | ZKP 应用：Multiparty Computation | Handout | |
| 22 | 11/04 | ZKP 应用：Multiparty Computation | | |
| 23 | 11/09 | Presentation | | |
| 24 | 11/11 | **Veterans Day，停课** | | |
| 25 | 11/16 | Presentation | | |
| 26 | 11/18 | Presentation | | |
| 27 | 11/23 | Presentation（Thanksgiving 前最后一节） | | |
| 28 | 11/25 | **Thanksgiving，停课** | | |
| 29 | 11/30 | Presentation | | |
| 30 | 12/02 | Presentation | | |

## 学术诚信

网页原文：抄袭及其他 anti-intellectual behavior 会被严肃处理，包括挂科或被学校开除。

## 目前进度对照

- 已有笔记：Lec1、Lec3、Lec4、Lec6、Lec7、Lec8，每讲在 `lectures/lecNN-MMDD/` 下（索引见 [README](../README.md)）
- ==缺笔记：Lec2（08/26 Shamir）==
- `homework/hw1/hw1.pdf` 不在课程主页上，来源应是课堂或 Brightspace
