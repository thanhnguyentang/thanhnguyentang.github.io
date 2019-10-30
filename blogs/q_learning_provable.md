---
layout: post
title: Q-learning is provably efficient
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 

**Disclaimer**: This post serves to explain, rearrange and reproduce the result of [1]. 

---
**Algorithm 1** Generic Q-learning algorithm with confidence bonus 

---

* **Input**: MDP$(\mathcal{S}, \mathcal{A}, H, P, r)$ where $H$ is the total number of steps per episode, $$r_h: \mathcal{S} \times \mathcal{A} \rightarrow [0,1], \forall h \in [H] := \{1,2,..,H\}$$ is the deterministic reward function,  $S = \|\mathcal{S}\|$, $A = \|\mathcal{A}\|$, $K$ is the total number of episodes, adaptive learning rate $$(\alpha_t)_{t=1}^{K} \in [0,1]^{K}$$, adaptive confidence bonus $$(b_t)_{t=1}^{K} \in [0,\infty)^{K}$$. 
* Initialize $Q_h(x,a) \leftarrow H, N_h(x,a) \leftarrow 0, V_h(x,a) \leftarrow H$ for all $(x,a,h) \in \mathcal{S} \times \mathcal{A} \times [H]$ where $$[H] = \{1,2,...,H\}$$. 
* **for** episode $k = 1,...,K$ **do**:
    * receive initial state from adversary $x_1$. 
    * **for** step $h = 1,..., H$ **do**:
        * Execute $a_h \leftarrow arg\max_{a'} Q_h(x_h, a')$, and observe $x_{h+1}$. 
        * $t = N_h(x_h,a_h) \leftarrow N_h(x_h,a_h) + 1$  
        * $Q_h(x_h, a_h) \leftarrow (1-\alpha_t) Q_h(x_h,a_h) + \alpha_t[r_h(x_h, a_h) + V_{h+1}(x_{h+1}) + b_t].$
        * $V_h(x_h) \leftarrow$ $$\min\{H,\max_{a'} Q_h(x_h, a') \}$$.   

---

We start with more explicit notations. We denote $Q^k_h, V^k_h, N^k_h$ the $Q_h, V_h, N_h$ at the beginning of episode $k$ in Algorithm 1. Thus, $N^k_h(x,a)$ is the number of times $(x,a)$ is visited at step $h$ counting over $k-1$ previous episodes. Intuitively, $Q_h^k$ and $V_h^k$ becomes "closer" to the optimal $Q$ at step $h$, $$Q^*_h$$ and optimal $V$ function at step $h$, $$V^*_h$$, respecitvely, when more number of episodes (i.e., larger $k$) have been played. A Similar definition for $V^*_h$. Given these explicit notations, we can write their recursive form of Algorithm 1 as: 

$$
Q^{k}_h(x,a) = 
\begin{cases}
(1 - \alpha_t) Q^{k-1}_h(x,a) + \alpha_t[r_h(x,a) + V^{k-1}_{h+1}(x^{k-1}_{h+1}) + b_t], \text{ if } (x,a) = (x^{k-1}_h, a^{k-1}_h) \nonumber \\
Q^{k-1}_h(x,a), \text{ otherwise}. 
\end{cases}
$$

where $$V^k_h(x) = \min \{ H, \max_{a'} Q^k_h(x,a') \}$$ if $h \in [H]$ and $V^k_h(x) = 0$ if $h > H$, $t=N^{k}_h(x,a)$ denotes the number of times $(x,a)$ are visited at step $h$ counting over $k-1$ previous episodes. 

Recall the recursive form for the optimal $Q$ function:

