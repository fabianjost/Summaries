### Bayes' Rule
Given propositions $A$ and $B$ where $P(a) \not= 0$ and $P(b) \not= 0$:
$$\bf P(a|b) = \frac{P(b|a) · P(a)}{P(b)}$$
- Conditional Probability: $P(a|b) := \frac{P(a \land b)}{P(b)}$  
- Conditional Independence: $P(a|b) = \frac{P(a \land b)}{P(b)} = P(a)$  
- **Product Rule**: $\bf P(a \land b) = P(a, b) =  P(a|b) · P(b)$
- **Chain Rule:** $P(X_1 ,..., X_n) = P(X_n | X_{n−1} ,..., X_1 ) · P(X_{n−1} | X_n−2,..., X_1 ) · \ ... \ · P(X_2 | X_1) · P(X_1 )$
- Given "query variable" $X$, "observed event" $e$, and "hidden variables" set $Y$:
   $$\bf P(X|e) = \alpha\ · P(X,e) = \alpha\ · \sum_{y\ \Large \epsilon\ \normalsize Y} P(X,e,y)$$
- If $Z_1\ and\ Z_2$ are **conditionally independent** given $Z$, then:
$$\bf P(Z_1|Z_2,Z) = P(Z_1|Z)$$
<br>
### Exploiting Conditional Independence
1. Graph captures variable dependencies:
   -> Given evidence $e$, we want to know $P(X|e)$
2. Normalization and Marginalization:
    $P(X|e) = \alpha\ P(X,e);\ if\ Y \not=\ \emptyset\ then\ P(X|e) = \alpha\ \sum_{y\ \Large\epsilon\ \normalsize Y}\ P(X,e,y)$ 
3. Chain rule: Order $X_1, ..., X_n$ consistently with dependency graph.
	$P(X_1, ..., X_n) = P(X_n|X_{n-1}, ..., X_1) · P(X_{n-1}|X_{n-2}, ..., X_1) · ... · P(X_1)$ 
4. Exploit conditional independence: Instead of $P(X_i|X_{i-1}, ..., X_1)$, we can use $P(X_i|Parents(X_i))$.
<br>
### Filtering equation
$f_{1:w+1} = \alpha (O_{w+1}\ ·\ T^t f_{1:w})$  

with the diagonal sensor matrizes.
<br>
### Value Iteration (Bellman Equation)
$U(s) = R(s) + \gamma\ ·\ max_{a∈A(s)} (\sum_{s'} U(s')\ ·\ P(s'\ |\ s,a))$ 
<br>
### Decision Trees
- **Entropy**: If the prior probability is $\langle P_1,…,P_n \rangle$, then the information in an answer is:
$$I(\langle P_1,…,P_n \rangle) := \sum_{i=1}^n - P_i\ ·\ log_2 (P_i)$$
- The **information gain** from an attribute test A and a target attribute C is:
$$Gain(A) := I(P(C)) - \sum_a P(A=a)\ ·\ I(P(C\ |\ A=a))$$
<br>
### Artificial Networks
$$\bf a_i = g(in_j) = g(\sum_j \omega_{j,i}\ a_j)$$
with the: 
- **input function** $in_j = \sum_j \omega_{j,i}\ a_j$  
- **activation function** g  
- **output function** $a_j$

**Backpropagation algorithm:**
1. Compute the activations of all units
2. Determine the error between network output and target result and propagate it back to all inner units
3. use the propagated values to update all weights
<br>
### Natural Language Processing
For a trigram model, $P(c_i\ |\ c_{1:i-1}) = P(c_i\ |\ c_{i-2}, c_{i-1})$. Factoring with the chain rule and then using the Markov property, we obtain:
$$P(c_{1:N})=\prod_{i=1}^N P(c_i\ |\ c_{1:i-1}) = \prod_{i=1}^N P(c_i\ |\ c_{i-2},c_{i-1})$$

**Language identification:**
1. Build a trigram language model $P(c_i\ |\ c_{(c-2):(i-1)},l)$ for each candidate laguage l by counting trigrams in an l-corpus.
2. Apply Bayes' rule and the Markov property to get the most likely language:
$$\begin{align}
l^* &= argmax_l\ (P(l\ |\ c_{1:N}))\\[3ex]
&=argmax_l\ (P(l)\ P(c_{1:N}\ |\ l))\\[2ex]
&= argmax_l\ (P(l)\ \prod_{i=1}^N P(c_i\ |\ c_{i-2:i-1},l))
\end{align}$$

**Other applications of Character N-Gram Models:**
- **Genre classification**: Deciding whether a text is a news story, a legal document, a scientific article etc.
- **Named entity recognition (NRE):** is the task of finding names of things in a document and deciding what class they belong to.

The **perplexity** of a sequence $c_{1:N}$ is defined as>
$$Perplexity(c_{1:N}) := P(c_{1:N})^{-\frac{1}{N}}$$
