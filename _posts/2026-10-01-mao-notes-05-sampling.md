---
layout: post
title: "Probability & Statistics Review 05 | Sampling Distributions, Order Statistics and Sufficient Statistics"
author: Ziyuan Zhang
tags:
- Probability & Statistics Review
- mathematical statistics
- sampling distributions
- order statistics
- sufficient statistics
- study notes
date: 2026-10-01 10:05 -0400
toc: true
math: true
---
> **Overview**
>
> From here on we are in mathematical statistics. The sample mean $$\bar x$$, the sample variance $$s^2$$, and the three sampling distributions $$\chi^2$$, t and F are the "raw materials" for all the interval estimation and hypothesis testing that follows.
>
> The single most important theorem is: **for a normal sample, $$\bar x$$ and $$s^2$$ are independent** (Theorem 5.4.1). Almost every pivotal quantity later is built on it, so make sure you can prove it.

---

## 1. Sample mean and sample variance

**Sample variance (P267)**

$$s^2=\frac1{n-1}\sum_{i=1}^n(x_i-\bar x)^2,\qquad s_n^2=\frac1n\sum_{i=1}^n(x_i-\bar x)^2.$$

A computational identity (worth memorizing):

$$\sum_{i=1}^n(x_i-\bar x)^2=\sum x_i^2-\frac{\left(\sum x_i\right)^2}{n}=\sum x_i^2-n\bar x^2.$$

Often useful in proofs (Exercise 5.3.10): $$\left(\sum_{i=1}^nx_i\right)^2=\sum_{i=1}^nx_i^2+2\sum_{i<j}x_ix_j$$.

**Recursive update (Exercise 5.3.4):** with $$\bar x_n=\frac1n\sum_{i=1}^nx_i$$ and $$s_n^2=\frac1{n-1}\sum_{i=1}^n(x_i-\bar x_n)^2$$,

$$s_{n+1}^2=\frac{n-1}{n}s_n^2+\frac1{n+1}(x_{n+1}-\bar x_n)^2.$$

**The sample mean minimizes squared deviation (P264, Theorem 5.3.2):** $$\sum(x_i-\bar x)^2\le\sum(x_i-c)^2$$ for any constant $$c$$.

**Theorem 5.3.3 (P265): distribution of $$\bar x$$**
- Normal population $$N(\mu,\sigma^2)$$: $$\bar x\sim N(\mu,\sigma^2/n)$$ (exactly)
- Arbitrary population with finite variance $$\sigma^2$$: for large $$n$$, $$\bar x\ \dot\sim\ N(\mu,\sigma^2/n)$$ (by the CLT)

**Theorem 5.3.4 (P268, know the proof):** if the population has a finite second moment, then

$$E(\bar x)=\mu,\qquad\mathrm{Var}(\bar x)=\frac{\sigma^2}n,\qquad E(s^2)=\sigma^2.$$

<details markdown="1"><summary>Proof</summary>

The first two follow directly from properties of expectation and variance. For $$s^2$$:

$$E\left[\sum(x_i-\bar x)^2\right]=\sum E(x_i^2)-nE(\bar x^2)=n(\sigma^2+\mu^2)-n\left(\frac{\sigma^2}n+\mu^2\right)=(n-1)\sigma^2.$$

So $$E(s^2)=\sigma^2$$. This is also why $$s^2$$ divides by $$n-1$$: replacing $$\mu$$ with $$\bar x$$ "uses up" one degree of freedom.

</details>

---

## 2. Order statistics

**Distribution of a single order statistic (P273, memorize):** for a population with density $$p(x)$$ and distribution function $$F(x)$$,

$$p_k(x)=\frac{n!}{(k-1)!\,(n-k)!}\,[F(x)]^{k-1}\,[1-F(x)]^{n-k}\,p(x).$$

In particular,

$$p_{(1)}(x)=n[1-F(x)]^{n-1}p(x),\qquad p_{(n)}(x)=n[F(x)]^{n-1}p(x).$$

> **Note:** the density formula only applies to continuous populations; it fails for discrete ones. In the discrete case, work from the distribution function: $$F_{(n)}(x)=[F(x)]^n$$, $$F_{(1)}(x)=1-[1-F(x)]^n$$.

<details markdown="1"><summary>Memory aid: "split into three groups"</summary>

