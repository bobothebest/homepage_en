---
layout: post
title: "Stochastic Calculus Notes 02 | Random Walks, Quadratic Variation and Brownian Motion"
author: Ziyuan Zhang
tags:
- Stochastic Calculus Notes
- BU MF 790
- stochastic calculus
- random walk
- quadratic variation
- Brownian motion
- study notes
date: 2026-10-21 10:02 -0400
toc: true
math: true
---
> **Overview**
>
> Brownian motion is easiest to understand as the limit of a coin-tossing game. This post starts from the random walk, introduces the one quantity that makes stochastic calculus different from ordinary calculus — **quadratic variation** — and then shows that Brownian motion inherits everything: it is a martingale, $$W_t^2-t$$ is a martingale, and $$[W,W]_t=t$$.
>
> The punchline to keep in mind: **$$(dW_t)^2=dt$$.** Ordinary smooth curves have zero quadratic variation; Brownian paths do not, and that single fact is where Itô's extra term will come from.

---

## 1. The random walk

Take the infinite fair coin-toss space, $$\omega=(\omega_1,\omega_2,\dots)$$, with independent tosses. Define

$$Z_j(\omega)=\begin{cases}+1,&\omega_j=H\\-1,&\omega_j=T\end{cases},\qquad M_0=0,\quad M_n=\sum_{j=1}^nZ_j\ (n\ge1).$$

The $$Z_j$$ are i.i.d. with $$E Z_j=0$$, $$\mathrm{Var}(Z_j)=E Z_j^2=1$$. $$M$$ is the **random walk** and the $$Z_n=M_n-M_{n-1}$$ are its increments.

**Properties.**

1. **$$M$$ is a martingale.** $$\mathcal F_n$$ = first $$n$$ tosses; $$Z_{n+1}$$ only depends on toss $$n+1$$, so $$Z_{n+1}\perp\!\!\!\perp\mathcal F_n$$:

   $$E[M_{n+1}\mid\mathcal F_n]=E[M_n+Z_{n+1}\mid\mathcal F_n]=M_n+E[Z_{n+1}]=M_n.$$

2. **Independent increments.** For $$0\le m<n\le p<q$$, $$M_n-M_m=\sum_{j=m+1}^nZ_j$$ and $$M_q-M_p=\sum_{j=p+1}^qZ_j$$ use disjoint tosses, so they are independent.
3. **Increment mean and variance.** $$E[M_n-M_m]=0$$ and

   $$\mathrm{Var}(M_n-M_m)=\sum_{i,j=m+1}^nE[Z_iZ_j]=n-m,\qquad E[Z_iZ_j]=\begin{cases}1,&i=j\\0,&i\ne j.\end{cases}$$

## 2. The squared random walk

$$M_n^2$$ is **not** a martingale — it drifts up:

$$E[M_{n+1}^2\mid\mathcal F_n]=E[(M_n+Z_{n+1})^2\mid\mathcal F_n]=M_n^2+2M_nE[Z_{n+1}]+E[Z_{n+1}^2]=M_n^2+1.$$

So $$M^2$$ is a sub-martingale, and subtracting the drift fixes it: $$Y_n:=M_n^2-n$$ satisfies $$E[Y_{n+1}\mid\mathcal F_n]=M_n^2+1-(n+1)=Y_n$$, a **martingale**.

## 3. Quadratic variation (discrete time)

For a process $$X=\{X_n\}$$, the **quadratic variation** is the process

$$[X,X]_0=0,\qquad [X,X]_n=\sum_{j=1}^n\big(X_j-X_{j-1}\big)^2 .$$

It adds up squared increments and measures **how rough** the path is. (The definition does not need $$X$$ to be a martingale.)

- Random walk: every increment is $$\pm1$$, so $$[M,M]_n=n$$ for **every** $$\omega$$ — and this holds even for an unfair coin. This is special; in general $$[X,X]_n$$ depends on both $$n$$ and $$\omega$$.
- $$Y_n=M_n^2-n$$: using $$M_j=M_{j-1}+Z_j$$ and $$Z_j^2=1$$,

  $$[Y,Y]_n=\sum_{j=1}^n\big(Z_j^2+2Z_jM_{j-1}-1\big)^2=\sum_{j=1}^n4M_{j-1}^2,$$

  which genuinely varies with $$\omega$$.

