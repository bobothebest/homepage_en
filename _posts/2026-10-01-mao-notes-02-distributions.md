---
layout: post
title: "Probability & Statistics Review 02 | Common Distributions: Moments, Memorylessness, Additivity and How They Relate"
author: Ziyuan Zhang
tags:
- Probability & Statistics Review
- probability
- distributions
- study notes
date: 2026-10-01 10:02 -0400
toc: true
math: true
---
> **Overview**
>
> The common distributions are the foundation of all of probability: characteristic functions, sampling distributions and parameter estimation later on all lean on their means, variances and relationships.
>
> The goal for this part is "**derive it + remember it**". I suggest starting with the relationship map in Section 5: once you remember identities like $$Ga(1,\lambda)=Exp(\lambda)$$, $$Ga(\frac n2,\frac12)=\chi^2(n)$$ and $$Be(1,1)=U(0,1)$$, many distributions can be derived from one another instead of memorized. For each distribution you should be able to write down the pmf or density, the mean and the variance; ☆ marks the key ones.

---

## 1. The 17 common distributions at a glance

Notation: $$q=1-p$$.

| # | Distribution | pmf / density | $$E(X)$$ | $$\mathrm{Var}(X)$$ |
|---|---|---|---|---|
| 1 | Binomial $$b(n,p)$$ | $$\binom nk p^kq^{n-k},\ k=0,\dots,n$$ | $$np$$ | $$npq$$ |
| 2 | Two-point / 0-1 / Bernoulli $$b(1,p)$$ | $$p^xq^{1-x},\ x=0,1$$ | $$p$$ | $$pq$$ |
| 3 | Poisson $$P(\lambda)$$ | $$\frac{\lambda^k}{k!}e^{-\lambda},\ k=0,1,\dots$$ | $$\lambda$$ | $$\lambda$$ |
| 4 | Hypergeometric $$h(n,N,M)$$ | $$\dfrac{\binom Mk\binom{N-M}{n-k}}{\binom Nn}$$ | $$n\frac MN$$ | $$\dfrac{nM(N-M)(N-n)}{N^2(N-1)}$$ |
| 5 | Geometric $$Ge(p)$$ | $$q^{k-1}p,\ k=1,2,\dots$$ | $$\frac1p$$ | $$\frac{q}{p^2}$$ |
| 6 | Negative binomial / Pascal $$Nb(r,p)$$ | $$\binom{k-1}{r-1}p^rq^{k-r},\ k=r,r+1,\dots$$ | $$\frac rp$$ | $$\frac{rq}{p^2}$$ |
| 7 | Normal $$N(\mu,\sigma^2)$$ | $$\frac1{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$ | $$\mu$$ | $$\sigma^2$$ |
| 8 | Uniform $$U(a,b)$$ | $$\frac1{b-a},\ a<x<b$$ | $$\frac{a+b}2$$ | $$\frac{(b-a)^2}{12}$$ |
| 9 | Exponential $$Exp(\lambda)$$ | $$\lambda e^{-\lambda x},\ x>0$$ | $$\frac1\lambda$$ | $$\frac1{\lambda^2}$$ |
| 10 | ☆ Gamma $$Ga(\alpha,\lambda)$$ | $$\frac{\lambda^\alpha}{\Gamma(\alpha)}x^{\alpha-1}e^{-\lambda x},\ x>0$$ | $$\frac\alpha\lambda$$ | $$\frac\alpha{\lambda^2}$$ |
| 11 | ☆ Beta $$Be(a,b)$$ | $$\frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}x^{a-1}(1-x)^{b-1},\ 0<x<1$$ | $$\frac a{a+b}$$ | $$\frac{ab}{(a+b)^2(a+b+1)}$$ |
| 12 | Lognormal $$LN(\mu,\sigma^2)$$ | $$\frac1{x\sigma\sqrt{2\pi}}e^{-\frac{(\ln x-\mu)^2}{2\sigma^2}},\ x>0$$ | $$e^{\mu+\sigma^2/2}$$ | $$(e^{\sigma^2}-1)e^{2\mu+\sigma^2}$$ |
| 13 | Cauchy $$Cau(\mu,\lambda)$$ | $$\frac1\pi\cdot\frac{\lambda}{\lambda^2+(x-\mu)^2}$$ | does not exist | does not exist |
| 14 | Chi-square $$\chi^2(n)$$ (P283) | i.e. $$Ga(\frac n2,\frac12)$$ | $$n$$ | $$2n$$ |
| 15 | $$F(m,n)$$ (P286) | see Part 5 | $$\frac n{n-2}\ (n>2)$$ | $$\frac{2n^2(m+n-2)}{m(n-2)^2(n-4)}\ (n>4)$$ |
| 16 | $$t(n)$$ (P288) | see Part 5 | $$0\ (n>1)$$ | $$\frac n{n-2}\ (n>2)$$ |
| 17 | Weibull | $$F(x)=1-e^{-(x/\eta)^m},\ x>0$$ | $$\eta\Gamma(1+\frac1m)$$ | $$\eta^2\left[\Gamma(1+\frac2m)-\Gamma^2(1+\frac1m)\right]$$ |

