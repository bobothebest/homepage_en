---
layout: post
title: "Stochastic Calculus Notes 06 | Risk-Neutral Pricing, Change of Measure and Girsanov"
author: Ziyuan Zhang
tags:
- Stochastic Calculus Notes
- BU MF 790
- stochastic calculus
- risk-neutral pricing
- change of measure
- Girsanov theorem
- study notes
date: 2026-10-21 10:06 -0400
toc: true
math: true
---
> **Overview**
>
> Part 05 priced a call. This post prices **any** payoff with one formula,
>
> $$V_t=E^{\mathbb Q}\big[e^{-r(T-t)}V_T\mid\mathcal F_t\big]$$:
>
> the price is a **net present value — but under a different probability measure $$\mathbb Q$$**, one in which the stock earns the risk-free rate. Then we answer the obvious questions: what is a change of measure, why do $$\mathbb P$$ and $$\mathbb Q$$ agree on what *can* happen but not on how *likely* it is, and how does Girsanov's theorem turn $$\mu$$ into $$r$$ by shifting the Brownian motion.

---

## 1. From the call to any contingent claim

**Recap of the Black–Scholes setup.** $$S\sim GBM(\mu,\sigma)$$ under $$\mathbb P$$; money market at constant $$r$$; self-financing strategy $$\Delta$$. Discounted wealth:

$$d(e^{-rt}X_t)=e^{-rt}\big((\mu-r)\Delta_tS_t\,dt+\sigma\Delta_tS_t\,dW_t\big).$$

For the call we found: with $$\widetilde S\sim GBM(r,\sigma)$$ under $$\widetilde{\mathbb P}$$,

$$c(t,s)=E^{\widetilde{\mathbb P}}\big[e^{-r(T-t)}(\widetilde S_T-K)^+\mid\widetilde{\mathcal F}_t,\ \widetilde S_t=s\big],$$

initial capital $$x=c(0,S_0)$$, strategy $$\Delta_t=c_s(t,S_t)$$, terminal wealth $$X_T=(S_T-K)^+$$, so by no-arbitrage $$c(0,S_0)$$ is the price. Now generalize.

**Contingent claims.** A claim pays $$V_T$$ at $$T$$, where $$V_T$$ can be any $$\mathcal F_T$$-measurable random variable with enough integrability:

| Claim | Payoff $$V_T$$ |
|---|---|
| Call | $$(S_T-K)^+$$ |
| Put | $$(K-S_T)^+$$ |
| Asian call | $$\Big(\frac1T\int_0^TS_t\,dt-K\Big)^+$$ |
| Lookback | $$\max_{t\le T}S_t-S_T$$ |
| Up-and-out call | $$(S_T-K)^+\,\mathbf 1_{\{\max_{t\le T}S_t\le L\}}$$ |

## 2. Two assumptions and the pricing formula

1. **Risk-neutral measure.** There is a measure $$\mathbb Q$$ on $$(\Omega,\mathcal F)$$ under which $$S\sim GBM(r,\sigma)$$. Equivalently there is a $$\mathbb Q$$-Brownian motion $$W^{\mathbb Q}$$ with

   $$\frac{dS_t}{S_t}=\underbrace{\mu\,dt+\sigma\,dW_t}_{\text{under }\mathbb P}=\underbrace{r\,dt+\sigma\,dW^{\mathbb Q}_t}_{\text{under }\mathbb Q}.$$

2. **Replication.** There are $$x$$ and $$\Delta$$ with $$\mathbb P[X^{x,\Delta}_T=V_T]=\mathbb Q[X^{x,\Delta}_T=V_T]=1$$.

**Discounted wealth is a $$\mathbb Q$$-martingale.** Under $$\mathbb Q$$ the drift term vanishes:

$$d(e^{-rt}X_t)=e^{-rt}\sigma\Delta_tS_t\,dW^{\mathbb Q}_t .$$

That is why $$\mathbb Q$$ is also called a **martingale measure**.

**Replication over $$[t,T]$$.** The no-arbitrage argument works from any starting time: if you start at $$t$$ with $$X_t$$ (known at $$t$$), trade self-financingly on $$[t,T]$$ and end with $$V_T$$, then the price at $$t$$ must be $$X_t$$.

**Determining $$V_t$$.** By replication, $$e^{-rT}X_T=e^{-rT}V_T$$. By the martingale property,

