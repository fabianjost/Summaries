# Chapter 24: Making Complex Decisions
  
## 24.1 Sequential Decision Problems
In **sequential decision problems**, the agent's utility depends on a sequence of decisions.

### Markov Decision Process
A **sequential decision problem** in a fully observable, stochastic environment with a Markovian transition model and an additive reward function is called a **Markov decision process (MDP)**. 
- a set of States $s ∈ S$ (with initial state $s_0 ∈ S$)  
- sets Actions(s) of actions $a ∈ A$ for each state s.    
- **Transition Model**: $\bf P(s'\ |\ s,a)\ \widehat{=}$ probability that $a$ in $s$ leads to $s'$.  
- **Reward function** $R:\ S\ \to\ \mathbb{R}$:  
$$R(s,a,s') := 
\begin{cases}
-0.04\ &\text{if (small penalty) for nonterminal states}\\
± 1\ &\text{if for terminal states}
\end{cases}$$
- In **search problems**, the aim is to find an **optimal sequence of actions**.  
- In **MDPs**, the aim is to find an **optimal policy $\pi(s)$** i.e. best **action** for every possible **state s**, because we can't predict where one will end up.
- A **policy** is a mapping from states to actions. An **optimal policy** maximizes the expected sum of rewards.


## 24.2 Utilities over Time
### Utilities of State Sequences
For stationary preferences, there are only two ways to combine rewards over time:
- **additive rewards**: $U([s_o,s_1,s_2,…]) = R(s_0) + R(s_1) + R(s_2) + …$  
- **discounted rewards**: $U([s_o,s_1,s_2,…]) = R(s_0) + \gamma\ R(s_1) +\gamma^2\ R(s_2) + …$  where $\gamma$ is called the **discount factor**.  

With infinite lifetimes the additive utilities become infinite. Therefore we can either choose a finite horizon or use discounting:
- **Finite horizon**: Terminate utility computation at a fixed time T.
$$U([s_0,…,s_\infty]) = R(s_0) + …+ R(s_T)$$
$\leadsto \text{nonstationary policy: } \pi(s)$ depends on time left
- **Discounting**: assuming $\gamma\ < 1, R(s) \leq R_{max}$  
$$U([s_0,…,s_\infty]) = \sum_{t=0}^\infty \gamma^t R(s_t) \leq \sum_{t=0}^\infty \gamma^t R_{max} ?= R_{max}/(1-\gamma)$$
Smaller $\gamma \leadsto$ shorter horizon.
- Idea: **Maximize system gain** $\widehat{=}$ average reward per time step.
- The **optimal policy has constant gain after initial transient**.  
e.g. Taxi driver's daily scheme cruising for passengers.  

### Utility of States
Given a policy $\pi$, let $s_t$ be the state the agent reaches at time t starting at state $s_0$. Then the **expected utility** obtained by executing $\pi$ starting in s is given by:
$$\bf U^\pi (s) := E\Big[\sum_{t=0}^\infty \gamma^t R(s_t)\Big]$$
we define $\pi_s^* := argmax_\pi (U^\pi (s))$, with $\pi^* := \pi_s^*$ as the optimal policy.  
- The **utility U(s)** of a state s is $U^{\pi*}(s)$.  
- Remark $R(s) \widehat{=}$ "short-term reward", whereas $U \widehat{=}$ "long-term reward"
- Given the utilities of the states, choosing the best action is just **MEU**:
-> maximize the expected utility of the immediate successors:
$$\pi^*(s) = argmax_{a ∈ A(s)}(\sum_{s'} P(s'\ |\ s,a)\ ·\ U(s'))$$

## 24.3 Value/Policy Iteration

### Dynamic Programming: The Bellman equation
expected sum of rewards = current reward + $\gamma\ ·$ expected reward sum after best action  
**Bellman equation (1957)**:
$$U(s) = R(s) + \gamma\ ·\ max_{a∈A(s)} (\sum_{s'} U(s')\ ·\ P(s'\ |\ s,a))$$
with the s' being all possible next states and the "max" therefore "choosing" the best possible next state based on the utility U(s').  

### Value Iteration Algorithm
1. start with arbitrary utility values
2. update to make them locally consistent with the Bellman equation
3. everywhere locally consistent $\leadsto$ global optimality
(For the Algorithm see notes slide 858)  

### Convergence
For any two approximations $U^t$ and $V^t$:
$$||U^{t+1} - V^{t+1} || \leq || U^t - V^t ||$$
i.e. any distinct approximations must get closer to each other so any approximations must get closer to the true U and value iteration converges to an unique, stable, optimal solution.
If $|| U^{t+1} - U^t || < \epsilon$, them $|| U^{t+1} - U || < 2 \epsilon(\gamma/1- \gamma)$. i.e. once the change in $U^t$ becomes small, we are almost done.
-> MEU policy using $U^t$ may be optimal long before convergence of values.  

### Policy Iteration
Search for **optimal policy** and **utility values simultaneously**:
- **policy evaluation**: given policy $\pi_i$, calculate $U_i = U^{\pi_i}$, the utility of each state were $\pi_i$ to be executed.
- **policy improvement**: calculate a new MEU policy $\pi_{i+1}$ using 1-lookahead
Terminate if **policy improvement** yields no change in utilities.
$$U(s) = R(s) + \gamma\ \sum_{s'} U(s')\ ·\ P(s'\ |\ s,\pi(s))$$

### Modified Policy Iteration
- Policy iteration often converges in few iterations, but each is expensive  
- Use a few steps of value iteration (but with fixed $\pi$), starting from the value function produced the last time to produce an approximate value determination step.  
- Often converges much faster than pure VI or PI  
- Leads to much more general algorithms where Bellman value update and Howard policy update can be performed locally in any order.  
- **Reinforcement learning** algorithms operate by performing such updates based on the observed transition made in an initially unknown environment.  


## 24.4 Partially Observable MDPs
A **partially observable MDP (POMDP)** is a MDP together with an sensor model $O$ that has the sensor Markov property and is stationary: $O(s,e) = P(e\ |\ s)$.  
Because an agent does not know which state it is in, it does not make sense to talk about policy $\pi(s)$.  
-> **Astrom 1965:** The optimal policy in a POMDP is a function $\pi(b)$ where $b$ is the belief state (probability distribution over states).  
(Instead of s states we use b belief states)  

If $B(s)$ is the previous belief state and agent does action $a$ and then perceives $e$, then the new belief state is  
$$b'(s') = \alpha\ ·\ P(e\ |\ s')\ ·\ \sum_s P(s'\ |\ s,a)\ ·\ b(s)$$
We write $b' = FORWARD(b,a,e)$ in analogy to **recursive estimation**.  
The **POMDP decision cycle** is to iterate over:
1. Given the current **belief state** $\bf b$, execute the action $a=\pi^*(b)$.  
2. Recieve percept $e$.  
3. Set the current **belief state** to $FORWARD(b,,a,e)$ and repeat.
Problem: The belief state is continous: If there are $n$ states, $b$ is an $n$-dimensional real-valued vector.
-> Solving POMDPs is very hard (PSPACE- hard)  
-> The real world is a POMDP (with initially unknown transition model $T$ and sensor model $O$)  


## 24.5 Online Agents with POMDPs
**Dynamic Decision Networks:**
- **Transition** and **sensor model** are represented as a DBN (dynamic Bayesian network).  
- **Action nodes** and **utility nodes** are added to create a **dynamic decision network (DDN)**.  
- **Filtering algorithm** is used to incorporate each new percept and action and to update the belief state representation.  
- **Decisions** are made by projecting forward possible action sequences and choosing the best one.  
- A **POMDP-agent** automatically takes into account the value of information and executes information-gathering actions where appropriate.  
- **Time complexity** for exhaustive search up to depth $d$ is $\mathcal O(|A|^d\ ·\ |E|^d)$.  ($|A| \widehat{=}$ number of actions, $|E| \widehat{=}$ number of percepts).  