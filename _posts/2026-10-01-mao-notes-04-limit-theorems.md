---
layout: post
title: "Probability & Statistics Review 04 | Characteristic Functions, the Law of Large Numbers and the Central Limit Theorem"
author: Ziyuan Zhang
tags:
- Probability & Statistics Review
- probability
- characteristic functions
- law of large numbers
- central limit theorem
- study notes
date: 2026-10-01 10:04 -0400
toc: true
math: true
---
> **Overview**
>
> With the laws of large numbers and the central limit theorems, what people mix up is not the conclusions but the **conditions**: which one needs i.i.d.? Which only needs pairwise uncorrelated? Which doesn't even need a variance?
>
> This part first sets up the tool — the characteristic function — then lines up the conditions of the limit theorems side by side in tables, and finally walks through the characteristic-function proofs of Khinchin's LLN and the Lindeberg–Lévy CLT.

---

## 1. Characteristic functions

**Definition (P216)**

$$\varphi(t)=E(e^{itX})=\begin{cases}\sum_k e^{itx_k}P(X=x_k),&\text{discrete}\\[4pt]\int_{-\infty}^{\infty}e^{itx}p(x)\,dx,&\text{continuous}\end{cases}$$

Remember Euler's formula $$e^{i\theta}=\cos\theta+i\sin\theta$$. Since $$\vert e^{itX}\vert =1$$, every random variable has a characteristic function (the moment generating function need not exist).

**Properties**
1. $$\vert \varphi(t)\vert \le\varphi(0)=1$$, $$\varphi(-t)=\overline{\varphi(t)}$$
2. If $$Y=aX+b$$, then $$\varphi_Y(t)=e^{ibt}\varphi_X(at)$$
3. If $$X$$ and $$Y$$ are independent, then $$\varphi_{X+Y}(t)=\varphi_X(t)\varphi_Y(t)$$
4. If $$E(X^k)$$ exists, then $$\varphi^{(k)}(0)=i^kE(X^k)$$
5. **Inversion formula:** if $$x_1<x_2$$ are continuity points of $$F$$, then

   $$F(x_2)-F(x_1)=\lim_{T\to\infty}\frac1{2\pi}\int_{-T}^{T}\frac{e^{-itx_1}-e^{-itx_2}}{it}\varphi(t)\,dt$$

6. **Uniqueness theorem:** characteristic functions and distribution functions are in one-to-one correspondence. If $$\int\vert \varphi(t)\vert \,dt<\infty$$, then $$X$$ is continuous with

   $$p(x)=\frac1{2\pi}\int_{-\infty}^{\infty}e^{-itx}\varphi(t)\,dt$$

**Dirichlet integral (P223, used in proving the inversion formula)**

$$D(a)=\frac1\pi\int_0^{\infty}\frac{\sin at}{t}\,dt=\begin{cases}\tfrac12,&a>0\\0,&a=0\\-\tfrac12,&a<0\end{cases}$$

**Characteristic functions of common distributions (P220; Guide P227)**

| Distribution | $$\varphi(t)$$ |
|---|---|
| Degenerate $$P(X=a)=1$$ | $$e^{ita}$$ |
| 0-1 $$b(1,p)$$ | $$pe^{it}+q$$ |
| Binomial $$b(n,p)$$ | $$(pe^{it}+q)^n$$ |
| Poisson $$P(\lambda)$$ | $$e^{\lambda(e^{it}-1)}$$ |
| Geometric $$Ge(p)$$ | $$\dfrac{pe^{it}}{1-qe^{it}}$$ |
| Negative binomial $$Nb(r,p)$$ | $$\left(\dfrac{pe^{it}}{1-qe^{it}}\right)^r$$ |
| Normal $$N(\mu,\sigma^2)$$ | $$\exp\left\{i\mu t-\frac{\sigma^2t^2}2\right\}$$ |
| Standard normal $$N(0,1)$$ | $$e^{-t^2/2}$$ |
| Cauchy $$Cau(\mu,\lambda)$$ | $$\exp\{i\mu t-\lambda\lvert t\rvert\}$$ |
| Uniform $$U(a,b)$$ | $$\dfrac{e^{itb}-e^{ita}}{it(b-a)}$$ |
| Uniform $$U(-a,a)$$ | $$\dfrac{\sin at}{at}$$ |
| Exponential $$Exp(\lambda)$$ | $$\left(1-\frac{it}{\lambda}\right)^{-1}$$ |
| Gamma $$Ga(\alpha,\lambda)$$ | $$\left(1-\frac{it}{\lambda}\right)^{-\alpha}$$ |
| Chi-square $$\chi^2(n)$$ | $$(1-2it)^{-n/2}$$ |
| Beta $$Be(a,b)$$ | $$\dfrac{\Gamma(a+b)}{\Gamma(a)}\sum_{j=0}^{\infty}\dfrac{\Gamma(a+j)\,(it)^j}{\Gamma(a+b+j)\,\Gamma(j+1)}$$ |

