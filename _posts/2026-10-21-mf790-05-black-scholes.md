---
layout: post
title: "Stochastic Calculus Notes 05 | Black–Scholes: Replication, the PDE and the Greeks"
author: Ziyuan Zhang
tags:
- Stochastic Calculus Notes
- BU MF 790
- stochastic calculus
- Black-Scholes
- option pricing
- Greeks
- implied volatility
- study notes
date: 2026-10-21 10:05 -0400
toc: true
math: true
---
> **Overview**
>
> The price of an option is **the capital needed to replicate it**. This post sets up the Black–Scholes market, writes down the wealth of a self-financing trader, and matches it term by term ($$dW$$ terms and $$dt$$ terms) against the option's price process. Matching the $$dW$$ terms gives the **delta hedge**; matching the $$dt$$ terms gives the **Black–Scholes PDE**. Solving the PDE gives the Black–Scholes formula, and differentiating it gives the Greeks.
>
> A surprise worth noticing early: the stock's expected return $$\mu$$ **disappears** from the price.

---

## 1. The market

Fix $$(\Omega,\mathcal F,\mathbb F,\mathbb P)$$ and a Brownian motion $$W$$. Two assets trade:

1. **Stock** (risky), $$S\sim GBM(\mu,\sigma)$$: $$\dfrac{dS_t}{S_t}=\mu\,dt+\sigma\,dW_t$$, $$S_0>0$$. $$S_t(\omega)$$ is the price of one share at time $$t$$ in scenario $$\omega$$.
2. **Money market** (riskless), $$\dfrac{dB_t}{B_t}=r\,dt$$, $$B_0=1$$: one dollar at time $$0$$ grows to $$e^{rt}$$ dollars at time $$t$$, with constant $$r>0$$.

## 2. Self-financing strategies and wealth

A **trading strategy** $$\Delta$$ is an adapted process; $$\Delta_t(\omega)$$ is the number of shares held at $$(t,\omega)$$. Start with capital $$x$$. **Self-financing**: all money not in the stock sits in the money market, and no money enters or leaves after time $$0$$.

The wealth $$X=X^{x,\Delta}$$ splits as

$$X_t=\underbrace{\Delta_tS_t}_{\text{stock}}+\underbrace{X_t-\Delta_tS_t}_{\text{money market}},$$

and the money-market part buys $$(X_t-\Delta_tS_t)/B_t$$ "shares" of $$B$$.

**Wealth dynamics.** Fix positions at the start of $$[t,t+dt]$$:

$$X_{t+dt}-X_t=\Delta_t(S_{t+dt}-S_t)+\frac{X_t-\Delta_tS_t}{B_t}(B_{t+dt}-B_t)\ \xrightarrow{dt\to0}\ dX_t=\Delta_t\,dS_t+(X_t-\Delta_tS_t)\frac{dB_t}{B_t}.$$

Substituting the dynamics of $$S$$ and $$B$$:

$$dX_t=\big(rX_t+(\mu-r)\Delta_tS_t\big)dt+\sigma\Delta_tS_t\,dW_t,$$

so $$X$$ is an Itô process. Read it as: earn $$r$$ on everything, plus the excess return $$\mu-r$$ on the part in stock, plus noise.

## 3. What "price" means

We want to price a European **call**: the right to buy $$S$$ at strike $$K$$ at maturity $$T$$, with payoff $$(S_T-K)^+=\max(S_T-K,0)$$.

**Arbitrage-free price = replication cost.** If starting from $$x$$ and trading self-financingly with some $$\Delta$$ we end with $$X^{x,\Delta}_T=(S_T-K)^+$$ with probability one, then the call's price must be $$x$$. Any other price would let you sell the expensive one, buy the cheap one, and pocket the difference risk-free.

**Same start + same dynamics = same process.** If $$dY_t=\Theta^Y_tdt+\Delta^Y_tdW_t$$ and $$dZ_t=\Theta^Z_tdt+\Delta^Z_tdW_t$$ with $$Y_0=Z_0$$, $$\Theta^Y\equiv\Theta^Z$$, $$\Delta^Y\equiv\Delta^Z$$, then $$Y_t=Z_t$$ for all $$t\le T$$ with probability one. This is the replication argument in one line: match the starting point and match both the $$dt$$ and $$dW$$ terms.

