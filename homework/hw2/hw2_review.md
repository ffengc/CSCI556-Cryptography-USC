# HW2 复习

> 目标：10/05 Quiz，**快速过一遍**。HW2 已经逐题做过，这次只抓每问的**思路一句话**和**最容易丢分的地方**。\
> 对照材料：官方答案 `hw2_sol.pdf`（本地）、自己的作业 [hw2-fengcheng-private.md](hw2-fengcheng-private.md)。

## 复习进度

- [x] **P1** 可忽略函数（Lec 7）
  - [x] (a) $n^{-10}$ 不可忽略，$2^{-\sqrt n}$ 可忽略
  - [x] (b) 多项式次尝试 × 可忽略 = 可忽略（并集界）
  - [x] (c) 要可忽略的是 $\varepsilon(n)$，不是成功概率
- [x] **P2** 单向函数的构造（Lec 9、Lec 10）
  - [x] (a) $f(x) \oplus g(x)$ 不一定单向：取 $f = g$
  - [x] (b) ⭐ $g$ 单向，但 $g \circ g$ 不单向（归约证明）
  - [x] (c) $g(x_1) \Vert f(x_2)$ 一定单向（归约证明）
- [x] **P3** 组合伪随机生成器（Lec 7）
  - [x] (a) ⭐ $G(r_1) \Vert G(r_2)$ 是 PRG（混合论证）
  - [x] (b) $G(r) \Vert G(r)$ 不是 PRG（比较两半）
- [x] **P4** 中国剩余定理和 RSA 正确性（Lec 9、Lec 10）
  - [x] (a) ⭐ CRT 的存在性（积木公式）和唯一性
  - [x] (b) ⭐ RSA 对所有 $m$ 都正确：mod $p$、mod $q$ 分开，再用 CRT 拼回
- [x] **P5** 共享素因子的 RSA 模数（Lec 8、Lec 10）
  - [x] (a) $\gcd(N_1, N_2)$ 分解两个模数，算 $\varphi$
  - [x] (b) 扩展欧几里得算 $d$，为什么是多项式时间
- [x] **P6** textbook RSA 的 CPA / CCA 攻击（Lec 8、Lec 10）
  - [x] (a) 确定性加密不是 CPA 安全
  - [x] (b) 用 $c \cdot 2^e$ 问一次 oracle，算出 $m$
  - [x] (c) 询问合法、输出正确
  - [x] (d) 乘法同态；不用分解；只拒绝 $c$ 挡不住

---

## P1 可忽略函数

### Reading

> **Reading:** Katz–Lindell §§3.1.2 and 3.2.1; Exercise 3.1.

### (a) Classification

Determine which of the following functions are negligible. Prove each conclusion.

$\mu_1(n) = n^{-10}, \qquad \mu_2(n) = 2^{-\sqrt{n}}.$

#### Review

> [!note] 
> $\mu(n)$ is negligible if for every positive integer $c$ there is $n_c$ such that $\mu(n) < n^{-c}$ for all $n \ge n_c$.（比任何 1/多项式都小）

1. $n^{-10}$ is **not negligible**. Choose $c = 11$. For every $n > 1$, $n^{-10} > n^{-11}$, so the required eventual bound $\mu_1(n) < n^{-11}$ never holds.
2. $2^{-\sqrt{n}}$ is **negligible**. Fix any positive integer $c$. Since $\sqrt{n} / \log_2 n \to \infty$, eventually $\sqrt{n} > c \log_2 n$. Hence $2^{-\sqrt{n}} < 2^{-c \log_2 n} = n^{-c}$.


### (b) Polynomially many attempts

An attacker makes at most $q(n)$ attempts, where $q$ is polynomially bounded in the security parameter $n$: that is, there exist constants $k \ge 0$ and $n_0$ such that $q(n) \le n^k$ for all $n \ge n_0$. Let $E_i$ be the event that attempt $i$ succeeds, and suppose $\Pr[E_i] \le \mu(n)$ for each attempt, where $\mu$ is negligible. Prove that the probability that at least one attempt succeeds is negligible. Do you need the events $E_i$ to be independent?

#### Review

