# Chapter 25: Learning from Observations
<br>

## 25.1 Forms of Learning
Learning is essential for unknown environments, the world is a POMDP with initially unknown transition and sensor models. It is easier to expose an agent to reality rather than trying to write it down.  
Learning modifies the agent's decision mechanism to improve performance.

**Elements of Learning Agents:**
- **Performance element** is what we have called "agent" until now
- **Critic/learning element/problem generator** do the "improving"
- **Performance standard** is fixed (outside the environment) e.g pain, hunger
-> **Learning element** may use knowledge already acquired in the performance element.
-> Learning may require **experimentation**-actions an agent might not normally consider such as dropping rocks from the Tower of Pisa

Different kinds of learning:
- **Supervised learning**: correct answers for each instance
- **Reinforcement learning**: occasional rewards
<br>

## 25.2 Inductive Learning
-> Definition: **Learn a function from examples**  
- An **example** is a pair $(x,y)$ of an input sample $x$ and a classification y. we call a set $S$ of examples consistent, if $S$ is a function.  
- The **inductive learning** problem $\mathcal P := \langle\mathcal H, f \rangle$ consists in finding a **hypothesis** $h ∈ \mathcal H$ such that $f \simeq h|_{dom(f)}$ for a consistent **training set** $f$ of **examples** and a **hypothesis space** $\mathcal H$. We also call $f$ the **target function**.  
- **Inductive learning** algorithms solve this problem.  
- Note this is a highly simplified model of real learning, it ignores: prior knowledge, assumes a deterministic, observable environment, assumes examples are given and assumes that the agent wants to learn $f$.  
<br>
### Inductive Learning Method
-> Construct/adjust hypothesis $h ∈ \mathcal H$ to agree with a training set $f$.  
We call $h$ **consistent with** $f$ (on a set $T \subseteq dom(f))$, if  it agrees with $f$ in all examples in $T$.  
<br>
### Choosing the Hypothesis Space
Whether we can find a **consistent hypothesis** for a given **training set** depends in the chose **hypothesis space**.  
We say that an inductive learning problem $\langle \mathcal H, f \rangle$ is **realizable**, iff there is a $h ∈ \mathcal H$ consistent with $f$.  

**Problem**: We do not know whether a given learning problem is realizable, unless we have prior knowledge.
-> Make $\mathcal H$ large, e.g. the class of all Turing machines.  

**Note:** But computational complexity depends on hypothesis space. Consistency is not even decidable for general Turing machines. Therefore much of the research in machine learning has concentrated on simple hypothesis space.
-> We will concentrate on **propositional logic** and related languages.
<br>
  
## 25.3 Learning Decision Trees
A **decision tree** for a given attribute-based representation is a tree, where the non-leaf nodes are labeled by attributes, their outgoing edges, by the corresponding attribute values, and the leaf nodes are labeled by the classification.  

-> We cut the paths when the output is predefined by the training set. E.g. When we already know based on the training set, that the outcome will be true, no matter what the values of the other attributes are, we can cut the path and directly add the leaf node true.  
  
### Decision Tree Learning
**Aim:** Find a small tree consistent with the training examples.  
**Idea:** Recursively choose the "most significant" attribute as root of (sub)tree.  
**Algorithm:**  
**function** DTL($examples, attributes, default$) **returns** a decision tree  
	**if** $examples$ is empty **then return** default  
	**else if** all $examples$ have the same classification **then return** the classification  
	**else if** $attributes$ is empty **then return** MODE($examples$)  
	**else**  
		$best$ := Choose−Attribute($attributes, examples$)  
		$tree$ := a new decision tree with root test $best$  
		$m$ := MODE($examples$)  
		**for** each value $v_i$ of $best$ do  
			$examples_i$ := {elements of $examples$ with $best = v_i$ }  
			$subtree$ := DTL($examples_i, attributes / best, m$)  
			add a branch **to** $tree$ with label $v_i$ and subtree $subtree$  
	**return** tree  
  
With Mode($examples$) = most frequent value in $example$.  
The recursive steps pick an attribute and then subdivide the examples.  

### Choosing an Attribute
A good attribute splits the examples into subsets that are (ideally) "all positive" or "all negative".  
-> Choose an attribute where for all the subsets the target attribute is either true or false. This way we can make the tree more compact, and write directly the value for the target attribute instead of going through all attributes on each path of the tree.
<br>
## 25.4 Using Information Theory

