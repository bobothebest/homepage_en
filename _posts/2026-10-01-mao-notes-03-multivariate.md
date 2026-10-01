---
layout: post
title: "Probability & Statistics Review 03 | Random Vectors, Covariance and Conditional Expectation"
author: Ziyuan Zhang
tags:
- Probability & Statistics Review
- probability
- random vectors
- conditional expectation
- study notes
date: 2026-10-01 10:03 -0400
toc: true
math: true
---
> **Overview**
>
> The chapter on random vectors is where my notes have the densest cluster of "know the proof" marks: Chebyshev's inequality, the Schwarz inequality, the necessary and sufficient condition for a correlation of ±1, non-negative definiteness of the covariance matrix, the law of total expectation…
>
> The good news is that these proofs share one idea: **build a non-negative quantity, then ask when it equals 0**. Keep an eye out for that thread as you read.

---

## 1. Common multivariate distributions

| Distribution | Page | Key point |
|---|---|---|
| Multinomial $$M(n;p_1,\dots,p_r)$$ | P145 | $$P(X_1=n_1,\dots,X_r=n_r)=\dfrac{n!}{n_1!\cdots n_r!}p_1^{n_1}\cdots p_r^{n_r}$$, $$\sum n_i=n$$ |
| Multivariate hypergeometric | P146 | $$\dfrac{\binom{N_1}{n_1}\cdots\binom{N_r}{n_r}}{\binom Nn}$$ |
| Multivariate uniform | P147 | $$p(x)=\frac1{S_D}$$ on a region $$D$$ |
| ☆ Bivariate normal $$N(\mu_1,\mu_2,\sigma_1^2,\sigma_2^2,\rho)$$ | P148 | see below |
| Bivariate exponential | P152 | $$F(x,y)=1-e^{-x}-e^{-y}+e^{-x-y-\lambda xy}$$, $$x,y>0$$ |
| n-variate normal | P189 | $$p(\boldsymbol x)=\dfrac{1}{(2\pi)^{n/2}\vert \boldsymbol B\vert ^{1/2}}\exp\left\{-\tfrac12(\boldsymbol x-\boldsymbol a)^T\boldsymbol B^{-1}(\boldsymbol x-\boldsymbol a)\right\}$$ |

**Density of the bivariate normal**

$$p(x,y)=\frac{1}{2\pi\sigma_1\sigma_2\sqrt{1-\rho^2}}\exp\left\{-\frac{1}{2(1-\rho^2)}\left[\frac{(x-\mu_1)^2}{\sigma_1^2}-2\rho\frac{(x-\mu_1)(y-\mu_2)}{\sigma_1\sigma_2}+\frac{(y-\mu_2)^2}{\sigma_2^2}\right]\right\}$$

---

## 2. Joint vs. marginal distributions (P135)

- The one-dimensional marginals of a multinomial are binomial: $$X_i\sim b(n,p_i)$$ (remember it).
- The marginals of a bivariate normal are univariate normal: $$X\sim N(\mu_1,\sigma_1^2)$$, $$Y\sim N(\mu_2,\sigma_2^2)$$ (P150, know the derivation).
- **Different joint distributions can share the same marginals (P156):** in the bivariate exponential, both marginals are $$Exp(1)$$ whatever $$\lambda$$ is, so marginals do not determine the joint distribution. Likewise, the marginals of a bivariate normal do not depend on $$\rho$$.

<details markdown="1"><summary>Sketch: marginals of the bivariate normal</summary>

Complete the square in $$y$$. With $$u=\frac{x-\mu_1}{\sigma_1}$$ and $$v=\frac{y-\mu_2}{\sigma_2}$$, the exponent can be written as

$$-\frac{u^2}{2}-\frac{(v-\rho u)^2}{2(1-\rho^2)}.$$

When integrating over $$y$$, the second term is a normal kernel with mean $$\rho u$$ and variance $$1-\rho^2$$, which integrates to a constant, leaving

$$p_X(x)=\frac{1}{\sqrt{2\pi}\sigma_1}e^{-\frac{(x-\mu_1)^2}{2\sigma_1^2}}.$$

</details>

---

## 3. Independence