For $$x_{(k)}$$ to land near $$x$$, you need $$k-1$$ observations to the left of $$x$$ (probability $$F$$), $$1$$ near $$x$$ (probability $$p(x)dx$$), and $$n-k$$ to the right (probability $$1-F$$). The coefficient is the multinomial coefficient for splitting $$n$$ observations into three groups, $$\frac{n!}{(k-1)!\,1!\,(n-k)!}$$.

</details>

**Joint distribution of several order statistics (P274; see Exercises 5.3.22, 5.3.23 for applications of Theorem 5.3.6):** the joint density of $$(x_{(i)},x_{(j)})$$, $$i<j$$, is

$$p_{ij}(y,z)=\frac{n!}{(i-1)!\,(j-i-1)!\,(n-j)!}[F(y)]^{i-1}[F(z)-F(y)]^{j-i-1}[1-F(z)]^{n-j}p(y)p(z),\quad y\le z.$$

The joint density of all order statistics is $$n!\prod_{i=1}^np(y_i)$$, $$y_1<\dots<y_n$$.  
Example (Exercise 5.3.34): for $$X\sim U(0,\theta)$$, the joint density of $$(x_{(1)},\dots,x_{(n)})$$ is $$\frac{n!}{\theta^n}$$, $$0<u_1\le\dots\le u_n\le\theta$$.

**Order statistics of the uniform**
- Example 5.3.8 (P274): for a $$U(0,1)$$ population, $$x_{(k)}\sim Be(k,\ n-k+1)$$
- Example 5.3.9 (P275): for a $$U(0,1)$$ population, the range $$x_{(n)}-x_{(1)}\sim Be(n-1,\ 2)$$

**☆ Exercise 5.3.28 (turning any continuous population into a uniform):** if the population distribution function $$F$$ is continuous, let $$\eta_i=F(x_{(i)})$$. Then
1. $$\eta_1\le\dots\le\eta_n$$ are exactly the order statistics of a $$U(0,1)$$ sample (since $$F(X)\sim U(0,1)$$ and $$F$$ is monotone);
2. $$E(\eta_i)=\frac{i}{n+1}$$, $$\mathrm{Var}(\eta_i)=\frac{i(n+1-i)}{(n+1)^2(n+2)}$$ (from the mean and variance of $$Be(i,n-i+1)$$);
3. the covariance matrix of $$\eta_i,\eta_j$$ ($$i<j$$) is

$$\begin{pmatrix}\frac{a_1(1-a_1)}{n+2}&\frac{a_1(1-a_2)}{n+2}\\[2pt]\frac{a_1(1-a_2)}{n+2}&\frac{a_2(1-a_2)}{n+2}\end{pmatrix},\qquad a_1=\frac i{n+1},\ a_2=\frac j{n+1}.$$

**Asymptotic distribution of the sample $$p$$-quantile (Theorem 5.3.7, P276):** if the population density $$p(x)$$ is continuous at $$x_p$$ with $$p(x_p)>0$$, then

$$m_p\ \dot\sim\ N\left(x_p,\ \frac{p(1-p)}{n\,p^2(x_p)}\right).$$

In particular, the sample median $$m_{0.5}\ \dot\sim\ N\left(x_{0.5},\ \frac{1}{4n\,p^2(x_{0.5})}\right)$$.

**The median is robust (P277):** a few outliers barely affect the median, but can move the mean a lot.

---

## 3. The three sampling distributions

| Distribution | Construction | Density | Page |
|---|---|---|---|
| $$\chi^2(n)$$ | $$X_1,\dots,X_n\overset{iid}{\sim}N(0,1)$$, $$\sum X_i^2$$ | $$\frac{1}{2^{n/2}\Gamma(n/2)}x^{\frac n2-1}e^{-x/2},\ x>0$$ | P283 |
| $$F(m,n)$$ | $$\dfrac{X/m}{Y/n}$$, $$X\sim\chi^2(m)$$, $$Y\sim\chi^2(n)$$ independent | $$\frac{\Gamma(\frac{m+n}2)}{\Gamma(\frac m2)\Gamma(\frac n2)}\left(\frac mn\right)^{\frac m2}x^{\frac m2-1}\left(1+\frac mnx\right)^{-\frac{m+n}2},\ x>0$$ | P286 |
| $$t(n)$$ | $$\dfrac{X}{\sqrt{Y/n}}$$, $$X\sim N(0,1)$$, $$Y\sim\chi^2(n)$$ independent | $$\frac{\Gamma(\frac{n+1}2)}{\sqrt{n\pi}\,\Gamma(\frac n2)}\left(1+\frac{x^2}n\right)^{-\frac{n+1}2}$$ | P288 |