Additional results:
- **Lognormal** (Exercises 2.7.3, 2.7.11): the median is $$e^\mu$$ and the $$p$$-quantile is $$x_p=\exp\{\mu+\sigma u_p\}$$ (because $$\ln X\sim N(\mu,\sigma^2)$$, and taking logs is monotone, so quantiles transform along with it).
- **Gamma** (Exercise 2.7.4): $$E(X^k)=\dfrac{\Gamma(\alpha+k)}{\Gamma(\alpha)\lambda^k}$$.
- **Exponential** (Exercise 2.7.5): $$E(X^k)=\dfrac{k!}{\lambda^k}$$, coefficient of variation $$C_v=1$$, skewness $$\beta_s=2$$, kurtosis $$\beta_k=6$$.

<details markdown="1"><summary>Worked derivation: mean and variance of the lognormal</summary>

Let $$Y=\ln X\sim N(\mu,\sigma^2)$$. Then $$E(X^k)=E(e^{kY})$$ is just the normal moment generating function evaluated at $$k$$:

$$E(e^{kY})=e^{k\mu+\frac12k^2\sigma^2}.$$

Taking $$k=1$$ gives $$E(X)=e^{\mu+\sigma^2/2}$$; taking $$k=2$$ gives $$E(X^2)=e^{2\mu+2\sigma^2}$$, so

$$\mathrm{Var}(X)=e^{2\mu+2\sigma^2}-e^{2\mu+\sigma^2}=(e^{\sigma^2}-1)e^{2\mu+\sigma^2}.$$

</details>

---

## 2. The gamma and beta functions

**Gamma function (P115):** $$\Gamma(\alpha)=\int_0^{\infty}x^{\alpha-1}e^{-x}\,dx$$, $$\alpha>0$$.
- $$\Gamma(1)=1$$, $$\Gamma(\tfrac12)=\sqrt\pi$$
- $$\Gamma(\alpha+1)=\alpha\Gamma(\alpha)$$ (integration by parts), hence $$\Gamma(n+1)=n!$$

**Meaning of the gamma parameters:** $$\alpha$$ is the shape parameter and $$\lambda$$ the scale parameter (strictly speaking, $$1/\lambda$$ is the scale). For $$\alpha\le1$$ the density is decreasing; for $$\alpha>1$$ it rises then falls (unimodal).

**Beta function (P117):** $$B(a,b)=\int_0^1x^{a-1}(1-x)^{b-1}\,dx$$, $$a,b>0$$.
- Symmetry: $$B(a,b)=B(b,a)$$
- Relation to the gamma function: $$B(a,b)=\dfrac{\Gamma(a)\Gamma(b)}{\Gamma(a+b)}$$

**Shape of the beta distribution:** symmetric about $$\frac12$$ when $$a=b$$; $$a=b=1$$ is exactly $$U(0,1)$$; right-skewed when $$a<b$$, left-skewed when $$a>b$$.

**Three handy integrals:**

$$\int_{-\infty}^{\infty}e^{-u^2/2}\,du=\sqrt{2\pi},\qquad \int_{-\infty}^{\infty}e^{-x^2}\,dx=\sqrt\pi,\qquad \int_0^{\infty}x^2e^{-x^2}\,dx=\frac{\sqrt\pi}4.$$