$$e^{-rt}X_t=E^{\mathbb Q}\big[e^{-rT}X_T\mid\mathcal F_t\big]=E^{\mathbb Q}\big[e^{-rT}V_T\mid\mathcal F_t\big].$$

And since $$V_T=X_T=X_t+\int_t^TdX_u$$, holding $$X_t$$ at time $$t$$ is exactly what it takes to deliver $$V_T$$, so

$$\boxed{\ V_t=X_t=E^{\mathbb Q}\big[e^{-r(T-t)}V_T\mid\mathcal F_t\big]\ }$$

**How to read it.** The chain of reasoning is: (i) the replicating portfolio *is* the claim, so the claim's price is the portfolio's value; (ii) under $$\mathbb Q$$ every discounted self-financing portfolio is a fair game, so today's discounted value equals the expected discounted terminal value; (iii) the terminal value is the payoff. Prices are NPVs — **not under the real-world measure**, but under one where the risky and riskless assets have the same expected return.

The two assumptions raise five questions: how do we get $$\mathbb Q$$; how are $$\mathbb P$$ and $$\mathbb Q$$ related; how do we switch between them; why is $$S\sim GBM(r,\sigma)$$ under $$\mathbb Q$$; and where does $$W^{\mathbb Q}$$ come from. The rest of the post answers them.

## 3. What a change of measure changes

**Not affected by the measure** (they only involve sets and maps):

1. $$\Omega$$, the set of outcomes;
2. $$\mathcal F$$, the information (collection of subsets of $$\Omega$$);
3. $$\mathbb F$$, the flow of information;
4. a random variable $$X:\Omega\to\mathbb R$$ — it is just a map;
5. a process $$\{X_t\}$$;
6. adaptedness: $$\{\omega:X_t(\omega)\in E\}\in\mathcal F_t$$.

**Affected by the measure** (they involve likelihood):

1. probabilities $$P(A)=E^P[\mathbf 1_A]$$;
2. expectations $$E\big[\sum_i\alpha_i\mathbf 1_{A_i}\big]=\sum_i\alpha_iP(A_i)$$;
3. moment generating functions $$\alpha\mapsto E^P[e^{\alpha X}]$$;
4. distributions $$P(X\le t)$$;
5. the martingale property $$E[X_t\mid\mathcal F_s]=X_s$$;
6. independence $$P(A\cap B)=P(A)P(B)$$.

So the stock price paths are the same; only the **weights** we put on them change — and with them, the drift.

## 4. Change of measure: the construction

Given $$(\Omega,\mathcal F,\mathbb P)$$, take any random variable $$Z$$ with $$\mathbb P[Z>0]=1$$ (write $$Z>0$$) and $$E^{\mathbb P}[Z]=1$$. Define

$$\mathbb Q(A):=E^{\mathbb P}[Z\,\mathbf 1_A],\qquad A\in\mathcal F .$$

$$\mathbb Q$$ is a probability measure: $$\mathbb Q(A)\ge0$$; $$\mathbb Q(\Omega)=E^{\mathbb P}[Z]=1$$; and countable additivity holds, e.g. for disjoint $$A,B$$, $$\mathbb Q(A\cup B)=E^{\mathbb P}[(\mathbf 1_A+\mathbf 1_B)Z]=\mathbb Q(A)+\mathbb Q(B)$$.

**Example: fair coin to unfair coin.** Two tosses, fair and independent under $$\mathbb P$$. Set

$$Z(HH)=\tfrac49,\quad Z(HT)=Z(TH)=\tfrac89,\quad Z(TT)=\tfrac{16}9,\qquad E^{\mathbb P}Z=\tfrac14\Big(\tfrac49+\tfrac89+\tfrac89+\tfrac{16}9\Big)=1 .$$

Then $$\mathbb Q(HH)=\frac14\cdot\frac49=\frac19$$, $$\mathbb Q(HT)=\mathbb Q(TH)=\frac29$$, $$\mathbb Q(TT)=\frac49$$. Under $$\mathbb Q$$ the tosses are still independent ($$\frac19=\frac13\cdot\frac13$$, etc.) but the coin is **unfair**: $$\mathbb Q(H)=\frac13$$.

**Equivalence.** Because $$Z>0$$: $$\mathbb P(A)>0\Rightarrow\mathbb Q(A)=E^{\mathbb P}[Z\mathbf 1_A]>0$$, and conversely. So $$\mathbb P(A)>0\iff\mathbb Q(A)>0$$ and we call $$\mathbb P,\mathbb Q$$ **equivalent**. Equivalent is not equal: they agree on what can and cannot happen, not on how likely it is ($$\mathbb P(A)=0.01$$, $$\mathbb Q(A)=0.99$$ is fine).

