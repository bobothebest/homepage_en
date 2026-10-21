---
layout: post
title: "Stochastic Calculus Notes 03 | Stochastic Integration"
author: Ziyuan Zhang
tags:
- Stochastic Calculus Notes
- stochastic calculus
- study notes
date: 2026-10-21 10:03 -0400
toc: true
math: true
---
> **Overview**
>
> A stochastic integral is just **cumulative trading P/L**: hold $$\Delta$$ shares while the price moves by $$dW$$, and add it up. This post builds $$\int_0^t\Delta_s\,dW_s$$ in three steps — discrete time, "buy-and-hold" (simple) strategies, and general strategies by approximation — and shows the same five properties survive every step:
>
> **linear · adapted · martingale · Itô isometry $$\mathrm{Var}[I_t]=E\int_0^t\Delta_s^2ds$$ · $$[I,I]_t=\int_0^t\Delta_s^2ds$$.**

---

## 1. Discrete time: the integral as trading P/L

Let $$M=\{M_n\}$$ be a martingale (think: a share price). For a trading strategy $$\Delta=\{\Delta_n\}$$ define

$$I_0=0,\qquad I_n=\sum_{j=1}^n\underbrace{\Delta_j}_{\text{shares held}}\ \underbrace{dM_j}_{\text{price change}},\qquad dM_j=M_j-M_{j-1}.$$

$$\Delta_j\,dM_j$$ is the P/L over $$[j-1,j]$$ and $$I_n$$ the cumulative P/L over $$[0,n]$$.

**Predictable strategies.** We hold $$\Delta_j$$ over $$[j-1,j]$$, and we cannot look into the future, so $$\Delta_j$$ must be known at time $$j-1$$: $$\Delta_j$$ is $$\mathcal F_{j-1}$$-measurable. Such a $$\Delta=\{\Delta_n\}_{n\ge1}$$ is called **predictable** ($$\Delta_0$$ is not defined since $$\mathcal F_{-1}$$ does not exist).

### Properties of $$I=\sum\Delta\,dM$$ ($$\Delta$$ predictable, $$M$$ a martingale)

1. **Linear in $$\Delta$$:** $$I(\alpha\Delta+\beta\psi)=\alpha I(\Delta)+\beta I(\psi)$$ — just split the sum.
2. **Adapted:** $$\Delta_1,\dots,\Delta_n$$ are known by time $$n-1$$ and $$M_0,\dots,M_n$$ by time $$n$$, so $$I_n$$ is $$\mathcal F_n$$-measurable.
3. **Martingale:** $$I_{n+1}-I_n=\Delta_{n+1}(M_{n+1}-M_n)$$ and $$\Delta_{n+1}$$ is $$\mathcal F_n$$-measurable, so

   $$E[I_{n+1}-I_n\mid\mathcal F_n]=\Delta_{n+1}\,E[M_{n+1}-M_n\mid\mathcal F_n]=0.$$

4. **Quadratic variation:** $$[I,I]_n=\sum_j\Delta_j^2(dM_j)^2=\sum_j\Delta_j^2\,d[M,M]_j$$.
5. **Variance:** since $$I$$ is a martingale with $$I_0=0$$, Fact 1 of Part 02 gives

   $$\mathrm{Var}[I_n]=E\big[[I,I]_n\big]=E\Big[\sum_{j=1}^n\Delta_j^2\,d[M,M]_j\Big].$$

For the random walk, $$d[M,M]_j=1$$, so $$[I,I]_n=\sum_j\Delta_j^2$$ and $$\mathrm{Var}[I_n]=E\sum_j\Delta_j^2$$.

Intuition for "martingale": **you cannot beat a fair game by choosing your bet size**, as long as you choose it before the coin is tossed.

## 2. Continuous time: why the obvious definition fails

We want $$I_t=\int_0^t\Delta_s\,dW_s$$ ($$\Delta$$: integrand, $$W$$: integrator; same P/L reading). The natural guess is a Riemann sum,

$$\int_0^t\Delta_s\,dW_s\approx\sum_{j=1}^n\Delta_{t_{j-1}}\big(W_{t_j}-W_{t_{j-1}}\big).$$

If $$W$$ were smooth, the mean value theorem would turn this into $$\int_0^t\Delta_s\dot W_s\,ds$$. But $$W$$ is nowhere differentiable (Part 02), so $$\dot W_s$$ does not exist and **path-by-path** the limit does not exist in the ordinary sense. We need a new kind of limit.

## 3. Simple (buy-and-hold) integrands

First bring things back to almost discrete time. Fix $$0=t_0<t_1<\dots<t_n$$ and $$\mathcal F_{t_j}$$-measurable r.v.'s $$\Delta_j$$, and set

$$\Delta_t=\Delta_0\mathbf 1_{\{t=0\}}+\sum_{j=1}^n\Delta_{j-1}\mathbf 1_{\{t\in(t_{j-1},t_j]\}}:$$