1. **Union bound.** $\Pr\big[\bigcup_{i=1}^{q(n)} E_i\big] \le \sum_{i=1}^{q(n)} \Pr[E_i] \le q(n)\,\mu(n)$.
2. **$q\mu$ is negligible.** Fix any positive integer $c$ and let $d = c + k + 1$. Since $\mu$ is negligible, there is $n_d$ with $\mu(n) < n^{-d}$ for all $n \ge n_d$. For all $n \ge \max\{n_0, n_d\}$:
   $q(n)\,\mu(n) < n^k \cdot n^{-(c+k+1)} = n^{-c-1} < n^{-c}.$
   Since $c$ was arbitrary, the success probability is negligible.
3. **Independence is not needed:** the union bound holds for arbitrary events.

> [!caution] 丢分点
>
> 1. 要写 $n \ge \max\{n_0, n_d\}$：$q(n) \le n^k$ 只在 $n \ge n_0$ 时成立，两个门槛都要过。
> 2. 不等号要严格：$\mu(n) < n^{-d}$，最后 $< n^{-c}$，和定义一致。


### (c) Guessing advantage

In an encryption indistinguishability experiment, a PPT attacker guesses a uniformly random challenge bit $b$. Its success probability is $1/2 + \varepsilon(n)$, where $\varepsilon(n) \ge 0$. Which quantity must be negligible for computational security: the success probability or $\varepsilon(n)$? Explain why.

#### Review

1. **$\varepsilon(n)$ must be negligible** for every PPT attacker: it is the excess success probability over random guessing.
2. An attacker can always succeed with probability $1/2$ by guessing, so the success probability itself can never be negligible.
3. Under the convention that defines advantage as $2\varepsilon(n)$, the requirement is equivalent, because multiplying by a constant preserves negligibility.

> [!caution] 丢分点
> 1. 一定要写**理由**：瞎猜就能拿到 $1/2$，所以成功概率本身不可能可忽略。只答"$\varepsilon(n)$"不给分。
> 2. 第 3 条容易漏：优势定义成 $2\varepsilon$ 也等价，因为常数倍不影响可忽略。

---

## P2 单向函数的构造

### Reading

> **Reading:** Katz–Lindell §7.1.1; Exercises 7.2 and 7.8.

Let $f, g : \{0, 1\}^* \to \{0, 1\}^*$ be length-preserving one-way functions; $f$ and $g$ need not be distinct. These functions must be polynomial-time computable. The inversion requirement is that, for every PPT algorithm $A$,

$\Pr_{x \leftarrow U_n}\big[f(A(1^n, f(x))) = f(x)\big]$

is negligible, where the probability includes the randomness of $A$ and success requires an $n$-bit output from $A$; the analogous definition applies to $g$. An inverter need only find some preimage, not necessarily the original input.

In parts (a) and (c), determine whether the construction is necessarily one-way for arbitrary $f$ and $g$. Give a proof or a counterexample. Part (b) supplies a construction for you to analyze. Counterexamples may use an assumed length-preserving one-way function; no unconditional construction of one-way functions is requested.

### (a)

$h(x) = f(x) \oplus g(x).$

#### Review

**Not necessarily one-way.** Choose $f = g = F$ for any length-preserving one-way function $F$. Then

$h(x) = F(x) \oplus F(x) = 0^n$ for every $x$.

On the only possible challenge $0^n$, an inverter outputs $0^n$; since $h(0^n) = 0^n$, it succeeds with probability 1. (It need not recover the original input: every $x \in \{0,1\}^n$ is a preimage of $0^n$.) $h$ is still poly-time and length-preserving; only one-wayness fails.

> [!caution] 丢分点
> 1. 要说明反例是**允许的**：题目写了 $f$ 和 $g$ need not be distinct，所以可以取 $f = g$。
> 2. 要写出倒推者具体输出什么（$0^n$），并说明成功概率是 1。


### (b) Guided counterexample for self-composition

Let $F$ be a length-preserving one-way function. For input length $L \ge 4$, set $k = \lfloor L/2 \rfloor$ and $t = L - k - 1$. Parse the input as $u \Vert b \Vert x$, where $|u| = k$, $|b| = 1$, and $|x| = t$ (the bit $b$ is ignored by $g$; it only fixes the output length). Define

