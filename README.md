# Intro to Mathematical Finance — Midsem Targeted Notes

This note is written **only for the midsem topics you said you want to study**. It is meant to be **self-contained**, so you should be able to read it without needing the original slides open beside you.

The four topics covered are:

1. **Binomial option pricing**
2. **Option payoff graphs and option combinations**
3. **Markov chains**
4. **The umbrella problem**

The goal of this note is not just to give formulas. The goal is to make the ideas feel natural.

---

# 1. Binomial Option Pricing

## 1.1 What is an option?

An **option** is a contract that gives you a **choice in the future**.

Usually:
- a **call option** gives you the right to **buy** an asset later at a fixed price,
- a **put option** gives you the right to **sell** an asset later at a fixed price.

You are **not forced** to use the contract. You use it only if it helps you.

### Simple example
Suppose you have a call option that lets you buy a stock for ₹100 after one month.

- If the stock price after one month is ₹130, the option is useful because you can buy for ₹100 something worth ₹130.
- If the stock price after one month is ₹80, you simply do not use the option.

So an option has value because it gives you **favorable flexibility**.

---

## 1.2 What does “pricing an option” mean?

Pricing an option means answering this question:

> What is the fair amount someone should pay **today** for that future right?

At first this sounds difficult, because the future stock price is uncertain.

The big idea in mathematical finance is:

> Instead of guessing what the option is worth, we try to **copy its future payoff** using ordinary traded assets.

If we can copy the payoff exactly, then the option must cost the same as the cost of making that copy.

That is the heart of the method.

---

## 1.3 The core principle: no-arbitrage

This is the single most important idea in the entire topic.

> If two portfolios give exactly the same payoff in every possible future state, then they must have the same price today.

Why?

Because if they had different prices, people could make a **riskless profit**, called **arbitrage**.

### Very simple intuition
Imagine two things that will definitely give you the exact same amount tomorrow.

- Item A costs ₹100 today.
- Item B costs ₹80 today.

Then nobody should buy A. They would just buy B and get the same future result for less money.

So in a market with no arbitrage, equal future payoffs force equal present prices.

---

## 1.4 What is a replicating portfolio?

A **replicating portfolio** is a portfolio made from basic traded assets that is designed to produce **exactly the same payoff as the option** at maturity.

In the binomial model, the basic ingredients are:
- some number $\Delta$ of shares of the stock, and
- some amount invested in, or borrowed from, the **risk-free asset**.

Here:
- $\Delta$ is called **Delta**,
- the risk-free asset means something that grows at a known interest rate.

### Layman interpretation
Think of the option as a product, and the replicating portfolio as a **homemade version** of the same product.

If the homemade version and the market version do the exact same thing in every future case, then they should have the same cost.

---

## 1.5 What is the binomial model?

The binomial model is the simplest stock price model.

It assumes that over one time step, the stock can move to only **two possible values**:

- it goes **up**, or
- it goes **down**.

If the current stock price is $S_0$, then after one time step the stock price becomes either:

- $S_0 u$ in the up state,
- $S_0 d$ in the down state,

where:
- $u$ is the **up factor**,
- $d$ is the **down factor**.

Usually $u > 1$ and $0 < d < 1$.

### Example
If $S_0 = 100$, $u = 1.2$, and $d = 0.8$, then after one step the stock is either:

- $100 \times 1.2 = 120$,
- $100 \times 0.8 = 80$.

That is why it is called **binomial**: there are two possibilities.

---

## 1.6 The one-period model setup

Suppose:

- current stock price is $S_0$,
- up factor is $u$,
- down factor is $d$,
- time to maturity is $T$,
- continuously compounded risk-free rate is $r$.

Then at time $T$ the stock price is either:

- $S_u = S_0 u$,
- $S_d = S_0 d$.

Suppose the option payoff at time $T$ is:

- $V_u$ in the up state,
- $V_d$ in the down state.

We want to find the option price today, denoted by $V_0$.

---

## 1.7 How replication works

We try to build a portfolio with:

- $\Delta$ shares of stock,
- and an amount $B$ in the risk-free asset.

If $B>0$, it means money is invested safely.