### Two facts about martingales and quadratic variation

**Fact 1.** If $$X$$ is a martingale with $$X_0=0$$, then $$\mathrm{Var}(X_n)=E\big[[X,X]_n\big]$$.

<details markdown="1"><summary>Proof</summary>

Since $$E X_n=E X_0=0$$, $$\mathrm{Var}(X_n)=E X_n^2$$. Write $$X_n=\sum_j(X_j-X_{j-1})$$ and expand the square:

$$X_n^2=\sum_{i=1}^n(X_i-X_{i-1})^2+\sum_{i\ne j}(X_i-X_{i-1})(X_j-X_{j-1}).$$

For $$i<j$$, condition on $$\mathcal F_{j-1}$$ (Tower: condition on the second-largest time):

$$E[(X_i-X_{i-1})(X_j-X_{j-1})]=E\big[(X_i-X_{i-1})\,E[X_j-X_{j-1}\mid\mathcal F_{j-1}]\big]=E[(X_i-X_{i-1})\cdot0]=0.$$

So the cross terms vanish and $$E X_n^2=E[X,X]_n$$. For the random walk this reads $$\mathrm{Var}(M_n)=n=E[M,M]_n$$.

</details>

**Fact 2 (quadratic variation as compensator).** If $$X$$ is a martingale with $$E X_n^2<\infty$$, then $$\{X_n^2-[X,X]_n\}$$ is a martingale.

<details markdown="1"><summary>Proof</summary>

Let $$Y_n=X_n^2-[X,X]_n$$. Then

$$Y_n-Y_{n-1}=X_n^2-X_{n-1}^2-(X_n-X_{n-1})^2=2X_{n-1}(X_n-X_{n-1}),$$

and $$E[2X_{n-1}(X_n-X_{n-1})\mid\mathcal F_{n-1}]=2X_{n-1}\cdot0=0$$.

</details>

Since $$[X,X]_n$$ only goes up, $$X_n^2$$ itself is a sub-martingale; we must **compensate** (subtract) $$[X,X]$$ to get a martingale. That is why $$[X,X]$$ is also called the compensator. $$M_n^2-n$$ above is the special case $$[M,M]_n=n$$.

## 4. From random walk to Brownian motion

Random walk and Brownian motion share independent increments with mean $$0$$ and variance equal to the time elapsed. They differ in that $$M$$ lives in discrete time with integer jumps, while $$W$$ lives in continuous time and takes any real value.

**Scaled random walk.** Speed up time by $$n$$ and shrink jumps to $$1/\sqrt n$$:

$$W^{(n)}_t:=\frac1{\sqrt n}M_{\lfloor nt\rfloor}.$$

- **Independent increments:** inherited from $$M$$.
- **Right mean and variance:** for $$ns,nt$$ integers,

  $$E[W^{(n)}_t-W^{(n)}_s]=0,\qquad \mathrm{Var}[W^{(n)}_t-W^{(n)}_s]=\frac1n\,\mathrm{Var}[M_{nt}-M_{ns}]=\frac{nt-ns}{n}=t-s.$$

- **Gaussian in the limit (CLT):**

  $$W^{(n)}_t-W^{(n)}_s=\underbrace{\sqrt{\frac{\lfloor nt\rfloor-\lfloor ns\rfloor}{n}}}_{\to\sqrt{t-s}}\cdot\underbrace{\frac1{\sqrt{\lfloor nt\rfloor-\lfloor ns\rfloor}}\sum_{j=\lfloor ns\rfloor+1}^{\lfloor nt\rfloor}Z_j}_{\to N(0,1)\text{ by the CLT}}\ \Longrightarrow\ N(0,t-s).$$

So $$W^{(n)}\to W$$: **Brownian motion exists**, and it is literally a coin-tossing game watched from very far away. (Be careful which "$$n$$" the CLT is applied to — it is the number of tosses in the window, $$\lfloor nt\rfloor-\lfloor ns\rfloor$$.)