- **Quantiles of the F distribution (P287):** $$F_\alpha(n,m)=\dfrac{1}{F_{1-\alpha}(m,n)}$$ (since $$F\sim F(m,n)$$ implies $$\frac1F\sim F(n,m)$$)
- $$t(1)$$ is the standard Cauchy; $$t(n)\to N(0,1)$$ as $$n\to\infty$$; if $$T\sim t(n)$$, then $$T^2\sim F(1,n)$$
- $$\chi^2(n)$$ has mean $$n$$ and variance $$2n$$; for the means and variances of $$F(m,n)$$ and $$t(n)$$, see Part 2

<details markdown="1"><summary>Deriving the F density (know it)</summary>

First find the density of $$Z=X/Y$$. By the formula for the density of a quotient,

$$p_Z(z)=\int_0^{\infty}p_X(zy)\,p_Y(y)\,y\,dy=\frac{z^{\frac m2-1}}{2^{\frac{m+n}2}\Gamma(\frac m2)\Gamma(\frac n2)}\int_0^{\infty}y^{\frac{m+n}2-1}e^{-\frac{(1+z)y}{2}}\,dy.$$

The remaining integral is a gamma integral equal to $$\Gamma(\frac{m+n}2)\left(\frac{2}{1+z}\right)^{\frac{m+n}2}$$, so

$$p_Z(z)=\frac{\Gamma(\frac{m+n}2)}{\Gamma(\frac m2)\Gamma(\frac n2)}z^{\frac m2-1}(1+z)^{-\frac{m+n}2}.$$

Finally $$F=\frac nmZ$$; apply the linear change of variables. The $$t$$ derivation is similar: first find the density of $$\sqrt{Y/n}$$, then use the quotient formula.

</details>

**☆ Theorem 5.4.1 (P284, know the proof):** let $$x_1,\dots,x_n$$ be a sample from $$N(\mu,\sigma^2)$$. Then
1. $$\bar x$$ and $$s^2$$ are independent;
2. $$\bar x\sim N(\mu,\sigma^2/n)$$;
3. $$\dfrac{(n-1)s^2}{\sigma^2}\sim\chi^2(n-1)$$.

<details markdown="1"><summary>Proof sketch (orthogonal transformation)</summary>

WLOG $$\mu=0$$. Take an orthogonal matrix $$A$$ whose first row is $$\left(\frac1{\sqrt n},\dots,\frac1{\sqrt n}\right)$$ and set $$\boldsymbol y=A\boldsymbol x$$.
- Orthogonal transformations preserve i.i.d. normality: $$y_1,\dots,y_n\overset{iid}{\sim}N(0,\sigma^2)$$
- $$y_1=\sqrt n\,\bar x$$
- $$\sum y_i^2=\sum x_i^2$$, so $$(n-1)s^2=\sum x_i^2-n\bar x^2=\sum_{i=2}^ny_i^2$$

So $$\bar x$$ depends only on $$y_1$$ and $$s^2$$ only on $$y_2,\dots,y_n$$, hence they are independent; and $$\frac{(n-1)s^2}{\sigma^2}=\sum_{i=2}^n\left(\frac{y_i}\sigma\right)^2\sim\chi^2(n-1)$$.

</details>

**Corollaries (the building blocks of pivotal quantities)**
- $$\dfrac{\sqrt n(\bar x-\mu)}{s}\sim t(n-1)$$
- Two independent normal samples: $$\dfrac{s_x^2/\sigma_1^2}{s_y^2/\sigma_2^2}\sim F(m-1,n-1)$$
- If $$\sigma_1=\sigma_2$$: $$\dfrac{(\bar x-\bar y)-(\mu_1-\mu_2)}{s_w\sqrt{\frac1m+\frac1n}}\sim t(m+n-2)$$, where $$s_w^2=\dfrac{(m-1)s_x^2+(n-1)s_y^2}{m+n-2}$$

---

## 4. Sufficient statistics

