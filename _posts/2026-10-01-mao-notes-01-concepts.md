---
layout: post
title: "Probability & Statistics Review 01 | Core Definitions"
author: Ziyuan Zhang
tags:
- Probability & Statistics Review
- probability
- mathematical statistics
- study notes
date: 2026-10-01 10:01 -0400
toc: true
math: true
---
> **About this series.** These are my review notes on *Probability Theory and Mathematical Statistics* by Mao Shisong, Cheng Yiming and Pu Xiaolong (茆诗松《概率论与数理统计教程》, "the Mao book" below), a standard Chinese textbook for graduate entrance exams and PhD qualifiers. There are 7 parts.  
> `P×××` = page in the textbook; "Guide" = its companion study guide and solutions manual.
>
> **How to use this part.** Short-answer questions often ask you to "state the definition of …". Each concept below gives an **exam-ready definition** you can write down directly, plus a one-line intuition.

---

## 1. Random phenomena, sample spaces and events

**Random phenomenon (P1)**  
A phenomenon that, under fixed conditions, does not always produce the same outcome.  
Intuition: there is more than one possible outcome, and you don't know in advance which one will occur.

**Sample space (P2)**  
The set of all possible basic outcomes of a random phenomenon, written $$\Omega=\{\omega\}$$; each element $$\omega$$ is called a sample point.

**Random event (P2)**  
A set of sample points, i.e. a subset of $$\Omega$$, usually denoted $$A,B,C$$.
- Elementary event: an event containing exactly one sample point
- Sure event: $$\Omega$$
- Impossible event: $$\varnothing$$

**Random variable (P3, P62)**  
A real-valued function $$X=X(\omega)$$ defined on the sample space $$\Omega$$.  
Intuition: it translates "outcomes" into "numbers", so that calculus can take over.

**Event field / σ-algebra (P9)**  
Let $$\Omega$$ be a sample space and $$\mathscr F$$ a class of subsets of $$\Omega$$. If $$\mathscr F$$ satisfies
1. $$\Omega\in\mathscr F$$;
2. if $$A\in\mathscr F$$, then $$\bar A\in\mathscr F$$ (closed under complement);
3. if $$A_n\in\mathscr F,\ n=1,2,\dots$$, then $$\bigcup_{n=1}^{\infty}A_n\in\mathscr F$$ (closed under countable union),

then $$\mathscr F$$ is called an event field, or a $$\sigma$$-algebra, and $$(\Omega,\mathscr F)$$ is called a measurable space.  
Intuition: the event field is the list of events we are "allowed to talk about probabilities of", and the list is closed under complements and countable unions.

> **Fact.** If the sample space has $$n$$ sample points, there are $$2^n$$ events in total (P9) — the number of all subsets.

---

## 2. Probability and how to assign it

**Axiomatic definition of probability (P13)**  
Let $$\Omega$$ be a sample space and $$\mathscr F$$ an event field. If a real-valued function $$P(A)$$ defined on $$\mathscr F$$ satisfies, for every $$A\in\mathscr F$$:
1. **Non-negativity:** $$P(A)\ge0$$;
2. **Normalization:** $$P(\Omega)=1$$;
3. **Countable additivity:** if $$A_1,A_2,\dots$$ are mutually exclusive, then $$P\left(\bigcup_{i=1}^{\infty}A_i\right)=\sum_{i=1}^{\infty}P(A_i)$$,

then $$P(A)$$ is called the probability of $$A$$, and $$(\Omega,\mathscr F,P)$$ is called a probability space.

**Four ways to assign probabilities**