## 4. Matching wealth against the option price

Let $$c(t,s)$$ be the call price at time $$t$$ when $$S_t=s$$. We know $$c(T,s)=(s-K)^+$$ and want $$c$$ for $$t<T$$. Work with **discounted** processes (it simplifies the algebra): $$e^{-rt}X_t$$ and $$e^{-rt}c(t,S_t)$$.

**Discounted wealth.** By the discounting rule from Part 04,

$$d(e^{-rt}X_t)=e^{-rt}(\mu-r)\Delta_tS_t\,dt+e^{-rt}\sigma\Delta_tS_t\,dW_t .$$

**Discounted price.** Assume $$c$$ is smooth (strictly speaking the payoff is not differentiable at $$s=K$$, but $$c$$ is smooth for $$t<T$$). With $$d[S,S]_t=\sigma^2S_t^2dt$$ and writing $$(\cdot)=(t,S_t)$$:

$$d\big(e^{-rt}c(t,S_t)\big)=e^{-rt}\Big(-rc+c_t+c_s\,\mu S_t+\tfrac12c_{ss}\,\sigma^2S_t^2\Big)(\cdot)\,dt+e^{-rt}c_s(\cdot)\,\sigma S_t\,dW_t .$$

Set $$x=c(0,S_0)$$ and match:

- **$$dW$$ terms:** $$e^{-rt}\Delta_t\sigma S_t=e^{-rt}c_s(t,S_t)\sigma S_t$$, so

  $$\boxed{\ \Delta_t=c_s(t,S_t)\ }\quad\text{— the delta hedge.}$$

- **$$dt$$ terms:** $$\Delta_t(\mu-r)S_t=-rc+c_t+c_s\mu S_t+\frac12c_{ss}\sigma^2S_t^2$$. Plug in $$\Delta_t=c_s$$; the $$c_s\mu S_t$$ terms **cancel**, leaving

  $$0=-rc(t,S_t)+c_t(t,S_t)+rS_t\,c_s(t,S_t)+\tfrac12\sigma^2S_t^2\,c_{ss}(t,S_t).$$

It suffices to require this for every $$t<T$$ and $$s>0$$, which gives the **Black–Scholes PDE**:

$$\begin{cases}0=-rc(t,s)+c_t(t,s)+rs\,c_s(t,s)+\tfrac12\sigma^2s^2\,c_{ss}(t,s),&t<T,\ s>0,\\[4pt] c(T,s)=(s-K)^+,&s>0.\end{cases}$$

**$$\mu$$ is gone.** The hedge cancels the stock's risk exactly, so how fast the stock is expected to grow is irrelevant to the price.

## 5. Solving the PDE

Two steps: (1) assume a solution $$c$$ exists and find a formula for it; (2) check that the formula solves the PDE.

**Step 1.** Let $$\widetilde S\sim GBM(r,\sigma)$$ under some measure $$\widetilde{\mathbb P}$$ with Brownian motion $$\widetilde W$$ — note the **new measure** and the drift **$$r$$ instead of $$\mu$$**. By Itô,

$$d\big(e^{-rt}c(t,\widetilde S_t)\big)=e^{-rt}\big(\underbrace{-rc+c_t+r\widetilde S_tc_s+\tfrac12\sigma^2\widetilde S_t^2c_{ss}}_{=0\ \text{by the PDE}}\big)dt+e^{-rt}c_s\,\sigma\widetilde S_t\,d\widetilde W_t,$$

so the discounted price is a **martingale under $$\widetilde{\mathbb P}$$**. Together with the terminal condition,

$$e^{-rt}c(t,\widetilde S_t)=E^{\widetilde{\mathbb P}}\big[e^{-rT}c(T,\widetilde S_T)\mid\widetilde{\mathcal F}_t\big]\quad\Longrightarrow\quad c(t,\widetilde S_t)=E^{\widetilde{\mathbb P}}\big[e^{-r(T-t)}(\widetilde S_T-K)^+\mid\widetilde{\mathcal F}_t\big].$$

