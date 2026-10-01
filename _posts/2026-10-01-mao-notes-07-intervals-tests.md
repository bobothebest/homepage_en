---
layout: post
title: "Probability & Statistics Review 07 | Interval Estimation, Hypothesis Testing and Results from the Exercises"
author: Ziyuan Zhang
tags:
- Probability & Statistics Review
- mathematical statistics
- confidence intervals
- hypothesis testing
- study notes
date: 2026-10-01 10:07 -0400
toc: true
math: true
---
> **Overview**
>
> The last part of the series puts all of interval estimation and hypothesis testing into tables: the one-sample and two-sample normal cases, large-sample methods, and the u, t, χ² and F tests.
>
> A trick for remembering these tables: **intervals and tests use the same pivotal quantity**. First get clear on "under which conditions, which statistic follows which distribution", and you can derive both the interval and the rejection region yourself. At the end there is a summary of handy results derived in the exercises.

---

> Notation: $$u_p$$, $$t_p$$, $$\chi^2_p$$, $$F_p$$ are all **lower** $$p$$-quantiles. The confidence level is $$1-\alpha$$.

---

## 1. Confidence intervals and pivotal quantities (P343–P352, Guide P369)

**Pivotal method:** construct a function $$G$$ (the pivot) that involves only the parameter of interest and whose distribution is known and free of parameters; solve $$P(a\le G\le b)=1-\alpha$$ for the parameter to get the interval.

### One normal population $$N(\mu,\sigma^2)$$

| Parameter | Condition | Pivot | $$1-\alpha$$ confidence interval |
|---|---|---|---|
| $$\mu$$ | $$\sigma$$ known | $$\dfrac{\bar x-\mu}{\sigma/\sqrt n}\sim N(0,1)$$ | $$\bar x\pm u_{1-\alpha/2}\dfrac{\sigma}{\sqrt n}$$ |
| $$\mu$$ | $$\sigma$$ unknown | $$\dfrac{\sqrt n(\bar x-\mu)}{s}\sim t(n-1)$$ | $$\bar x\pm t_{1-\alpha/2}(n-1)\dfrac{s}{\sqrt n}$$ |
| $$\sigma^2$$ | $$\mu$$ unknown | $$\dfrac{(n-1)s^2}{\sigma^2}\sim\chi^2(n-1)$$ | $$\left[\dfrac{(n-1)s^2}{\chi^2_{1-\alpha/2}(n-1)},\ \dfrac{(n-1)s^2}{\chi^2_{\alpha/2}(n-1)}\right]$$ |

### Two normal populations $$X\sim N(\mu_1,\sigma_1^2)$$ (sample size $$m$$), $$Y\sim N(\mu_2,\sigma_2^2)$$ (sample size $$n$$)

**Confidence intervals for $$\mu_1-\mu_2$$**

| Case | Interval |
|---|---|
| ① $$\sigma_1^2,\sigma_2^2$$ known | $$\bar x-\bar y\pm u_{1-\alpha/2}\sqrt{\dfrac{\sigma_1^2}{m}+\dfrac{\sigma_2^2}{n}}$$ |
| ② $$\sigma_1^2=\sigma_2^2$$ unknown | $$\bar x-\bar y\pm\sqrt{\dfrac{m+n}{mn}}\,s_w\,t_{1-\alpha/2}(m+n-2)$$, $$s_w^2=\dfrac{(m-1)s_x^2+(n-1)s_y^2}{m+n-2}$$ |
| ③ $$\sigma_1^2/\sigma_2^2=\theta$$ known | $$\bar x-\bar y\pm\sqrt{\dfrac{m\theta+n}{mn}}\,s_t\,t_{1-\alpha/2}(m+n-2)$$, $$s_t^2=\dfrac{(m-1)s_x^2+(n-1)s_y^2/\theta}{m+n-2}$$ |
| ④ $$m,n$$ both large (approximate) | $$\bar x-\bar y\pm u_{1-\alpha/2}\sqrt{\dfrac{s_x^2}{m}+\dfrac{s_y^2}{n}}$$ |
| ⑤ General case, small samples (approximate t, Welch) | $$\bar x-\bar y\pm s_0\,t_{1-\alpha/2}(l)$$, $$s_0^2=\dfrac{s_x^2}m+\dfrac{s_y^2}n$$, $$l=\dfrac{s_0^4}{\frac{s_x^4}{m^2(m-1)}+\frac{s_y^4}{n^2(n-1)}}$$ (round to the nearest integer) |