| Method | Page | Core idea | When to use |
|---|---|---|---|
| Frequency | P15 | Over many repetitions, the relative frequency $$f_n(A)=n_A/n$$ stabilizes around a constant; take that constant as the probability | Experiments that can be repeated many times |
| Classical | P17 | Finitely many, equally likely sample points: $$P(A)=\dfrac{\#\text{ points in }A}{\#\text{ points in }\Omega}=\dfrac kn$$ | Finite, equally likely |
| Geometric | P25 | The sample space is a measurable region and outcomes are equally likely: $$P(A)=\dfrac{S_A}{S_\Omega}$$ (length / area / volume) | Infinite, equally likely |
| Subjective | P29 | A personal degree of belief based on experience and available information | One-off events that cannot be repeated |

**Combinations with repetition (P14)**  
The number of ways to choose $$r$$ items from $$n$$ distinct items **with replacement and ignoring order** is $$\binom{n+r-1}{r}$$.

<details markdown="1"><summary>Derivation sketch</summary>

Think of one selection as "putting $$r$$ balls into $$n$$ boxes": use $$n-1$$ dividers to split the $$r$$ balls into $$n$$ segments; the number of balls in segment $$i$$ is how many times item $$i$$ is chosen. The problem becomes choosing $$r$$ of the $$r+(n-1)$$ positions for the balls, giving $$\binom{n+r-1}{r}$$ ways (stars and bars).

</details>

**Random simulation / Monte Carlo method (P27, P231, P236)**  
Use computer-generated random numbers to simulate a random experiment, and estimate a probability by a relative frequency, or an integral by a sample mean. The theoretical justification is the law of large numbers.
- Classic example: Buffon's needle to estimate $$\pi$$ (P27)
- Computing $$\int_0^1 f(x)\,dx$$: take $$U_i\overset{iid}{\sim}U(0,1)$$ and estimate it by $$\frac1n\sum f(U_i)$$ (P231)

---

## 3. Independence

**Two independent events (P33)**  
$$A$$ and $$B$$ are independent if $$P(AB)=P(A)P(B)$$.

**Mutual independence of several events, with an example (P34)**  
Let $$A_1,\dots,A_n$$ be $$n$$ events. If for all $$1\le i<j<k<\dots\le n$$ the following $$2^n-n-1$$ equations all hold:

$$
\begin{aligned}
P(A_iA_j)&=P(A_i)P(A_j),\\
P(A_iA_jA_k)&=P(A_i)P(A_j)P(A_k),\\
&\ \ \vdots\\
P(A_1A_2\cdots A_n)&=P(A_1)P(A_2)\cdots P(A_n),
\end{aligned}
$$

then $$A_1,\dots,A_n$$ are mutually independent.

**Counterexample: pairwise independent $$\not\Rightarrow$$ mutually independent** (Bernstein's tetrahedron)  
Take a fair regular tetrahedron. Paint one face red, one white, one black, and the fourth face with all three colors. Toss it once, and let $$A,B,C$$ be "the bottom face shows red / white / black".  
Then $$P(A)=P(B)=P(C)=\tfrac12$$ and $$P(AB)=P(BC)=P(AC)=\tfrac14$$, so they are pairwise independent;  
but $$P(ABC)=\tfrac14\ne\tfrac18=P(A)P(B)P(C)$$, so they are not mutually independent.

---

## 4. Distribution functions and numerical characteristics

**Properties of a distribution function (P64)**  
$$F(x)=P(X\le x)$$ has three basic properties:
1. **Monotonicity:** $$F(x)$$ is non-decreasing;
2. **Boundedness:** $$0\le F(x)\le1$$, with $$F(-\infty)=0$$ and $$F(+\infty)=1$$;
3. **Right-continuity:** $$F(x+0)=F(x)$$.

Conversely, any function with these three properties is the distribution function of some random variable, so the three properties are necessary and sufficient.

**Generating exponential random numbers (P126)**  
If $$U\sim U(0,1)$$, then $$X=-\frac1\lambda\ln(1-U)\sim Exp(\lambda)$$. Since $$1-U$$ is also $$U(0,1)$$, you can simply use $$X=-\frac1\lambda\ln U$$.  
This rests on $$F_X(X)\sim U(0,1)$$ (see Part 2) — the "inverse-transform method".

**Moments of order k (P129)**
- $$k$$-th raw moment: $$\mu_k=E(X^k)$$
- $$k$$-th central moment: $$\nu_k=E\big(X-E(X)\big)^k$$

**Coefficient of variation (P131)**  
$$C_v(X)=\dfrac{\sqrt{\mathrm{Var}(X)}}{E(X)}$$. It is unit-free, so it can compare the spread of random variables measured in different units or with very different means.

**Quantile (P132 → P276)**  
For $$0<p<1$$, if $$x_p$$ satisfies $$F(x_p)=\int_{-\infty}^{x_p}p(x)\,dx=p$$, then $$x_p$$ is called the $$p$$-quantile (lower quantile) of the distribution.

**Median (P133)**  
The quantile $$x_{0.5}$$ at $$p=0.5$$. Compared with the mean, the median is not affected by extreme values, so it is more robust.

**Skewness (P134)**  
$$\beta_s=\dfrac{\nu_3}{\nu_2^{3/2}}$$ describes symmetry: $$\beta_s>0$$ means right-skewed (positively skewed, long tail on the right), $$\beta_s<0$$ means left-skewed, and $$\beta_s=0$$ is (a necessary condition for) symmetry about the mean.

**Kurtosis (P135)**  
$$\beta_k=\dfrac{\nu_4}{\nu_2^2}-3$$, benchmarked against the normal distribution (for which $$\beta_k=0$$). $$\beta_k>0$$ means more peaked than the normal (heavier tails); $$\beta_k<0$$ means flatter.

---

## 5. Random vectors

**Joint distribution function (P140)**  
$$F(x,y)=P(X\le x,\ Y\le y)$$.

**Properties of a joint distribution function (P140)**
1. Monotonicity: non-decreasing in $$x$$ and in $$y$$ separately;
2. Boundedness: $$0\le F\le1$$, $$F(-\infty,y)=F(x,-\infty)=0$$, $$F(+\infty,+\infty)=1$$;
3. Right-continuity: right-continuous in $$x$$ and in $$y$$ separately;
4. Non-negativity: for any $$a<b$$, $$c<d$$, $$F(b,d)-F(a,d)-F(b,c)+F(a,c)\ge0$$.

**Counterexample (P141):** $$G(x,y)=\begin{cases}1,&x+y\ge0\\0,&x+y<0\end{cases}$$ satisfies the first three properties, but on the rectangle $$(-1,1]\times(-1,1]$$ the fourth gives $$1-1-1+0=-1<0$$, so it is not a distribution function. In two dimensions you must **check non-negativity separately**.

**Covariance (P178)**  
$$\mathrm{Cov}(X,Y)=E\big[(X-E X)(Y-E Y)\big]=E(XY)-E(X)E(Y)$$.

**Correlation coefficient (P182)**  
$$\mathrm{Corr}(X,Y)=\dfrac{\mathrm{Cov}(X,Y)}{\sqrt{\mathrm{Var}(X)}\sqrt{\mathrm{Var}(Y)}}$$. It measures the strength of the **linear** relationship between $$X$$ and $$Y$$.

---

## 6. Limit theory

**Convergence in probability (P209)**  
If for every $$\varepsilon>0$$, $$\lim_{n\to\infty}P(\vert Y_n-Y\vert \ge\varepsilon)=0$$, then $$Y_n$$ converges in probability to $$Y$$, written $$Y_n\xrightarrow{P}Y$$.

**Convergence in distribution (P211)**  
If $$\lim_{n\to\infty}F_n(x)=F(x)$$ at every continuity point of $$F(x)$$, then $$F_n$$ converges weakly to $$F$$, and correspondingly $$X_n$$ converges in distribution to $$X$$, written $$X_n\xrightarrow{L}X$$.

**Characteristic function (P216)**  
$$\varphi(t)=E(e^{itX})$$, $$-\infty<t<\infty$$. It exists for every random variable (because $$\vert e^{itX}\vert =1$$) and is in one-to-one correspondence with the distribution (see Part 4).

**Generating normal random numbers (P241)**  
By the central limit theorem, with 12 independent $$U(0,1)$$ random numbers $$U_1,\dots,U_{12}$$, $$\sum_{i=1}^{12}U_i-6$$ is approximately $$N(0,1)$$ (mean $$12\times\frac12=6$$, variance $$12\times\frac1{12}=1$$).  
(Aside: an exact method is the Box–Muller transform $$Z=\sqrt{-2\ln U_1}\cos(2\pi U_2)$$.)

---

## 7. Basic statistical concepts

**Population (P253):** the entire collection of objects under study.

**Sample, sample size, sample unit (P254):** the individuals drawn from the population form the sample; the number $$n$$ of individuals is the sample size; each individual in the sample is a sample unit. A simple random sample must be **independent and identically distributed**.

**Glivenko–Cantelli theorem (P258)**  
Let $$x_1,\dots,x_n$$ be a sample from a population with distribution function $$F(x)$$, and $$F_n(x)$$ the empirical distribution function. Then

$$P\left(\lim_{n\to\infty}\sup_{-\infty<x<\infty}\vert F_n(x)-F(x)\vert =0\right)=1.$$

Intuition: with a large enough sample, the empirical distribution function uniformly hugs the true distribution. This is the theoretical basis for "using a sample to infer the population".

**Statistic (P263)**  
A function of the sample that contains no unknown parameters.

**Order statistics (P271)**  
Sorting the sample in increasing order gives $$x_{(1)}\le x_{(2)}\le\dots\le x_{(n)}$$; $$x_{(k)}$$ is called the $$k$$-th order statistic.

**Five-number summary (P277)**  
$$x_{(1)},\ Q_1,\ m_{0.5},\ Q_3,\ x_{(n)}$$: minimum, lower quartile, median, upper quartile, maximum. A box plot is drawn from the five-number summary.

**Sufficient statistic (P286)**  
Let $$x_1,\dots,x_n$$ be a sample from the family $$\{F_\theta\}$$. If, given $$T=t$$, the conditional distribution of the sample does not depend on $$\theta$$, then $$T$$ is a sufficient statistic for $$\theta$$.  
Intuition: $$T$$ has "packed up and taken away" all of the information about $$\theta$$ in the sample; what is left over has nothing to do with $$\theta$$.

---

## 8. Parameter estimation

**Estimation (P302):** constructing a statistic $$\hat\theta=\hat\theta(x_1,\dots,x_n)$$ from the sample to estimate an unknown parameter $$\theta$$ is called point estimation.

**Unbiased estimator (P303):** if $$E_\theta(\hat\theta)=\theta$$ for every $$\theta\in\Theta$$, then $$\hat\theta$$ is an unbiased estimator of $$\theta$$.

**Asymptotically unbiased estimator (P303):** $$\lim_{n\to\infty}E(\hat\theta_n)=\theta$$. Example: $$s_n^2=\frac1n\sum(x_i-\bar x)^2$$ is an asymptotically unbiased estimator of $$\sigma^2$$.

**Estimable vs. non-estimable (P305):** a parameter is estimable if it has an unbiased estimator, and non-estimable otherwise. Example: for $$b(1,p)$$, $$\frac1p$$ is not estimable.

**Efficiency (P306):** let $$\hat\theta_1,\hat\theta_2$$ both be unbiased for $$\theta$$. If $$\mathrm{Var}(\hat\theta_1)\le\mathrm{Var}(\hat\theta_2)$$ for all $$\theta$$, with strict inequality for at least one $$\theta$$, then $$\hat\theta_1$$ is more efficient than $$\hat\theta_2$$.

**Consistency (P309):** if $$\hat\theta_n\xrightarrow{P}\theta$$ for every $$\theta$$, then $$\hat\theta_n$$ is a consistent estimator of $$\theta$$. Consistency is the **most basic** requirement for an estimator.

**Likelihood function and maximum likelihood estimator (P314)**  
The likelihood function is $$L(\theta)=L(\theta;x_1,\dots,x_n)=\prod_{i=1}^n p(x_i;\theta)$$. If a statistic $$\hat\theta$$ satisfies $$L(\hat\theta)=\max_{\theta\in\Theta}L(\theta)$$, it is called the maximum likelihood estimator (MLE) of $$\theta$$.  
Intuition: among all possible parameters, pick the one that "best explains the data we actually observed".

**Mean squared error (P323)**  
$$\mathrm{MSE}(\hat\theta)=E(\hat\theta-\theta)^2=\mathrm{Var}(\hat\theta)+\big(E\hat\theta-\theta\big)^2$$, i.e. variance plus squared bias.

**Uniformly minimum variance unbiased estimator, UMVUE (P324)**  
Let $$\hat\theta$$ be unbiased for $$\theta$$. If for every unbiased estimator $$\tilde\theta$$ of $$\theta$$ we have $$\mathrm{Var}(\hat\theta)\le\mathrm{Var}(\tilde\theta)$$ for all $$\theta\in\Theta$$, then $$\hat\theta$$ is the UMVUE of $$\theta$$.

---

## 9. Hypothesis testing

**Basic steps of a hypothesis test (P357)**
1. State the null hypothesis $$H_0$$ and the alternative $$H_1$$;
2. Choose a test statistic and the form of the rejection region;
3. Choose the significance level $$\alpha$$;
4. Determine the critical value from $$P(\text{reject }H_0\mid H_0\text{ true})\le\alpha$$, giving the rejection region $$W$$;
5. Make a decision from the observed sample (or from the p-value).

**Power function (P360)**  
If the rejection region of a test is $$W$$, the probability that the observed sample falls in the rejection region,

$$g(\theta)=P_\theta(X\in W),\quad \theta\in\Theta=\Theta_0\cup\Theta_1,$$

is called the power function of the test. For $$\theta\in\Theta_0$$, $$g(\theta)=\alpha(\theta)$$ is the probability of a type I error; for $$\theta\in\Theta_1$$, $$g(\theta)=1-\beta(\theta)$$, where $$\beta(\theta)$$ is the probability of a type II error.

---

**Series contents**

1. **01 | Core Definitions** (this post)
2. [02 | Common Distributions]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-02-distributions %})
3. [03 | Random Vectors, Covariance and Conditional Expectation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-03-multivariate %})
4. [04 | Characteristic Functions, LLN and CLT]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-04-limit-theorems %})
5. [05 | Sampling Distributions, Order Statistics and Sufficiency]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-05-sampling %})
6. [06 | Point Estimation]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-06-estimation %})
7. [07 | Interval Estimation, Hypothesis Testing and Exercise Results]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-07-intervals-tests %})

[Next: Common Distributions →]({{ site.baseurl }}{% post_url 2026-10-01-mao-notes-02-distributions %})

*This series collects my review notes on* Probability Theory and Mathematical Statistics *by Mao Shisong, Cheng Yiming and Pu Xiaolong, in 7 parts. P××× refers to page numbers in the book; "Guide" refers to its companion study guide and solutions manual. Corrections are welcome. A Chinese version is on my [Chinese homepage](https://bobothebest.github.io/homepage_cn/).*