$$
\begin{cases}
Q^*_h(x,a) = r_h(x,a) + \mathbb{E}_{P(x'|x,a)} V^*_{h+1}(x') \nonumber \\ 
V^*_h(x) = arg\max_{a'} Q^*_h(x,a') \\ 
V^*_{H+1}(x) = 0 
\end{cases}
$$

Now, we flatten the recursion above to make it easier for analysis. 
First, let us fix $(x,a,h,k) \in \mathcal{S} \times \mathcal{A} \times [H] \times [K]$. At episode $k$, the pair $(x,a)$ has been visited at step $h$ for $t = N_h^k(x,a) \leq k-1$ times.  Denote by $1 \leq k_1 \leq ... \leq k_t < k$ the episodes in which $(x,a)$ is visited at step $h$.  We have  

$$Q^k_h(x,a) = Q^{k-1}_h(x,a) = ... = Q^{k_t+1}_h(x,a) = (1 - \alpha_t) Q^{k_t}_h(x,a) + \alpha_t[r_h(x,a) + V^{k_t}_{h+1}(x^{k_t}_{h+1}) + b_t] \nonumber .$$

Similarly, we have 

$$
Q^{k_t - 1}_h(x,a) = Q^{k_t -2}_h(x,a) = ... = Q^{k_{t-1} + 1}_h(x,a) = (1 - \alpha_{t-1}) Q^{k_{t-1}}_h(x,a) + \alpha_{t-1}[r_h(x,a) + V^{k_{t-1}}_{h+1}(x^{k_{t-1}}_{h+1}) + b_{t-1}], \nonumber
$$

where $$N^{k_{t-1}+1}_h(x,a)=t-1$$. 


We further denote $\alpha_t^i = \alpha_i \prod_{j=i+1}^t (1-\alpha_j), \forall 1 \leq i \leq t$, and $\alpha_t^0 = \prod_{j=1}^t (1-\alpha_j)$. We have

$$
Q^k_h(x,a) = \alpha_t^0 Q^{k_1}_h(x,a) + \sum_{i=1}^t \alpha_t^i [r_h(x,a) + V^{k_i}_{h+1}(x^{k_i}_{h+1}) + b_i] \nonumber \\ 
= \alpha_t^0 H + \sum_{i=1}^t \alpha_t^i [r_h(x,a) + V^{k_i}_{h+1}(x^{k_i}_{h+1}) + b_i], \nonumber
$$

where the second equation follows from $Q^{k_1}_h(x,a) = H$. The flattened equation of $Q^k_h(x,a)$ gives an explicit intuition: The current $Q$ estimate is a weighted sum of the previous estimates. If $$\alpha_1 = 1$$, by induction, we have $$\sum_{i=1}^t \alpha_t^i = 1$$ for $t \geq 1$. From now on, we consider only the learning rates  $$(\alpha_t)_{t=1}^K$$ where $\alpha_1 = 1$. We adopt the convention that $$\sum_{\emptyset}=0, \prod_{\emptyset} = 1$$; thus $\alpha_t^0 = 1$ and $$\sum_{i=1}^t \alpha_t^i = 0$$ for $t=0$. As in the bandit setting, in Q-learning presented in algorithm 1, the choice of learning rates  $$(\alpha_t)_{t=1}^K$$, thus the running weights $$(\alpha_t^i)_{i=1}^t$$, is crucial to the convergence rate and sample complexity of the algorithm.


It follows from the property above of the running weights that

$$
Q^*_h(x,a) = \alpha_t^0 Q^*_h(x,a)  + \sum_{i=1}^t  \alpha_t^i [r_h(x,a) + \mathbb{E}_{P(x'|x,a)} V^*_{h+1}(x')], \nonumber
$$

for all $t \geq 0$.

Similar to the bandit setting, we substract the optimal $Q$ function from the $Q$ estimates to reveal the convergence rate and sample complexity of the algorithm: 

$$
Q^k_h(x,a) - Q^*_h(x,a) = \alpha_t^0 (H - Q^*_h(x,a)) + \sum_{i=1}^t \alpha_t^i[ V^{k_i}_{h+1}(x^{k_i}_{h+1}) -V^*_{h+1}(x^{k_i}_{h+1}) + V^*_{h+1}(x^{k_i}_{h+1}) - \mathbb{E}_{P(x'|x,a)} V^*_{h+1}(x') + b_i  ]. \nonumber
$$

Now, we bound the error term  defined for every tuple $(x,a,h,k)$: 

$$
U_i(x,a,h,k) := \alpha_t^i [V^*_{h+1}(x^{k_i}_{h+1}) - \mathbb{E}_{P(x'|x,a)} V^*_{h+1}(x')], \forall 1 \leq i \leq t = N_h^k(x,a). \nonumber
$$

The key idea is to observe that $(U_i(x,a,h,k))_{i=1}^t$ is a martingale difference sequence (MDS); thus we can apply the Azuma-Hoeffding inequality to bound the sum of this sequnece. See [2] for the details about MDS and Azuma-Hoeffding inequality. It follows from the Azuma-Hoeffding inequality that for a fixed $\delta >0$ and fixed $(x,a,h,k)$, the following holds with probability at least $1 - \delta / (SAKH)$:

$$
|\sum_{i=1}^t U_i(x,a,h,k)| \leq H \sqrt{ 2 \log \frac{2SAKH}{\delta} } \sqrt{  \sum_{i=1}^t (\alpha_t^i)^2}. \nonumber
$$

It follows from a union bound that with probability at least $1 -\delta$, the following holds simulteneously for all $(x,a,h,k) \in \mathcal{S} \times \mathcal{A} \times [H] \times [K]$: 

$$
|\sum_{i=1}^t U_i(x,a,h,k)| \leq H \sqrt{ 2 \log \frac{2SAKH}{\delta} } \sqrt{  \sum_{i=1}^t (\alpha_t^i)^2}. \nonumber
$$

Notice that thus far, we have not made any specific design choice for the learning rates $$(\alpha_t)_{t=1}^K$$ and confidence bonus $$(b_t)_{t=1}^K$$ except that we assume $$\alpha_1 = 1$$. One option from [1] is to choose them such that $Q^k_h$ behaves like a upper bound confidence (UCB) for $Q^*_h$. 

In the latter parts, [1] carefully controls the **error propagation**  and use Azuma-Hoeffding inequality to bound the regret of Q-learning. The results in summary are: choose $\alpha_t = \frac{H+1}{H+t}$, choose $b_t = c \sqrt{H^3 \log(SAT/\delta) / t}$, then we can achieve sublinear regret: 

$$
Regret(K) = \sum_{k=1}^K [ V^*_1(x_1^k) - V^{\pi_k}_1(x^k_1)] \leq O\left(H^2 SA + H^2 \sqrt{SAT \log(SAT/\delta) } \right)\nonumber
$$

where a starting state $x^k_1$ for each episode $k$ is chosen by an adversary, and $T=KH$ is the total number of steps. I will explain more details for the derivation of this result in the next update of this post. 

## References 
[1] Chi Jin et al., [https://arxiv.org/abs/1807.03765](https://arxiv.org/abs/1807.03765)   
[2] [https://thanhnguyentang.github.io//blogs/measure_theory](https://thanhnguyentang.github.io//blogs/measure_theory) 

(Work in progress)