**Radon–Nikodym derivative.** $$\dfrac{d\mathbb Q}{d\mathbb P}:=Z$$, also called the **likelihood ratio**. On a finite space,

$$\frac{d\mathbb Q}{d\mathbb P}(\omega)=\frac{\mathbb Q(\{\omega\})}{\mathbb P(\{\omega\})}=Z(\omega).$$

## 5. Shifting the mean of a normal

Let $$X\sim N(0,\sigma^2)$$ under $$\mathbb P$$, $$\theta\in\mathbb R$$, and $$Z=e^{-\theta X-\frac12\sigma^2\theta^2}$$. Then $$Z>0$$ and $$E[Z]=e^{-\frac12\sigma^2\theta^2}E[e^{-\theta X}]=1$$. Under $$\mathbb Q(A)=E^{\mathbb P}[Z\mathbf 1_A]$$:

$$E^{\mathbb Q}[e^{\alpha X}]=E^{\mathbb P}\big[e^{(\alpha-\theta)X}\big]e^{-\frac12\sigma^2\theta^2}=e^{\frac12\sigma^2(\alpha-\theta)^2-\frac12\sigma^2\theta^2}=e^{-\sigma^2\theta\alpha+\frac12\sigma^2\alpha^2},$$

so $$X\overset{\mathbb Q}{\sim}N(-\sigma^2\theta,\ \sigma^2)$$, i.e. $$X+\sigma^2\theta\overset{\mathbb Q}{\sim}N(0,\sigma^2)$$. **Tilting by an exponential shifts the mean and leaves the variance alone.**

**General version.** To move $$X$$ from $$N(\mu_P,\sigma^2)$$ under $$\mathbb P$$ to $$N(\mu_Q,\sigma^2)$$ under $$\mathbb Q$$, use $$\frac{d\mathbb Q}{d\mathbb P}=\frac{e^{-\theta X}}{E^{\mathbb P}[e^{-\theta X}]}=e^{-\theta X-\frac12\sigma^2\theta^2+\theta\mu_P}$$. Then

$$E^{\mathbb Q}[e^{\alpha X}]=\exp\Big\{\alpha(\mu_P-\theta\sigma^2)+\tfrac12\alpha^2\sigma^2\Big\},\qquad\text{so we need}\quad \theta=\frac{\mu_P-\mu_Q}{\sigma^2}.$$

## 6. Application to Brownian motion and the stock

Fix $$T$$; $$W_T\sim N(0,T)$$ under $$\mathbb P$$. Take $$\theta=\frac{\mu-r}{\sigma}$$ and

$$Z=e^{-\theta W_T-\frac12\theta^2T}.$$

By Section 5 (with $$\sigma^2=T$$, $$\mu_P=0$$), $$W^{\mathbb Q}_T:=W_T+\theta T\sim N(0,T)$$ under $$\mathbb Q$$. Since $$\mu-\sigma\theta=r$$,

$$S_T=S_0\exp\Big\{\big(\mu-\tfrac12\sigma^2\big)T+\sigma W_T\Big\}=S_0\exp\Big\{\big(r-\tfrac12\sigma^2\big)T+\sigma W^{\mathbb Q}_T\Big\}.$$

So **at the fixed time $$T$$**, $$S_T$$ looks like a $$GBM(r,\sigma)$$ under $$\mathbb Q$$. To have $$S\sim GBM(r,\sigma)$$ under $$\mathbb Q$$ **for all** $$t\le T$$ we need more: is $$t\mapsto W^{\mathbb Q}_t=W_t+\theta t$$ a $$\mathbb Q$$-Brownian motion on $$[0,T]$$? Equivalently, is

$$\frac{dS_t}{S_t}=\mu\,dt+\sigma\,dW_t\overset{?}{=}r\,dt+\sigma\,dW^{\mathbb Q}_t,\qquad W^{\mathbb Q}_t=W_t+\frac{\mu-r}{\sigma}t ?$$

The core is to verify that $$W^{\mathbb Q}$$ is a Brownian motion. (Note $$Z_t=E^{\mathbb P}[Z_T\mid\mathcal F_t]=e^{-\theta W_t-\frac12\theta^2t}$$ is a $$\mathbb P$$-martingale — the exponential martingale of Part 04.)