### Information Entropy
The more clueless I am about the answer initially, the more information is contained in the answer.
**Scale**: 1 b $\widehat{=}$ 1 bit $\widehat{=}$ answer to Boolean question with prior probability (0.5,0.5).
**Entropy**: If the prior probability is $\langle P_1,…,P_n \rangle$, then the information in an answer is:
$$I(\langle P_1,…,P_n \rangle) := \sum_{i=1}^n - P_i\ ·\ log_2 (P_i)$$
The case $P_i = 0$ requires special treatment.
e.g. coin toss: $I(\langle \frac{1}{2}, \frac{1}{2} \rangle) = - \frac{1}{2} log_2 (\frac{1}{2}) - \frac{1}{2} log_2 (\frac{1}{2}) = 1 b$  
<br>
### Information Gain in Decision Trees
- We can estimate the **probability distribution** of the **classification C** with:
$$P(C) = \langle p/(p+n),n/(p+n) \rangle$$
-  The **information gain** from an attribute test A is:
$$Gain(A) := I(P(C)) - \sum_a P(A=a)\ ·\ I(P(C\ |\ A=a))$$
- Assume we know the results of some attribute tests b. Then the **conditional information gain** from an attribute test A is:
$$Gain(A\ |\ b) := I(P(C\ |\ b)) - \sum_a P(A=a\ |\ b)\ ·\ I(P(C\ |\ a,b))$$
- E.g. if the **classification** $C$ is boolean and we have $p$ **positive** and $n$ **negative examples**, the **information gain** is:  
$$Gain(A) = I(\langle \frac{p}{p+n}, \frac{n}{p+n} \rangle)-\sum_a \frac{p_a+n_a}{p+n} I(\langle \frac{p_a}{p_a+n_a}, \frac{n_a}{p_a+n_a} \rangle)$$
where $p_a$ and $n_a$ are the number of positive and negative values of the target attribute of the examples given the specific attribute and $n$ and $p$ are the number of the overall positive and negative values for the target attribute.  
<br>
### Generalization and Overfitting
We speak of **overfitting**, if a hypothesis $h$ describes random error in the (limited) training set rather than the underlying relationship.  
**Underfitting** occurs when $h$ cannot capture the underlying trend of the data.

Overfitting increases with the size of the hypothesis space and the number of attributes, but decreases with number of examples.