If $B<0$, it means money is borrowed.

### Portfolio value at maturity
At time $T$, the bank amount becomes $B e^{rT}$ if interest is continuously compounded.

So the portfolio value at maturity is:

- in the up state: $\Delta S_0 u + B e^{rT}$,
- in the down state: $\Delta S_0 d + B e^{rT}$.

To replicate the option, we force these values to equal the option payoff:

$$
\Delta S_0 u + B e^{rT} = V_u
$$

$$
\Delta S_0 d + B e^{rT} = V_d
$$

These are the two replication equations.

---

## 1.8 Solving for Delta and the bond position

Subtract the second equation from the first:

$$
\Delta S_0(u-d) = V_u - V_d
$$

So:

$$
\Delta = \frac{V_u - V_d}{S_0(u-d)}
$$

This tells you how many shares are needed.

Then solve for $B$ using either replication equation:

$$
B = e^{-rT}(V_u - \Delta S_0 u)
$$

or equivalently

$$
B = e^{-rT}(V_d - \Delta S_0 d)
$$

Once you know $\Delta$ and $B$, the option price today must be:

$$
V_0 = \Delta S_0 + B
$$

That is the cost of the replicating portfolio.

---

## 1.9 Why this gives the correct option price

This is the important intuition.

- The option gives some payoff in the up state and some payoff in the down state.
- The replicating portfolio gives the **same** payoff in the up state and the **same** payoff in the down state.
- So by maturity, they are financially identical.

Therefore, **today** they must have the same value.

You do **not** build the replicating portfolio because you necessarily want to trade it.

You build it because it tells you the fair price of the option.

### One-line intuition
The option is worth **whatever it costs to copy its payoff**.

---

## 1.10 Risk-neutral pricing formula

The replication argument leads to a shorter pricing formula.

Define the **risk-neutral probability** $q$ by:

$$
q = \frac{e^{rT} - d}{u-d}
$$

Then the option price can be written as:

$$
V_0 = e^{-rT}\big(qV_u + (1-q)V_d\big)
$$

This looks like:

> discounted expected payoff

but the expectation is taken under the **risk-neutral probability**, not the real-world probability.

### Important warning
$q$ is usually **not** the true probability that the stock goes up.

It is a mathematical probability chosen so that prices become consistent with no-arbitrage.

---

## 1.11 No-arbitrage condition

For the one-period model to make sense, we need:

$$
d < e^{rT} < u
$$

This says that the risk-free growth factor must lie strictly between the stock’s down and up growth factors.

This condition guarantees:

$$
0 < q < 1
$$

which is exactly what we need if $q$ is to behave like a probability.

---

## 1.12 Worked example: one-period call option

Suppose:

- $S_0 = 100$
- $u = 2$
- $d = \frac{1}{2}$
- $r = 0.10$ (continuous compounding)
- $T = 1$
- strike price $K = 110$

We want the price of a **call option**.

### Step 1: stock prices at maturity

The stock can become:

$$
S_u = 100 \times 2 = 200
$$

$$
S_d = 100 \times \frac{1}{2} = 50
$$

### Step 2: call payoffs at maturity
A call payoff is:

$$
(S_T - K)^+
$$

So:

$$
V_u = \max(200-110,0) = 90
$$

$$
V_d = \max(50-110,0) = 0
$$

---

### Method A: replication

Compute Delta:

$$
\Delta = \frac{V_u - V_d}{S_0(u-d)} = \frac{90-0}{100\left(2-\frac12\right)} = \frac{90}{150} = 0.6
$$

So we need **0.6 share of stock**.

Now solve for $B$:

$$
B = e^{-0.1}(90 - 0.6\times 200)
$$

$$
B = e^{-0.1}(90 - 120) = -30e^{-0.1}
$$

Since $e^{-0.1} \approx 0.904837$,

$$
B \approx -27.145
$$

So the portfolio is:

- buy $0.6$ share,
- borrow about $27.145$.

Today’s cost is:

$$
V_0 = \Delta S_0 + B = 0.6 \times 100 - 27.145 = 32.855
$$

Hence the call option price is approximately:

$$
V_0 \approx 32.86
$$

---

### Method B: risk-neutral pricing

Compute $q$:

$$
q = \frac{e^{0.1} - \frac12}{2-\frac12}
$$

Using $e^{0.1} \approx 1.105170$,

$$
q \approx \frac{1.105170 - 0.5}{1.5} \approx 0.403447
$$

Now price the option:

$$
V_0 = e^{-0.1}\big(q\cdot 90 + (1-q)\cdot 0\big)
$$

$$
V_0 \approx 0.904837 \times 90 \times 0.403447 \approx 32.855
$$

Same answer, as expected.

---

## 1.13 Worked example: one-period put option

Using the same stock model, now price a **put option** with strike $K=110$.

A put payoff is:

$$
(K-S_T)^+
$$

So:

$$
V_u = \max(110-200,0)=0
$$

$$
V_d = \max(110-50,0)=60
$$

Now apply the risk-neutral formula:

$$
P_0 = e^{-0.1}\big(q\cdot 0 + (1-q)\cdot 60\big)
$$

With $q \approx 0.403447$,

$$
P_0 \approx 0.904837 \times 60 \times 0.596553 \approx 32.39
$$

So the put price is approximately:

$$
P_0 \approx 32.39
$$

---

## 1.14 Two-period binomial model

Now suppose the stock can move up or down **twice**.

Starting from $S_0$:

- after one step it becomes either $S_0u$ or $S_0d$,
- after two steps the terminal values are:
  - $S_0u^2$,
  - $S_0ud$,
  - $S_0d^2$.

The middle value appears from two paths:
- up then down,
- down then up.

This is called a **recombining tree**.

---

## 1.15 Backward induction

For two-period binomial pricing, the standard method is:

1. Draw the stock price tree.
2. Compute the option payoff at the final time.
3. Move backward one step at a time.
4. At each node, apply the one-step risk-neutral formula.

If one step has length $\Delta t$, then at each node:

$$
V = e^{-r\Delta t}\big(qV_u + (1-q)V_d\big)
$$

This method is called **backward induction**.

---

## 1.16 Worked example: two-period call option

Suppose:

- $S_0 = 100$
- $u = 2$
- $d = \frac12$
- each period is 1 year
- $r = 0.10$ continuously compounded
- strike $K = 110$
- maturity is 2 years

### Step 1: stock tree

At time 0:

$$
100
$$

At time 1:

$$
200,\; 50
$$

At time 2:

$$
400,\; 100,\; 25
$$

### Step 2: terminal call payoffs

$$
\max(400-110,0)=290
$$

$$
\max(100-110,0)=0
$$

$$
\max(25-110,0)=0
$$

So terminal option values are:

$$
290,\; 0,\; 0
$$

### Step 3: compute one-step risk-neutral probability

$$
q = \frac{e^{0.1}-\frac12}{2-\frac12} \approx 0.403447
$$

### Step 4: move backward

At the upper node at time 1 (stock = 200):

$$
V_{\text{up}} = e^{-0.1}\big(q\cdot 290 + (1-q)\cdot 0\big)
$$

$$
V_{\text{up}} \approx 0.904837 \times 290 \times 0.403447 \approx 105.88
$$

At the lower node at time 1 (stock = 50), both future payoffs are zero, so:

$$
V_{\text{down}} = 0
$$

Now go back to time 0:

$$
V_0 = e^{-0.1}\big(qV_{\text{up}} + (1-q)V_{\text{down}}\big)
$$

$$
V_0 = e^{-0.1}qV_{\text{up}}
$$

$$
V_0 \approx 0.904837 \times 0.403447 \times 105.88 \approx 38.72
$$

So the option price is approximately:

$$
V_0 \approx 38.72
$$

---

## 1.17 Direct terminal shortcut in this example

Because only the $uu$ path leads to a positive payoff, we can also write:

$$
V_0 = e^{-2r}\cdot 290 \cdot q^2
$$

So:

$$
V_0 \approx e^{-0.2}\cdot 290 \cdot (0.403447)^2 \approx 38.72
$$

Same answer.

This shortcut works nicely when very few terminal states have nonzero payoff.