## 5. Brownian motion and its properties

Recall the definition from Part 01: $$W_0=0$$; continuous paths; $$W_t-W_s\sim N(0,t-s)$$; independent increments. $$W$$ is a BM **with respect to $$\mathbb F$$** if additionally it is adapted and $$W_t-W_s\perp\!\!\!\perp\mathcal F_s$$.

**1. Martingale.** Shown in Part 01. In continuous time, super/sub-martingales and the monotonicity of $$E X_t$$ work exactly as in discrete time.

**2. Covariance.** For $$s<t$$, add and subtract $$W_s$$ to create an independent increment:

$$\mathrm{Cov}(W_s,W_t)=E[W_sW_t]=E[W_s(W_t-W_s)]+E[W_s^2]=0+s=\min(s,t).$$

So $$(W_s,W_t)$$ is jointly normal with

$$\begin{pmatrix}W_s\\W_t\end{pmatrix}\sim N\left(\begin{pmatrix}0\\0\end{pmatrix},\begin{pmatrix}s&s\\s&t\end{pmatrix}\right).$$

**3. Conditional law and the Markov property.** Using $$E[e^{\alpha Y}]=e^{\alpha\mu+\frac12\alpha^2\sigma^2}$$ for $$Y\sim N(\mu,\sigma^2)$$:

$$E[e^{\alpha W_t}\mid\mathcal F_s]=e^{\alpha W_s}\,E[e^{\alpha(W_t-W_s)}]=e^{\alpha W_s+\frac12\alpha^2(t-s)}\ \Longrightarrow\ W_t\mid\mathcal F_s\sim N(W_s,\ t-s).$$

Given everything up to time $$s$$, the law of $$W_t$$ depends only on $$W_s$$ — the **Markov property**. The **transition density** is

$$p(\tau,y,x)=\frac1{\sqrt{2\pi\tau}}\,e^{-\frac{(x-y)^2}{2\tau}},\qquad E[h(W_{s+\tau})\mid W_s=y]=\int_{\mathbb R}h(x)\,p(\tau,y,x)\,dx,$$

read as "go from $$y$$ to near $$x$$ in time $$\tau$$".

**4. $$W_t^2-t$$ is a martingale.** With $$W_t^2=(W_t-W_s)^2+2(W_t-W_s)W_s+W_s^2$$:

$$E[W_t^2-t\mid\mathcal F_s]=(t-s)+0+W_s^2-t=W_s^2-s.$$

This is the continuous-time version of $$M_n^2-n$$.

## 6. Quadratic variation in continuous time

Fix $$t$$ and a partition $$\Pi=\{0=t_0<t_1<\dots<t_n=t\}$$ with mesh $$\Vert \Pi\Vert =\max_j(t_j-t_{j-1})$$. Define

$$[X,X]^\Pi_t=\sum_{j=1}^n\big(X_{t_j}-X_{t_{j-1}}\big)^2,\qquad [X,X]_t=\lim_{\Vert \Pi\Vert \to0}[X,X]^\Pi_t .$$

Over a window, $$[X,X]_{[s,t]}$$ is defined the same way and quadratic variation accumulates: $$[X,X]_t=[X,X]_s+[X,X]_{[s,t]}$$.

**Theorem.** $$[W,W]_t=t$$ with probability one, and $$[W,W]_{[s,t]}=t-s$$.

*Intuition.* For the scaled walk, $$[W^{(n)},W^{(n)}]_t=\frac1n\lfloor nt\rfloor\in[t-\frac1n,t]$$, and $$W^{(n)}\to W$$.

<details markdown="1"><summary>Proof sketch</summary>

Take the uniform partition $$t_j=jt/n$$. Then

$$[W,W]^\Pi_t=\frac tn\sum_{j=1}^n\left(\frac{W_{t_j}-W_{t_{j-1}}}{\sqrt{t_j-t_{j-1}}}\right)^2=t\cdot\frac1n\sum_{j=1}^nZ_j^2,$$