$g(u \Vert b \Vert x) = \begin{cases} 0^L, & u = 0^k, \\ 0^k \Vert 1 \Vert F(x), & u \ne 0^k. \end{cases}$

At smaller input lengths, define $g$ to be the identity.

1. Compute the probability that a uniform $L$-bit input takes the first branch. Explain why every valid preimage of $0^k \Vert 1 \Vert F(x)$ must take the second branch.
2. Prove that $g$ is one-way by reducing inversion on the second branch to inversion of $F$. You may use that $L$ and $t$ are within constant factors and that the two possible lengths for a fixed $t$ are $L = 2t + 1$ and $L = 2t + 2$.
   *Hint:* Bound the total inversion probability by the first-branch probability plus the conditional success probability on the second branch.
3. Compute $g(g(z))$ for arbitrary $z \in \{0, 1\}^L$. Conclude whether self-composition necessarily preserves one-wayness, and explain why this does not contradict the one-wayness of $g$ (which concerns uniform inputs, not inputs drawn from $g(U_L)$).

#### Review

**1.** A uniform input takes the first branch exactly when $u = 0^k$, which happens with probability $2^{-k}$. The first branch always outputs $0^L$, whose $(k+1)$-th bit is $0$, while $0^k \Vert 1 \Vert F(x)$ has a $1$ in position $k+1$. So every preimage of $0^k \Vert 1 \Vert F(x)$ must take the second branch.

**2.**

1. **Goal.** Let $A$ be any PPT inverter for $g$, with success probability $\alpha(L)$. We show $\alpha(L)$ is negligible. ($g$ is clearly poly-time computable.)

2. **Split by branch.** Let $\beta(L)$ be $A$'s success probability conditioned on the second branch. Then $\alpha(L) \le 2^{-k} + \beta(L)$.

3. **First term.** $2^{-k}$ is negligible, so it suffices to show $\beta(L)$ is negligible.

4. ‼️**Assume for contradiction** that $\beta(L)$ is non-negligible.

5. **Second-branch output.** It is $0^k \Vert 1 \Vert F(x)$ with $x \leftarrow U_t$ ($x$ stays uniform, since the branch depends only on $u$).

6. **Reduction $B$ for $F$.** Given $y = F(x)$ with $x \leftarrow U_t$: set $L = 2t+1$, $k = t$; run $A$ on $0^k \Vert 1 \Vert y$ with $1^L$; output the last $t$ bits of $A$'s answer. (For even $L$, use $L = 2t+2$, $k = t+1$.)

7. **$B$ succeeds whenever $A$ does.** $A$'s input has exactly the distribution of $g(U_L)$ on the second branch, so $A$ succeeds with probability $\beta(L)$. Its answer $u' \Vert b' \Vert x'$ must have $u' \ne 0^k$, because the first branch outputs only $0^L$ while the target has a $1$ in position $k+1$. Hence $F(x') = y$. So $B$ inverts $F$ with probability $\beta(L)$.

> [!tip] 注意
> 这一步也不用记这么复杂，这里就是要记住 same distribution 就行了。\
> 主要是这句话：B succeeds whenever A does. -> same distribution

8. **Contradiction.** If $\beta(L)$ were non-negligible for infinitely many $L$, it would be so for infinitely many $L$ of one parity, and the corresponding $B$ would invert $F$ with non-negligible probability. This contradicts the one-wayness of $F$. ($L$ and $t$ differ by a constant factor.)

9.  **Conclusion.** So $\beta(L)$ is negligible, hence $\alpha(L) \le 2^{-k} + \beta(L)$ is negligible, and $g$ is one-way.

**3.** Every output of $g$ begins with $0^k$ (both $0^L$ and $0^k \Vert 1 \Vert F(x)$ do), so the second application of $g$ always takes the first branch: $g(g(z)) = 0^L$ for every $z \in \{0,1\}^L$. Thus $g \circ g$ is constant and trivially invertible (any $L$-bit string is a preimage of $0^L$), so self-composition does not necessarily preserve one-wayness.

This does not contradict the one-wayness of $g$, which only concerns uniformly random inputs. The input to the second $g$ is drawn from $g(U_L)$, which always starts with $0^k$, i.e., exactly the first-branch inputs that a uniform input hits with probability only $2^{-k}$.

