# Chapter 26: Statistical Learning
In contrast to Part V, we will now see how models can - at least partially - be learned from observing the environment, instead of being designed upfront.  
<br>
## 26.1 Full Bayesian Learning
**Bayesian learning:** learning probabilistic models (e.g. Bayesian networks) from observations. It calculates the probability of each hypothesis and makes predictions based on this:

- The **likelihood** of the data is given by:
$$P(\textbf{d} |\ h_i) = \prod_j P(d_j\ |\ h_i)$$
- Given the data, each hypothesis has a posterior probability:
$$P(h_i\ |\ \textbf{d}) = \alpha\ ·\ P(\textbf{d}\ |\ h_i)\ ·\ P(h_i)$$
  with $P(h_i)$ being the hypothesis prior.

- **Bayesian predictions** use a likelihood-weighted average over the hypotheses:
$$P(X\ |\ \textbf{d}) = \sum_i P(X\ |\ \textbf{d},h_i)\ ·\ P(h_i\ |\ \textbf{d}) = \sum_i P(X\ |\ h_i)\ ·\ P(h_i\ |\ \textbf{d})$$
<br>  
## 26.2 Approximations of Bayesian Learning
- **Maximum a posteriori learning (MAP learning):** Choose the MAP hypothesis $h_{MAP}$ that maximizes $P(h_i\ |\ \textbf{d})$.   
  i.e. maximize $P(\textbf{d}\ |\ h_i)\ ·\ P(h_i)$ or (even better) $log_2(P(\textbf{d}\ |\ h_i)) + log_2(P(h_i))$.   
  -> Idea: Make predictions wrt. the "most probable hypothesis"  
  -> **Note:** Finding MAP hypotheses is often much easier than Bayesian learning because it requires solving an optimization problem instead of a large summation (or integration) problem.  
- **Minimum description length learning (MDL learning):** The MDL hypothesis $h_{MDL}$ minimizes the information entropy of the hypothesis likelihood.  
  -> Reinterpret the log terms $log_2(P(\textbf{d}\ |\ h_i)) + log_2(P(h_i))$ in MAP learning:  
  -> Maximizing $P(\textbf{d}\ |\ h_i)\ ·\ P(h_i) \widehat{=}$  minimizing $- log_2(P(\textbf{d}\ |\ h_i)) - log_2(P(h_i))$.  
- **Maximum likelihood learning (ML):** Choose the ML hypothesis $h_{ML}$ maximizing $P(\textbf{d}\ |\ h_i)$. (also called maximum likelihood approximation)  
  -> ML learning is the standard non-Bayesian statistical learning method.  
  
  \
   
## 26.3 Parameter Learning for Bayesian Networks

### ML parameter learning
Say we open a bag of candies (with lime and cherry candies) where $\theta$ is the parameter giving the fraction candies being cherry. We observe $N$ times and get data $d$. $c$ times we observe cherries and $l=N-c$ times limes This gives us the likelihood:  
$$P(\textbf{d}\ |\ h_{\theta}) = \prod_{j=1}^N P(d_j\ |\ h_{\theta}) = \theta^c\ ·\ (1 - \theta)^l$$
**Trick:** When optimizing a product, optimize the **logarithm** instead.
The **log-likelihood** is just the binary logarithm of the likelihood.  
  $$\begin{align}
  L(\textbf{d}\ |\ h_{\theta}) &= log_2(P(\textbf{d}\ |\ h_{\theta}))\\[1ex]
  &= \sum_{j=0}^N log_2(P(d_j\ |\ h_{\theta}))\\[1ex]
  &= c\ ·\ log_2(\theta) + l\ ·\ log_2(1- \theta)
  \end{align}$$
Maximize this wrt. $\theta$  
  $$\begin{align}
&\frac{\delta}{\delta \theta} (L(\textbf{d}\ |\ h_{\theta})) = \frac{c}{\theta} - \frac{l}{1-\theta} = 0\\[2ex]
&\leadsto \theta = \frac{c}{c+l} = \frac{c}{N}
\end{align}$$
<br>

### ML learning for multiple parameters in Bayesian networks
1. Write down an expression for the likelihood of the data as a function of the parameter(s).  
2. Write down the derivative of the log likelihood with respect to each parameter.  
3. Find the parameter values such that the derivatives are zero.  
-> Basically the same as above but with more than one parameter and hence multiple derivatives. Do the same for each parameter separately.  

**Remaining problem:** Be careful with zero values! (Division by zero)  
<br>
## 26.4 Naive Bayes Models
**Recap:** A Bayesian network in which a single cause directly influences a number of effects, all of which are conditionally independent, given the cause is called a naive Bayes model or Bayesian classifier.
For more details about naive Bayes model see chapter [20.6 Conditional Independence](01_Chapter-20_Quantifying-Uncertainty.md#Naive%20Bayes%20Models)  

Naive Bayes models are probably the most commonly used Bayesian network models in machine learning:
- The "class" variable $C$ (which is to be predicted) is the root  
- The "attribute" variables $X_i$ are the leaves  

Idea: Once trained, use this model to classify new examples, where $C$ is unobserved:
- With observed values $x_1,…,x_n$, the probability of each class is given by:  
$$P(C\ |\ x_1,…,x_n) = \alpha\ P(C)\ ·\ \prod_i P(x_i\ |\ C)$$
- A deterministic prediction can be obtained by choosing the most likely class  
- Naive Bayes learning turns out to do surprisingly well in a wide range of applications.  
- Naive Bayes learning scales well: with $n$ Boolean attributes, there are just $2n+1$ parameters, and no search is required to find $h_{ML}$.  
- Naive Bayes learning systems have no difficulty with noisy or missing data and can give probabilistic predictions when appropriate.  