## 7. Girsanov's theorem

**Simple version.** If $$W$$ is a $$\mathbb P$$-Brownian motion, $$Z_T=e^{-\theta W_T-\frac12\theta^2T}$$ and $$\mathbb Q(A)=E^{\mathbb P}[\mathbf 1_AZ_T]$$, then

$$t\mapsto W^{\mathbb Q}_t:=W_t+\theta t\ \text{ is a }\mathbb Q\text{-Brownian motion on }[0,T].$$

**General version.** Fix $$(\widetilde\Omega,\widetilde{\mathcal F},\widetilde{\mathbb F},\widetilde{\mathbb P})$$ with a $$\widetilde{\mathbb P}$$-Brownian motion $$\widetilde W$$, and an adapted $$\widetilde\theta$$. Let

$$\widetilde Z_t=\exp\Big(-\int_0^t\widetilde\theta_u\,d\widetilde W_u-\frac12\int_0^t\widetilde\theta_u^2\,du\Big),\quad t\le T,\qquad \frac{d\widetilde{\mathbb Q}}{d\widetilde{\mathbb P}}=\widetilde Z_T\ \text{on }\widetilde{\mathcal F}_T .$$

Then $$\widetilde W^{\widetilde{\mathbb Q}}_t=\widetilde W_t+\int_0^t\widetilde\theta_u\,du$$, $$t\le T$$, is a $$\widetilde{\mathbb Q}$$-Brownian motion.

Intuition: reweighting the paths by $$Z$$ makes paths that drift down more likely and paths that drift up less likely, by exactly the amount that turns "Brownian motion plus drift $$\theta$$" back into a driftless Brownian motion.

### Lévy's characterization

Proving something is a Brownian motion straight from the definition is painful. Lévy's characterization is the shortcut: **if $$\widetilde M$$ is a $$\widetilde{\mathbb Q}$$-martingale with continuous paths, $$\widetilde M_0=0$$ and $$[\widetilde M,\widetilde M]_t=t$$, then $$\widetilde M$$ is a $$\widetilde{\mathbb Q}$$-Brownian motion.**

So to prove Girsanov we only need: (1) $$[\widetilde W^{\widetilde{\mathbb Q}},\widetilde W^{\widetilde{\mathbb Q}}]_t=t$$ and (2) $$\widetilde W^{\widetilde{\mathbb Q}}$$ is a $$\widetilde{\mathbb Q}$$-martingale.

<details markdown="1"><summary>Proof of Girsanov (constant θ)</summary>

**Step 1.** Quadratic variation does not care about the measure, and the drift contributes nothing:

$$d[\widetilde W^{\widetilde{\mathbb Q}},\widetilde W^{\widetilde{\mathbb Q}}]_t=(d\widetilde W_t+\widetilde\theta\,dt)^2=(d\widetilde W_t)^2=dt .$$

**Step 2.** For $$s<t$$ and $$A_s\in\widetilde{\mathcal F}_s$$, move to $$\widetilde{\mathbb P}$$, use Tower and the martingale $$\widetilde Z_t=E^{\widetilde{\mathbb P}}[\widetilde Z_T\mid\widetilde{\mathcal F}_t]$$:

$$E^{\widetilde{\mathbb Q}}[\mathbf 1_{A_s}\widetilde W^{\widetilde{\mathbb Q}}_t]=E^{\widetilde{\mathbb P}}[\mathbf 1_{A_s}\widetilde W^{\widetilde{\mathbb Q}}_t\widetilde Z_T]=E^{\widetilde{\mathbb P}}[\mathbf 1_{A_s}\widetilde W^{\widetilde{\mathbb Q}}_t\widetilde Z_t]=E^{\widetilde{\mathbb P}}\big[\mathbf 1_{A_s}f(t,\widetilde W_t)\big],$$

with $$f(t,w)=(w+\widetilde\theta t)\,e^{-\widetilde\theta w-\frac12\widetilde\theta^2t}$$. Direct differentiation gives

$$f_t=e^{-\widetilde\theta w-\frac12\widetilde\theta^2t}\,\widetilde\theta\big(1-\tfrac12\widetilde\theta w-\tfrac12\widetilde\theta^2t\big),\quad f_w=e^{-\widetilde\theta w-\frac12\widetilde\theta^2t}\big(1-\widetilde\theta w-\widetilde\theta^2t\big),\quad f_{ww}=e^{-\widetilde\theta w-\frac12\widetilde\theta^2t}\big(-2\widetilde\theta+\widetilde\theta^2w+\widetilde\theta^3t\big),$$