> [!caution] 丢分点
> 1. 最容易漏：要解释**为什么不矛盾**。单向只管随机输入，而第二次套 $g$ 的输入 $g(U_L)$ 不是随机的，开头永远是 $0^k$。
> 2. 原像不只是 $0^L$：$g(g(z)) = 0^L$ 对所有 $z$ 成立，所以任何 $L$ 位的串都是原像。

### (c)

For independent $x_1, x_2 \leftarrow U_n$,

$h(x_1 \Vert x_2) = g(x_1) \Vert f(x_2).$

Consider the family with input length $2n$; the split into two $n$-bit blocks is known to the inverter.

#### Review

> [!note] 思路链条（和 (b)-2 同一个套路）
> ```
> 造 B：拿到 y = g(x₁)，自己随机选 x₂，把 y ‖ f(x₂) 交给 A，输出 A 答案的前半段 x₁′
>   ↓
> 分布一样：y ‖ f(x₂) 和 h(U₂ₙ) 分布相同，所以 A 以 ε(n) 的概率成功
>   ↓
> A 成功 ⟹ g(x₁′) ‖ f(x₂′) = g(x₁) ‖ f(x₂)，比较前 n 位得 g(x₁′) = g(x₁)，B 也成功
>   ↓
> g 是单向的 ⟹ ε(n) 可忽略 ⟹ h 是单向的
> ```

**Claim.** $h$ is necessarily one-way.