- If the components of a random vector are mutually independent, then any one group of components is independent of any other (Guide P149).
- If the components are mutually independent, the joint distribution is uniquely determined by the marginals (their product).
- Independence can be checked from the definition ($$p(x,y)=p_X(x)p_Y(y)$$), or argued from the practical setup.
- For discrete random variables, exams mostly test **applying** independence: be familiar with the typical checks in the book.

**Convolution formula, continuous case (P167):** if $$X,Y$$ are independent, the density of $$Z=X+Y$$ is

$$p_Z(z)=\int_{-\infty}^{\infty}p_X(z-y)\,p_Y(y)\,dy=\int_{-\infty}^{\infty}p_X(x)\,p_Y(z-x)\,dx.$$

**Identities for $$\max$$ and $$\min$$** (Exercise 3.3.12)

$$\min\{a,b\}=\tfrac12\big[a+b-\vert b-a\vert \big],\qquad \max\{a,b\}=\tfrac12\big[a+b+\vert b-a\vert \big].$$

Application (Exercises 3.4.14, 3.4.18): for $$X,Y\overset{iid}{\sim}N(0,1)$$, $$E\max\{X,Y\}=\frac1{\sqrt\pi}$$; for $$X,Y\overset{iid}{\sim}N(a,\sigma^2)$$, $$E\max\{X,Y\}=a+\frac{\sigma}{\sqrt\pi}$$.

<details markdown="1"><summary>Derivation</summary>

$$X-Y\sim N(0,2)$$, and $$E\vert Z\vert =\sigma_Z\sqrt{2/\pi}$$, so $$E\vert X-Y\vert =\sqrt2\cdot\sqrt{2/\pi}=\frac{2}{\sqrt\pi}$$.  
Hence $$E\max\{X,Y\}=\frac12\left[E(X+Y)+E\vert X-Y\vert \right]=\frac1{\sqrt\pi}$$. For the general case, write $$X=a+\sigma X'$$.

</details>

---

## 4. Expectation, variance and covariance

**Chebyshev's inequality (P90, know the proof)**

$$P\big(\vert X-E(X)\vert \ge\varepsilon\big)\le\frac{\mathrm{Var}(X)}{\varepsilon^2},\qquad\forall\varepsilon>0.$$

<details markdown="1"><summary>Proof (continuous case)</summary>

$$P(\vert X-EX\vert \ge\varepsilon)=\int_{\vert x-EX\vert \ge\varepsilon}p(x)\,dx\le\int_{\vert x-EX\vert \ge\varepsilon}\frac{(x-EX)^2}{\varepsilon^2}p(x)\,dx\le\frac1{\varepsilon^2}\int_{-\infty}^{\infty}(x-EX)^2p(x)\,dx=\frac{\mathrm{Var}(X)}{\varepsilon^2}.$$

More generally (Exercise 2.3.12): if $$g(x)$$ is non-negative and non-decreasing and $$E g(X)$$ exists, then $$P(X>\varepsilon)\le\dfrac{E\,g(X)}{g(\varepsilon)}$$.

</details>

**Theorem 2.3.2: $$\mathrm{Var}(X)=0\iff P(X=a)=1$$ (know the proof)**

<details markdown="1"><summary>Proof</summary>

($$\Leftarrow$$) $$X$$ equals the constant $$a$$ almost surely, so $$E(X)=a$$ and $$\mathrm{Var}(X)=E(X-a)^2=0$$.

($$\Rightarrow$$) Let $$a=E(X)$$. By Chebyshev, for each $$n$$, $$P\left(\vert X-a\vert \ge\frac1n\right)\le n^2\,\mathrm{Var}(X)=0$$, so

$$P(X\ne a)=P\left(\bigcup_{n=1}^{\infty}\left\{\vert X-a\vert \ge\tfrac1n\right\}\right)\le\sum_{n=1}^{\infty}P\left(\vert X-a\vert \ge\tfrac1n\right)=0.$$

</details>

**Optimality of the mean and the median** (Exercise 2.3.9)
- $$E(X-EX)^2\le E(X-c)^2$$: squared deviation is minimized at the mean (Theorem 5.3.2 on P264 is the sample version).
- $$E\vert X-m\vert \le E\vert X-c\vert$$: absolute deviation is minimized at the median $$m$$.