**Confidence interval for $$\sigma_1^2/\sigma_2^2$$** ($$\mu_1,\mu_2$$ unknown)

$$F=\frac{s_x^2/\sigma_1^2}{s_y^2/\sigma_2^2}\sim F(m-1,n-1),\qquad\left[\frac{s_x^2}{s_y^2}\cdot\frac{1}{F_{1-\alpha/2}(m-1,n-1)},\ \frac{s_x^2}{s_y^2}\cdot\frac{1}{F_{\alpha/2}(m-1,n-1)}\right].$$

### Large-sample confidence intervals (know the derivation)

For $$X\sim b(1,p)$$, the CLT gives $$\dfrac{\bar x-p}{\sqrt{p(1-p)/n}}\ \dot\sim\ N(0,1)$$. Replacing $$p$$ in the denominator by $$\bar x$$ gives

$$\bar x\pm u_{1-\alpha/2}\sqrt{\frac{\bar x(1-\bar x)}{n}}.$$

<details markdown="1"><summary>A more accurate version (solve the quadratic instead of plugging in \(\bar x\))</summary>

From $$\left\vert \bar x-p\right\vert \le u_{1-\alpha/2}\sqrt{p(1-p)/n}$$, squaring both sides gives a quadratic inequality in $$p$$:

$$\left(1+\frac{u^2}n\right)p^2-\left(2\bar x+\frac{u^2}n\right)p+\bar x^2\le0,\qquad u=u_{1-\alpha/2}.$$

The interval between the two roots is the confidence interval for $$p$$. For large $$n$$ it is almost identical to the simplified interval above.

</details>

---

## 2. Hypothesis tests at a glance

**Basic steps of a test (P357):** see Part 1.  
**p-value:** the probability, assuming $$H_0$$ is true, of getting a result at least as extreme as the one observed. Reject $$H_0$$ when the p-value is $$\le\alpha$$.

### Tests for the mean $$\mu$$ of one normal population (P369)

| Test | $$H_0$$ | $$H_1$$ | Test statistic | Rejection region $$W$$ | p-value |
|---|---|---|---|---|---|
| u test ($$\sigma$$ known) | $$\mu\le\mu_0$$ | $$\mu>\mu_0$$ | $$u=\dfrac{\bar x-\mu_0}{\sigma/\sqrt n}$$ | $$u\ge u_{1-\alpha}$$ | $$1-\Phi(u_0)$$ |
| | $$\mu\ge\mu_0$$ | $$\mu<\mu_0$$ | | $$u\le u_\alpha$$ | $$\Phi(u_0)$$ |
| | $$\mu=\mu_0$$ | $$\mu\ne\mu_0$$ | | $$\lvert u\rvert\ge u_{1-\alpha/2}$$ | $$2[1-\Phi(\lvert u_0\rvert)]$$ |
| t test ($$\sigma$$ unknown) | $$\mu\le\mu_0$$ | $$\mu>\mu_0$$ | $$t=\dfrac{\bar x-\mu_0}{s/\sqrt n}$$ | $$t\ge t_{1-\alpha}(n-1)$$ | $$P(T\ge t_0)$$ |
| | $$\mu\ge\mu_0$$ | $$\mu<\mu_0$$ | | $$t\le t_\alpha(n-1)$$ | $$P(T\le t_0)$$ |
| | $$\mu=\mu_0$$ | $$\mu\ne\mu_0$$ | | $$\lvert t\rvert\ge t_{1-\alpha/2}(n-1)$$ | $$2P(T\ge\lvert t_0\rvert)$$ |

($$u_0$$, $$t_0$$ are the observed values of the statistics; $$T\sim t(n-1)$$.)

### Tests for the difference of two normal means (P373)

The null is always written $$H_0:\mu_1-\mu_2=0$$ (or $$\le0$$, $$\ge0$$); the direction of the rejection region follows the table above.

