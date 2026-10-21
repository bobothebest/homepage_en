---
layout: post
title: "Stochastic Calculus Notes 07 | SDEs, Feynman–Kac and Stopping Times"
author: Ziyuan Zhang
tags:
- Stochastic Calculus Notes
- study notes
date: 2026-10-21 10:07 -0400
toc: true
math: true
---
> **Overview**
>
> The last part zooms out. Stock prices, interest rates and volatilities are all modelled as solutions of **stochastic differential equations**. We solve the Ornstein–Uhlenbeck SDE explicitly, meet the CIR process, and see why every SDE solution is **Markov**. Then **Feynman–Kac** closes the loop of the whole course: a conditional expectation of a payoff and a PDE are the same object — Black–Scholes is just one instance. We finish with dividend-paying stocks, stopping times and the optional sampling theorem.

---

## 1. Stochastic differential equations

Fix $$(\Omega,\mathcal F,\mathbb F,\mathbb P)$$ with a Brownian motion $$W$$ and $$\mathbb F=\mathbb F^W$$. An **SDE** is an equation of the form

$$dX_t=\mu(t,X_t)\,dt+\sigma(t,X_t)\,dW_t,\qquad X_0=x,$$

where $$\mu:[0,\infty)\times\mathbb R\to\mathbb R$$ is the **drift** function and $$\sigma:[0,\infty)\times\mathbb R\to\mathbb R$$ the **diffusion** (dispersion) function. Unlike an Itô process, the coefficients depend on the unknown $$X$$ itself.

A **solution** is a process $$X$$ such that for all $$t\ge0$$

$$\mathbb P\Big[X_t=x+\int_0^t\mu(u,X_u)\,du+\int_0^t\sigma(u,X_u)\,dW_u\Big]=1,$$

with all integrals well defined.

**GBM is an SDE.** With $$\mu(t,x)=\mu x$$ and $$\sigma(t,x)=\sigma x$$: $$dX_t=\mu X_tdt+\sigma X_tdW_t$$, solved by $$X_t=x\,e^{(\mu-\frac12\sigma^2)t+\sigma W_t}$$ — the Black–Scholes stock.

**Existence and uniqueness.** Not every SDE has a solution, and few have closed-form ones. Under mild conditions one can prove existence and uniqueness; the key property is that $$\mu$$ and $$\sigma$$ are **Lipschitz** in space,

$$\vert f(x)-f(y)\vert\le K\vert x-y\vert,$$

just as for ordinary differential equations. In this course we always assume solutions exist.

## 2. The Ornstein–Uhlenbeck process

$$dX_t=\kappa(\theta-X_t)\,dt+\sigma\,dW_t,\qquad \kappa>0 .$$

$$\theta$$: long-run mean; $$\kappa$$: speed of mean reversion; $$\sigma$$: volatility. When $$X$$ is above $$\theta$$ the drift pulls it down, and vice versa — a spring with noise.

**Solving it.** (1) Assume a solution exists and find its form; (2) check it. Multiply by the integrating factor $$e^{\kappa t}$$ and apply Itô (product rule with a deterministic factor):

$$d(e^{\kappa t}X_t)=e^{\kappa t}\big(\kappa X_t\,dt+\kappa(\theta-X_t)\,dt+\sigma\,dW_t\big)=e^{\kappa t}\big(\kappa\theta\,dt+\sigma\,dW_t\big).$$

Integrate: $$e^{\kappa t}X_t=x+\theta(e^{\kappa t}-1)+\int_0^te^{\kappa u}\sigma\,dW_u$$, so

$$X_t=\theta+e^{-\kappa t}\Big((x-\theta)+\int_0^te^{\kappa u}\sigma\,dW_u\Big).$$

Check: differentiating, $$dX_t=-\kappa e^{-\kappa t}\big((x-\theta)+\int_0^te^{\kappa u}\sigma dW_u\big)dt+\sigma\,dW_t=\kappa(\theta-X_t)dt+\sigma dW_t$$. ✓

**Distribution.** The integrand $$e^{\kappa u}\sigma$$ is deterministic, so by Part 04 the stochastic integral is Gaussian, with variance from the Itô isometry:

$$e^{-\kappa t}\int_0^te^{\kappa u}\sigma\,dW_u\sim N\big(0,\sigma_t^2\big),\qquad \sigma_t^2=\sigma^2e^{-2\kappa t}\int_0^te^{2\kappa u}\,du=\frac{\sigma^2}{2\kappa}\big(1-e^{-2\kappa t}\big).$$

Hence

$$X_t\sim N\Big(\theta+e^{-\kappa t}(x-\theta),\ \frac{\sigma^2}{2\kappa}\big(1-e^{-2\kappa t}\big)\Big)\ \xrightarrow{t\to\infty}\ N\Big(\theta,\ \frac{\sigma^2}{2\kappa}\Big).$$

The starting point is forgotten exponentially fast; $$\theta$$ is the **long-run mean** and $$\sigma^2/(2\kappa)$$ the **long-run variance**.

