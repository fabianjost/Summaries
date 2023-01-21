# Chapter 21: Probabilistic Reasoning: Bayesian Networks
- What is a bayesia network
- What is the meaning of a bayesian network
- Constructing bayesian networks
- Inference in bayesian networks

The most wide-spread and successful framework for probabilistic reasoning.


## 21.1 What is a Bayesian Network
**Idea**:
1. Design the random variables $X_1, ..., X_n$  
2. Identify their dependencies  
3. Insert the conditional probability tables $P(X_i\ |\ Parents(X_i))$  

Given **random variables** $X_1, ..., X_n$ with **finite domains** $D_1, ..., D_n$, a **Bayesian network** (also **belief network** or **probabilistic network***) is a **DAG** (directed asyclic graph). We denote Parents($X_i$). Each $X_i$ is associated with a function that is the **conditional probability table**.


## 21.2 What is the Meaning of a Bayesian Network?
Each node $X$ in a BN is **conditionally independent** of its non-decendants given its parents Parent($X$).
-> We can use the chain rule for any ordering $X_1, ..., X_n$:
$$P(X_1, ..., X_n) = P(X_n\ |\ X_{n-1}, ..., X_1) · P(X_{n-1}\ |\ X_{n-2}, ..., X_1)\ ·\ ...\ ·\ P(X_1)$$
Choose $X_1, ..., X_n$ consistent with $\mathcal{B}:  X_j ∈ Parents(X_i)\ \leadsto\ j\ <\ i$  
$$\bf P(X_1, ..., X_n) = \prod_{i=1} ^n\ P(X_i\ |\ Parents(X_i))$$
This does **not** hold for cyclic BNs. Cyclic BNs may be self-contradictory.


## 21.3 Constructing Bayesian Networks

### BN construction algorithm
1. Initialize $BN:= \langle {X_1, ..., X_n}, E\ \rangle$ where $E = \emptyset$.  
2. Fix any order of the variables, $X_1, ..., X_n$.  
3. **for** $i\ :=\ 1, ..., n$  **do**  
	1. Choose minimal set $Parents(X_i) \subseteq \{X_1, ..., X_{i-1}\}$ so that $$P(X_i\ |\ X_{i-1}, ..., X_1) = P(X_i\ |\ Parents(X_i))$$
	2. For each $X_j ∈ Parents(X_i)$, insert $(X_j, X_i)$ into E.  
	3. Associate $X_i$ with $CPT(X_i)$ corresponding to $P(X_i\ |\ Parents(X_i))$.  

**Attention**: Which variables we need to include into $Parents(X_i)$ depends on what "${X_1, ..., X_{i-1}}$" is!  
The size of the resulting BN depends on the chosen order $X_1, ..., X_n$ and is not a fixed property of the domain, but depends in the skill of the designer.  

Conditional probabilities $\bf P(Symptom\ |\ Cause)$ are more robust and often easier to assess than $\bf P(Cause\ |\ Symptom)$.    
**-> We should order causes before symptoms.**

### Deterministic Nodes
A node $X$ in a Bayesian network is called **deterministic**, if its value is completely determined by the values of Parents($X$).  

### Noisy Nodes
Sometimes values of nodes are only "almost deterministic". (Uncertain but mostly logical)
The CPT of a noisy disjunction node $X$ in a Bayesian network is given by
$$P(x_i\ |\ Parents(X_i)) = \prod_{\{j\ |\ X_j=T\}}\ q_j$$

In general, noisy logical relationships in which a variable depends on $k$ parents can be described by $\mathcal O(k)$ parameters instead of $\mathcal O)(2^k)$ for the full conditional probability table. This can make assessment and learning tractable.


## 21.4 Inference in Bayesian Networks
Given random variables $X_1, ..., X_n$, a **probabilistic inference task** consists of a set $\bf X \subseteq \{X_1, ..., X_n\}$ of **query variables**, a set $\bf E \subseteq \{X_1, ..., X_n\}$ of **evidence variables**, and an **event** $\bf \large e$ that assigns values to $\bf \large E$. We wish to compute the **conditional probability distribution** $\bf P(X\ |\ e)$.
$Y\ :=\ \{X_1, ..., X_n\}\\ X \cup E$ are the **hidden variables**.

### Inference by Enumeration
1. Bayesian network: Construct a Bayesian network $\mathcal B$ that captures variable dependencies.
2. Normalization + Marginalization:
$$P(X\ |\ e) = \alpha\ P(X,e);\ if\ Y \not= \emptyset\ then P(X\ |\ e) = \alpha\ \sum_{y ∈ Y}\ P(X,e,y)$$
3. Chain Rule: Order $X_1, ..., X_n$ consistent with $\mathcal B$.
$$P(X_1, ..., X_n) = P(X_n\ |\ X_{n-1}, ..., X_1)\ ·\ P(X_{n-1}\ |\ X_{n-2}, ..., X_1)\ ·\ ...\ P(X_1)$$
4. Exploit conditional independence: Instead of $P(X_i\ |\ X_{i-1}, ..., X_1)$, use $P(X_i\ |\ Parents(X_i))$.  

### Variable Elimination
Avoid repeated computation: Evaluate expression from right to left, storing all intermediate results.  
1. CPTs of BN yield **factors** (probability tables):
$$\begin{align}
P(B\ |\ j,m) &= \alpha\ P(B)\ ·\ \sum_{v_E} P(v_E)\ \sum_{v_A} P(v_A\ |\ B, v_E)\ ·\ P(j\ |\ v_A)\ ·\ P(m\ |\ v_A) =\\
&=f_1(B)\ ·\ \sum_{v_E} f_2(E)\ ·\ \sum_{v_A} f_3(A,B,E)\ ·\ f_4(A)\ ·\ f_5(A)
\end{align}$$
2. With this the computation is performed in terms of factor product and summing out variables from factors.  

-> Avoid irrelevant computation by repeatedly removing hidden variables that are leaf nodes.

### The Complexity of Exact Inference
- A graph G is called **singly connected**, or a **polytree** (otherwise **multiply connected**), if there is at most one **undirected path** between any two nodes in G.
- On singly connected Bayesian networks, variable elimination runs in polynomial time.
- For multiply connected Bayesian networks, probabilistic inference is $\# P$-hard. ($\# P$ is harder than $NP,\ NP \subseteq \# P$) -> if need be we can approximate

