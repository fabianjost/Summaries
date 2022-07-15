# Chapter 22: Making Simple Decisions Rationally
**Decisions theory** investigates **decision problems**, i.e. how an agent $a$ deals with choosing among actions based on the desirability of their outcomes given by a **utility function** on state.  
**Idea:** Rational decisions $\widehat{=}$ choose actions that maximize expected utility (MEU).  

**Utility-based agents:**
Uses a world model aling with a **utility function** that influences its preferences amon the states of that world. It chooses the action that leads to the best expected utility, which is computed by averaging over all possible outcome states, weighted by the probability of the outcome.


## 22.1 Rational Preferences

### Preferences in Deterministic Environments
**Problem:** We cannot directly measure utility (or satisfaction/hapiness in) of a state.
Given states A and B (we call them prizes) an agent can express **preferences** of the form:
- A ≻ B: A **preferred** over B
- A ∼ B: **indifference** between A and B
- A ⪰ B: B **not preferred** over A

### Preferences in Non-Deterministic Environments
**Problem:** In non-deterministic environments we do not have full information about the states we choose between.

### Rational Preferences
**Idea:** Preferences of a rational agent must obey constraints:
Rational preferences $\leadsto$ behavior describable as maximization of expected utility.
We call a set ≻ of **preferences rational**, iff the following constraints hold:
$$\begin{align}
&Orderability: &A ≻ B ∨ B ≻ A ∨ A ∼ B\\
&Transitivity: &A ≻ B ∧ B ≻ C ⇒ A ≻ C\\
&Continuity: &A ≻ B ≻ C ⇒ (∃p [p, A; (1 − p), C] ∼ B)\\
&Substitutability: &A ∼ B ⇒ [p, A; (1 − p), C] ∼ [p, B; (1 − p), C]\\
&Monotonicity: &A ≻ B ⇒ (p>q) ⇔ [p, A; (1 − p), B] ⪰ [q, A; (1 − q), B]\\
&Decomposability: &[p, A; (1 − p), [q, B; (1 − q), C]] ∼ [p, A; (1 − p)q, B; (1 − p)(1 − q), C]
\end{align}$$
- **Orderarbility**: Given any two prizes or lotteries, a rational agent must either prefer one to the other or else rate the two as equally preferable. That is, the agent cannot avoid deciding. Refusing to bet is like refusing to allow time to pass.
- **Transitivity**: A ≻ B ∧ B ≻ C ⇒ A ≻ C
- **Continuity:** If some lottery B is between A and C in preference, then there is some probability p for which the rational agent will be indifferent between getting B for sure and the lottery that yields A with probability p and C with probability 1 − p.
- **Substitutability:** If an agent is indifferent between two lotteries A and B, then the agent is indifferent between two more complex lotteries that are the same except that B is substituted for A in one of them. This holds regardless of the probabilities and the other outcome(s) in the lotteries.
- **Monotonicity:** Suppose two lotteries have the same two possible outcomes, A and B. If an agent prefers A to B, then the agent must prefer the lottery that has a higher probability for A (and vice versa).
- **Decomposability:** Compound lotteries can be reduced to simpler ones using the laws of probability. This has been called the “no fun in gambling” rule because it says that two consecutive lotteries can be compressed into a single equivalent lottery.


## 22.2 Utilities and Money
Let $\mathcal A$ be an agent with a set $\Omega$ of states and a utility function $U: \Omega\ \rightarrow \mathbb{R}_0 ^+$, then for each action a, we define a random variable $R_a$ whose values are the results of performing a in the current state.
The **expected utility** $EU(a\ |\ e)$ of an action a (given evidence $\large \mathrm e$) is:
$$EU(a\ |\ \large \mathrm e) := \sum_{s∈\Omega} P(R_a = s\ |\ a,\large \mathrm e)\ ·\ U(s)$$
- **Normalize utilities**: $u_\top = 1, u_\bot = 0$ 
- **Micromorts**: one-millionsth chance of instant death
- **QALYs**: quality-adjusted life years


## 22.3 Multi-Attribute Utility
How can we handle **Utility functions** of many variables $X_1, ..., X_n$?