---

## 1.18 Discrete compounding versus continuous compounding

This is a common place where students lose marks.

If the question says interest is **continuously compounded**, then use $e^{r\Delta t}$.

If the question says interest is **compounded discretely**, then use the corresponding discrete growth factor.

### Example
If the annual rate is 10% compounded every 6 months, then the effective one-year growth factor is:

$$
\left(1+\frac{0.10}{2}\right)^2 = 1.1025
$$

Then in the formula for $q$, you use:

$$
q = \frac{1.1025-d}{u-d}
$$

not $e^{0.1}$.

Always match the formula to the interest convention given in the question.

---

## 1.19 Chooser option

A **chooser option** lets the holder decide at some intermediate date whether the contract will continue as a call or as a put.

This sounds fancy, but the method is straightforward.

### Main idea
At the choice date:

1. compute the value of the remaining call,
2. compute the value of the remaining put,
3. choose the larger one.

Then continue backward in the tree.

So at the choice node:

$$
W = \max(C,P)
$$

where:
- $C$ is the call value at that node,
- $P$ is the put value at that node.

---

## 1.20 Worked example: chooser option

Suppose:

- $S_0 = 100$
- $u = 1.25$
- $d = 0.75$
- $r = 0.05$
- strike $K = 100$
- choice is made at $t=1$
- final maturity is $t=2$

### Step 1: stock tree

At time 0:

$$
100
$$

At time 1:

$$
125,\; 75
$$

At time 2:

- from 125, next prices are $156.25$ and $93.75$,
- from 75, next prices are $93.75$ and $56.25$.

So the terminal prices are:

$$
156.25,\; 93.75,\; 56.25
$$

### Step 2: compute $q$

$$
q = \frac{e^{0.05}-0.75}{1.25-0.75}
$$

Using $e^{0.05} \approx 1.051271$,

$$
q \approx \frac{1.051271-0.75}{0.5} \approx 0.60254
$$

### Step 3: value at the upper node $S=125$

From 125, the next stock prices are 156.25 and 93.75.

#### Call values at maturity

$$
\max(156.25-100,0)=56.25
$$

$$
\max(93.75-100,0)=0
$$

So the call value at stock 125 is:

$$
C(125)=e^{-0.05}\big(q\cdot 56.25 + (1-q)\cdot 0\big)
$$

$$
C(125) \approx 32.24
$$

#### Put values at maturity

$$
\max(100-156.25,0)=0
$$

$$
\max(100-93.75,0)=6.25
$$

So the put value at stock 125 is:

$$
P(125)=e^{-0.05}\big(q\cdot 0 + (1-q)\cdot 6.25\big)
$$

$$
P(125) \approx 2.36
$$

So the chooser value at stock 125 is:

$$
W(125)=\max\big(C(125),P(125)\big)=32.24
$$

### Step 4: value at the lower node $S=75$

From 75, the next stock prices are 93.75 and 56.25.

#### Call values at maturity

$$
\max(93.75-100,0)=0
$$

$$
\max(56.25-100,0)=0
$$

So:

$$
C(75)=0
$$

#### Put values at maturity

$$
\max(100-93.75,0)=6.25
$$

$$
\max(100-56.25,0)=43.75
$$

Thus:

$$
P(75)=e^{-0.05}\big(q\cdot 6.25 + (1-q)\cdot 43.75\big)
$$

$$
P(75) \approx 20.08
$$

So the chooser value at stock 75 is:

$$
W(75)=\max(0,20.08)=20.08
$$

### Step 5: move back to time 0

Now the chooser values after one year are:

- upper node: $32.24$
- lower node: $20.08$

Therefore:

$$
W_0=e^{-0.05}\big(q\cdot 32.24 + (1-q)\cdot 20.08\big)
$$

$$
W_0 \approx 26.03
$$

So the chooser option price is approximately:

$$
W_0 \approx 26.03
$$

---

## 1.21 Exam checklist for binomial option pricing

When you see a binomial pricing question, follow this order:

1. Write down $S_0$, $u$, $d$, $r$, strike, and maturity.
2. Draw the stock tree carefully.
3. Compute terminal option payoffs.
4. Compute $q$ with the correct interest convention.
5. Use backward induction if there are multiple steps.
6. State the final numerical answer clearly.

### Common mistakes
- forgetting the discount factor,
- mixing up call and put payoffs,
- using $e^r$ when the question uses discrete compounding,
- writing the tree incorrectly,
- making arithmetic mistakes in the middle node of a two-period tree.

---

# 2. Option Payoff Graphs and Option Combinations

## 2.1 What is a payoff graph?

A **payoff graph** shows how much a strategy pays at maturity as a function of the terminal stock price $S_T$.

It does **not** usually show the initial premium paid unless the question explicitly asks for **profit** instead of **payoff**.

So always check whether the question asks for:
- **payoff**, or
- **profit**.

Payoff ignores the initial option price.
Profit = payoff minus initial cost.

---

## 2.2 Basic building blocks

You should first know the shape of the simplest options.

### Long call
Payoff:

$$
(S_T-K)^+
$$

This means:
- if $S_T \le K$, payoff is 0,
- if $S_T > K$, payoff is $S_T-K$.

So the graph is:
- flat at 0 until $K$,
- then rises with slope 1.

### Long put
Payoff:

$$
(K-S_T)^+
$$

This means:
- if $S_T \ge K$, payoff is 0,
- if $S_T < K$, payoff is $K-S_T$.

So the graph is:
- downward sloping until $K$,
- flat at 0 after $K$.

### Short call and short put
A short position just flips the payoff sign.

- short call payoff is $-(S_T-K)^+$,
- short put payoff is $-(K-S_T)^+$.

So once you understand the long version, the short version is the same graph reflected across the horizontal axis.

---

## 2.3 Straddle

A **straddle** is:

- long one call with strike $K$,
- long one put with strike $K$.

The total payoff is:

$$
(S_T-K)^+ + (K-S_T)^+
$$

This simplifies to:

$$
|S_T-K|
$$

### Piecewise form

$$
|S_T-K|=
\begin{cases}
K-S_T, & S_T<K \\
0, & S_T=K \\
S_T-K, & S_T>K
\end{cases}
$$

### Intuition
A straddle benefits when the stock moves **a lot**, either upward or downward.

That is why people say a straddle is a **volatility bet**.

### Shape
The payoff graph is a **V-shape** with the lowest point at $S_T=K$.

---

## 2.4 Worked example: straddle with strike 100

Suppose $K=100$.

Then the payoff is:

$$
|S_T-100|
$$

Some values are:

- if $S_T=60$, payoff is 40,
- if $S_T=80$, payoff is 20,
- if $S_T=100$, payoff is 0,
- if $S_T=120$, payoff is 20,
- if $S_T=150$, payoff is 50.

So the graph falls linearly toward 0 at 100 and then rises linearly after 100.

---

## 2.5 Long call plus short put with the same strike

This combination appears often because it simplifies beautifully.

Payoff:

$$
(S_T-K)^+ - (K-S_T)^+
$$

This becomes:

$$
S_T-K
$$

for **all** values of $S_T$.

### Why?

#### Case 1: $S_T \ge K$
Then:
- call payoff = $S_T-K$,
- put payoff = 0,

so total = $S_T-K$.

#### Case 2: $S_T < K$
Then:
- call payoff = 0,
- put payoff = $K-S_T$,

so total =

$$
0-(K-S_T)=S_T-K
$$

Same answer again.

### Shape
A straight line with slope 1 crossing zero at $S_T=K$.

### Interpretation
This behaves like a **forward contract payoff**.

That identity is very useful.

---

## 2.6 Worked example: long call + short put with strike 100

If $K=100$, the payoff is simply:

$$
S_T-100
$$

So:

- if $S_T=60$, payoff is $-40$,
- if $S_T=100$, payoff is $0$,
- if $S_T=140$, payoff is $40$.

Graph: a straight line rising from left to right.

---

## 2.7 Bull spread using two calls

A standard bull spread is created by:

- buying one call with lower strike $K_1$,
- selling one call with higher strike $K_2$,
- where $K_2 > K_1$.

Payoff:

$$
(S_T-K_1)^+ - (S_T-K_2)^+
$$

### Case-by-case analysis

#### Case 1: $S_T \le K_1$
Both options expire worthless:

$$
\text{payoff}=0
$$

#### Case 2: $K_1 < S_T \le K_2$
The lower-strike call is active but the higher-strike call is still worthless:

$$
\text{payoff}=S_T-K_1
$$

#### Case 3: $S_T > K_2$
Both options are active:

$$
\text{payoff}=(S_T-K_1)-(S_T-K_2)=K_2-K_1
$$

### Piecewise form

$$
\text{payoff}=
\begin{cases}
0, & S_T \le K_1 \\
S_T-K_1, & K_1 < S_T \le K_2 \\
K_2-K_1, & S_T > K_2
\end{cases}
$$

### Shape
- flat at 0,
- then rising,
- then flat again.

### Interpretation
This is a **limited bullish strategy**.

You make money if the stock rises, but the gain is capped.

---

## 2.8 Worked example: bull spread with $K_1=100$ and $K_2=130$

Then:

$$
\text{payoff}=
\begin{cases}
0, & S_T \le 100 \\
S_T-100, & 100 < S_T \le 130 \\
30, & S_T > 130
\end{cases}
$$

Sample values:

- if $S_T=90$, payoff is 0,
- if $S_T=110$, payoff is 10,
- if $S_T=125$, payoff is 25,
- if $S_T=150$, payoff is 30.

---

## 2.9 How to draw payoff graphs in exams

The easiest method is:

1. identify the strike points where the formula changes,
2. break the line into regions,
3. compute the payoff formula in each region,
4. test 2 or 3 values to verify the shape.

### Common mistakes
- drawing profit when the question asked for payoff,
- forgetting that short positions are negative,
- missing the kink point at the strike,
- not writing the piecewise formula.

---

# 3. Markov Chains

## 3.1 What is a Markov chain?

A **Markov chain** is a model for a system that moves randomly from one state to another.

The important idea is:

> The future depends only on the present state, not on the full past history.

This is called the **Markov property**.

### Everyday example
Suppose the weather tomorrow depends only on today’s weather:
- if today is sunny, tomorrow is sunny with probability 0.7,
- if today is rainy, tomorrow is sunny with probability 0.4.

Then weather can be modeled as a Markov chain.

We do not care about whether it rained 5 days ago. We only care about the current state.

---

## 3.2 States and state space

A **state** is one possible situation the system can be in.

The **state space** is the full list of all possible states.

### Example
If a system can be in one of three conditions:

- state 1,
- state 2,
- state 3,

then the state space is:

$$
\{1,2,3\}
$$

The hardest part of many Markov chain questions is often **choosing the right state definition**.

---

## 3.3 Transition probabilities

A Markov chain moves from one state to another with certain probabilities.

These are called **transition probabilities**.

If $p_{ij}$ is the probability of moving from state $i$ to state $j$ in one step, then all these numbers form the **transition matrix**.

---

## 3.4 Transition matrix

A transition matrix is usually denoted by $P$.

For a 2-state chain, it looks like:

$$
P=
\begin{pmatrix}
p_{11} & p_{12} \\
p_{21} & p_{22}
\end{pmatrix}
$$

Each row must sum to 1, because from a given state the process must go somewhere.

---

## 3.5 Worked example: a 2-state chain

Suppose there are two states, $A$ and $B$.

- From $A$, the chain stays in $A$ with probability 0.7 and moves to $B$ with probability 0.3.
- From $B$, the chain moves to $A$ with probability 0.4 and stays in $B$ with probability 0.6.

Then:

$$
P=
\begin{pmatrix}
0.7 & 0.3 \\
0.4 & 0.6
\end{pmatrix}
$$

Interpretation:
- first row describes what happens if you are currently in $A$,
- second row describes what happens if you are currently in $B$.

---

## 3.6 $n$-step probabilities

If you want probabilities after several steps, you use powers of the transition matrix:

$$
P^2,\; P^3,\; \dots
$$

The entry $(i,j)$ of $P^n$ gives the probability of being in state $j$ after $n$ steps when starting from state $i$.

