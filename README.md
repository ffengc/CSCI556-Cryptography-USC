# CSCI 556 · Introduction to Cryptography

<p align="center">
  <a href="README.md"><img src="https://img.shields.io/badge/English-blue?style=for-the-badge" alt="English"></a>
  <a href="README.zh-CN.md"><img src="https://img.shields.io/badge/简体中文-lightgrey?style=for-the-badge" alt="简体中文"></a>
</p>

Course notes for **CSCI 556: Introduction to Cryptography** at USC (Fall 2026, taught by Prof. Shang-Hua Teng).

[Course Website](https://viterbi-web.usc.edu/~shanghua/teaching/Fall2026-556/index.html) · [Syllabus Digest (Chinese)](course/syllabus.md) · [Katz–Lindell 1st/2nd Edition Chapter Map](course/katz-lindell-edition-map.md)

> [!NOTE]
> These are personal study notes, not official course material, and may contain mistakes. The [course website](https://viterbi-web.usc.edu/~shanghua/teaching/Fall2026-556/index.html) is the authoritative source for the schedule.
>
> The notes are written in Chinese, with technical terms kept in English.

## Course Info

| | |
| :-- | :-- |
| Course | CSCI 556 Introduction to Cryptography, Fall 2026 |
| Instructor | Prof. Shang-Hua Teng |
| Time & Place | Mon/Wed 10:00–11:50 AM, VPD 105 |
| Required Text | Katz & Lindell, [*Introduction to Modern Cryptography*](https://www.cs.umd.edu/~jkatz/imc.html), 2nd ed. |
| Supplementary Text | Boneh & Shoup, [*A Graduate Course in Applied Cryptography*](https://toc.cryptobook.us/) (free online) |
| Grading | Participation 15% · Quiz 20% (10/05) · Theoretical Thinking 20% · Team Project Presentation 20% · Term Report 25% |

## Lecture Notes

**Notes** are the full notes, organized by topic. **Personal notes** are my own additions after class.

| Lec | Date | Topic | Notes |
| :-: | :-: | :-- | :-- |
| 1 | 08/24 | Course overview: modeling security, perfect vs. computational security, P vs. NP, symmetric vs. public-key encryption | [Notes](lectures/lec01-0824/notes.md) |
| 2 | 08/26 | Shamir's secret sharing | — |
| 3 | 08/31 | Secret sharing II: access structures, monotone formulas | [Notes](lectures/lec03-0831/notes.md) |
| 4 | 09/02 | The private-key encryption framework, perfect secrecy | [Notes](lectures/lec04-0902/notes.md) |
| 5 | 09/07 | *Labor Day, no class* | |
| 6 | 09/09 | Cryptography vs. machine learning, cryptanalysis, types of attacks | [Notes](lectures/lec06-0909/notes.md) · [Personal](lectures/lec06-0909/my-notes.md) |
| 7 | 09/14 | Computational security, complexity theory basics, randomness vs. pseudorandomness | [Notes](lectures/lec07-0914/notes.md) · [Personal](lectures/lec07-0914/my-notes.md) |
| 8 | 09/16 | Public-key encryption, CPA security, number theory basics (GCD, Euclid's algorithm) | [Notes](lectures/lec08-0916/notes.md) · [Personal](lectures/lec08-0916/my-notes.md) |
| 9 | 09/21 | Group theory, Chinese remainder theorem, Euler's theorem, RSA setup | [Notes](lectures/lec09-0921/notes.md) · [Personal](lectures/lec09-0921/my-notes.md) |
| 10 | 09/23 | RSA (cont.), number theory basics | In progress |
| 11 | 09/28 | Rabin encryption, probabilistic encryption | |
| 12 | 09/30 | Diffie–Hellman key exchange | |
| 13 | 10/05 | **Quiz** | |
| 14 | 10/07 | Discrete logarithms, El Gamal | |
| 15 | 10/12 | Pseudorandom generation: Blum–Micali | |
| 16 | 10/14 | Pseudorandom number generation | |
| 17 | 10/19 | Indistinguishability & unpredictability, Blum–Blum–Shub | |
| 18 | 10/21 | One-way / trapdoor functions, hardcore bits | |
| 19 | 10/26 | Interactive & zero-knowledge proofs | |
| 20 | 10/28 | Zero-knowledge proofs | |
| 21–22 | 11/02–11/04 | Applications of ZKP: multiparty computation | |
| 23–30 | 11/09–12/02 | Student presentations (no class 11/11 Veterans Day, 11/25 Thanksgiving) | |

## Repository Layout

```
.
├── course/
│   ├── syllabus.md                   Syllabus digest (Chinese)
│   └── katz-lindell-edition-map.md   Katz–Lindell 1st/2nd edition chapter map
└── lectures/                         One folder per lecture: lec<number>-<date>
    └── lec08-0916/
        ├── notes.md                  Notes
        └── my-notes.md               Personal notes
```

## References

- Jonathan Katz, Yehuda Lindell. [*Introduction to Modern Cryptography*](https://www.cs.umd.edu/~jkatz/imc.html), 2nd ed. CRC Press, 2014. (Required text)
- Dan Boneh, Victor Shoup. [*A Graduate Course in Applied Cryptography*](https://toc.cryptobook.us/). (Supplementary text)
- Adi Shamir. [How to Share a Secret](https://doi.org/10.1145/359168.359176). *Communications of the ACM*, 1979. (Lec 2)
- Josh Benaloh, Jerry Leichter. [Generalized Secret Sharing and Monotone Functions](https://doi.org/10.1007/0-387-34799-2_3). *CRYPTO '88*. (Lec 3)

## Reading Tips

- The notes are written in [Typora](https://typora.io) and read best there. GitHub renders the `$...$` math but not `==highlights==` (which mark key points).
- Board photos are not published, so image links to them do not render on GitHub.

## License

[Apache License 2.0](LICENSE)