**Theorem 5.5.1, factorization theorem (P297):** $$T(\boldsymbol x)$$ is sufficient for $$\theta$$ if and only if there exist functions $$g(t;\theta)$$ and $$h(\boldsymbol x)$$ such that

$$p(\boldsymbol x;\theta)=g\big(T(\boldsymbol x);\theta\big)\,h(\boldsymbol x),$$

where $$h$$ does not involve $$\theta$$, and $$g$$ depends on the sample only through $$T$$.

**Sufficient statistics of common distributions (Guide P300)**

| Distribution | Parameter(s) | Sufficient statistic |
|---|---|---|
| $$b(1,p)$$ | $$p$$ | $$\sum x_i$$ |
| $$P(\lambda)$$ | $$\lambda$$ | $$\sum x_i$$ |
| $$Ge(\theta)$$ | $$\theta$$ | $$\sum x_i$$ |
| $$Exp(\lambda)$$ | $$\lambda$$ | $$\sum x_i$$ |
| $$U(0,\theta)$$ | $$\theta$$ | $$x_{(n)}$$ |
| $$U(\theta_1,\theta_2)$$ | $$\theta_1,\theta_2$$ | $$(x_{(1)},x_{(n)})$$ |
| $$U(\theta,2\theta)$$ | $$\theta$$ | $$(x_{(1)},x_{(n)})$$ |
| $$N(\mu,\sigma^2)$$ | $$\mu,\sigma^2$$ | $$\left(\bar x,\ \sum(x_i-\bar x)^2\right)$$ |
| Power distribution $$\theta x^{\theta-1},\ 0<x<1$$ | $$\theta$$ | $$\prod x_i$$ or $$\sum\ln x_i$$ |
| Two-parameter exponential $$\frac1\theta e^{-\frac{x-\mu}\theta},\ x>\mu$$ | $$\mu,\theta$$ | $$\left(x_{(1)},\ \sum x_i\right)$$ |
| $$Ga(\alpha,\lambda)$$ | $$\alpha,\lambda$$ | $$\left(\sum x_i,\ \prod x_i\right)$$ |
| $$LN(\mu,\sigma^2)$$ | $$\mu,\sigma^2$$ | $$\left(\sum\ln x_i,\ \sum(\ln x_i)^2\right)$$ |
| $$Be(a,b)$$ | $$a,b$$ | $$\left(\sum\ln x_i,\ \sum\ln(1-x_i)\right)$$ |

- A one-to-one transformation of a sufficient statistic is still sufficient (Guide P300); e.g. $$\sum x_i$$ and $$\bar x$$ are equivalent.
- The pattern: for an exponential family, the sufficient statistics are the sums of the sample functions that multiply the parameters in the exponent; when the support depends on the parameter (e.g. the uniform), the sufficient statistics are the extremes.

<details markdown="1"><summary>Example: \(U(0,\theta)\)</summary>

$$p(\boldsymbol x;\theta)=\prod_{i=1}^n\frac1\theta I_{\{0<x_i<\theta\}}=\underbrace{\theta^{-n}I_{\{x_{(n)}<\theta\}}}_{g(x_{(n)};\theta)}\cdot\underbrace{I_{\{x_{(1)}>0\}}}_{h(\boldsymbol x)},$$

so $$x_{(n)}$$ is sufficient.

</details>

---

**Series contents**

1. [01 · Core Definitions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-01-concepts %})
2. [02 · Common Distributions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-02-distributions %})
3. [03 · Random Vectors, Covariance and Conditional Expectation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-03-multivariate %})
4. [04 · Characteristic Functions, LLN and CLT]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-04-limit-theorems %})
5. **05 · Sampling Distributions, Order Statistics and Sufficiency** (this post)
6. [06 · Point Estimation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-06-estimation %})
7. [07 · Interval Estimation, Hypothesis Testing and Exercise Results]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-07-intervals-tests %})

[← Previous: Characteristic Functions, LLN and CLT]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-04-limit-theorems %})　·　[Next: Point Estimation →]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-06-estimation %})

*This series collects my review notes on* Probability Theory and Mathematical Statistics *by Mao Shisong, Cheng Yiming and Pu Xiaolong, in 7 parts. P××× refers to page numbers in the book; "Guide" refers to its companion study guide and solutions manual. Corrections are welcome. A Chinese version is on my [Chinese homepage](https://bobothebest.github.io/homepage_cn/).*