---

## 3.7 Irreducible and reducible chains

A Markov chain is **irreducible** if every state can be reached from every other state, perhaps after several steps.

It is **reducible** if some states are cut off from others.

### Intuition
Irreducible means the chain is one connected world.
Reducible means the chain has separate parts or one-way traps.

---

## 3.8 Periodic and aperiodic chains

A state is **periodic** if the chain can return to it only at times that are multiples of some integer greater than 1.

A chain is **aperiodic** if this kind of forced cycle does not happen.

### Simple example
If a chain jumps back and forth between two states forever, then returns happen only after even numbers of steps, so the period is 2.

That chain is periodic.

Aperiodicity is important when discussing long-run behavior.

---

## 3.9 Stationary distribution

A **stationary distribution** is a probability vector $\pi$ such that:

$$
\pi P = \pi
$$

and the entries of $\pi$ add up to 1.

### Meaning
If the chain starts with distribution $\pi$, then after one step it still has distribution $\pi$.

So $\pi$ is an equilibrium distribution.

In many irreducible and aperiodic chains, the stationary distribution also describes the **long-run proportion of time** spent in each state.

---

## 3.10 Worked example: stationary distribution of a 2-state chain

Use the same matrix:

$$
P=
\begin{pmatrix}
0.7 & 0.3 \\
0.4 & 0.6
\end{pmatrix}
$$

Let:

$$
\pi=(x,1-x)
$$

The stationary condition is:

$$
(x,1-x)
\begin{pmatrix}
0.7 & 0.3 \\
0.4 & 0.6
\end{pmatrix}
=(x,1-x)
$$

Using the first coordinate:

$$
0.7x + 0.4(1-x) = x
$$

$$
0.7x + 0.4 - 0.4x = x
$$

$$
0.3x + 0.4 = x
$$

$$
0.4 = 0.7x
$$

$$
x = \frac{4}{7}
$$

Hence:

$$
\pi = \left(\frac{4}{7},\frac{3}{7}\right)
$$

### Interpretation
In the long run:
- the chain spends about $4/7$ of the time in state $A$,
- and about $3/7$ of the time in state $B$.

---

## 3.11 How Markov chain questions usually look in exams

Common exam tasks are:

- define the state space,
- write the transition matrix,
- say whether the chain is irreducible,
- say whether it is periodic or aperiodic,
- compute a stationary distribution,
- interpret the stationary distribution.

So when reading a problem, train yourself to ask:

1. What are the states?
2. What is the one-step transition rule?
3. What is the transition matrix?
4. What long-run quantity are they asking for?

---

# 4. The Umbrella Problem

## 4.1 Why the umbrella problem matters

The umbrella problem is a classic example of how to turn a real-life story into a Markov chain.

It is important because it tests several skills at once:

- defining the correct state,
- building the transition matrix,
- understanding long-run behavior,
- using the stationary distribution to answer a practical question.

---

## 4.2 The story behind the problem

A person travels back and forth between two locations, usually home and office.

There are $n$ umbrellas in total, split between the two places.

Whenever the person is about to leave one location:
- if it is raining and an umbrella is available there, they carry one,
- if it is not raining, they do not carry one,
- if it is raining but there is no umbrella there, they get wet.

Each trip has rain with probability $p$.

The key question is usually:

> In the long run, what is the probability that the person gets wet?

---

## 4.3 The correct state definition

The standard state is:

> the number of umbrellas at the person’s current location just before a trip.

Why is this the right state?

Because whether the person gets wet on the next trip depends only on:
- whether it rains now,
- and how many umbrellas are currently where the person is standing.

That is exactly the information captured by the state.

So the state space is:

$$
\{0,1,2,\dots,n\}
$$

where state $i$ means:

> there are $i$ umbrellas at the current location right before departure.

---

## 4.4 Transition idea

Suppose the current state is $i$.

### If it does not rain
This happens with probability $1-p$.

The person leaves without carrying an umbrella.

So at the destination, the number of umbrellas there will be the number that were already at the destination, namely $n-i$.

So the next state becomes:

$$
n-i
$$

