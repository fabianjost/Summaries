# Part V: Reasoning with Uncertain Knowledge

This part addresses inference and agent decision making in particular observable environments, i.e. where we only know probabilities instead of certainties whether propositions are true/false.

## Chapter 20: Quantifying Uncertainty
Logic does not allow to weigh different alternatives and it does not allow to express incomplete knowledge. We therefore need other models to cope with problem were we have incomplete knowledge (partially observable environments).  
  
A **rational agent** chooses the action with the **maximum expected utility**:

- We have a choice of **actions**, which can lead to different probabilities.
- The actions have different **costs**.
- The results have different **utilities**.  
  
In this chapter we will focus on all the basic machinery at use in **Bayesian networks**.  
In the next chapter we will see what they are and how to build and use them.  

### 20.1 Unconditional Probabilities
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

### 20.2 Conditional Probabilities
- Probabilities model our belief, thus they depend on our knowledge. Therefore in the presence of additional knowledge, we can no longer use *unconditional probabilities*.  
e.g. The probability of cavity increases when the doctor is informed that the patient has toothache.
- $\bf{P(a|b)}$ denotes the **conditional probability** of a given that all we know is b.  
\
$$\bf P(a|b) := \frac{P(a \land b)}{P(b)}$$
\
**Conditional Probability Distributions:**  
$\textbf{P} (X|Y)$ (with boldface **P**) is the table of all conditional probabilities similar to *full joint diustributions* of *unconditional probabilities*.

### 20.3 Independence
It is not a good idea to use *full joint distributions*, because given $n$ random variables with $k$ values each, the *full joint distribution* contains $k^n$ probabilities.  
Given independent events a and b where $P(b) \not= 0$, we have $P(a|b) = P(a)$:

$$\bf P(a|b) = \frac{P(a \land b)}{P(b)} = P(a)$$

Independence can be exploited to represent the *full joint probability distribution* more compactly.  
Sometimes variables are only independent under certain conditions: *conditional independence*.













