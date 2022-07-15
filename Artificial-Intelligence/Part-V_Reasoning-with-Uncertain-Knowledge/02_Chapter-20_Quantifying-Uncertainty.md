# Chapter 20: Quantifying Uncertainty
Logic does not allow to weigh different alternatives and it does not allow to express incomplete knowledge. We therefore need other models to cope with problem were we have incomplete knowledge (partially observable environments).  
  
A **rational agent** chooses the action with the **maximum expected utility**:

- We have a choice of **actions**, which can lead to different probabilities.
- The actions have different **costs**.
- The results have different **utilities**.  
  
In this chapter we will focus on all the basic machinery at use in **Bayesian networks**.  
In the next chapter we will see what they are and how to build and use them.  



## 20.1 Unconditional Probabilities
- $P(X = x)$ denotes the prior or **unconditional probability**, that $X$ has value $x$ in the **absence of any other information**.
- *uppercase "$X$"* stands for a random variable and *lowercase "$x$"* for one of its values.
- By convention, we denote *boolean random variables* with $A, B$, and more general *finite-domain random variables* with $X, Y$.
- For a *boolean random variable* Name, we write *name* for the outcome Name = T and $\neg name$ for Name = F.  

**Full Joint Probability Distribution:**  
Given *random variables* ${X_1, ..., X_n}$ with *domains* $D_1, ..., D_n$, the *full joint probability distribution*, denoted $P(X_1, ..., X_n)$, lists the probabilities of all *atomic events*.  
**The sum of all fields is 1.**

|					| $toothache$ |  $\neg toothache$ |
|------------------|:-----------:|:-----------------:|
| $cavity$			|  0.12			|		0.8			|
| $\neg cavity$		|    0.08	   |  	 0.72	 		|

e.g. $P(cavity \land toothache) = 0.12$ is the probability that some given person has both a cavity and a toothache.  
*Instead of $P(a \land b)$ we often write $P(a,b)$.*



## 20.2 Conditional Probabilities
- Probabilities model our belief, thus they depend on our knowledge. Therefore in the presence of additional knowledge, we can no longer use *unconditional probabilities*.
  e.g. The probability of cavity increases when the doctor is informed that the patient has toothache.
- $\bf{P(a|b)}$ denotes the **conditional probability** of a given that all we know is b.  
  
$$\bf P(a|b) := \frac{P(a \land b)}{P(b)}$$
  
**Conditional Probability Distributions:**  
$\textbf{P} (X|Y)$ (with boldface **P**) is the table of all conditional probabilities similar to *full joint diustributions* of *unconditional probabilities*.



## 20.3 Independence
It is not a good idea to use *full joint distributions*, because given $n$ random variables with $k$ values each, the *full joint distribution* contains $k^n$ probabilities.  
Given independent events a and b where $P(b) \not= 0$, we have $P(a|b) = P(a)$:

$$\bf P(a|b) = \frac{P(a \land b)}{P(b)} = P(a)$$

Independence can be exploited to represent the *full joint probability distribution* more compactly.  
Sometimes variables are only independent under certain conditions: *conditional independence*.



## 20.4 Basic Probabilistic Reasoning Methods

### Product Rule
$$\bf P(a \land b) = P(a, b) =  P(a|b) · P(b)$$
### Chain Rule
$$\begin{align} 
& \bf P(X_1 ,..., X_n) = \\[2ex]
& \bf P(X_n | X_{n−1} ,..., X_1 ) · P(X_{n−1} | X_n−2,..., X_1 ) · \ ... \ · P(X_2 | X_1) · P(X_1 )\\[2ex]
&e.g.:\\[1ex]
& P(\neg brush \land cavity \land toothache) = \\[1ex]
& = P(toothache | cavity, \neg brush) · P(cavity, \neg brush) = \\[1ex]
& = P(toothache | cavity, \neg brush) · P(cavity | \neg brush) · P(\neg brush)
\end{align}$$

### Marginalization
  Given sets $\bf X$ and $\bf Y$ of *random variables* we have:  $$\begin{align}
  &\bf P(X) = \sum_{y \ \Large{\epsilon} \ \normalsize Y} \ P(X,y)\\[1ex]
  &e.g.:\\
  &P(cavity) = \sum_{y \ \Large{\epsilon} \ \normalsize toothache} \ P(cavity,y)\\
  &P(cavity) = P(cavity, toothache) + P(cavity, \neg toothache)\\
  &P(\neg cavity) = P(\neg cavity, toothache) + P(\neg cavity, \neg toothache)
  \end{align}$$

### Normalization
   Problem: We know $P(cavity \land toothache)$ bit don't know $P(toothache)$
   1. Step: Case distinction over the values of Cavity:
$$\begin{align}
	  &P(cavity\ |\ tootache) = \frac{P(cavity \land toothache)}{P(toothache)} = \frac{0.12}{P(toothache)}\\[2ex]
	  &P(\neg cavity\ |\ toothache) = \frac{P(\neg cavity \land toothache)}{P(toothache)} = \frac{0.08}{P(toothache)}
	  \end{align}$$
   2. Step: Assuming placeholder $\alpha\ :=\ 1/P(toothache)$:
$$\begin{align}
	  &P(cavity\ |\ tootache) = \alpha\ P(cavity \land toothache = \alpha\ 0.12\\[2ex]
	  &P(\neg cavity\ |\ toothache) = \alpha\ P(\neg cavity \land toothache) = \alpha\ 0.08
	  \end{align}$$
   3. Step: Toothache can be viewed as true, iff $A_1 = P(cavity \land toothache) \cup  A_2 = P(\neg cavity \land toothache)$. We view $A_1\ vs.\ A_2$ as the "relative weights of $P(cavity)$ vs. $P(\neg cavity)$ within toothache" and then normalize their summed-up weight to 1:
$$ 1 = \alpha\ (0.12 + 0.08) \leadsto \alpha = \frac{1}{0.12 + 0.08} = \frac{1}{0.2} = 5$$
		-> $\alpha$ is a **normalization constant** scaling the sum of the relative weights to 1.


### Computation Rule: Normalization + Marginalization
   Given "query variable" $X$, "observed event" $e$, and "hidden variables" set $Y$:
   $$\bf P(X|e) = \alpha\ · P(X,e) = \alpha\ · \sum_{y\ \Large \epsilon\ \normalsize Y} P(X,e,y)$$
   -> Second of the four basic techniques in **Bayesian networks**.



## 20.5 Bayes' Rule
Given propositions $A$ and $B$ where $P(a) \not= 0$ and $P(b) \not= 0$:
$$P(a|b) = \frac{P(b|a) · P(a)}{P(b)}$$
**Proof:**
1. By definition $P(a|b) = \frac{P(a \land b)}{P(b)}$ 
2. By the **product rule** $P(a \land b) = P(b|a) · P(a)$

**Kinds of Dependencies:**
- **Causal**: Robust, meaning the probability will not change even under changing environment conditions.
- **Diagnostic**: Probability changes under certain conditions
e.g. If there is an cavity epidemic the probability of $P(toothache|cavity)$ stays the same, but the probability of $P(cavity|toothache)$ changes.

-> Bayes' rule allows to perform diagnosis (oberserving a symptom, what is the cause?) based on prio probabilities and causal dependencies.



## 20.6 Conditional Independence
Given sets of random variables $Z_1, Z_2,\ and\ Z$, we say that $Z_1\ and Z_2$ are **conditionally independent** given $Z$ if:
$$\bf P(Z_1, Z_2|Z) = P(Z_1|Z) · P(Z_2|Z)$$
If $Z_1\ and\ Z_2$ are **conditionally independent** given $Z$, then:
$$\bf P(Z_1|Z_2,Z) = P(Z_1|Z)$$
-> In the presence of **conditional independence**, we can drop variables from the right-hand side of conditional probabilities.


### Exploiting Conditional Independence
1. Graph captures variable dependencies:
   -> Given evidence $e$, we want to know $P(X|e)$
2. Normalization and Marginalization:
    $P(X|e) = \alpha\ P(X,e);\ if\ Y \not=\ \emptyset\ then\ P(X|e) = \alpha\ \sum_{y\ \Large\epsilon\ \normalsize Y}\ P(X,e,y)$ 
3. Chain rule: Order $X_1, ..., X_n$ consistently with dependency graph.
	$P(X_1, ..., X_n) = P(X_n|X_{n-1}, ..., X_1) · P(X_{n-1}|X_{n-2}, ..., X_1) · ... · P(X_1)$ 
4. Exploit conditional independence: Instead of $P(X_i|X_{i-1}, ..., X_1)$, we can use $P(X_i|Parents(X_i))$.


### Naive Bayes Models
A Bayesian network in which a single cause directly influences a number if effects, all of which are cinditionally independent, given the cause is called a **naive Bayes model** or **Bayesian classifier**.
In a naive Bayes model the full joint probability distribution can be written as:
$$P(cause\ |\ effect) = P(cause) · \prod_i\ P(effect_i\ |\ cause)$$
This kind of model is called naive since it is often used as a simplifying model if the effects are not conditionally independent.
In Practice, naive Bayes models can work surprisingly well, even when the conditional independence assumption is not true.