so $$f_t+\frac12f_{ww}=0$$ and by Itô $$df(t,\widetilde W_t)=f_w\,d\widetilde W_t$$: $$f(t,\widetilde W_t)$$ is a $$\widetilde{\mathbb P}$$-martingale. Hence

$$E^{\widetilde{\mathbb Q}}[\mathbf 1_{A_s}\widetilde W^{\widetilde{\mathbb Q}}_t]=E^{\widetilde{\mathbb P}}[\mathbf 1_{A_s}f(s,\widetilde W_s)]=E^{\widetilde{\mathbb P}}\big[\mathbf 1_{A_s}(\widetilde W_s+\widetilde\theta s)e^{-\widetilde\theta\widetilde W_s-\frac12\widetilde\theta^2s}\big].$$

The same Tower/TOWK computation for $$\widetilde W^{\widetilde{\mathbb Q}}_s$$ gives the identical right-hand side, so

$$E^{\widetilde{\mathbb Q}}[\mathbf 1_{A_s}\widetilde W^{\widetilde{\mathbb Q}}_t]=E^{\widetilde{\mathbb Q}}[\mathbf 1_{A_s}\widetilde W^{\widetilde{\mathbb Q}}_s]\quad\text{for all }A_s\in\widetilde{\mathcal F}_s .$$

Since $$\widetilde W^{\widetilde{\mathbb Q}}_s$$ is $$\widetilde{\mathcal F}_s$$-measurable, partial averaging gives $$E^{\widetilde{\mathbb Q}}[\widetilde W^{\widetilde{\mathbb Q}}_t\mid\widetilde{\mathcal F}_s]=\widetilde W^{\widetilde{\mathbb Q}}_s$$. A time-varying $$\widetilde\theta_t$$ works the same way with more bookkeeping.

</details>

### Price dynamics under $$\mathbb Q$$

By Girsanov, $$W^{\mathbb Q}_t=W_t+\theta t$$ is a $$\mathbb Q$$-Brownian motion on $$[0,T]$$, and since $$\mu-\sigma\theta=r$$,

$$dS_t=\mu S_t\,dt+\sigma S_t\big(dW^{\mathbb Q}_t-\theta\,dt\big)=rS_t\,dt+\sigma S_t\,dW^{\mathbb Q}_t\iff S\overset{\mathbb Q}{\sim}GBM(r,\sigma).$$

$$\theta=\dfrac{\mu-r}{\sigma}$$ is the **market price of risk**: excess return per unit of volatility, i.e. the **Sharpe ratio**. This answers questions 1–5 of Section 2 for assumption 1.

## 8. Assumption 2: finding the hedge

Good news first: $$\frac{d\mathbb Q}{d\mathbb P}\big\vert_{\mathcal F_T}=Z_T>0$$, so $$\mathbb P$$ and $$\mathbb Q$$ are equivalent and it **suffices to show $$\mathbb Q[X_T=V_T]=1$$**.

Two facts:

1. $$e^{-rt}X_t$$ is a $$\mathbb Q$$-martingale: $$d(e^{-rt}X_t)=e^{-rt}\Delta_tS_t\sigma\,dW^{\mathbb Q}_t$$.
2. $$M^V_t:=E^{\mathbb Q}[e^{-rT}V_T\mid\mathcal F_t]$$ is a $$\mathbb Q$$-martingale on $$[0,T]$$, by Tower: $$E^{\mathbb Q}[M^V_t\mid\mathcal F_s]=E^{\mathbb Q}[e^{-rT}V_T\mid\mathcal F_s]=M^V_s$$.

We want to make these two martingales the same process. **Suppose** $$M^V$$ can be written as a stochastic integral, $$dM^V_t=\Gamma_t\,dW^{\mathbb Q}_t$$ for some adapted $$\Gamma$$. Then choose

$$\Delta_t=\frac{\Gamma_t}{e^{-rt}\sigma S_t},\qquad t\le T,$$

so that $$d(e^{-rt}X_t)=e^{-rt}\Delta_tS_t\sigma\,dW^{\mathbb Q}_t=\Gamma_t\,dW^{\mathbb Q}_t=dM^V_t$$. Starting both at $$x=M^V_0=E^{\mathbb Q}[e^{-rT}V_T]$$, "same start + same dynamics" gives $$e^{-rt}X_t=M^V_t$$ for all $$t$$, and at $$T$$, $$X_T=V_T$$.