-> To combat overfitting, we generalize the decision tree by pruning nodes.  
-> For **decision tree pruning** repeat the following on a learned decision tree:  
1. Find a **terminal** test node $n$ (only result leaves as descendant)  
2. If test is **irrelevant**, i.e. has low information gain, **prune** it by replacing $n$ with a leaf node.
<br>
### Determining Attribute Irrelevance
A result has **statistical significance**, if the probability they could arise from the **null hypothesis** (the assumption that there is no underlying pattern= is very low (usually 5%).  
-> The attribute is irrelevant

To prove this, we compute the probability that the examples distribution for a terminal node deviates from the expected distribution under the null hypothesis.
-> For attribute A with d values, compare the actual numbers $p_k$ and $n_k$ in each subset with $s_k$ with the expected numbers.  
$$\widehat{p_k} = p\ ·\ \frac{p_k+n_k}{p+n} \text{ and } \widehat{n_k} = n\ ·\ \frac{p_k + n_k}{p+n}$$
A convenient measure of the total deviation is:
$$\Delta = \sum_{k=1}^d \frac{(p_k - \widehat{p_k})^2}{\widehat{p_k}}\ +\ \frac{(n_k - \widehat{n_k})^2}{\widehat{n_k}}$$
**Neyman-Pearson:** Under the null hypothesis, the value of $\Delta$ is distributed to the $\chi^2$ with $d-1$ degrees of freedom for $\Delta$ is called $\chi^2$ **pruning**.  
-> E.g. If an attribute has four values, it has three degrees of freedom, so $\Delta = 7.82$ would reject the null hypothesis at the 5% level.  
<br>
## 25.5 Evaluating and Choosing the Best Hypothesis
A sequence $E_j$ of random variables is **independent and identically distributed (IID)**, iff they are:
- **independent**: $P(E_j\ |\ E_{j-1},E_{j-2,…}) = P(E_j)$  
- **identically distributed**: $P(E_i) = P(E_j)$ for all $i$ and $j$  
  
Splitting the data available for learning into a
1. Training set from which the learning algorithm produces a hypothesis $h$  
2. and a test set, which is used for evaluating $h$  
is called **holdout cross-validation**.  
Ration between training set and test set: In $k$**-fold cross-validation**, we perform $k$ rounds of learning, each with 1/$k$ of the data as test set and average over the $k$ test scores.  
<br>
### Loss Functions
It is much worse to classify ham as spam than vice-versa
-> Decision-makers should maximize expected utility (MEU) and not minimize the error.  
The **loss  function** $L$ is defined by setting $L(x,y,\hat{y})$ to be the amount of utility lost by prediction $h(x) = \hat{y}$  instead of $f(x)=y$. If L is independent of $x$, we often use $L(y,\hat{y})$.  
E.g. $L(spam,ham) = 1$, while $L(hamm,spam)=10$.  
<br>
### Generalization Loss
Popular general loss function:
$$\begin{align*}
&\textbf{absolute value loss:} &&L_1(y,\hat{y}:=|y-\hat{y}| &&\text{small errors are good}\\
&\textbf{squared error loss:} &&L_2(y,\hat{y}):=(y-\hat{y})^2 &&dito\\
&\textbf{0/1 loss} &&L_{0/1}(y,\hat{y}):=0, \text{ if } y=\hat{y}, \text{ else } 1 &&error rate
\end{align*}$$
Idea: Maximize expected utility by choosing hypothesis $h$ that minimizes expected loss over all $(x,y) ∈ f$.  
Let $\Large \varepsilon$ be the set of all possible examples and $P(X,Y)$ the prior probability distribution over its components, then the expected **generalization loss** for a hypothesis $h$ with respect to a loss function $L$ is  
$$GenLoss_L(h) := \sum_{(x,y)\ ∈\ \large\varepsilon} L(y,h(x))\ ·\ P(x,y)$$
and the hypothesis $h^* := argmin:{h ∈ \mathcal H} (GenLoss_L(h))$.  
<br>
### Empirical Loss
If $P(X,Y) is unknown, the learner can only estimate the **generalization loss**.  
Let $L$ be a loss function and $E$ a set of examples with #(E) = N, then we call  
$$EmpLoss_{L,E}(h) := \frac{1}{N} \sum_{(x,y) ∈ E} L(y,h(x))$$
the **empirical loss** and $\widehat{h}^* := argmin_{h∈\mathcal H} (EmpLoss_{L,E}(h))$ the **estimated best hypothesis**.  
There are four reasons why $\widehat{h}^*$ may differ from $f$:  
- **realizability**: then we have to settle for an approximation $\widehat{h}^*$ of $f$  
- **variance**: different subsets of $f$ give different $\widehat{h}^* \leadsto$ more Examples
- **noise**: if $f$ is non-deterministic, then we cannot expect perfect results
- **computational complexity**: if $\mathcal H$ is too large to systematically explore, we make due with subset and get an approximation
<br>
### Regularization
Let $\lambda ∈ \mathbb R,\ h ∈ \mathcal H$ and $E$ a set of examples, then we call
$$Cost_{L,E}(h) := EmpLoss_{L,E}(h) + \lambda\ Complexity(h)$$
the **total cost** of $h$ on $E$.  
The Process of finding a total cost minimizing hypothesis
$$\widehat{h}^* := argmin_{h∈\mathcal H}(Cost_{L,E}(h))$$
is called **regularization**; Complexity is called the **regularization function** or **hypothesis complexity**.  
<br>
### Minimal Description Length
**Note**: in regularization, the empirical loss and the hypothesis complexity are not measured in the same scale $\leadsto\ \lambda$ mediates between the scales.  
**Idea**: Measure both in the same scale $\leadsto$ use information content, i.e. in bits
Let $h ∈ \mathcal H$ be a hypothesis and $E$ a set of examples, then the **description length** of $(h,E)$ is computed as follows:  
1. encode the hypothesis as a Turing machine program, count bits.
2. count data bist:
	- correctly predicted examples $\leadsto$ 0 b
	- incorrectly predicted example $\leadsto$ according to size of error
The **minimum description length** or **MDL hypothesis** minimizes the total number of bits required.  
<br>
## 25.6 Computational Learning Theory
**Main Question**: How can we be sure that our learning algorithm has produced a hypothesis $h$ that will predict the correct value for previously unseen inputs, i.e is close to the target function $f$ ?  
<br>
### PAC Learning
If $h$ is consistent with a sufficiently large set of training examples it is unlikely that $h$ is seriously wrong. -> $h$ is **approximately correct**  
Any learning algorithm that returns hypotheses that are **probably approximately correct** is called a **PAC learning algorithm**.

The **error rate** $error(h) of a hypothesis $h$ is the probability that $h$ misclassifies a new example.
$$error(h) := GenLoss_{L_{0/1}}(h)$$
A hypothesis $h$ is called **approximately correct**, iff $error(h) \le \large \epsilon$ for some small $\large \epsilon > 0$.  
We write $\mathcal H_b:=\{h∈\mathcal H|erro(h)>\large \epsilon \normalsize\}$ for the "seriously bad" hypotheses.  
<br>
### Sample Complexity
The number of required **examples** as a function of $\large \epsilon$ and $\delta$ is called the **sample complexity** of $\mathcal H$.  
e.g. If $\mathcal H$ is the set of $n$-ary Boolean function, then $\# (\mathcal H) = 2^{2^n}$.  
-> sample complexity grows with $\mathcal O(log_2(2^{2^n}) = \mathcal O(2^n)$.  
-> PAC learning for Boolean functions needs to see (nearly) all examples.  

To solve this we need to restrict $\mathcal H$ in some way:  
1. bring prior knowledge into the problem  
2. prefer simple hypotheses  
3. focus on "learning subsets" of $\mathcal H$  
<br>
### Decision Lists
A **decision list** consists of a sequence of tests, each of which is a conjunction of literals.  
- If a test succeeds when applied to an example description, the decision list specifies the value to be returned.  
- If the test fails, processing continues with the next test in the list.  
$$\begin{align}
Patrons&(x,Some) \xrightarrow{No} &Patrons(x,Full) &\land Fri/Sat(x) \xrightarrow{No} No\\[1ex]
&\downarrow &\downarrow\\[1ex]
&Yes &Ye&s
\end{align}$$
**Idea**: Use greedy algorithm that repeats
1. **find** test that agrees exactly with some subset $E$ of the training set  
2. **add** it to the decision list under construction and remove $E$,  
3. **construct** the remainder of the DL using just the remaining examples,
until there are not examples left.
<br>
## 25.7 Regression and Classification with Linear Models