Write $$\widetilde S_T$$ in terms of $$\widetilde S_t$$:

$$\widetilde S_T=\widetilde S_t\exp\Big\{\big(r-\tfrac12\sigma^2\big)(T-t)+\sigma(\widetilde W_T-\widetilde W_t)\Big\}.$$

$$\widetilde S_t$$ is known at time $$t$$ and $$\widetilde W_T-\widetilde W_t\perp\!\!\!\perp\widetilde{\mathcal F}_t$$, so we may freeze $$\widetilde S_t=s$$. With $$\widetilde W_T-\widetilde W_t\sim N(0,T-t)$$:

$$c(t,s)=\frac{e^{-r(T-t)}}{\sqrt{2\pi(T-t)}}\int_{-\infty}^{\infty}\Big(s\,e^{(r-\frac12\sigma^2)(T-t)+\sigma z}-K\Big)^+e^{-\frac{z^2}{2(T-t)}}\,dz .$$

Computing the integral gives the **Black–Scholes formula**:

$$\boxed{\ c(t,s)=s\,N\big(d_+(T-t,s)\big)-Ke^{-r(T-t)}N\big(d_-(T-t,s)\big)\ }$$

$$d_\pm(\tau,s)=\frac1{\sigma\sqrt\tau}\Big(\log\frac sK+\big(r\pm\tfrac12\sigma^2\big)\tau\Big),\qquad N(y)=\frac1{\sqrt{2\pi}}\int_{-\infty}^ye^{-z^2/2}\,dz .$$

Only the **time to maturity** $$\tau=T-t$$ enters, not calendar time.

**Step 2.** Verify directly that this $$c$$ satisfies the PDE and $$c(T,s)=(s-K)^+$$ (a homework exercise; the identity checked in Section 7 below is the key step).

## 6. The Greeks

Think of $$c=c(t,s;T,K,r,\sigma)$$. Sensitivities to these inputs are the **Greeks**. Write $$\tau=T-t$$ and $$d_\pm=d_\pm(\tau,s)$$.

