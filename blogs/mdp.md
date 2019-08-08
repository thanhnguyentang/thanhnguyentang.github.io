---
layout: post 
title: MDP basics
date: 2019-05-13
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 


# Preliminaries 

* The expected discounted reward: 

$$
J(\pi) = \mathbb{E}_{\mu(s)} \mathbb{E}_{\pi} \left[ \sum_{t=0}^{\infty} \gamma^t R(s_t, a_t) \bigg| s_0 = s\right]
$$

* The Bellman optimality operator: 

$$
T: \mathbb{R}^{\mathcal{S}} \rightarrow \mathbb{R}^{\mathcal{S}} \\ 
\forall s \in \mathcal{S}, V(s) \mapsto (TV)(s) := \max_{a \in \mathcal{A}} \{ R(s,a) + \gamma \mathbb{E}_{p(s'|s,a)} [V(s')] \}
$$

The optimal value function $V^* $ (i.e., $V^* \geq V, \forall V$) is the fixed point of $T$, i.e., $TV^* = V^*$. 
The optimal policy can then be constructed from the optimal value function: $$\pi^*(s) = arg\max_{a \in \mathcal{A}} \{ R(s,a) + \gamma \mathbb{E}_{p(s'|s,a)} [ V^{ * }(s')] \}$$. 
* Given $V_1, V_2 \in \mathbb{R}^{\mathcal{S}}$, we denote "$V_1(s) \geq V_2(s), \forall s \in \mathcal{S}$" by "$V_1 \geq V_2$".  

> **Theorem** (monotonicity): $T$ is monotonic, i.e., $V_1 \geq V_2 \implies TV_1 \geq TV_2$.  

> **Theorem** (contraction): $$
\max_{s \in \mathcal{S}} |(TV_1)(s) - (TV_2)(s) | \leq \gamma \max_{s\in\mathcal{S}} |V_1(s) - V_2(s) |, \forall V_1, V_2.
$$

*Proof*: 

$$
|(TV_1)(s) - (TV_2)(s)| =  \bigg|\max_{a \in \mathcal{A}} \{ R(s,a) + \gamma \mathbb{E}_{p(s'|s,a)} [V_1(s')] \} -  \max_{a \in \mathcal{A}} \{ R(s,a) + \gamma \mathbb{E}_{p(s'|s,a)} [V_2(s')] \} \bigg| \\
\leq \gamma \max_{a \in \mathcal{A}} \bigg|  \mathbb{E}_{p(s'|s,a)} [V_1(s')] -  \mathbb{E}_{p(s'|s,a)} [V_2(s')]\bigg| \\ 
\leq \gamma  \max_{a \in \mathcal{A}} \bigg|  \mathbb{E}_{p(s'|s,a)} [  \max_{s\in\mathcal{S}} |V_1(s) - V_2(s) | ]\bigg| = \gamma \max_{s\in\mathcal{S}} |V_1(s) - V_2(s) |.
$$

> **Auxilary**: $T^{\infty}V = V^*, \forall V. $ 

* The Bellman optimality problem reframed as **linear program**:  The optimal value function $V^*$ is the unique solution of the following linear program,

$$
P^* := \min_{V} (1 - \gamma) \mathbb{E}_{\mu(s)} [ V(s)] \\
\text{subject to } V(s) \geq R(s,a) + \gamma \mathbb{E}_{p(s'|s,a)} [V(s')], \forall (s,a).
$$

* The **dual form** (which can be easily derived using [1]) of the LP above : 

$$
D^* := \max_{\lambda \geq 0} \sum_{s,a} \lambda(s,a) R(s,a) \\ 
\text{subject to } (1 - \gamma) \mu(s') + \gamma \sum_{s,a} \lambda(s,a) p(s' | s, a) = \sum_{a} \lambda(s',a), \forall s'.
$$

> **Theorem**: 
$$
\sum_{s,a} \lambda^*(s,a) = 1, \pi^*(a|s) = \frac{\lambda^*(s,a)}{ \sum_{a'} \lambda^*(s,a')}.
$$

# References
[1] https://thanhnguyentang.github.io/blogs/lagrangian  
[2] [Boosting the Actor with Dual Critic](https://arxiv.org/abs/1712.10282)

(work in progress)