### If it rains and $i>0$
This happens with probability $p$.

The person carries one umbrella to the other side.

So the destination will now have one extra umbrella compared to before.

The next state becomes:

$$
n-i+1
$$

### If it rains and $i=0$
There is no umbrella available, so the person gets wet.

They still travel, and at the destination all $n$ umbrellas are there.

So the next state becomes:

$$
n
$$

---

## 4.5 The famous long-run answer

A standard result for the umbrella problem is:

> the long-run probability of getting wet is

$$
\frac{p}{n+1}
$$

This is a very useful formula to remember.

### Meaning
- $p$ is the probability that it rains on a trip,
- $n$ is the total number of umbrellas.

As $n$ increases, the chance of getting wet decreases.

---

## 4.6 Why this formula makes sense intuitively

You only get wet when two things happen together:

1. it rains,
2. there are zero umbrellas at your current location.

So the long-run wet probability is:

$$
\Pr(\text{rain}) \times \Pr(\text{state }0 \text{ in the long run})
$$

In the classical umbrella chain, the stationary distribution turns out to be uniform over the states $0,1,2,\dots,n$.

So each state has long-run probability:

$$
\frac{1}{n+1}
$$

Therefore:

$$
\Pr(\text{wet}) = p \cdot \frac{1}{n+1} = \frac{p}{n+1}
$$

That is where the result comes from.

---

## 4.7 Worked example: minimum number of umbrellas

Suppose the probability of rain on each trip is:

$$
p=0.6
$$

How many umbrellas are needed so that the long-run probability of getting wet is less than 0.1?

We want:

$$
\frac{0.6}{n+1} < 0.1
$$

Multiply both sides by $n+1$:

$$
0.6 < 0.1(n+1)
$$

Multiply by 10:

$$
6 < n+1
$$

So:

$$
n+1 > 6
$$

Hence the smallest integer that works is:

$$
n=6
$$

So the person needs **6 umbrellas**.

---

## 4.8 What examiners want in umbrella questions

Usually they want to see that you can:

1. define the state properly,
2. describe the transitions clearly,
3. connect the problem to a Markov chain,
4. use long-run probabilities correctly.

Even if you already know the final formula $p/(n+1)$, it is good to explain **why** it is true.

---

# 5. Final Revision Summary

## Binomial option pricing
Remember these ideas:
- option price is determined by **no-arbitrage**,
- replication means matching the option payoff using stock and risk-free borrowing/lending,
- one-period formula:

$$
q = \frac{e^{rT}-d}{u-d}, \qquad V_0=e^{-rT}(qV_u+(1-q)V_d)
$$

- for multi-step trees, use **backward induction**,
- chooser option means: at the choice date, take the larger of call value and put value.

## Option payoff graphs
Remember these shapes:
- long call: flat then rising,
- long put: falling then flat,
- straddle: V-shape,
- long call + short put: straight line $S_T-K$,
- bull spread: flat, then rising, then flat.

## Markov chains
Remember:
- define states carefully,
- write the transition matrix row by row,
- stationary distribution solves

$$
\pi P = \pi
$$

with probabilities summing to 1,
- long-run interpretation matters.

## Umbrella problem
Remember:
- state = number of umbrellas at the current location before a trip,
- wet probability in the long run is

$$
\frac{p}{n+1}
$$

---

# 6. Common Pitfalls Across the Whole Midsem

- confusing **payoff** with **profit**,
- writing option payoff formulas incorrectly,
- forgetting to discount in option pricing,
- using the wrong interest convention,
- choosing the wrong state in a Markov chain,
- finding a stationary distribution but not explaining what it means,
- memorizing formulas without understanding the core intuition.

---

# 7. One Big Picture to Remember

These four topics may look different, but there is a common pattern:

- In option pricing, you build a simple model and use no-arbitrage to price fairly.
- In payoff graphs, you break a complicated position into simple option pieces.
- In Markov chains, you break a random process into states and transition rules.
- In the umbrella problem, you turn a real story into a Markov chain and then read its long-run behavior.

So the exam is really testing whether you can take a complicated story and reduce it to a clean mathematical structure.

That is the real skill behind the formulas.