**Time-dependent coefficients.** With non-random $$\kappa(t),\theta(t),\sigma(t)$$ (they may vary with $$t$$ but not with $$\omega$$), $$dX_t=\kappa(t)\big(\theta(t)-X_t\big)dt+\sigma(t)dW_t$$ is solved by

$$X_t=e^{-\int_0^t\kappa(u)du}\Big(x+\int_0^te^{\int_0^u\kappa(v)dv}\kappa(u)\theta(u)\,du+\int_0^te^{\int_0^u\kappa(v)dv}\sigma(u)\,dW_u\Big).$$

## 3. The Cox–Ingersoll–Ross process

$$dX_t=\kappa(\theta-X_t)\,dt+\xi\sqrt{X_t}\,dW_t,\qquad X_0=x>0 .$$

Same mean reversion as OU, but the instantaneous conditional variance is $$\xi^2X_t\,dt$$: noise shrinks as $$X$$ approaches $$0$$. The solution must stay positive for $$\sqrt{X_t}$$ to make sense; strict positivity holds if $$\kappa\theta\ge\frac12\xi^2$$ (the Feller condition). There is no closed-form solution, but a solution exists and the distribution of $$X_t$$ can be computed. It is a standard interest-rate and variance model.

## 4. The Markov property

Solutions of $$dX_t=\mu(t,X_t)dt+\sigma(t,X_t)dW_t$$ are **Markov**: to evaluate the future of the process at time $$t$$, knowing $$X_t$$ is enough — all other information in $$\mathcal F_t$$ is irrelevant. In practice:

$$E[g(X_u)\mid\mathcal F_t]=E[g(X_u)\mid X_t],\qquad E\Big[e^{-\int_t^Tg(X_u)du}\,\Big\vert\,\mathcal F_t\Big]=E\Big[e^{-\int_t^Tg(X_u)du}\,\Big\vert\,X_t\Big].$$

Why: at time $$t$$, the next step $$dX_t$$ is determined by $$X_t$$ (through $$\mu$$ and $$\sigma$$) and by $$dW_t$$, which is independent of $$\mathcal F_t$$. Expanding $$E[g(X_t+dX_t)\mid\mathcal F_t]$$ and $$E[g(X_t+dX_t)\mid X_t]$$ gives the same answer, step after step.

## 5. Feynman–Kac

Let $$dX_t=\mu(t,X_t)dt+\sigma(t,X_t)dW_t$$, let $$g$$ be a payoff and $$r(t,x)$$ a discount rate. Then

$$f(t,x):=E\Big[e^{-\int_t^Tr(u,X_u)du}\,g(X_T)\,\Big\vert\,X_t=x\Big],\qquad t\le T,\ x\in\mathbb R,$$

corresponds one-to-one with the PDE

$$\begin{cases}0=\Big(f_t+\mu f_x+\tfrac12\sigma^2f_{xx}-rf\Big)(t,x),&t<T,\ x\in\mathbb R,\\[4pt] f(T,x)=g(x).\end{cases}$$

Without discounting, $$u(t,x)=E[g(X_T)\mid X_t=x]$$ solves $$0=u_t+\mu u_x+\frac12\sigma^2u_{xx}$$ with $$u(T,x)=g(x)$$.

**Why it is true (one line).** By Markov, $$e^{-\int_0^tr\,du}f(t,X_t)=E\big[e^{-\int_0^Tr\,du}g(X_T)\mid\mathcal F_t\big]$$ is a martingale. Apply Itô to it: the $$dt$$ coefficient is $$e^{-\int_0^tr}\big(f_t+\mu f_x+\frac12\sigma^2f_{xx}-rf\big)$$, and a martingale has no $$dt$$ term.

**Black–Scholes is a special case.** Take $$X=S$$ under $$\mathbb Q$$, i.e. $$\mu(t,s)=rs$$, $$\sigma(t,s)=\sigma s$$, constant $$r$$, and $$g(s)=(s-K)^+$$. The PDE becomes exactly the Black–Scholes PDE of Part 05, and the expectation becomes the risk-neutral formula of Part 06. The two halves of the course — PDEs and expectations — are the same thing.

## 6. Dividend-paying stocks

If the stock pays a continuous dividend at rate $$a_t$$, its price drops by the dividend:

$$\frac{dS_t}{S_t}=\mu_t\,dt+\sigma_t\,dW_t-a_t\,dt .$$

**Wealth.** Holding $$\Delta_t$$ shares over $$[t,t+dt]$$, we also receive dividends $$\Delta_tS_ta_t\,dt$$:

$$X_{t+dt}=\Delta_tS_{t+dt}+\Delta_tS_ta_t\,dt+(X_t-\Delta_tS_t)\frac{B_{t+dt}}{B_t},$$