The remaining question — *is every $$\mathbb Q$$-martingale of the form $$\int\Gamma\,dW^{\mathbb Q}$$?* — is answered (yes, when the filtration is generated by the Brownian motion) by the **martingale representation theorem**. In words: in a Brownian world, every fair game is a trading strategy in the stock, so every claim can be hedged — the market is **complete**.

## 9. Quadratic covariation and integration by parts

For continuous $$X,Y$$ and a partition $$\Pi$$ of $$[0,t]$$,

$$[X,Y]^\Pi_t=\sum_{j}\big(X_{t_j}-X_{t_{j-1}}\big)\big(Y_{t_j}-Y_{t_{j-1}}\big),\qquad [X,Y]_t=\lim_{\Vert \Pi\Vert \to0}[X,Y]^\Pi_t .$$

For Itô processes driven by the same $$W$$, $$dX_t=\Theta_tdt+\Delta_tdW_t$$ and $$dY_t=\phi_tdt+\psi_tdW_t$$, the heuristic $$d[X,Y]_t=dX_t\,dY_t$$ gives

$$d[X,Y]_t=\Delta_t\psi_t\,dt,\qquad [X,Y]_t=\int_0^t\Delta_s\psi_s\,ds,$$

and $$[X,Y]\equiv0$$ if either $$\Delta\equiv0$$ or $$\psi\equiv0$$.

<details markdown="1"><summary>Proof via polarization</summary>

For any numbers, $$4(a-b)(c-d)=\big((a+c)-(b+d)\big)^2-\big((a-c)-(b-d)\big)^2$$, so

$$[X,Y]=\tfrac14\big([X+Y,X+Y]-[X-Y,X-Y]\big).$$

$$X\pm Y$$ are Itô processes with $$dW$$-coefficients $$\Delta_t\pm\psi_t$$, so

$$d[X,Y]_t=\tfrac14\big((\Delta_t+\psi_t)^2-(\Delta_t-\psi_t)^2\big)dt=\Delta_t\psi_t\,dt .$$

</details>

**Stochastic integration by parts.**

$$d(X_tY_t)=X_t\,dY_t+Y_t\,dX_t+d[X,Y]_t=\big(X_t\phi_t+Y_t\Theta_t+\Delta_t\psi_t\big)dt+\big(X_t\psi_t+Y_t\Delta_t\big)dW_t,$$

again an Itô process. For smooth (no-$$dW$$) $$f,g$$ the bracket vanishes and we recover the ordinary product rule $$d(f_tg_t)=f_t\,dg_t+g_t\,df_t$$.

---

**Series contents**

1. [01 · Probability Spaces, Filtrations, Conditional Expectation and Martingales]({{ site.baseurl }}{% post_url 2026-10-21-mf790-01-foundations %})
2. [02 · Random Walks, Quadratic Variation and Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-02-brownian-motion %})
3. [03 · Stochastic Integration]({{ site.baseurl }}{% post_url 2026-10-21-mf790-03-stochastic-integration %})
4. [04 · Itô's Formula, Itô Processes and Geometric Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-04-ito-formula %})
5. [05 · Black–Scholes: Replication, the PDE and the Greeks]({{ site.baseurl }}{% post_url 2026-10-21-mf790-05-black-scholes %})
6. **06 · Risk-Neutral Pricing, Change of Measure and Girsanov** (this post)
7. [07 · SDEs, Feynman–Kac and Stopping Times]({{ site.baseurl }}{% post_url 2026-10-21-mf790-07-sde-feynman-kac %})

[← Previous: Black–Scholes: Replication, the PDE and the Greeks]({{ site.baseurl }}{% post_url 2026-10-21-mf790-05-black-scholes %})　·　[Next: SDEs, Feynman–Kac and Stopping Times →]({{ site.baseurl }}{% post_url 2026-10-21-mf790-07-sde-feynman-kac %})

*This series collects my notes from QST MF 790 Stochastic Calculus at Boston University's Questrom School of Business, in 7 parts. The content follows the order of the course; wording, intuition and a few corrections are my own. Throughout, $$N(\mu,\sigma^2)$$ uses the variance as its second argument. Corrections are welcome. Chinese version of this post: [中文版](https://bobothebest.github.io/homepage_cn/2026/10/21/mf790-06-risk-neutral-pricing/).*