---

## 3. Memorylessness (know both proofs)

**Geometric distribution (P103):** if $$X\sim Ge(p)$$, then for any positive integers $$m,n$$,

$$P(X>m+n\mid X>m)=P(X>n).$$

**Exponential distribution (P114):** if $$X\sim Exp(\lambda)$$, then for any $$s,t>0$$,

$$P(X>s+t\mid X>s)=P(X>t).$$

<details markdown="1"><summary>Proof</summary>

Geometric: $$P(X>n)=\sum_{k=n+1}^{\infty}q^{k-1}p=q^n$$, so

$$P(X>m+n\mid X>m)=\frac{P(X>m+n)}{P(X>m)}=\frac{q^{m+n}}{q^m}=q^n=P(X>n).$$

Exponential: $$P(X>t)=e^{-\lambda t}$$, so

$$P(X>s+t\mid X>s)=\frac{e^{-\lambda(s+t)}}{e^{-\lambda s}}=e^{-\lambda t}=P(X>t).$$

</details>

Intuition: the fact that you "have already waited $$s$$ minutes" does not change the distribution of "how much longer you still have to wait". The geometric is the only memoryless discrete distribution, and the exponential is the only memoryless continuous one.

---

## 4. Additivity (know how to derive each)

All random variables below are **independent**:

| Distribution | Additivity | Page |
|---|---|---|
| Binomial | $$b(n,p)+b(m,p)=b(n+m,p)$$ (same $$p$$) | P163 |
| Poisson | $$P(\lambda_1)+P(\lambda_2)=P(\lambda_1+\lambda_2)$$ | P163 |
| Normal | $$N(\mu_1,\sigma_1^2)+N(\mu_2,\sigma_2^2)=N(\mu_1+\mu_2,\sigma_1^2+\sigma_2^2)$$ | P167 |
| Gamma | $$Ga(\alpha_1,\lambda)+Ga(\alpha_2,\lambda)=Ga(\alpha_1+\alpha_2,\lambda)$$ (same $$\lambda$$) | P168 |
| Chi-square | $$\chi^2(n)+\chi^2(m)=\chi^2(n+m)$$ | P169 |

> **Note: the exponential distribution is not additive.** $$Exp(\lambda)+Exp(\lambda)=Ga(2,\lambda)$$, which is no longer exponential.

<details markdown="1"><summary>How to derive (Poisson and gamma as examples)</summary>

**Method 1: convolution.** Poisson:

$$P(Z=k)=\sum_{i=0}^{k}\frac{\lambda_1^i}{i!}e^{-\lambda_1}\frac{\lambda_2^{k-i}}{(k-i)!}e^{-\lambda_2}=\frac{e^{-(\lambda_1+\lambda_2)}}{k!}\sum_{i=0}^k\binom ki\lambda_1^i\lambda_2^{k-i}=\frac{(\lambda_1+\lambda_2)^k}{k!}e^{-(\lambda_1+\lambda_2)}.$$

**Method 2: characteristic functions** (faster). The characteristic function of an independent sum is the product of the characteristic functions:
- Poisson: $$e^{\lambda_1(e^{it}-1)}\cdot e^{\lambda_2(e^{it}-1)}=e^{(\lambda_1+\lambda_2)(e^{it}-1)}$$
- Gamma: $$(1-\frac{it}\lambda)^{-\alpha_1}(1-\frac{it}\lambda)^{-\alpha_2}=(1-\frac{it}\lambda)^{-(\alpha_1+\alpha_2)}$$

Then the uniqueness theorem for characteristic functions gives the result.

</details>

---

## 5. How the distributions relate (good to be able to write out)

| Relationship | Page |
|---|---|
| The sum of $$r$$ i.i.d. $$Ge(p)$$ variables is $$Nb(r,p)$$ | P104 |
| In a Poisson process with $$P(\lambda)$$ events per unit time, the waiting time until the first event is $$Exp(\lambda)$$ | P114 |
| $$Ga(1,\lambda)=Exp(\lambda)$$ | P116 |
| $$Ga(\tfrac n2,\tfrac12)=\chi^2(n)$$ | P116 |
| The time of the $$n$$-th event is $$Ga(n,\lambda)$$; link to the Poisson below | P116 |
| $$Be(1,1)=U(0,1)$$ | P118 |
| $$X\sim Ga(\alpha,\lambda)\Rightarrow kX\sim Ga(\alpha,\lambda/k)$$, $$k>0$$ | P125 |
| $$X\sim N(0,1)\Rightarrow X^2\sim\chi^2(1)$$ | P127 |
| $$t(1)=Cau(0,1)$$ | |
| $$X\sim t(n)\Rightarrow X^2\sim F(1,n)$$ | |