$$\begin{aligned}
dX_t&=\Delta_t\,dS_t+\Delta_tS_ta_t\,dt+(X_t-\Delta_tS_t)r_t\,dt\\
&=\Delta_tS_t\big((\mu_t-a_t)dt+\sigma_tdW_t\big)+\Delta_tS_ta_t\,dt+(X_t-\Delta_tS_t)r_t\,dt\\
&=r_tX_t\,dt+\Delta_t\sigma_tS_t\big(dW_t+\theta_t\,dt\big),\qquad\theta_t=\frac{\mu_t-r_t}{\sigma_t}.
\end{aligned}$$

The **wealth dynamics are the same as without dividends** (the dividend you receive offsets the price drop), but the **stock dynamics change**. So European option prices do change, because the stock price is different. Under $$\mathbb Q$$,

$$dX_t=r_tX_t\,dt+\Delta_tS_t\sigma_t\,dW^{\mathbb Q}_t,\qquad V_t=E^{\mathbb Q}\Big[e^{-\int_t^Tr_udu}V_T\,\Big\vert\,\mathcal F_t\Big],$$

and under $$\mathbb Q$$ the stock now drifts at $$r_t-a_t$$. In practice, write the payoff as a function of $$S_T=S_t\cdot\frac{S_T}{S_t}$$, freeze $$S_t=s$$, and compute — exactly as in the Black–Scholes derivation.

## 7. Stopping times

A random time $$\tau$$ is a **stopping time** if

$$\{\omega:\tau(\omega)\le t\}\in\mathcal F_t\quad\text{for all }t,$$

i.e. at every time you can tell from current information whether you have already stopped. Example: a non-random time $$\tau\equiv t_0$$, for which $$\{\tau\le t\}$$ is $$\Omega$$ if $$t_0\le t$$ and $$\varnothing$$ otherwise.

**Hitting times.** $$\tau(\omega)=\inf\{t:X_t(\omega)=\lambda\}$$, the first time $$X$$ hits level $$\lambda$$. For adapted $$X$$ with continuous paths, hitting times are stopping times. (By contrast, "the last time $$X$$ hits $$\lambda$$ before $$T$$" is not: you need the future to know it.)

**Stopped values and stopped processes.** $$X_\tau$$ is the random variable $$\omega\mapsto X_{\tau(\omega)}(\omega)$$ — e.g. $$X_1(\omega)$$ if $$\tau(\omega)=1$$, $$X_3(\omega)$$ if $$\tau(\omega)=3$$. The **stopped process** $$X^\tau$$ is

$$X^\tau_t(\omega):=X_{t\wedge\tau(\omega)}(\omega):$$

it follows $$X$$ until $$\tau$$ and then stays flat.

## 8. The optional sampling theorem

**Theorem (OST).** Let $$M$$ be a martingale and $$\alpha\le\tau$$ two **bounded** stopping times. Then

$$E[M_\tau\mid\mathcal F_\alpha]=M_\alpha .$$

**Corollaries** for a bounded stopping time $$\tau$$:

1. the stopped process $$t\mapsto M_{t\wedge\tau}$$ is a martingale;
2. $$E[M_\tau]=M_0$$;
3. if $$\tau\le T$$, the price of a claim at the random time $$\tau$$ is $$V_\tau=E^{\mathbb Q}\big[e^{-r(T-\tau)}V_T\mid\mathcal F_\tau\big]$$ — the pricing formula of Part 06 with $$t$$ replaced by $$\tau$$.

Intuition: a fair game stays fair even if you decide **when to look** using a rule that does not peek into the future. Boundedness matters: "play until you are ahead" is a stopping time, but an unbounded one, and with it $$E[M_\tau]>M_0$$ is possible.

---

**Series contents**

1. [01 · Probability Spaces, Filtrations, Conditional Expectation and Martingales]({{ site.baseurl }}{% post_url 2026-10-21-mf790-01-foundations %})
2. [02 · Random Walks, Quadratic Variation and Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-02-brownian-motion %})
3. [03 · Stochastic Integration]({{ site.baseurl }}{% post_url 2026-10-21-mf790-03-stochastic-integration %})
4. [04 · Itô's Formula, Itô Processes and Geometric Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-04-ito-formula %})
5. [05 · Black–Scholes: Replication, the PDE and the Greeks]({{ site.baseurl }}{% post_url 2026-10-21-mf790-05-black-scholes %})
6. [06 · Risk-Neutral Pricing, Change of Measure and Girsanov]({{ site.baseurl }}{% post_url 2026-10-21-mf790-06-risk-neutral-pricing %})
7. **07 · SDEs, Feynman–Kac and Stopping Times** (this post)

[← Previous: Risk-Neutral Pricing, Change of Measure and Girsanov]({{ site.baseurl }}{% post_url 2026-10-21-mf790-06-risk-neutral-pricing %})

*This series collects my notes from QST MF 790 Stochastic Calculus at Boston University's Questrom School of Business, in 7 parts. The content follows the order of the course; wording, intuition and a few corrections are my own. Throughout, $$N(\mu,\sigma^2)$$ uses the variance as its second argument. Corrections are welcome. Chinese version of this post: [中文版](https://bobothebest.github.io/homepage_cn/2026/10/21/mf790-07-sde-feynman-kac/).*