| Test | Condition | Test statistic | Distribution under $$H_0$$ |
|---|---|---|---|
| Two-sample u test | $$\sigma_1,\sigma_2$$ known | $$u=\dfrac{\bar x-\bar y}{\sqrt{\sigma_1^2/m+\sigma_2^2/n}}$$ | $$N(0,1)$$ |
| Two-sample t test | $$\sigma_1=\sigma_2=\sigma$$ unknown | $$t=\dfrac{\bar x-\bar y}{s_w\sqrt{\frac1m+\frac1n}}$$ | $$t(m+n-2)$$ |
| Large-sample u test | $$m,n$$ large enough | $$u=\dfrac{\bar x-\bar y}{\sqrt{s_x^2/m+s_y^2/n}}$$ | approx. $$N(0,1)$$ |
| Approximate t test | $$m,n$$ not very large, variances unknown and unequal | $$t=\dfrac{\bar x-\bar y}{\sqrt{s_x^2/m+s_y^2/n}}$$ | approx. $$t(l)$$, $$l$$ as above |

### Tests for variances

| Test | $$H_0$$ | Test statistic | Rejection region (two-sided / right / left) |
|---|---|---|---|
| $$\chi^2$$ test | $$\sigma^2=\sigma_0^2$$ | $$\chi^2=\dfrac{(n-1)s^2}{\sigma_0^2}\sim\chi^2(n-1)$$ | $$\chi^2\le\chi^2_{\alpha/2}$$ or $$\chi^2\ge\chi^2_{1-\alpha/2}$$ ／ $$\chi^2\ge\chi^2_{1-\alpha}$$ ／ $$\chi^2\le\chi^2_\alpha$$ |
| F test | $$\sigma_1^2=\sigma_2^2$$ | $$F=\dfrac{s_x^2}{s_y^2}\sim F(m-1,n-1)$$ | $$F\le F_{\alpha/2}$$ or $$F\ge F_{1-\alpha/2}$$ ／ $$F\ge F_{1-\alpha}$$ ／ $$F\le F_\alpha$$ |

**Duality between intervals and tests:** the two-sided test of $$\mu=\mu_0$$ at level $$\alpha$$ accepts if and only if $$\mu_0$$ lies inside the $$1-\alpha$$ confidence interval.

**Power function (P360):** e.g. for the u test of $$H_0:\mu\le\mu_0$$ vs $$H_1:\mu>\mu_0$$,

$$g(\mu)=P_\mu(u\ge u_{1-\alpha})=1-\Phi\left(u_{1-\alpha}-\frac{\mu-\mu_0}{\sigma/\sqrt n}\right),$$

which is increasing in $$\mu$$ and equals $$\alpha$$ at $$\mu=\mu_0$$.

---

## 3. Results derived in the exercises