<details markdown="1"><summary>Worked derivations: Poisson and exponential</summary>

Poisson: $$\varphi(t)=\sum_{k=0}^{\infty}e^{itk}\frac{\lambda^k}{k!}e^{-\lambda}=e^{-\lambda}\sum_{k=0}^{\infty}\frac{(\lambda e^{it})^k}{k!}=e^{-\lambda}e^{\lambda e^{it}}$$.

Exponential: $$\varphi(t)=\int_0^{\infty}e^{itx}\lambda e^{-\lambda x}\,dx=\frac{\lambda}{\lambda-it}=\left(1-\frac{it}\lambda\right)^{-1}$$.

Memory trick: a gamma is $$\alpha$$ "exponentials" added up, so its characteristic function is the exponential's raised to the power $$\alpha$$; the chi-square is $$Ga(\frac n2,\frac12)$$ — just plug in.

</details>

---

## 2. Relationships between modes of convergence

**Theorem 4.1.1 (P209):** if $$X_n\xrightarrow{P}a$$ and $$Y_n\xrightarrow{P}b$$, then
1. $$X_n\pm Y_n\xrightarrow{P}a\pm b$$
2. $$X_nY_n\xrightarrow{P}ab$$
3. $$X_n/Y_n\xrightarrow{P}a/b$$ ($$b\ne0$$)

More generally, if $$g$$ is continuous at $$a$$, then $$g(X_n)\xrightarrow{P}g(a)$$.

**Theorem 4.1.2 (P212):** $$X_n\xrightarrow{P}X\ \Rightarrow\ X_n\xrightarrow{L}X$$ (the converse fails in general).

**Theorem 4.1.3:** if $$c$$ is a constant, then $$X_n\xrightarrow{P}c\iff X_n\xrightarrow{L}c$$.

**Continuity theorem:** $$F_n\to F$$ weakly $$\iff\varphi_n(t)\to\varphi(t)$$ for every $$t$$. This is the key tool for proving limit theorems with characteristic functions.

---

## 3. Laws of large numbers

**Common form:**

$$\lim_{n\to\infty}P\left(\left\vert \frac1n\sum_{i=1}^nX_i-\frac1n\sum_{i=1}^nE(X_i)\right\vert <\varepsilon\right)=1.$$

| Name | Page | Conditions | What to know |
|---|---|---|---|
| Bernoulli's LLN | P230 | $$S_n\sim b(n,p)$$; conclusion $$\frac{S_n}n\xrightarrow{P}p$$ | Proof |
| Chebyshev's LLN | P233 | $$\{X_i\}$$ pairwise uncorrelated, variances exist with a common upper bound | |
| Markov's LLN | P234 | $$\frac1{n^2}\mathrm{Var}\left(\sum_{i=1}^nX_i\right)\to0$$ (Markov condition) | |
| Khinchin's LLN | P235 | $$\{X_i\}$$ i.i.d. and $$E(X_i)$$ exists (no variance needed) | Proof |

Intuition: Bernoulli's LLN says "frequencies settle down to probabilities", and Khinchin's says "sample means settle down to the population mean" — the basis for the method of moments and for Monte Carlo.

