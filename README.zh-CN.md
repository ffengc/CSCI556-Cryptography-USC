# CSCI 556 · Introduction to Cryptography

<p align="center">
  <a href="README.md"><img src="https://img.shields.io/badge/English-lightgrey?style=for-the-badge" alt="English"></a>
  <a href="README.zh-CN.md"><img src="https://img.shields.io/badge/简体中文-blue?style=for-the-badge" alt="简体中文"></a>
</p>

USC **CSCI 556 密码学导论**（Fall 2026，Prof. Shang-Hua Teng）的课程笔记。

[课程主页](https://viterbi-web.usc.edu/~shanghua/teaching/Fall2026-556/index.html) · [课程大纲（中文整理）](course/syllabus.md) · [K&L 第 1 / 2 版章节对照](course/katz-lindell-edition-map.md)

> [!NOTE]
> 个人学习笔记，不是官方课程材料，难免有错。课程安排以[课程主页](https://viterbi-web.usc.edu/~shanghua/teaching/Fall2026-556/index.html)为准。

## 课程信息

| | |
| :-- | :-- |
| 课程 | CSCI 556 Introduction to Cryptography，Fall 2026 |
| 老师 | Prof. Shang-Hua Teng |
| 时间地点 | 周一、周三 10:00–11:50，VPD 105 |
| 必读教材 | Katz & Lindell，[*Introduction to Modern Cryptography*](https://www.cs.umd.edu/~jkatz/imc.html)，第 2 版 |
| 补充教材 | Boneh & Shoup，[*A Graduate Course in Applied Cryptography*](https://toc.cryptobook.us/)（作者免费公开） |
| 考核 | 课堂参与 15% · Quiz 20%（10/05）· Theoretical Thinking 20% · 小组项目展示 20% · 期末报告 25% |

## 笔记目录

「笔记」是按知识点整理的完整笔记，「随记」是自己课后补充的笔记。

| 讲次 | 日期 | 主题 | 笔记 |
| :-: | :-: | :-- | :-- |
| 1 | 08/24 | 课程概览：安全建模、Perfect vs Computational Security、P vs NP、对称与公钥加密 | [笔记](lectures/lec01-0824/notes.md) |
| 2 | 08/26 | Shamir's Secret Sharing | — |
| 3 | 08/31 | Secret Sharing II：Access Structure、Monotone Formula | [笔记](lectures/lec03-0831/notes.md) |
| 4 | 09/02 | Private-Key Encryption 框架、Perfect Secrecy | [笔记](lectures/lec04-0902/notes.md) |
| 5 | 09/07 | *Labor Day，停课* | |
| 6 | 09/09 | Cryptography vs Machine Learning、Cryptanalysis、攻击类型 | [笔记](lectures/lec06-0909/notes.md) · [随记](lectures/lec06-0909/my-notes.md) |
| 7 | 09/14 | 计算安全、复杂性理论基础、Randomness vs Pseudorandomness | [笔记](lectures/lec07-0914/notes.md) · [随记](lectures/lec07-0914/my-notes.md) |
| 8 | 09/16 | Public-Key Encryption、CPA、数论基础（GCD、欧几里得算法） | [笔记](lectures/lec08-0916/notes.md) · [随记](lectures/lec08-0916/my-notes.md) |
| 9 | 09/21 | RSA、中国剩余定理 | 整理中 |
| 10 | 09/23 | RSA（续）、数论基础 | 整理中 |
| 11 | 09/28 | Rabin Encryption、Probabilistic Encryption | |
| 12 | 09/30 | Diffie–Hellman Key Exchange | |
| 13 | 10/05 | **Quiz** | |
| 14 | 10/07 | Discrete Logarithms、El Gamal | |
| 15 | 10/12 | Pseudorandom Generation：Blum–Micali | |
| 16 | 10/14 | Pseudorandom Number Generation | |
| 17 | 10/19 | Indistinguishability & Unpredictability、Blum–Blum–Shub | |
| 18 | 10/21 | One-Way / Trapdoor Functions、Hardcore Bits | |
| 19 | 10/26 | Interactive & Zero-Knowledge Proofs | |
| 20 | 10/28 | Zero-Knowledge Proofs | |
| 21–22 | 11/02–11/04 | ZKP 应用：Multiparty Computation | |
| 23–30 | 11/09–12/02 | 学生展示（11/11 Veterans Day、11/25 Thanksgiving 停课） | |

## 仓库结构

```
.
├── course/
│   ├── syllabus.md                   课程大纲（中文整理）
│   └── katz-lindell-edition-map.md   K&L 第 1 / 2 版章节对照
└── lectures/                         每讲一个文件夹：lec<讲次>-<日期>
    └── lec08-0916/
        ├── notes.md                  笔记
        └── my-notes.md               随记
```

## 参考资料

- Jonathan Katz, Yehuda Lindell. [*Introduction to Modern Cryptography*](https://www.cs.umd.edu/~jkatz/imc.html), 2nd ed. CRC Press, 2014. —— 必读教材
- Dan Boneh, Victor Shoup. [*A Graduate Course in Applied Cryptography*](https://toc.cryptobook.us/). —— 补充教材
- Adi Shamir. [How to Share a Secret](https://doi.org/10.1145/359168.359176). *Communications of the ACM*, 1979. —— Lec 2
- Josh Benaloh, Jerry Leichter. [Generalized Secret Sharing and Monotone Functions](https://doi.org/10.1007/0-387-34799-2_3). *CRYPTO '88*. —— Lec 3

## 阅读说明

- 笔记用 [Typora](https://typora.io) 写，推荐用 Typora 打开。GitHub 能显示 `$...$` 公式，但不显示 `==高亮==`（标的是重点）。
- 中文行文，专业术语保留英文，如 Access Structure、Perfect Security。
- 板书照片不公开，笔记里引用板书的图片在 GitHub 上不显示。

## License

[Apache License 2.0](LICENSE)