### Strict Dominance
Choice B **strictly dominates** choice A iff $X_i(B) \geq X_i(A)$  for all $i$.
**Observation:** Strict dominance seldom holds in practice but is uesful for narrowing down the field of contenders.

### Stochastic Dominance
Distribution $p_2$ **stochastically dominates** distribution $p_1$ iff the cummulative distribution of $p_2$ strictly dominates that for $p_1$ for all t, i.e.
$$\int_t ^{-\infty}p_1(x)dx \leq \int_t ^{-\infty}p_2(x)dx$$
$X \stackrel{+}\rightarrow Y$ (X positively influences Y) means that $P(Y\ |\ x_1,z)$ stochastically dominates $P(Y\ |\ x_2, z)$ for every value z of Y's other parents Z and all $x_1$ and $x_2$ with $x_1 \geq x_2$.
-> Label the arrows of the Bayesian network with plus or minuses.

### Preference Structure: Deterministic
$X_1$ and $X_2$ are **preferentially independent** of $X_3$ iff preference between $\langle x_1,x_2,x_3 \rangle$ and $\langle x_1',x_2',x_3 \rangle$ does not depend on $x_3$.  
e.g. $\langle Noise,\ Cost,\ Safety \rangle$ are preferentially independent because Safety stays the when Noise and Gas Cost changes.
-> **Additive Value Function:** $V(S) = \sum_i V_i(X_i(S))$  
e.g. $V(noise, cost, deaths) = -noise\ ·\ 10^4\ -\ cost\ -\ deaths\ ·\ 10^{12}$  

### Preference Structure: Stochastic
- $X$ is **utility-independent** of $Y$ iff preferences over lotteries in $X$ do not depend on particular values in $Y$.  
- A set $X$ is **mutually utility-independent**, iff each subset is utility-independent of its complement.
- For mutually utility-independent sets there is a multiplicative utility function:
$$U = k_1U_1 + k_2U_2 + k_3U_3 + k_1k_2U_1U_2 + k_2k_3U_2U_3 + k_3k_1U_3U_1 + k_1k_2k_3U_1U_2U_3$$  

## 22.4 Decision Networks
A **decision network** is a **Bayesian network** with added **action nodes** and **utility nodes** (also called **value nodes**) that enable **rational** decision making.  

**Algorithm:** 
- For each value of action node 
- compute expected value of **utility node** given action, evidence
- return **MEU action** (via argmax)

How to creat a model (based on medical example):
1. **Create a causal model:** A graph with nodes for symptoms, disorders, treatments, outcomes, and their influence (edges)
2. **Simplify to a qualitative decision model:** remove vars not involved in treatment decisions
3. **Assign probabilities:** e.g. from patient databases, literature studies, or the expert's subjective assessments
4. **Assign utilities:** e.g. QUALYs or micromorts
5. **Varify and refine the model:** a gold standard given by experts e.g. "running the model backwards" and compare with literature
6. **Perform sensitive analysis:** is the optimal treatment decision robust against small changes in the paramenters?


## 22.5 The Value of Information
One of the most important parts of decision making is knowing what questions to ask. We do not expect to have all the results and diagnostic tests at the beginning. Tests are often expensive, and sometimes hazardous (e.g. delays the treatment). Therefore only test, if:
- knowing the results lead to a significantly better treatment plan
- information from test results os not drowned out by a-priori likelihood
  
-> **Information value theory** enables the agent to make decisions on **information gathering** rationally.

**General Idea:** Compute expected value of information. Then calculate the value of the best action given the information minus expectes value of best action without information.
-> A simple **information gathering agent** gathers information before acting:
- $\textbf{function}$ Information−Gathering−Agent (percept) $\textbf{returns}$ an action
	- $\textbf{persistent}$: D, a decision network
	- integrate percept into D
	- $j := argmax (VPI_E (E_k) / Cost(E_k))$
	- $\textbf{if}\ VPI_E (E_j) > Cost(E_j)\ \textbf{return}\ Request(E_j)$
	- $\textbf{else\ return}$ the best action from D

-> **Note:** This **information gathering agent** is **myopic**, meaning it calculates the VPI as if only a single evidence variable will be acquired, but it works relatively well in practice (outperforms humans in diagnostic tests).