**Poisson vs. exponential and gamma:** let $$N(t)$$ be the number of events in $$[0,t]$$, with $$N(t)\sim P(\lambda t)$$.
- Time of the first event $$T_1$$: $$P(T_1>t)=P(N(t)=0)=e^{-\lambda t}$$, so $$T_1\sim Exp(\lambda)$$.
- Time of the $$n$$-th event $$T_n$$: $$P(T_n>t)=P(N(t)<n)=\sum_{k=0}^{n-1}\frac{(\lambda t)^k}{k!}e^{-\lambda t}$$, so $$T_n\sim Ga(n,\lambda)$$.

**$$Y=F_X(X)\sim U(0,1)$$ (P126, know the proof):** if the distribution function $$F_X$$ of $$X$$ is continuous and strictly increasing, then $$Y=F_X(X)\sim U(0,1)$$. This is the basis of the inverse-transform method for generating random numbers.

<details markdown="1"><summary>Proof</summary>

For $$0<y<1$$,

$$F_Y(y)=P(F_X(X)\le y)=P\big(X\le F_X^{-1}(y)\big)=F_X\big(F_X^{-1}(y)\big)=y;$$

for $$y\le0$$, $$F_Y(y)=0$$, and for $$y\ge1$$, $$F_Y(y)=1$$. Hence $$Y\sim U(0,1)$$.  
Conversely, if $$U\sim U(0,1)$$, then $$X=F^{-1}(U)$$ has distribution function $$F$$.

</details>

**A linear transformation of a normal is normal (P124):** $$X\sim N(\mu,\sigma^2)\Rightarrow aX+b\sim N(a\mu+b,a^2\sigma^2)$$, $$a\ne0$$.

---

## 6. Shapes and approximations

**Shapes**
- Binomial: as $$p$$ increases, the peak shifts to the right (P95). Right-skewed for $$p<0.5$$, symmetric for $$p=0.5$$, left-skewed for $$p>0.5$$.
- Poisson: as $$\lambda$$ increases, it becomes more and more symmetric (from right-skewed to symmetric).

**Approximations**