**Expectation from the distribution function** (Exercises 2.2.20, 2.2.21)
- Continuous random variable: $$E(X)=\int_0^{\infty}[1-F(x)]\,dx-\int_{-\infty}^{0}F(x)\,dx$$
- Non-negative continuous random variable: $$E(X)=\int_0^{\infty}P(X>x)\,dx$$, $$E(X^n)=\int_0^{\infty}nx^{n-1}P(X>x)\,dx$$

**☆ Schwarz inequality (P183, know the proof)**

$$\big[\mathrm{Cov}(X,Y)\big]^2\le\mathrm{Var}(X)\,\mathrm{Var}(Y).$$

**Property 3.4.12 (P183, know the proof):** $$\mathrm{Corr}(X,Y)=\pm1$$ if and only if $$X$$ and $$Y$$ are almost surely linearly related, i.e. there exist $$a\ne0$$ and $$b$$ with $$P(Y=aX+b)=1$$.

<details markdown="1"><summary>Proof</summary>

Let $$X^*=X-EX$$, $$Y^*=Y-EY$$. For any real $$t$$,

$$g(t)=E(tX^*+Y^*)^2=t^2\mathrm{Var}(X)+2t\,\mathrm{Cov}(X,Y)+\mathrm{Var}(Y)\ge0.$$

This quadratic in $$t$$ is non-negative, so its discriminant is $$\le0$$: $$4[\mathrm{Cov}(X,Y)]^2-4\mathrm{Var}(X)\mathrm{Var}(Y)\le0$$, which is the Schwarz inequality.

$$\vert \mathrm{Corr}\vert =1$$ is equivalent to the discriminant being $$=0$$, i.e. there is a $$t_0$$ with $$g(t_0)=E(t_0X^*+Y^*)^2=0$$, that is, $$\mathrm{Var}(t_0X+Y)=0$$. By Theorem 2.3.2, $$t_0X+Y$$ is almost surely constant, i.e. $$Y=aX+b$$ with $$a=-t_0$$. And $$a\ne0$$, otherwise $$Y$$ would be constant and the correlation undefined. Conversely, if $$Y=aX+b$$, direct calculation gives $$\mathrm{Corr}=\mathrm{sgn}(a)=\pm1$$.

</details>

**The correlation coefficient of $$N(\mu_1,\mu_2,\sigma_1^2,\sigma_2^2,\rho)$$ is exactly $$\rho$$ (P182, know the proof).** So for the bivariate normal, **uncorrelated and independent are equivalent**.

**Theorem 3.4.2 (P188, know the proof):** the covariance matrix $$\mathrm{Cov}(\boldsymbol X)=\big(\mathrm{Cov}(X_i,X_j)\big)_{n\times n}$$ of an $$n$$-dimensional random vector is symmetric and non-negative definite.

<details markdown="1"><summary>Proof</summary>

Symmetry is obvious. For any $$\boldsymbol c=(c_1,\dots,c_n)^T$$,

$$\boldsymbol c^T\mathrm{Cov}(\boldsymbol X)\boldsymbol c=\sum_{i,j}c_ic_j\mathrm{Cov}(X_i,X_j)=\mathrm{Var}\left(\sum_i c_iX_i\right)\ge0.$$

</details>

**Variance of a sum:**

$$\mathrm{Var}\left(\sum_{i=1}^nX_i\right)=\sum_{i=1}^n\mathrm{Var}(X_i)+2\sum_{i<j}\mathrm{Cov}(X_i,X_j).$$

If only neighbors are correlated (covariance 0 whenever $$\vert i-j\vert \ge2$$), this simplifies to $$\sum\mathrm{Var}(X_i)+2\sum_{i=1}^{n-1}\mathrm{Cov}(X_i,X_{i+1})$$ (Exercise 4.3.4 — quite handy).

**Matching problem** (the gift-exchange problem): $$n$$ people each draw one gift back at random; let $$S_n$$ be the number who get their own gift. Then $$E(S_n)=1$$ and $$\mathrm{Var}(S_n)=1$$, regardless of $$n$$.

<details markdown="1"><summary>Derivation</summary>