with $$Z_j$$ i.i.d. $$N(0,1)$$. By the strong law of large numbers, $$\frac1n\sum Z_j^2\to E[Z^2]=1$$, so $$[W,W]^\Pi_t\to t$$. For a general partition one shows $$E\big[[W,W]^\Pi_t\big]=t$$ and $$\mathrm{Var}\big[[W,W]^\Pi_t\big]\le K\Vert \Pi\Vert $$ with $$K$$ depending only on $$t$$, which forces convergence to $$t$$.

</details>

Fact 2 carries over: if $$X$$ is a continuous martingale with $$E X_t^2<\infty$$, then $$X_t^2-[X,X]_t$$ is a martingale (proof needs Itô's formula, Part 04).

## 7. Why Brownian paths must be rough

The **first-order variation** of a path is $$FV(X)_t=\lim_{\Vert \Pi\Vert \to0}\sum_j\vert X_{t_j}-X_{t_{j-1}}\vert$$. For a smooth path, the mean value theorem gives

$$FV(X)_t=\lim\sum_j(t_j-t_{j-1})\vert X'(\xi_j)\vert=\int_0^t\vert X'_s\vert\,ds<\infty.$$

**Finite first variation forces zero quadratic variation** (for continuous paths):

$$\sum_j(X_{t_j}-X_{t_{j-1}})^2\le\underbrace{\max_j\vert X_{t_j}-X_{t_{j-1}}\vert}_{\to0\ \text{(continuity)}}\cdot\underbrace{\sum_j\vert X_{t_j}-X_{t_{j-1}}\vert}_{\to FV(X)_t<\infty}\ \to0.$$

Since $$[W,W]_t=t>0$$, Brownian paths cannot have finite first variation, and in fact they are **nowhere differentiable** with probability one.

This roughness is a feature, not a bug: a smooth price path would be locally predictable, and predictability means arbitrage.

**The heuristic everyone uses.** Differentiating $$[W,W]_t=t$$ gives $$d[W,W]_t=dt$$, and people write

$$(dW_t)^2=dt,$$

thinking of $$[W,W]_{[t,t+dt]}\approx\sum(W_{t_j}-W_{t_{j-1}})^2$$. Taking a square root to say "$$dW_t=\sqrt{dt}$$" is meaningless, but the multiplication rule $$(dW_t)^2=dt$$ is exactly right and drives all of Part 04.

---

**Series contents**

1. [01 · Probability Spaces, Filtrations, Conditional Expectation and Martingales]({{ site.baseurl }}{% post_url 2026-10-21-mf790-01-foundations %})
2. **02 · Random Walks, Quadratic Variation and Brownian Motion** (this post)
3. [03 · Stochastic Integration]({{ site.baseurl }}{% post_url 2026-10-21-mf790-03-stochastic-integration %})
4. [04 · Itô's Formula, Itô Processes and Geometric Brownian Motion]({{ site.baseurl }}{% post_url 2026-10-21-mf790-04-ito-formula %})
5. [05 · Black–Scholes: Replication, the PDE and the Greeks]({{ site.baseurl }}{% post_url 2026-10-21-mf790-05-black-scholes %})
6. [06 · Risk-Neutral Pricing, Change of Measure and Girsanov]({{ site.baseurl }}{% post_url 2026-10-21-mf790-06-risk-neutral-pricing %})
7. [07 · SDEs, Feynman–Kac and Stopping Times]({{ site.baseurl }}{% post_url 2026-10-21-mf790-07-sde-feynman-kac %})

[← Previous: Probability Spaces, Filtrations, Conditional Expectation and Martingales]({{ site.baseurl }}{% post_url 2026-10-21-mf790-01-foundations %})　·　[Next: Stochastic Integration →]({{ site.baseurl }}{% post_url 2026-10-21-mf790-03-stochastic-integration %})

*This series collects my notes from QST MF 790 Stochastic Calculus at Boston University's Questrom School of Business, in 7 parts. The content follows the order of the course; wording, intuition and a few corrections are my own. Throughout, $$N(\mu,\sigma^2)$$ uses the variance as its second argument. Corrections are welcome. Chinese version of this post: [中文版](https://bobothebest.github.io/homepage_cn/2026/10/21/mf790-02-brownian-motion/).*