hold $$\Delta_{j-1}$$ shares over $$(t_{j-1},t_j]$$. For such a **simple integrand** the integral is explicit:

$$I_t=\sum_{j=1}^n\underbrace{\Delta_{j-1}}_{\text{position}}\ \underbrace{\big(W_{t\wedge t_j}-W_{t\wedge t_{j-1}}\big)}_{\text{price change}},\qquad t\wedge s=\min\{t,s\}.$$

Reading it piece by piece: for $$t_{j-1}\le t\le t_j$$,

$$I_t=\underbrace{\sum_{i=1}^{j-1}\Delta_{i-1}\big(W_{t_i}-W_{t_{i-1}}\big)}_{\text{P/L over }[0,t_{j-1}]}+\underbrace{\Delta_{j-1}\big(W_t-W_{t_{j-1}}\big)}_{\text{P/L over }[t_{j-1},t]},$$

and for $$t\ge t_n$$ nothing changes any more — we stopped trading.

### Properties for simple $$\Delta$$

- **Linear** and **adapted**: as in discrete time.
- **Continuous paths** (new): each term $$t\mapsto\Delta_{j-1}(W_{t\wedge t_j}-W_{t\wedge t_{j-1}})$$ is continuous because $$W$$ is, and a finite sum of continuous processes is continuous.
- **Martingale.** For $$t_{j-1}<s<t_j<t$$ (adjacent buckets),

  $$\begin{aligned}
  E[I_t-I_s\mid\mathcal F_s]&=E[\Delta_{j-1}(W_{t_j}-W_s)\mid\mathcal F_s]+E\big[E[\Delta_j(W_t-W_{t_j})\mid\mathcal F_{t_j}]\mid\mathcal F_s\big]\\
  &=\Delta_{j-1}\cdot0+E[\Delta_j\cdot0\mid\mathcal F_s]=0.
  \end{aligned}$$

  It is crucial that the position is chosen at the **beginning** of each period.

- **Itô isometry:** $$\mathrm{Var}[I_t]=E\Big[\int_0^t\Delta_s^2\,ds\Big]$$.
- **Quadratic variation:** $$[I,I]_t=\int_0^t\Delta_s^2\,ds$$.

<details markdown="1"><summary>Proof: Itô isometry for simple integrands</summary>

$$I_0=0$$ and $$I$$ is a martingale, so $$\mathrm{Var}[I_t]=E[I_t^2]$$. Relabel so that $$t=t_j$$ and $$I_t=\sum_{i=1}^j\Delta_{t_{i-1}}(W_{t_i}-W_{t_{i-1}})$$. Cross terms vanish exactly as in Part 02 (condition on the later left endpoint), and each diagonal term is

$$E\big[\Delta_{t_{i-1}}^2(W_{t_i}-W_{t_{i-1}})^2\big]=E\big[\Delta_{t_{i-1}}^2\,E[(W_{t_i}-W_{t_{i-1}})^2\mid\mathcal F_{t_{i-1}}]\big]=E\big[\Delta_{t_{i-1}}^2(t_i-t_{i-1})\big].$$

Since $$\Delta$$ is constant on each $$(t_{i-1},t_i]$$,

$$\mathrm{Var}[I_t]=E\Big[\sum_{i=1}^j\Delta_{t_{i-1}}^2(t_i-t_{i-1})\Big]=E\Big[\int_0^t\Delta_s^2\,ds\Big].$$

</details>

<details markdown="1"><summary>Proof: quadratic variation for simple integrands</summary>

Partition one bucket $$[t_{i-1},t_i]$$ finely by $$s_0<s_1<\dots<s_n$$. Inside the bucket the position is constant, so $$I_{s_k}-I_{s_{k-1}}=\Delta_{t_{i-1}}(W_{s_k}-W_{s_{k-1}})$$ and

$$[I,I]^\Pi_{[t_{i-1},t_i]}=\Delta_{t_{i-1}}^2\,[W,W]^\Pi_{[t_{i-1},t_i]}\ \longrightarrow\ \Delta_{t_{i-1}}^2(t_i-t_{i-1}).$$

Summing over buckets, $$[I,I]_t=\sum_i\Delta_{t_{i-1}}^2(t_i-t_{i-1})=\int_0^t\Delta_s^2\,ds$$.

</details>

Note the difference between the last two: $$[I,I]_t$$ is a **random** quantity path by path; the isometry is its **expectation**.

## 4. General integrands: approximate, then take an $$L^2$$ limit

Fix $$T>0$$. Let $$\mathcal L$$ be the set of integrands $$\Delta$$ that are

- adapted, and
- square-integrable: $$E\Big[\int_0^T\Delta_s^2\,ds\Big]<\infty$$.

For $$\Delta\in\mathcal L$$ and partitions $$\Pi^n$$ of $$[0,T]$$ with mesh $$\to0$$, freeze $$\Delta$$ at the left endpoints:

$$\Delta^n_t=\Delta_0\mathbf 1_{\{t=0\}}+\sum_{j=1}^n\Delta_{t_{j-1}}\mathbf 1_{\{t\in(t_{j-1},t_j]\}},\qquad I^n_t=I(\Delta^n)_t .$$

$$\Delta^n$$ is a simple "rebalance only at grid times" version of $$\Delta$$, and $$I^n$$ is perfectly well defined. For a fixed $$\omega$$, $$\lim I^n_t(\omega)$$ need not exist; instead we take the limit **in mean square**.

**Step 1 — the approximation is good.** A key result of the course is that for $$\Delta\in\mathcal L$$,

$$\lim_{n\to\infty}E\Big[\int_0^T(\Delta^n_s-\Delta_s)^2\,ds\Big]=0,$$

written $$\Delta^n\to\Delta$$ in $$L^2([0,T]\times\Omega)$$. (Intuitively: as the rebalancing grid gets finer, the frozen strategy tracks the real one, on average over both time and scenarios.)

**Step 2 — so the approximations are Cauchy.** From $$(a+b)^2\le2a^2+2b^2$$,

$$(\Delta^n_u-\Delta^m_u)^2\le2(\Delta^n_u-\Delta_u)^2+2(\Delta_u-\Delta^m_u)^2\ \Longrightarrow\ \lim_{n,m\to\infty}E\int_0^T(\Delta^n_s-\Delta^m_s)^2ds=0.$$

**Step 3 — the isometry transfers this to the integrals.** By linearity, $$I^n_t-I^m_t=I(\Delta^n-\Delta^m)_t$$, and $$\Delta^n-\Delta^m$$ is simple, so

$$\mathrm{Var}\big[I^n_t-I^m_t\big]=E\Big[\int_0^t(\Delta^n_u-\Delta^m_u)^2du\Big]\le E\Big[\int_0^T(\Delta^n_u-\Delta^m_u)^2du\Big]\xrightarrow{n,m\to\infty}0.$$

**Step 4 — define the integral as the limit.** The integrals $$I^n$$ form a Cauchy sequence in mean square, so they converge (as processes) to a limit $$I$$, and we define

$$\Big(\int_0^t\Delta_s\,dW_s\Big)(\omega):=I_t(\omega),\qquad t\le T.$$

**Why the isometry is the hero.** It says the map "strategy $$\mapsto$$ P/L" preserves distance: two strategies that are close in $$E\int(\cdot)^2$$ produce P/Ls that are close in variance. So "approximate the strategy" automatically gives "approximate the integral" — even though no single path converges in the classical sense.

## 5. Properties for general integrands

Everything carries over to $$\Delta\in\mathcal L$$:

- **linear:** $$I(\alpha\Delta+\beta\psi)=\alpha I(\Delta)+\beta I(\psi)$$;
- **continuous sample paths**, $$I_0=0$$;
- **adapted**;
- **martingale**;
- **Itô isometry:** $$\mathrm{Var}[I_t]=E\Big[\int_0^t\Delta_s^2\,ds\Big]$$;
- **quadratic variation:** $$[I,I]_t=\int_0^t\Delta_s^2\,ds$$ path by path.

In differential shorthand, $$dI_t=\Delta_t\,dW_t$$ and $$d[I,I]_t=\Delta_t^2\,dt=(\Delta_t\,dW_t)^2$$ — consistent with $$(dW_t)^2=dt$$.

---

**Series contents**

1. [01 · Probability Spaces, Filtrations, Conditional Expectation and Martingales]({{ site.baseurl }}{% post_url 2026-10-21-mf790-01-foundations %})
2. [02 · Random Walks, Quadratic Variation and Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-02-brownian-motion %})
3. **03 · Stochastic Integration** (this post)
4. [04 · Itô's Formula, Itô Processes and Geometric Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-04-ito-formula %})
5. [05 · Black–Scholes: Replication, the PDE and the Greeks]({{ site.baseurl }}{% post_url 2026-10-21-mf790-05-black-scholes %})
6. [06 · Risk-Neutral Pricing, Change of Measure and Girsanov]({{ site.baseurl }}{% post_url 2026-10-21-mf790-06-risk-neutral-pricing %})
7. [07 · SDEs, Feynman–Kac and Stopping Times]({{ site.baseurl }}{% post_url 2026-10-21-mf790-07-sde-feynman-kac %})

[← Previous: Random Walks, Quadratic Variation and Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-02-brownian-motion %})　·　[Next: Itô's Formula, Itô Processes and Geometric Brownian Motion →]({{ site.baseurl }}{% post_url 2026-10-21-mf790-04-ito-formula %})

*This series collects my notes from QST MF 790 Stochastic Calculus at Boston University's Questrom School of Business, in 7 parts. The content follows the order of the course; wording, intuition and a few corrections are my own. Throughout, $$N(\mu,\sigma^2)$$ uses the variance as its second argument. Corrections are welcome. Chinese version of this post: [中文版](https://bobothebest.github.io/homepage_cn/2026/10/21/mf790-03-stochastic-integration/).*
