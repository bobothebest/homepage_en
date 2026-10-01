---
layout: post
title: "Probability & Statistics Review 06 | Point Estimation: Unbiasedness, Consistency, MLE, Fisher Information and the Cramér–Rao Bound"
author: Ziyuan Zhang
tags:
- Probability & Statistics Review
- mathematical statistics
- parameter estimation
- maximum likelihood
- UMVUE
- study notes
date: 2026-10-01 10:06 -0400
toc: true
math: true
---
> **Overview**
>
> The properties in the estimation chapter are especially easy to mix up: is unbiasedness invariant? Is the method-of-moments estimator unique? What about the MLE?
>
> This part first lines up all the "unique / invariant" results in one table, then collects the theorems you need to be able to prove — consistency, UMVUE, the sufficiency principle and the Cramér–Rao inequality — and ends with a conjugate-prior example of Bayesian estimation.

---

## 1. Two methods of point estimation

**Method of moments (substitution principle):** replace population moments with sample moments, and a function of population moments with the same function of sample moments.  
Example: $$E(X)=\mu\Rightarrow\hat\mu=\bar x$$; $$\mathrm{Var}(X)=\sigma^2\Rightarrow\hat\sigma^2=s_n^2$$.

**Maximum likelihood:** $$L(\theta)=\prod_{i=1}^np(x_i;\theta)$$; usually solve the log-likelihood equation $$\frac{\partial\ln L}{\partial\theta}=0$$.
- Normal population (P316): $$\hat\mu=\bar x$$, $$\hat\sigma^2=s_n^2=\frac1n\sum(x_i-\bar x)^2$$
- When the support depends on the parameter you cannot differentiate; look directly at how $$L$$ behaves. Example: for $$U(0,\theta)$$, $$L(\theta)=\theta^{-n}I_{\{\theta\ge x_{(n)}\}}$$ is decreasing on $$\theta\ge x_{(n)}$$, so $$\hat\theta=x_{(n)}$$.

---

## 2. Properties side by side (what is "unique", what is "invariant")