| Approximation | Page | Conditions and usage |
|---|---|---|
| Binomial → Poisson (Poisson's theorem, Thm 2.4.1) | P98 | For large $$n$$, small $$p$$ and moderate $$np=\lambda$$: $$\binom nkp^k(1-p)^{n-k}\approx\frac{\lambda^k}{k!}e^{-\lambda}$$ |
| Hypergeometric → binomial | P102 | When $$N$$ is large and $$n$$ is small relative to $$N$$, sampling without replacement ≈ with replacement: $$h(n,N,M)\approx b(n,\frac MN)$$ (just know the result) |
| Binomial → normal (de Moivre–Laplace) | P243 | For large $$n$$: $$P(a\le X\le b)\approx\Phi\left(\frac{b+0.5-np}{\sqrt{npq}}\right)-\Phi\left(\frac{a-0.5-np}{\sqrt{npq}}\right)$$ (the 0.5 correction improves accuracy) |

<details markdown="1"><summary>Proof of Poisson's theorem (know it)</summary>

**Theorem.** If $$\lim_{n\to\infty}np_n=\lambda>0$$, then for every fixed $$k$$, $$\lim_{n\to\infty}\binom nkp_n^k(1-p_n)^{n-k}=\frac{\lambda^k}{k!}e^{-\lambda}$$.

Write $$\lambda_n=np_n$$. Then

$$\binom nkp_n^k(1-p_n)^{n-k}=\frac{\lambda_n^k}{k!}\cdot\underbrace{\frac{n(n-1)\cdots(n-k+1)}{n^k}}_{\to1}\cdot\underbrace{\left(1-\frac{\lambda_n}n\right)^{n}}_{\to e^{-\lambda}}\cdot\underbrace{\left(1-\frac{\lambda_n}n\right)^{-k}}_{\to1}\to\frac{\lambda^k}{k!}e^{-\lambda}.$$

</details>

---

## 7. Moments, skewness and kurtosis

**Central moments in terms of raw moments, up to order 4 (P120)** ($$\mu_k$$ raw, $$\nu_k$$ central)

$$
\begin{aligned}
\nu_2&=\mu_2-\mu_1^2\\
\nu_3&=\mu_3-3\mu_2\mu_1+2\mu_1^3\\
\nu_4&=\mu_4-4\mu_3\mu_1+6\mu_2\mu_1^2-3\mu_1^4
\end{aligned}
$$

**Moments of the normal (P120; P130, Example 2.7.1):** for $$X\sim N(0,\sigma^2)$$, $$\mu_k=0$$ for odd $$k$$ and $$\mu_k=\sigma^k(k-1)!!$$ for even $$k$$. The first four are $$0,\ \sigma^2,\ 0,\ 3\sigma^4$$.

**Skewness and kurtosis of some common distributions (P137)**

| Distribution | $$E(X)$$ | $$\mathrm{Var}(X)$$ | Skewness $$\beta_s$$ | Kurtosis $$\beta_k$$ |
|---|---|---|---|---|
| $$U(a,b)$$ | $$\frac{a+b}{2}$$ | $$\frac{(b-a)^2}{12}$$ | 0 | −1.2 |
| $$N(\mu,\sigma^2)$$ | $$\mu$$ | $$\sigma^2$$ | 0 | 0 |
| $$Exp(\lambda)$$ | $$\frac1\lambda$$ | $$\frac1{\lambda^2}$$ | 2 | 6 |
| $$Ga(\alpha,\lambda)$$ | $$\frac\alpha\lambda$$ | $$\frac{\alpha}{\lambda^2}$$ | $$\frac{2}{\sqrt\alpha}$$ | $$\frac6\alpha$$ |

As $$\alpha\to\infty$$, both the skewness and kurtosis of the gamma tend to 0 — it gets closer and closer to normal.

**The 3σ rule for the normal (P111)**

$$P(\vert X-\mu\vert <k\sigma)=\begin{cases}0.6826,&k=1\\0.9545,&k=2\\0.9973,&k=3\end{cases}$$

**Application: process capability index** $$C_p=\dfrac{\text{USL}-\text{LSL}}{6\sigma}$$ (upper minus lower specification limit), used to judge whether a production process is stable; $$C_p\ge1.33$$ is considered normal. ($$C_{pk}$$ further accounts for the mean drifting off center.)

---

## 8. Other related results

- The density of a continuous distribution is not unique (P74): changing its values at finitely many points does not affect the integral.
- If the variance exists, the mean exists (P87), since $$\vert x\vert \le1+x^2$$.
- A bounded random variable always has a mean and variance: if $$X\in[a,b]$$, then $$a\le E(X)\le b$$ and $$\mathrm{Var}(X)\le\left(\frac{b-a}2\right)^2$$.

---

**Series contents**

1. [01 · Core Definitions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-01-concepts %})
2. **02 · Common Distributions** (this post)
3. [03 · Random Vectors, Covariance and Conditional Expectation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-03-multivariate %})
4. [04 · Characteristic Functions, LLN and CLT]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-04-limit-theorems %})
5. [05 · Sampling Distributions, Order Statistics and Sufficiency]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-05-sampling %})
6. [06 · Point Estimation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-06-estimation %})
7. [07 · Interval Estimation, Hypothesis Testing and Exercise Results]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-07-intervals-tests %})

[← Previous: Core Definitions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-01-concepts %})　·　[Next: Random Vectors, Covariance and Conditional Expectation →]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-03-multivariate %})

*This series collects my review notes on* Probability Theory and Mathematical Statistics *by Mao Shisong, Cheng Yiming and Pu Xiaolong, in 7 parts. P××× refers to page numbers in the book; "Guide" refers to its companion study guide and solutions manual. Corrections are welcome. A Chinese version is on my [Chinese homepage](https://bobothebest.github.io/homepage_cn/).*