| Greek | Definition | Formula | Sign |
|---|---|---|---|
| Delta | $$c_s$$ | $$N(d_+)$$ | $$>0$$ |
| Gamma | $$c_{ss}$$ | $$\dfrac{N'(d_+)}{\sigma s\sqrt\tau}$$ | $$>0$$ |
| Theta | $$c_t$$ | $$-rKe^{-r\tau}N(d_-)-\dfrac{\sigma s}{2\sqrt\tau}N'(d_+)$$ | $$<0$$ |
| Vega | $$c_\sigma$$ | $$s\sqrt\tau\,N'(d_+)$$ | $$>0$$ |

**Delta.** A call is worth more when the stock is higher. The replicating strategy is $$\Delta_t=c_s(t,S_t)$$: to first order in $$S$$ (pretend $$dt=0$$), $$dX_t=c_s\,dS_t=dc(t,S_t)$$, so the hedge matches the option's moves — we are **delta neutral**.

**Gamma.** $$c_{ss}>0$$: the call is **convex** in the stock price. To second order in $$S$$,

$$dX_t=c_s\,dS_t,\qquad dc(t,S_t)=c_s\,dS_t+\tfrac12c_{ss}\,d[S,S]_t .$$

*Intuition for "long convexity".* Own the call and short $$c_s$$ shares. A delta hedge is a tangent line to the convex price curve; whichever way the stock jumps, the curve ends up above the tangent, so the hedged position gains $$\frac12c_{ss}(dS)^2\ge0$$ from moves of either sign. Gamma is largest near the strike and becomes more concentrated as maturity approaches.

**Theta.** $$c_t<0$$: with everything else fixed, the call loses value as time passes. That is the price paid for long gamma: with all second-order changes included,

$$dX_t=c_s\,dS_t+(c-c_sS_t)\,r\,dt,\qquad dc(t,S_t)=c_t\,dt+c_s\,dS_t+\tfrac12c_{ss}\sigma^2S_t^2\,dt,$$

and the PDE says these are equal — a **perfect hedge**. Theta exactly pays for the gamma gains plus financing.

**Vega.** $$c_\sigma>0$$: higher volatility means bigger moves; up-moves help the call, down-moves hurt it only down to zero. Positive vega = **long volatility**.

## 7. Check: the hedge is perfect

With $$c=SN(d_+)-Ke^{-r\tau}N(d_-)$$, $$c_s=N(d_+)$$, $$c_{ss}=\frac{N'(d_+)}{\sigma S\sqrt\tau}$$ and $$c_t$$ from the table, we need $$(c-c_sS)\,r=c_t+\frac12c_{ss}\sigma^2S^2$$:

$$\text{LHS}=\big[SN(d_+)-Ke^{-r\tau}N(d_-)-SN(d_+)\big]r=-rKe^{-r\tau}N(d_-),$$

$$\text{RHS}=-rKe^{-r\tau}N(d_-)-\frac{\sigma S}{2\sqrt\tau}N'(d_+)+\frac12\cdot\frac{\sigma S}{\sqrt\tau}N'(d_+)=-rKe^{-r\tau}N(d_-).\ \checkmark$$

The theta term and the gamma term cancel exactly.

## 8. Implied volatility

Under Black–Scholes, conditional mean and variance of returns do not change over time:

$$E\Big[\frac{dS_t}{S_t}\,\Big\vert\,\mathcal F_t\Big]=\mu\,dt,\qquad\mathrm{Var}\Big[\frac{dS_t}{S_t}\,\Big\vert\,\mathcal F_t\Big]=\sigma^2\,d[W,W]_t=\sigma^2\,dt .$$

In reality volatility is not constant. Two ways to estimate it:

- **historical volatility** from past returns (backward-looking);
- **implied volatility** from option prices (forward-looking).

For a European call with maturity $$T$$ and strike $$K$$, let $$C_{BS}(\sigma,K,T)$$ be the Black–Scholes price and $$C_m(K,T)$$ the observed market price. The **implied volatility** $$\sigma_{imp}=\sigma_{imp}(K,T)$$ solves

$$C_{BS}(\sigma_{imp},K,T)=C_m(K,T).$$

Because vega $$>0$$, $$C_{BS}$$ is increasing in $$\sigma$$, so the solution is unique. Options are typically **quoted in implied volatility**, not price. If the world were exactly Black–Scholes, $$\sigma_{imp}(K,T)=\sigma$$ for every $$K$$ and $$T$$. Real data instead show $$\sigma_{imp}$$ varying with strike and maturity — the **volatility smile / skew** — which is the market telling us the model is only an approximation.

---

**Series contents**

1. [01 · Probability Spaces, Filtrations, Conditional Expectation and Martingales]({{ site.baseurl }}{% post_url 2026-10-21-mf790-01-foundations %})
2. [02 · Random Walks, Quadratic Variation and Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-02-brownian-motion %})
3. [03 · Stochastic Integration]({{ site.baseurl }}{% post_url 2026-10-21-mf790-03-stochastic-integration %})
4. [04 · Itô's Formula, Itô Processes and Geometric Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-04-ito-formula %})
5. **05 · Black–Scholes: Replication, the PDE and the Greeks** (this post)
6. [06 · Risk-Neutral Pricing, Change of Measure and Girsanov]({{ site.baseurl }}{% post_url 2026-10-21-mf790-06-risk-neutral-pricing %})
7. [07 · SDEs, Feynman–Kac and Stopping Times]({{ site.baseurl }}{% post_url 2026-10-21-mf790-07-sde-feynman-kac %})

[← Previous: Itô's Formula, Itô Processes and Geometric Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-04-ito-formula %})　·　[Next: Risk-Neutral Pricing, Change of Measure and Girsanov →]({{ site.baseurl }}{% post_url 2026-10-21-mf790-06-risk-neutral-pricing %})

*This series collects my notes from QST MF 790 Stochastic Calculus at Boston University's Questrom School of Business, in 7 parts. The content follows the order of the course; wording, intuition and a few corrections are my own. Throughout, $$N(\mu,\sigma^2)$$ uses the variance as its second argument. Corrections are welcome. Chinese version of this post: [中文版](https://bobothebest.github.io/homepage_cn/2026/10/21/mf790-05-black-scholes/).*
