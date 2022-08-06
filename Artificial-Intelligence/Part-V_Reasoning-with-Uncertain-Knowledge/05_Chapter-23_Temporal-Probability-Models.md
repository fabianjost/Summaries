# Chapter 23: Temporal Probability Models
  
## 23.1 Modeling Time and Uncertainty
When considering decision models, the **state can stay the same during diagnosis**, *episodic* (e.g. repairing a car), or it can **change during diagnosis**, *sequential* (e.g. medical patient).
A **temporal probability model** is a probability model, where possible worlds are indexed by a **time structure** $\langle S,\preceq \rangle$.  It has two sets of random variables:
- $X_t \widehat{=}$ set of (unobservable) **state variables** at time $t \geq 0$. (e.g. $BloodSugar_t$)  
- $E_t \widehat{=}$ set of (observable) **evidence variables** at time $t > 0$. (e.g. $MeasuredBloodSugar_t$)  
Notation: $X_{a:b} = X_a,X_{a+1},...,X_{b-1},X_b$.
  
### Markov Processes
- **First-order Markov property** (also called **Markov chain**) :
$$P(X_t\ |\ X_{0:t-1}) = P(X\ |\ X_{t-1})$$
- **Second-order Markov property:**
$$P (X_t\ |\ X_{0:t-1}) = P(X_t\ |\ X_{t-2},X_{t-1)}$$
First-order Markov properties are not exactly true in the real world therefore we have two options to solve this:
1. **Increase the order of the Markov process**, this however adds memory to the process.
2. **Add state variables**, e.g. if we have a robot, we can add the velocity instead of only the position. With this we get more of a network of variables and less a chain, but we can get around increasing the order of the Markov process.


## 23.2 Inference: Filtering, Prediction and Smoothing

### Filtering
$$\begin{align}
&\bf P(X_{t+1}\ |\ e_{1:t+1}) = \alpha\ P(e_{t+1}\ |\ X_{t+1})\ ·\ \sum_{x_t} P(X_{t+1}\ |\ x_t)\ ·\ P(x_t\ |\ e_{1:t})\\
&e.g.\\[1.5ex]
&P(R_1\ |\ u_1) = \alpha\ P(u_1\ |\ R_1)\ ·\ P(R_1) = \alpha\ \langle 0.9,0.2\rangle \langle0.5,0.5\rangle = \alpha\ \langle0.45,0.1\rangle \approx \langle0.818,0.182\rangle\\[1ex]
&with\ \alpha = \frac{1}{0.45 + 0.1}
\end{align}$$
-> $\bf P(X_{t+1}\ |\ X_t)$ is simply the **transition model**, $\bf P(x_t\ |\ e_{1:t})$ the "recursive call".
-> $\bf f_{1:t+1} = \alpha\ ·\ FORWARD(f_{1:t},e_{t+1})\ where\ f_{1:t} = P(X_t\ |\ e_{1:t})$ and FORWARD the update shown above

### Prediction
**Prediction** is **Filtering** without new evidence:
$$\bf P(X_{t+k+1}\ |\ e_{1:t}) = \sum_{x_{t+k}}P(X_{t+k+1}\ |\ x_{t+k})\ ·\ P(x_{t+k}\ |\ e_{1:t})$$
### Smoothing
**Smoothing** estimates past states by computing $\bf P(X_k\ |\ e_{1:t})$ for $0 \leq k <t$.  
-> Divide evidence $e_{1:t}$ into $e{1:k}$ (before k) and $e_{k+1:t}$ (after k):  
$$\begin{align}
P(X_k\ |\ e_{1:t}) &= P(X_k\ |\ e_{1:k},e_{k+1:t})\\
&= \alpha\ ·\ P(X_k\ |\ e_{1:k})\ ·\ P(e_{k+1:t}\ |\ X_k,e_{1:k})\\
&= \alpha\ ·\ P(X_k\ |\ e_{1:k})\ ·\ P(e_{k+1:t}\ |\ X_k)\\
&= \alpha\ ·\ f_{1:k}\ b_{k+1:t}
\end{align}$$
-> **Backward message** $\bf b_{k+1:t} = P(e_{k+1:t}\ |\ X_k)$ computed by a backwards recursion:  
$$\bf b_{k+1:t} = P(e_{k+1:t}\ |\ X_k) = \sum_{x_{k+1}} P(e_{k+1:t}\ |\ x_{k+1})\ ·\ P(e_{k+2:t}\ |\ x_{k+1})\ ·\ P(x_{k+1}\ |\ X_k)$$
$P(e_{k+1}\ |\ x_{k+1})$ and $P(x_{k+1}\ |\ X_k)$ can be directly obtained from the model, $P(e_{k+2:t}\ |\ x_{k+1})$ is the "recursive call" ($b_{k+2:t}$).  

### Most Likely Explanation
E.g in speech recognition it is useful to know what is the most likely word sequence.
**Idea:** Uses mooting to find posterior distribution in each time step, construct sequence of most likely states.  
Identical to filtering, except $\bf  f_{1:t}$ replaced by:
$$m_{1:t} = \max_{x_1,...,x_{t-1}} P($$


## 23.3 Hidden Markov Models
A **hidden Markov model (HMM)** is a Markov chain with a single, discrete variable $X_t$ with domain $\{1,...S\}$ and a single, discrete evidence variable.
**Transition model**: $\bf P(X_t\ |\ X_{t-1}) \widehat{=}$ a single $S \times S$ matrix: $T = P(X_t\ |\ X_{t-1}) = \left( {\begin{array}{cc} 0.7 & 0.3 \\ 0.3 & 0.7 \\ \end{array} } \right)$ 
**Sensor model**: $\bf O_t$ for each time step $\widehat{=}$ diagonal matrix with $\bf O_{tii} = P(e_t\ |\ X_t = i)$.  

Recasting the Markov inference as matrix computation gives us two identities:
- **HMM filtering equation**: $\bf f_{1:t+1} = \alpha\ ·\ (O_{t+1}\ T^t\ f_{1:t})$  
- **HMM smoothing equation**: $\bf b_{k+1:t} = T\ O_{k+1}\ b_{k+2:t}$  
-> The forward-backward algorithm for HMMs has time complexity $\mathcal O (S^2t)$ and space complexity $\mathcal O(S^2t)$.  


## 23.4 Dynamic Bayesian Networks
A Bayesian network $\mathcal D$ is called **dynamic (DBN)**, iff its random variables are indexed by a time structure. We assume that $\mathcal D$ is:  
- **time sliced**, i.e. that the **time slices** $\bf \mathcal D_t$ - the ubgraphs of t-indexed random variables and the edges between them are isomorphic.  
- a Markov chain, i.e. that variables $X_t$ can only have parents in $\mathcal D_t$ and $\mathcal D_{t-1}$.   