| Result | Page | Explanation / example |
|---|---|---|
| **Unbiasedness is not invariant** | P303 | $$s^2$$ is unbiased for $$\sigma^2$$, but $$s$$ is not unbiased for $$\sigma$$ (by Jensen's inequality, $$E s<\sigma$$) |
| Method of moments: the MoM estimator of $$g(\theta)$$ can be taken as $$g(\hat\theta)$$ | P309 | Direct consequence of the substitution principle |
| **MoM estimators are not unique** | P309 | For a Poisson population, both $$\bar x$$ and $$s_n^2$$ are MoM estimators of $$\lambda$$ (since $$E X=\mathrm{Var}X=\lambda$$) |
| **Consistent estimators are not unique** | P310 | If $$\hat\theta_n$$ is consistent, so is $$\hat\theta_n+\frac1n$$ |
| **Consistency is invariant** (Thm 6.2.2, know the proof) | P311 | $$\hat\theta_n\xrightarrow{P}\theta$$ and $$g$$ continuous $$\Rightarrow g(\hat\theta_n)\xrightarrow{P}g(\theta)$$ |
| **MoM estimators are generally consistent** | P312 | Khinchin's LLN + Thm 6.2.2 |
| **The MLE is invariant** | P316 | $$\hat\theta$$ is the MLE $$\Rightarrow g(\hat\theta)$$ is the MLE of $$g(\theta)$$. Example: the MLE of $$\sigma$$ is $$s_n$$ |
| **The MLE may not be unique** (Exercise 6.3.2) | | For $$U(\theta-\frac12,\theta+\frac12)$$, any point in $$[x_{(n)}-\frac12,\ x_{(1)}+\frac12]$$ is an MLE |

---

## 3. Consistency

**Theorem 6.2.1 (P310, know the proof):** if $$\lim_{n\to\infty}E(\hat\theta_n)=\theta$$ and $$\lim_{n\to\infty}\mathrm{Var}(\hat\theta_n)=0$$, then $$\hat\theta_n$$ is a consistent estimator of $$\theta$$.

<details markdown="1"><summary>Proof</summary>

By Markov's (Chebyshev-type) inequality,

$$P(\vert \hat\theta_n-\theta\vert \ge\varepsilon)\le\frac{E(\hat\theta_n-\theta)^2}{\varepsilon^2}=\frac{\mathrm{Var}(\hat\theta_n)+\big(E\hat\theta_n-\theta\big)^2}{\varepsilon^2}\to0.$$

</details>

**Theorem 6.2.2 (P311, know the proof):** if $$\hat\theta_{n1},\dots,\hat\theta_{nk}$$ are consistent for $$\theta_1,\dots,\theta_k$$, and $$\eta=g(\theta_1,\dots,\theta_k)$$ is continuous, then $$\hat\eta_n=g(\hat\theta_{n1},\dots,\hat\theta_{nk})$$ is consistent for $$\eta$$.

<details markdown="1"><summary>Proof sketch (one dimension)</summary>

Since $$g$$ is continuous at $$\theta$$, for any $$\varepsilon>0$$ there is $$\delta>0$$ such that $$\vert \hat\theta_n-\theta\vert <\delta$$ implies $$\vert g(\hat\theta_n)-g(\theta)\vert <\varepsilon$$. Hence

$$P\big(\vert g(\hat\theta_n)-g(\theta)\vert \ge\varepsilon\big)\le P(\vert \hat\theta_n-\theta\vert \ge\delta)\to0.$$

</details>

**Asymptotic normality of the MLE (Guide P328):** under suitable regularity conditions,

$$\hat\theta_{MLE}\ \dot\sim\ N\left(\theta,\ \frac{1}{nI(\theta)}\right),$$

where $$n$$ is the sample size and $$I(\theta)$$ the Fisher information. So the MLE is both **consistent** and **asymptotically normal**, and its asymptotic variance attains the C-R lower bound (asymptotically efficient).

---

## 4. Mean squared error and efficiency

**Mean squared error (P323)**

$$\mathrm{MSE}(\hat\theta)=E(\hat\theta-\theta)^2=\mathrm{Var}(\hat\theta)+\big(E\hat\theta-\theta\big)^2.$$

For an unbiased estimator, MSE is just the variance. A biased estimator can actually have smaller MSE. Example: for a normal population, the estimator $$\frac1{n+1}\sum(x_i-\bar x)^2$$ of $$\sigma^2$$ has smaller MSE than $$s^2$$.

---

## 5. UMVUE

**Theorem 6.4.1 (P325, know the proof):** let $$\hat\theta$$ be unbiased for $$\theta$$ with $$\mathrm{Var}(\hat\theta)<\infty$$. Then $$\hat\theta$$ is the UMVUE of $$\theta$$ if and only if, for every statistic $$\varphi$$ with $$E(\varphi)=0$$ and $$\mathrm{Var}(\varphi)<\infty$$ (an "unbiased estimator of zero"),

$$\mathrm{Cov}_\theta(\hat\theta,\varphi)=0,\qquad\forall\theta\in\Theta.$$

In one sentence: the UMVUE is uncorrelated with every unbiased estimator of zero.

<details markdown="1"><summary>Proof</summary>

**Sufficiency:** let $$\tilde\theta$$ be any unbiased estimator and $$\varphi=\tilde\theta-\hat\theta$$, so $$E\varphi=0$$. Then

$$\mathrm{Var}(\tilde\theta)=\mathrm{Var}(\hat\theta+\varphi)=\mathrm{Var}(\hat\theta)+\mathrm{Var}(\varphi)+2\underbrace{\mathrm{Cov}(\hat\theta,\varphi)}_{=0}\ge\mathrm{Var}(\hat\theta).$$

**Necessity** (by contradiction): if $$\mathrm{Cov}(\hat\theta,\varphi)\ne0$$ for some $$\theta_0$$, consider the unbiased estimator $$\hat\theta+\lambda\varphi$$:

$$\mathrm{Var}(\hat\theta+\lambda\varphi)=\mathrm{Var}(\hat\theta)+2\lambda\mathrm{Cov}(\hat\theta,\varphi)+\lambda^2\mathrm{Var}(\varphi).$$

Taking $$\lambda=-\mathrm{Cov}/\mathrm{Var}(\varphi)$$ makes the variance strictly smaller than $$\mathrm{Var}(\hat\theta)$$, contradicting the UMVUE property.

</details>

**Theorem 6.4.2, sufficiency principle (P327, know the proof):** let $$T$$ be a sufficient statistic, $$\hat\theta$$ an unbiased estimator of $$\theta$$, and $$\tilde\theta=E(\hat\theta\mid T)$$. Then $$\tilde\theta$$ is also unbiased for $$\theta$$, and $$\mathrm{Var}(\tilde\theta)\le\mathrm{Var}(\hat\theta)$$.

<details markdown="1"><summary>Proof</summary>

Because $$T$$ is sufficient, the conditional distribution does not depend on $$\theta$$, so $$E(\hat\theta\mid T)$$ involves no $$\theta$$ — it is a statistic.
- Unbiasedness: by the law of total expectation, $$E\tilde\theta=E\big(E(\hat\theta\mid T)\big)=E\hat\theta=\theta$$.
- Variance: by the law of total variance, $$\mathrm{Var}(\hat\theta)=\mathrm{Var}\big(E(\hat\theta\mid T)\big)+E\big(\mathrm{Var}(\hat\theta\mid T)\big)\ge\mathrm{Var}(\tilde\theta)$$.

</details>

Intuition: a good estimator should depend only on the sufficient statistic, and conditioning on the sufficient statistic can only make an estimator better (or at least no worse). So to find the UMVUE, you only need to search among functions of the sufficient statistic (Rao–Blackwell).

---

## 6. Fisher information and the C-R inequality

**Fisher information (P329)**

$$I(\theta)=E\left[\frac{\partial}{\partial\theta}\ln p(x;\theta)\right]^2=-E\left[\frac{\partial^2}{\partial\theta^2}\ln p(x;\theta)\right].$$

For the second equality see Exercise 6.4.5; the second-derivative form is usually easier to compute. The larger $$I(\theta)$$, the more information one observation carries about $$\theta$$.

**☆ Theorem 6.4.3, Cramér–Rao inequality (P329, know the proof):** under regularity conditions, if $$T$$ is an unbiased estimator of $$g(\theta)$$, then

$$\mathrm{Var}(T)\ge\frac{[g'(\theta)]^2}{nI(\theta)}.$$

In particular, every unbiased estimator of $$\theta$$ has variance $$\ge\dfrac1{nI(\theta)}$$. An unbiased estimator attaining the bound is called **efficient**.

<details markdown="1"><summary>Proof sketch</summary>

Let the score be $$S=\sum_{i=1}^n\frac{\partial}{\partial\theta}\ln p(x_i;\theta)$$; then $$E(S)=0$$ and $$\mathrm{Var}(S)=nI(\theta)$$.  
Differentiate both sides of $$E_\theta(T)=g(\theta)$$ with respect to $$\theta$$ (regularity lets us swap derivative and integral):

$$g'(\theta)=\int T(\boldsymbol x)\frac{\partial}{\partial\theta}p(\boldsymbol x;\theta)\,d\boldsymbol x=E(TS)=\mathrm{Cov}(T,S).$$

By the Schwarz inequality, $$[g'(\theta)]^2=[\mathrm{Cov}(T,S)]^2\le\mathrm{Var}(T)\,\mathrm{Var}(S)=\mathrm{Var}(T)\cdot nI(\theta)$$.

</details>

**Fisher information of common distributions (Guide P341)**

| Distribution | Fisher information |
|---|---|
| $$b(1,p)$$ | $$I(p)=\dfrac1{p(1-p)}$$ |
| $$P(\lambda)$$ | $$I(\lambda)=\dfrac1\lambda$$ |
| $$Exp(\lambda)$$, density $$\lambda e^{-\lambda x}$$ | $$I(\lambda)=\dfrac1{\lambda^2}$$ |
| $$N(\mu,1)$$ | $$I(\mu)=1$$ |
| $$N(0,\sigma^2)$$ | $$I(\sigma^2)=\dfrac1{2\sigma^4}$$ |
| $$N(\mu,\sigma^2)$$ | $$I(\mu,\sigma^2)=\begin{pmatrix}\frac1{\sigma^2}&0\\0&\frac1{2\sigma^4}\end{pmatrix}$$ |

Example: for a Poisson population, $$\mathrm{Var}(\bar x)=\frac\lambda n=\frac{1}{nI(\lambda)}$$, so $$\bar x$$ is an efficient estimator of $$\lambda$$.

---

## 7. Bayesian estimation

**Posterior distribution**

$$\pi(\theta\mid x_1,\dots,x_n)=\frac{h(x_1,\dots,x_n,\theta)}{m(x_1,\dots,x_n)}=\frac{p(x_1,\dots,x_n\mid\theta)\,\pi(\theta)}{\int_\Theta p(x_1,\dots,x_n\mid\theta)\,\pi(\theta)\,d\theta},$$

where $$\pi(\theta)$$ is the prior and $$m$$ is the marginal distribution of the sample. The posterior mean $$E(\theta\mid\boldsymbol x)$$ is usually taken as the Bayes estimator of $$\theta$$.

**Conjugate prior example:** $$x_i\sim b(1,p)$$ with prior $$p\sim Be(a,b)$$. The posterior is $$Be\left(a+\sum x_i,\ b+n-\sum x_i\right)$$, and the Bayes estimator is

$$\hat p_B=\frac{a+\sum x_i}{a+b+n}.$$

Intuition: the prior is like "having already seen $$a+b$$ trials, $$a$$ of them successes"; you pool those with the real data and compute the success rate.

---

**Series contents**

1. [01 · Core Definitions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-01-concepts %})
2. [02 · Common Distributions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-02-distributions %})
3. [03 · Random Vectors, Covariance and Conditional Expectation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-03-multivariate %})
4. [04 · Characteristic Functions, LLN and CLT]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-04-limit-theorems %})
5. [05 · Sampling Distributions, Order Statistics and Sufficiency]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-05-sampling %})
6. **06 · Point Estimation** (this post)
7. [07 · Interval Estimation, Hypothesis Testing and Exercise Results]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-07-intervals-tests %})

[← Previous: Sampling Distributions, Order Statistics and Sufficiency]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-05-sampling %})　·　[Next: Interval Estimation, Hypothesis Testing and Exercise Results →]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-07-intervals-tests %})

*This series collects my review notes on* Probability Theory and Mathematical Statistics *by Mao Shisong, Cheng Yiming and Pu Xiaolong, in 7 parts. P××× refers to page numbers in the book; "Guide" refers to its companion study guide and solutions manual. Corrections are welcome. A Chinese version is on my [Chinese homepage](https://bobothebest.github.io/homepage_cn/).*