<details markdown="1"><summary>Proof: Bernoulli (via Chebyshev's inequality)</summary>

$$E\left(\frac{S_n}{n}\right)=p$$ and $$\mathrm{Var}\left(\frac{S_n}{n}\right)=\frac{p(1-p)}{n}$$, so

$$P\left(\left\vert \frac{S_n}n-p\right\vert \ge\varepsilon\right)\le\frac{p(1-p)}{n\varepsilon^2}\le\frac{1}{4n\varepsilon^2}\to0.$$

The proofs of Chebyshev's and Markov's LLNs are exactly the same, with a different bound on the variance.

</details>

<details markdown="1"><summary>Proof: Khinchin (via characteristic functions)</summary>

Let $$E(X_i)=a$$ and $$\varphi(t)$$ be the characteristic function of $$X_i$$. Since $$\varphi'(0)=ia$$, $$\varphi(t)=1+iat+o(t)$$.  
The characteristic function of $$Y_n=\frac1n\sum X_i$$ is

$$\varphi_{Y_n}(t)=\left[\varphi\left(\frac tn\right)\right]^n=\left[1+\frac{iat}{n}+o\left(\frac1n\right)\right]^n\to e^{iat}.$$

$$e^{iat}$$ is the characteristic function of the degenerate distribution $$P(X=a)=1$$, so $$Y_n\xrightarrow{L}a$$, and by Theorem 4.1.3, $$Y_n\xrightarrow{P}a$$.

</details>

**Exercise 4.1.18:** if $$\{X_n\}$$ are i.i.d. with $$E(X_n)=0$$ and $$\mathrm{Var}(X_n)=\sigma^2$$, then $$\frac1n\sum_{k=1}^nX_k^2\xrightarrow{P}\sigma^2$$. (Apply Khinchin's LLN to $$X_k^2$$, with $$E(X_k^2)=\sigma^2$$.)

---

## 4. Central limit theorems

| Name | Page | Conditions | Conclusion |
|---|---|---|---|
| Lindeberg–Lévy (know the proof) | P240 | $$\{X_n\}$$ i.i.d., $$E(X_i)=\mu$$, $$\mathrm{Var}(X_i)=\sigma^2>0$$ exists | $$\dfrac{\sum X_i-n\mu}{\sigma\sqrt n}\xrightarrow{L}N(0,1)$$ |
| de Moivre–Laplace | P242 | $$S_n\sim b(n,p)$$ (Bernoulli trials) | $$\dfrac{S_n-np}{\sqrt{npq}}\xrightarrow{L}N(0,1)$$ |

<details markdown="1"><summary>Proof: Lindeberg–Lévy (characteristic functions)</summary>

Let $$Y_i=\frac{X_i-\mu}{\sigma}$$. Its characteristic function is $$\varphi(t)=1-\frac{t^2}{2}+o(t^2)$$ (since $$E Y_i=0$$ and $$E Y_i^2=1$$).  
The characteristic function of $$Z_n=\frac{1}{\sqrt n}\sum Y_i$$ is

$$\left[\varphi\left(\frac t{\sqrt n}\right)\right]^n=\left[1-\frac{t^2}{2n}+o\left(\frac1n\right)\right]^n\to e^{-t^2/2},$$

which is exactly the characteristic function of $$N(0,1)$$; the continuity theorem finishes the proof.

</details>

**Application: normal approximation to the binomial (P243)**

$$P(a\le S_n\le b)\approx\Phi\left(\frac{b+0.5-np}{\sqrt{npq}}\right)-\Phi\left(\frac{a-0.5-np}{\sqrt{npq}}\right)$$

(Adding/subtracting 0.5 is the continuity correction.)

Intuition: whatever a single $$X_i$$ looks like, as long as they are i.i.d. with finite variance, adding up many of them "grinds" the result into a bell shape. That is why the normal distribution shows up everywhere in nature.

---

**Series contents**

1. [01 · Core Definitions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-01-concepts %})
2. [02 · Common Distributions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-02-distributions %})
3. [03 · Random Vectors, Covariance and Conditional Expectation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-03-multivariate %})
4. **04 · Characteristic Functions, LLN and CLT** (this post)
5. [05 · Sampling Distributions, Order Statistics and Sufficiency]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-05-sampling %})
6. [06 · Point Estimation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-06-estimation %})
7. [07 · Interval Estimation, Hypothesis Testing and Exercise Results]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-07-intervals-tests %})

[← Previous: Random Vectors, Covariance and Conditional Expectation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-03-multivariate %})　·　[Next: Sampling Distributions, Order Statistics and Sufficiency →]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-05-sampling %})

*This series collects my review notes on* Probability Theory and Mathematical Statistics *by Mao Shisong, Cheng Yiming and Pu Xiaolong, in 7 parts. P××× refers to page numbers in the book; "Guide" refers to its companion study guide and solutions manual. Corrections are welcome. A Chinese version is on my [Chinese homepage](https://bobothebest.github.io/homepage_cn/).*