| Exercise | Result |
|---|---|
| 1.2.1(3) | $$\binom n0+\binom n1+\cdots+\binom nn=2^n$$ |
| 1.2.1(4) | $$\binom n1+2\binom n2+\cdots+n\binom nn=n2^{n-1}$$ |
| 1.2.1(5) | Vandermonde's identity: $$\sum_{k=0}^{n}\binom ak\binom b{n-k}=\binom{a+b}n$$, $$n\le\min\{a,b\}$$ |
| 1.3.2 | Probabilities don't determine events: $$P(AB)=0\not\Rightarrow AB=\varnothing$$ (e.g. a continuous random variable hits any single point with probability 0, yet that event is not impossible) |
| 2.2.20 | Continuous case: $$E(X)=\int_0^{\infty}[1-F(x)]dx-\int_{-\infty}^0F(x)dx$$ |
| 2.2.21 | Non-negative continuous case: $$E(X)=\int_0^{\infty}P(X>x)dx$$, $$E(X^n)=\int_0^{\infty}nx^{n-1}P(X>x)dx$$ |
| 2.3.8 | Use the two results above fluently; $$\int_0^{\infty}x^2e^{-x^2}dx=\frac{\sqrt\pi}4$$ |
| 2.3.9 | $$E(X-EX)^2\le E(X-c)^2$$ (minimized at the mean); $$E\lvert X-m\rvert\le E\lvert X-c\rvert$$ (minimized at the median) |
| 2.3.12 | $$g$$ non-negative, non-decreasing, $$E g(X)$$ exists $$\Rightarrow P(X>\varepsilon)\le\frac{E g(X)}{g(\varepsilon)}$$ |
| — | Bounded $$X\in[a,b]$$: $$a\le EX\le b$$, $$\mathrm{Var}X\le\left(\frac{b-a}2\right)^2$$ |
| 2.7.3(3) | The median of $$LN(\mu,\sigma^2)$$ is $$e^\mu$$ |
| 2.7.4 | $$Ga(\alpha,\lambda)$$: $$E(X^k)=\frac{\Gamma(\alpha+k)}{\Gamma(\alpha)\lambda^k}$$ |
| 2.7.5 | $$Exp(\lambda)$$: $$E(X^k)=\frac{k!}{\lambda^k}$$, $$C_v=1$$, $$\beta_s=2$$, $$\beta_k=6$$ |
| 2.7.11 | The $$p$$-quantile of $$LN(\mu,\sigma^2)$$ is $$e^{\mu+\sigma u_p}$$ |
| 3.3.12 | $$\min\{a,b\}=\frac12[a+b-\lvert b-a\rvert]$$, $$\max\{a,b\}=\frac12[a+b+\lvert b-a\rvert]$$ |
| 3.4.14 | $$X,Y\overset{iid}{\sim}N(0,1)$$: $$E\max\{X,Y\}=\frac1{\sqrt\pi}$$ |
| 3.4.18 | $$X,Y\overset{iid}{\sim}N(a,\sigma^2)$$: $$E\max\{X,Y\}=a+\frac\sigma{\sqrt\pi}$$ |
| Matching problem | Gift exchange: $$E(S_n)=1$$, $$\mathrm{Var}(S_n)=1$$ |
| 4.1.18 | i.i.d., $$EX=0$$, $$\mathrm{Var}X=\sigma^2$$ $$\Rightarrow\frac1n\sum X_k^2\xrightarrow{P}\sigma^2$$ |
| 4.3.4 | Only neighbors correlated: $$\mathrm{Var}(\sum X_i)=\sum\mathrm{Var}(X_i)+2\sum_{i=1}^{n-1}\mathrm{Cov}(X_i,X_{i+1})$$ |
| 5.3.4 | $$s_{n+1}^2=\frac{n-1}ns_n^2+\frac1{n+1}(x_{n+1}-\bar x_n)^2$$ |
| 5.3.10 | $$(\sum x_i)^2=\sum x_i^2+2\sum_{i<j}x_ix_j$$ |
| 5.3.22–23 | Applications of Theorem 5.3.6 (joint density of two order statistics) |
| 5.3.28 | $$\eta_i=F(x_{(i)})$$ are $$U(0,1)$$ order statistics; $$E\eta_i=\frac i{n+1}$$, $$\mathrm{Var}\eta_i=\frac{i(n+1-i)}{(n+1)^2(n+2)}$$ |
| 5.3.34 | The joint density of all order statistics of $$U(0,\theta)$$ is $$\frac{n!}{\theta^n}$$, $$0<u_1\le\dots\le u_n\le\theta$$ |
| 6.3.2 | The MLE may not be unique |
| 6.4.5 | $$I(\theta)=-E\left[\frac{\partial^2}{\partial\theta^2}\ln p(x;\theta)\right]$$ |

---

**Series contents**

1. [01 | Core Definitions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-01-concepts %})
2. [02 | Common Distributions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-02-distributions %})
3. [03 | Random Vectors, Covariance and Conditional Expectation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-03-multivariate %})
4. [04 | Characteristic Functions, LLN and CLT]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-04-limit-theorems %})
5. [05 | Sampling Distributions, Order Statistics and Sufficiency]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-05-sampling %})
6. [06 | Point Estimation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-06-estimation %})
7. **07 | Interval Estimation, Hypothesis Testing and Exercise Results** (this post)

**That's the end of the series** — thanks for reading all the way here.　·　[← Previous: Point Estimation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-06-estimation %})

*This series collects my review notes on* Probability Theory and Mathematical Statistics *by Mao Shisong, Cheng Yiming and Pu Xiaolong, in 7 parts. P××× refers to page numbers in the book; "Guide" refers to its companion study guide and solutions manual. Corrections are welcome. A Chinese version is on my [Chinese homepage](https://bobothebest.github.io/homepage_cn/).*