1. **Goal.** $h$ is clearly poly-time computable. Let $A$ be any PPT inverter for $h$ with success probability $\varepsilon(n)$ on a uniform $2n$-bit input. We show $\varepsilon(n)$ is negligible.
2. **Reduction $B$ for $g$.** Given $y = g(x_1)$ for uniform unknown $x_1 \in \{0,1\}^n$: sample $x_2 \leftarrow U_n$ independently and compute $f(x_2)$; run $A$ (input length $2n$) on $y \Vert f(x_2)$; if $A$ returns two $n$-bit blocks $x_1' \Vert x_2'$, output $x_1'$, otherwise fail.
3. **Same distribution.** Since $x_1, x_2$ are independent and uniform, $A$'s input has exactly the distribution $h(U_{2n})$, so $A$ succeeds with probability $\varepsilon(n)$.
4. **$B$ succeeds whenever $A$ does.** If $A$ succeeds, then $g(x_1') \Vert f(x_2') = g(x_1) \Vert f(x_2)$. Comparing the first $n$ bits gives $g(x_1') = g(x_1)$. So $B$ inverts $g$ with probability at least $\varepsilon(n)$.
5. **Conclusion.** $B$ is PPT. Since $g$ is one-way, $\varepsilon(n)$ is negligible (also in $h$'s input length $2n$, which differs from $n$ by a constant factor). Hence $h$ is one-way.

*Remark.* The reduction only uses that $g$ is one-way and $f$ is efficiently computable.

> [!caution] 丢分点
> 1. **分布一样**说的是"B 交给 A 的 $y \Vert f(x_2)$ 和 $h(U_{2n})$ 分布相同"，不是 $h$ 和 $f$ 分布相同。
> 2. B 倒推的是 **$g$**（前半段），后半段 $f(x_2)$ 是 B **自己随机选、自己算**的，要写出来。
> 3. 第 4 步要写出"比较前 $n$ 位"，说明 A 的答案为什么就是 $g$ 的原像。

---

## P3 组合伪随机生成器

### Reading

> **Reading:** Katz–Lindell §§3.3.1–3.3.2.

Let $G : \{0, 1\}^n \to \{0, 1\}^{n+1}$ be a pseudorandom generator (PRG). Thus $G$ is deterministic and polynomial-time computable, and for every PPT distinguisher $D$,

$\big|\Pr[D(1^n, G(U_n)) = 1] - \Pr[D(1^n, U_{n+1}) = 1]\big|$

is negligible. Measure negligibility as a function of $n$; negligibility in $n$ and in $2n$ are equivalent. A distinguisher for $G$ receives $1^n$; a distinguisher for $G'$ in part (a) receives $1^{2n}$. Distinct occurrences of $U_n$ below represent independent samples unless a seed is explicitly reused.

### (a)

Define

$G'(r_1 \Vert r_2) = G(r_1) \Vert G(r_2), \qquad r_1, r_2 \in \{0, 1\}^n.$

Prove that $G'$ is a PRG with seed length $2n$ and output length $2n + 2$. Give the intermediate distributions and the reductions explicitly, using a hybrid argument.

#### Review

1. **Basics.** $G'$ is deterministic, poly-time, and maps $2n$ bits to $2n+2$ bits. It remains to show pseudorandomness.
2. **Hybrids.** Let $R_1, R_2 \leftarrow U_n$ and $S_1, S_2 \leftarrow U_{n+1}$ be independent:
   - $H_0 = G(R_1) \Vert G(R_2)$ (output of $G'$)
   - $H_1 = S_1 \Vert G(R_2)$
   - $H_2 = S_1 \Vert S_2$ (uniform on $2n+2$ bits)

   Fix any PPT distinguisher $D$ for $G'$ and let $p_i = \Pr[D(1^{2n}, H_i) = 1]$.
3. **$|p_0 - p_1|$ is negligible.** Given a challenge $z \in \{0,1\}^{n+1}$, $B_1$ samples $r \leftarrow U_n$ and outputs $D(1^{2n}, z \Vert G(r))$. If $z = G(U_n)$, $D$ sees $H_0$; if $z = U_{n+1}$, $D$ sees $H_1$. So $B_1$'s gap for $G$ is $|p_0 - p_1|$, negligible since $G$ is a PRG.
4. **$|p_1 - p_2|$ is negligible.** Given $z$, $B_2$ samples $w \leftarrow U_{n+1}$ and outputs $D(1^{2n}, w \Vert z)$. Its two cases give $H_1$ and $H_2$, so $|p_1 - p_2|$ is negligible. $B_1, B_2$ are PPT because $D$ is.
5. **Conclusion.** By the triangle inequality, $|p_0 - p_2| \le |p_0 - p_1| + |p_1 - p_2|$, a sum of two negligible functions, hence negligible. So $G'$ is a PRG.

> [!caution] 丢分点
> 1. **最容易漏**：第 2 步开头要先声明随机变量，$R_1, R_2 \leftarrow U_n$、$S_1, S_2 \leftarrow U_{n+1}$，**并且相互独立**。不写的话 $H_0, H_1, H_2$ 没有定义。
> 2. 三个中间分布 $H_0, H_1, H_2$ 要**全部写出来**，题目明确要求 "intermediate distributions"。
> 3. 归约 $B_1, B_2$ 要**具体写出怎么构造**（题目要求 "reductions explicitly"），只说"同理"会扣分。
> 4. "Fix any PPT distinguisher $D$" 是单独一句话，不是第四个混合分布。
> 5. 结尾要用**三角不等式**，并说明"两个可忽略相加还是可忽略"。


### (b)

Instead define

$\tilde{G}(r) = G(r) \Vert G(r).$

Is $\tilde{G}$ a PRG? Give an efficient distinguisher and compute the absolute difference between its acceptance probabilities on $\tilde{G}(U_n)$ and $U_{2n+2}$.

#### Review

**Not a PRG.**

1. **Distinguisher.** $D$ splits its $(2n+2)$-bit input into two $(n+1)$-bit halves and outputs 1 iff they are equal. This runs in linear time.
2. On $\tilde{G}(U_n)$ the halves are always equal: $\Pr[D = 1] = 1$.
3. On $U_{2n+2}$ the halves are independent and uniform: $\Pr[D = 1] = 2^{-(n+1)}$.
4. The gap is $1 - 2^{-(n+1)}$, which is not negligible. So $\tilde{G}$ is not a PRG, even though each half alone is pseudorandom.

> [!caution] 丢分点
> 1. 两个概率都要**算出来**，再写出差值 $1 - 2^{-(n+1)}$，并说明它**不可忽略**。
> 2. 要说明 $D$ 是**高效的**（linear time），题目要的是 "efficient distinguisher"。

---

## P4 中国剩余定理和 RSA 正确性

### Reading

> **Reading:** Katz–Lindell §8.1.5; Exercise 8.10.

### (a)

Let $p, q \ge 2$ be coprime integers. Prove that for every pair of integers $a, b$, there is exactly one residue class modulo $pq$ satisfying

$x \equiv a \pmod p, \qquad x \equiv b \pmod q.$

Give an explicit formula for a solution using integers $u, v$ satisfying $up + vq = 1$, and prove uniqueness modulo $pq$.

#### Review

1. **Existence.** Since $\gcd(p, q) = 1$, Bézout gives integers $u, v$ with $up + vq = 1$. Let $x = avq + bup$. Modulo $p$: $up \equiv 0$, $vq \equiv 1$, so $x \equiv a$. Modulo $q$: $vq \equiv 0$, $up \equiv 1$, so $x \equiv b$.
2. **Uniqueness.** If $x, y$ both solve the system, then $p \mid (x - y)$ and $q \mid (x - y)$. Write $x - y = pj$. Then $j = j(up + vq) = u(x - y) + vqj$, and both terms are divisible by $q$, so $q \mid j$. Hence $pq \mid (x - y)$, i.e., $x \equiv y \pmod{pq}$.

> [!caution] 丢分点
> 1. 第一句要写**为什么有 $u, v$**：因为 $\gcd(p, q) = 1$，由 Bézout。
> 2. 唯一性不能只说"$p \mid (x-y)$、$q \mid (x-y)$ 所以 $pq \mid (x-y)$"，要写出 $j = u(x-y) + vqj$ 这一步证明 $q \mid j$（这里用到了互素）。


### (b)

Now let $p$ and $q$ be distinct primes, let $N = pq$, and let $e, d$ be positive integers satisfying

$ed \equiv 1 \pmod{(p - 1)(q - 1)}.$

Note that $(p - 1)(q - 1) = \varphi(N)$. Using part (a), prove that textbook RSA decrypts correctly for every $m \in \mathbb{Z}_N$:

$(m^e)^d \equiv m \pmod N.$

Your proof must also cover messages for which $\gcd(m, N) \ne 1$.

#### Review

1. Write $ed = 1 + \ell(p-1)(q-1)$ with $\ell \ge 0$.
2. **Mod $p$.** If $p \mid m$, both sides are $0$. If $p \nmid m$, Fermat gives $m^{p-1} \equiv 1$, so $m^{ed} = m\,(m^{p-1})^{\ell(q-1)} \equiv m \pmod p$.
3. **Mod $q$.** Same argument: $m^{ed} \equiv m \pmod q$.
4. **Combine.** $m^{ed}$ and $m$ both solve $x \equiv m \pmod p$, $x \equiv m \pmod q$, so by uniqueness in (a), $m^{ed} \equiv m \pmod N$.

> [!caution] 丢分点
> 1. 必须分 **$p \mid m$** 和 **$p \nmid m$** 两种情况：费马小定理只在 $p \nmid m$ 时能用。这正是题目要求覆盖 $\gcd(m, N) \ne 1$ 的地方。
> 2. 最后一步要明确写"**by uniqueness in (a)**"，题目要求 "Using part (a)"。

---

## P5 共享素因子的 RSA 模数

### Reading

> **Reading:** Katz–Lindell §11.5.6; Algorithm 11.25.

Alice and Bob use RSA public keys $(e_1, N_1)$ and $(e_2, N_2)$. Each modulus is a product of two distinct primes, and each exponent satisfies $\gcd(e_i, \varphi(N_i)) = 1$. Suppose

$s = \gcd(N_1, N_2), \qquad 1 < s < \min(N_1, N_2).$

Thus $s$ is a proper divisor of both moduli.

### (a)

Show how to recover the prime factors of both moduli and compute $\varphi(N_1)$ and $\varphi(N_2)$.

#### Review

1. Write $N_i = p_i q_i$ (distinct primes). The divisors of $N_1$ are $1, p_1, q_1, N_1$; since $s \mid N_1$ and $1 < s < N_1$, $s \in \{p_1, q_1\}$. Similarly $s \in \{p_2, q_2\}$. So $s$ is a prime factor of both moduli.
2. Let $t_i = N_i / s$; these are the other primes, so $N_i = s\, t_i$.
3. Hence $\varphi(N_i) = (s - 1)(t_i - 1)$ for $i = 1, 2$.

> [!caution] 丢分点
> 要写出**为什么 $s$ 是素数**：$N_i$ 只有 $1, p_i, q_i, N_i$ 四个因子，而 $1 < s < N_i$。不能直接说"$s$ 是共同的素因子"。


### (b)

Show how to recover valid private exponents $d_1, d_2$. Explain why this breaks both RSA keys and why the computation is polynomial-time in the bit lengths of the public keys.

#### Review

1. **Private exponents.** Compute $d_i = e_i^{-1} \bmod \varphi(N_i)$ with the extended Euclidean algorithm; the inverse exists since $\gcd(e_i, \varphi(N_i)) = 1$.
2. **Breaks both keys.** The attacker can decrypt any ciphertext as $c^{d_i} \bmod N_i$ (correct by Problem 4). $d_i$ need not equal the owner's stored integer; any valid inverse works.
3. **Polynomial time.** Euclid's gcd, division, multiplication, extended Euclid, and modular exponentiation are all polynomial in the bit lengths. No general factoring is needed: the shared factor is revealed by a single gcd.

> [!caution] 丢分点
> 1. 要写**逆元为什么存在**：$\gcd(e_i, \varphi(N_i)) = 1$。
> 2. 题目问了三件事（怎么算 $d$、为什么攻破、为什么多项式时间），**三件都要答**。
> 3. 多项式时间要**列出用到的每个算法**，并点出"不需要解一般的分解问题"。

---

## P6 textbook RSA 的 CPA / CCA 攻击

### Reading

> **Reading:** Katz–Lindell Definition 11.5, §§11.2.1 and 11.2.3; Construction 11.26.

### (a) Deterministic public-key encryption

Consider a deterministic public-key encryption scheme with perfect correctness. Assume its message space contains two efficiently selectable, distinct messages $m_0, m_1$ of equal length for every generated public key. In the IND-CPA experiment (Definition 11.5), an attacker knows the public key, chooses $m_0, m_1$, and receives an encryption of $m_b$ for a uniform bit $b$.

Construct an efficient attacker that recovers $b$ with probability 1. Prove, using perfect correctness, that $\mathrm{Enc}_{pk}(m_0) \ne \mathrm{Enc}_{pk}(m_1)$. State its excess success probability over random guessing, and explain why the result applies to textbook RSA with fixed-length message encodings. Does this attack require a decryption oracle?

#### Review

> [!note] 题目要回答 5 件事
> 1. 造一个攻击者，100% 猜对 $b$
> 2. 用完全正确性证明 $\mathrm{Enc}_{pk}(m_0) \ne \mathrm{Enc}_{pk}(m_1)$
> 3. 比瞎猜多出来的成功概率是多少
> 4. 为什么适用于 textbook RSA
> 5. 需不需要解密 oracle

1. **Attacker.** Given $pk$, choose distinct equal-length $m_0, m_1$ and compute $c_0 = \mathrm{Enc}_{pk}(m_0)$, $c_1 = \mathrm{Enc}_{pk}(m_1)$. Submit $(m_0, m_1)$, receive $c$, and output $0$ if $c = c_0$, else $1$.
2. **$c_0 \ne c_1$.** If $c_0 = c_1$, perfect correctness would require decrypting this one ciphertext to both $m_0$ and $m_1$, impossible since $m_0 \ne m_1$.
3. **Success.** Encryption is deterministic, so $c = c_b$ exactly and the attacker always recovers $b$. Its excess over guessing is $1 - \tfrac12 = \tfrac12$, not negligible (advantage 1 under the doubled convention).
4. **Textbook RSA** is deterministic and perfectly correct (Problem 4(b)); e.g. $m_0 = 1$, $m_1 = 2$ with the same fixed-length encoding. So it is not IND-CPA secure.
5. **No decryption oracle** is needed: the attacker uses only the public key and the public encryption algorithm.

> [!caution] 丢分点
> 1. 5 件事**一件都不能漏**，尤其是最后两件（为什么适用于 RSA、需不需要 oracle）容易忘。
> 2. $c_0 \ne c_1$ 必须**用完全正确性**来证，这是题目明确要求的。
> 3. 要写出**具体数值** $\tfrac12$，并说明它不可忽略。


### Setting for parts (b)–(d)

Let $N = pq$ be a product of distinct odd primes. Let $(e, N)$ be a valid RSA public key and let $d$ satisfy $ed \equiv 1 \pmod{\varphi(N)}$. For this problem, take the message and ciphertext spaces to be

$\mathbb{Z}^*_N = \{x \in \mathbb{Z}_N : \gcd(x, N) = 1\}.$

An attacker is given a target ciphertext $c = m^e \bmod N$, with unknown $m \in \mathbb{Z}^*_N$. The attacker may query a decryption oracle that returns $z^d \bmod N$ for any $z \in \mathbb{Z}^*_N$ other than $c$. Queries are compared as residue classes modulo $N$. In part (b), the attack must use one allowed query; parts (c) and (d) analyze that query.

### (b)

Construct an algorithm that recovers $m$ using a single allowed oracle query and polynomial-time computation. You may use that 2 is invertible modulo the odd integer $N$.

#### Review

**Idea.** Textbook RSA is multiplicative: multiplying the ciphertext by $2^e$ multiplies the plaintext by 2.

Since $N$ is odd, compute $a = 2^{-1} \bmod N$ with the extended Euclidean algorithm. Then:

1. Compute $c' = c \cdot 2^e \bmod N$.
2. Query the oracle on $c'$ and receive $m' = (c')^d \bmod N$.
3. Output $a\,m' \bmod N$.

One oracle query; every step is polynomial-time. (Part (c) shows the query is allowed and the output is $m$.)

> [!caution] 丢分点
> 1. 改的是 $c \cdot 2^e$，**不是 $2c$**：$(2c)^d = 2^d \cdot m$，而 $2^d$ 里有私钥，除不掉。
> 2. 要说明 $2^{-1} \bmod N$ **存在**（$N$ 是奇数）以及**怎么算**（扩展欧几里得）。


### (c)

Prove that the query you construct differs from $c$ modulo $N$, and prove that the algorithm returns $m$.

#### Review

1. **The query is allowed.** $c$ and $2$ are units, so $c' = c\, 2^e$ is a unit. Suppose $c' \equiv c \pmod N$. Multiplying by $c^{-1}$ gives $2^e \equiv 1$. Raising to the $d$-th power and using RSA correctness for the message 2: $2 \equiv 2^{ed} \equiv 1 \pmod N$, impossible for $N > 1$. So $c' \ne c$.
2. **The output is $m$.** $m' \equiv (c\, 2^e)^d \equiv c^d\, 2^{ed} \equiv 2m \pmod N$, so $a\,m' \equiv 2^{-1} \cdot 2m \equiv m \pmod N$.

> [!caution] 丢分点
> 1. **两件事都要证**：询问合法（$c' \ne c$）和输出正确，只证一件不给全分。
> 2. 证 $c' \ne c$ 时，"两边乘 $c^{-1}$" 要先说明 $c$ **是 unit**（在 $\mathbb{Z}^*_N$ 里，有逆元）。
> 3. $2^{ed} \equiv 2$ 和 $c^d \equiv m$ 用的都是 **RSA 正确性**，要写出来。


### (d)

Explain which algebraic property makes this attack possible. Does the attack require factoring $N$ or recovering $d$? Explain why refusing to decrypt just the target ciphertext does not prevent it.

#### Review

1. **Property.** Textbook RSA is multiplicative: $\mathrm{Enc}(m_1)\,\mathrm{Enc}(m_2) \equiv \mathrm{Enc}(m_1 m_2) \pmod N$. This turns the target into an encryption of $2m$ without knowing $m$.
2. **No factoring, no $d$.** The attacker uses only $(e, N)$ and one oracle answer.
3. **Refusing only $c$ fails.** The oracle blocks just the exact target ciphertext; a different ciphertext whose plaintext is algebraically related to $m$ (here $2m$) is still decrypted, and $m$ is recovered by reversing that relation.

> [!caution] 丢分点
> 题目问了**三件事**，三句都要答：哪个性质（乘法同态）、要不要分解 $N$ 或算 $d$（都不要）、为什么只拒绝 $c$ 挡不住。