Let $$X_i=1$$ if person $$i$$ draws their own gift. Then $$P(X_i=1)=\frac1n$$ and $$P(X_iX_j=1)=\frac1{n(n-1)}$$.  
$$E(S_n)=n\cdot\frac1n=1$$;  
$$\mathrm{Var}(S_n)=n\cdot\frac1n\left(1-\frac1n\right)+n(n-1)\left[\frac{1}{n(n-1)}-\frac1{n^2}\right]=1-\frac1n+\frac1n=1$$.

</details>

---

## 5. Conditional distributions and conditional expectation

**Deriving the conditional distribution function (P197):** in the continuous case, $$P(X\le x\mid Y=y)$$ is defined as $$\lim_{h\to0^+}P(X\le x\mid y\le Y\le y+h)$$, which leads to the conditional density

$$p(x\mid y)=\frac{p(x,y)}{p_Y(y)}.$$

**Conditionals of a bivariate normal are normal (P198, know the proof)**

$$X\mid Y=y\ \sim\ N\left(\mu_1+\rho\frac{\sigma_1}{\sigma_2}(y-\mu_2),\ \sigma_1^2(1-\rho^2)\right).$$

The conditional mean $$g_1(y)=E(X\mid Y=y)=\mu_1+\rho\frac{\sigma_1}{\sigma_2}(y-\mu_2)$$ is linear in $$y$$ — the regression line; the conditional variance does not depend on $$y$$ and is smaller than $$\sigma_1^2$$ (knowing $$Y$$ reduces our uncertainty about $$X$$). (Guide P204)

**Conditional expectation:** $$E(X\mid Y=y)$$ is a function of $$y$$; replacing $$y$$ by $$Y$$ gives $$E(X\mid Y)$$, which is a random variable.

**☆ Theorem 3.5.1, law of total expectation (P202, know the proof)**

$$E(X)=E\big(E(X\mid Y)\big).$$

<details markdown="1"><summary>Proof (discrete case)</summary>

$$E\big(E(X\mid Y)\big)=\sum_jE(X\mid Y=y_j)P(Y=y_j)=\sum_j\sum_ix_iP(X=x_i\mid Y=y_j)P(Y=y_j)=\sum_ix_i\sum_jP(X=x_i,Y=y_j)=\sum_ix_iP(X=x_i)=E(X).$$

</details>

Intuition: average within each group first, then average the group averages weighted by group size — you get the overall average.

**Expectation of a sum of a random number of random variables (P204, know the proof):** let $$X_1,X_2,\dots$$ be i.i.d., and $$N$$ a positive-integer-valued random variable independent of $$\{X_i\}$$. Then

$$E\left(\sum_{i=1}^NX_i\right)=E(X_1)\,E(N).$$

<details markdown="1"><summary>Proof</summary>

$$E\left(\sum_{i=1}^NX_i\right)=E\left[E\left(\sum_{i=1}^NX_i\,\Big\vert \,N\right)\right]=\sum_nP(N=n)\,E\left(\sum_{i=1}^nX_i\right)=\sum_nP(N=n)\,nE(X_1)=E(X_1)E(N).$$

</details>

---

**Series contents**

1. [01 | Core Definitions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-01-concepts %})
2. [02 | Common Distributions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-02-distributions %})
3. **03 | Random Vectors, Covariance and Conditional Expectation** (this post)
4. [04 | Characteristic Functions, LLN and CLT]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-04-limit-theorems %})
5. [05 | Sampling Distributions, Order Statistics and Sufficiency]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-05-sampling %})
6. [06 | Point Estimation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-06-estimation %})
7. [07 | Interval Estimation, Hypothesis Testing and Exercise Results]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-07-intervals-tests %})

[← Previous: Common Distributions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-02-distributions %})　·　[Next: Characteristic Functions, LLN and CLT →]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-04-limit-theorems %})

*This series collects my review notes on* Probability Theory and Mathematical Statistics *by Mao Shisong, Cheng Yiming and Pu Xiaolong, in 7 parts. P××× refers to page numbers in the book; "Guide" refers to its companion study guide and solutions manual. Corrections are welcome. A Chinese version is on my [Chinese homepage](https://bobothebest.github.io/homepage_cn/).*
