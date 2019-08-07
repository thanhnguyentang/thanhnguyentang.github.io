---
layout: post 
title: Actor-Critic Variants and Recursion-Preserving Principle for Incremental Learning
date: 2019-05-13
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 

# Summary 
> 
* An overview of actor-critic variants: REINFORCE, REINFORCE with baseline, Advantage Actor-Critic, TRPO. 
* The recursion-preserving principle for incremental learning. 
* TRPO can be considered as elastic weight consolidation (EWC) for preventing catastrophic forgetting within actor-critic framework. 

You might feel overwhelmed by many incremental updates (e.g., TD learning, Q-learning, Monte Carlo gradien policy) in RL and might wonder if these incremental updates are arbitrary. As always,  it is a good idea to seek for something invariant when facing complex systems or ideas with many variants (on this point, check out [3Blue1Brown's video](https://www.youtube.com/watch?v=M64HUIJFTZM&feature=youtu.be) as another example of how the mindset of **"seeking invariance under variance"** is useful). What's invariant here is an implicit principle that I would like to dub as recursion-preserving principle for incremental update: 

# Recursion-preserving principle for incremental learning (RPIL)
> No matter which incremental update (e.g., TD learning, Q-learning, Monte Carlo gradien policy) you use (or wish to design), the principle is to make sure that the incremental update preserves the recursive relationship derived from Bellman equation. This is to make sure the resulting estimate is unbiased; thus, it would likely converge to the Bellman optimality. 

# Monte Carlo Policy Gradient (REINFORCE): 

* For each episode
    * Sample $\tau = (s_0, a_0, r_0, ..., s_T, a_T, r_T)$ from $p(a\|s, \theta)$   
    * For $t = 0:T$ do
        * $G_t \leftarrow \sum_{i=0}^{T-t} \lambda^i r_{t+i} $  
        * $ \theta \leftarrow \theta + \alpha \gamma^t G_t \nabla_{\theta} \log \pi(a_t \| s_t, \theta) $

### Problems with PG: 
The underlying principle of PG is to update the policy towards the direction of the steepest ascent of the expected return. There are four potential (and indeed current) problems with that: 
* PG uses first-order derivative which is not good for a region with high curvature (the reason is that first-order derivative locally approximates a true region to be flat). Consequently, a region with locally flat area next to a high curvature can easily fool PG into making an aggressive update which falls off of the flat area into a significant drop in the expected return. This makes the training unstable. Second-order derivative is more informative about the curvature that the first-order derivative. 

* PG makes update only after a whole trajectory is observed. Using only return functions $G_t$ without value functions or Q-value functions, it does not make sense for PG to make an update when a whole trajectory is not fully observed yet (refer to the RPIL principle in the beginning of this post). 


# Monte Carlo Policy Gradient with baseline: 

* For each episode
    * Sample $\tau = (s_0, a_0, r_0, ..., s_T, a_T, r_T)$ from $p(a\|s, \theta)$   
    * For $t = 0:T$ do
        * $G_t \leftarrow \sum_{i=0}^{T-t} \lambda^i r_{t+i} $  
        * $\delta_t = G_t - \hat{V}(s_t, w)$  
        * $w \leftarrow w + \beta \delta_t \nabla \hat{V}_{w}(s_t, w)$
        * $ \theta \leftarrow \theta + \alpha \gamma^t \delta_t \nabla_{\theta} \log \pi(a_t \| s_t, \theta) $

# Actor-Critic (1-step): 
    
* For each episode
    * For $t = 0:T$ do
        * Sample $a_t \sim p(a \| s_t), r_t = R(s_t, a_t), s_{t+1} \sim p(s \|s_t, a_t)$
        * $\delta = r_t + \gamma \hat{V}(s_{t+1}, w) - \hat{V}(s_t, w)$  
        * $w \leftarrow w + \beta \delta \nabla \hat{V}_{w}(s_t, w)$
        * $ \theta \leftarrow \theta + \alpha \gamma^t \delta \nabla_{\theta} \log \pi(a_t \| s_t, \theta) $  

# Trust-Region Policy Optimization (TRPO) 

**Preliminaries**: Please refer to  [4] for notations and MDP setting. 

* Advantage function: 

$$
A_{\pi}(s,a) = Q_{\pi}(s,a) - V_{\pi}(s). 
$$

* The expected discounted reward under policy $\pi$ is:

$$\label{eq:edr}
J(\pi) = J(\theta) = \int d_{\pi}(s) V_{\pi}(s) ds = \mathbb{E}_{\tau \sim \pi } \left[ \sum_{t=0}^T \gamma^t r_t \right]
$$ 

* Relating expected discounted reward of different policies 

$$
J(\pi) = J(\pi_{old}) + \mathbb{E}_{\tau \sim \pi} \left[ \sum_{t=0}^{\infty} \gamma^t A_{\pi_{old}}(s_t, a_t) \right] \\ 
= J(\pi_{old}) + \sum_{s} \rho_{\pi}(s) \sum_{a} \pi(a|s) A_{\pi_{old}}(s,a) \\ 
= J(\pi_{old}) + \mathbb{E}_{s \sim \rho_{\pi}, a \sim \pi} \left[ A_{\pi_{old}}(s,a) \right]
$$

where $\rho_{\pi}(s)$ is the unormalized discounted visitation frequencies: 

$$
\rho_{\pi}(s) = \sum_{t=0}^{\infty} \gamma^t d_{\pi}(s_t = s).
$$

* Local approximation to $J(\pi')$ 

$$
L_{\pi_{old}}(\pi) = J(\pi_{old}) + \mathbb{E}_{ \underset{\text{sample from } \pi_{old} \text{ instead of } \pi}{\boxed{s \sim \rho_{\pi_{old}}}}, a \sim \pi} \left[ A_{\pi_{old}}(s,a) \right]  \\ 
= J(\pi_{old}) + \mathbb{E}_{s \sim \pi_{old}, a \sim q} \left[  \frac{\pi(a|s)}{q(a|s)} A_{\pi_{old}}(s,a) \right]
$$

Really, the main objective $L_{\pi_{old}}(\pi)$ is actually similar to the objective in the advantage actor critic [4] (with some minor variants such as importance sampling and whether to use the advantage function, Q-function and the expected return $G_t$ in the place of $A_{\pi_{old}}(s,a)$).


* TRPO problem 

$$
\max_{\pi}  L_{\pi_{old}}(\pi) \\
\text{ subject to } \bar{D}_{KL}(\theta_{old} \| \theta) := \mathbb{E}_{s \sim \rho_{\pi_{old}}} \left[ D_{KL}\left[ \pi_{old}(.|s) \| \pi(.|s) \right]\right] \leq \delta. 
$$

Thus, the key conceptial contribution of TRPO is the idea of KL constraint on the policy to prevent it from making aggressive change far away from the previous policy. 

* Technically, more care is needed to handle the KL constraint in TRPO. The tecniques the original TRPO [5] uses are:  
    * Idea: use linear approximation to the objective $L_{\pi_{old}}(\pi)$ and quadratic approximation to the KL constraint:

    $$
    \bar{D}_{KL}(\theta_{old} \| \theta) \approx \frac{1}{2} (\theta - \theta_{old})^T A (\theta - \theta_{old})
    $$

    where 
    $$
    A_{ij} = \frac{\partial^2}{\partial \theta_i \partial \theta_j} \bar{D}_{KL}(\theta_{old} \| \theta) 
    $$

    * Compute the search direction $s \sim A^{-1} g$ (using conjugate gradient) where $g$ is the gradient of $J$. 
    * Compute the maximal step length $\beta$ such that $\theta_{old} + \beta s$ still satisfies the KL constraint. Plugging this into the quadratic approximation of the KL, we can derive: $\beta_{max} = \sqrt{ 2 \delta / s^T A s}$. Then perform a line search in direction $\theta_{new} \leftarrow \theta_{old} + \beta s$ where $0 \leq \beta \leq \beta_{max}$ exponentially decays until the objective $L_{\theta_{old}}(\theta_{new})$ improves and $\theta_{new}$ satisfies the KL constraint.
<!-- COMMENT: Why use conjugate gradient + line search while we can simply inporate the KL constraint as a regularizer? -->

* **Comment**: This idea of KL constraint (e.g., the quadratic approximation of the KL) in policy change is actually similar to using EWC (elastic weight consolidation) for overcoming catastrophic forgetting [6-7]. The idea is that for parameters that are important for an old task (here a task is about the actor doing a good job aganst the criticism from the critic), one should not change these parameters too much for the new task because we do not want the actor to catastrophically forget about handling the previous criticism as a result of handling the current criticism). 


<!-- # Proximal Policy Optimization (PPO) 

* Policy gradient methods have low convergence rate. This can be improved with natural policy gradient. 
* Natural policy gradient however involves an expensive second-order derivative matrix. 
* PPO comes to rescue by relaxing a hard constraint and approximating second-order method.  -->

# References  
[1] [Deep Reinforcement Learning and Control Fall 2018, CMU 10703](http://www.andrew.cmu.edu/course/10-703/)   
[2] https://medium.com/@jonathan_hui/rl-trust-region-policy-optimization-trpo-explained-a6ee04eeeee9   
[3] https://medium.com/@jonathan_hui/rl-proximal-policy-optimization-ppo-explained-77f014ec3f12   
[4] [https://thanhnguyentang.github.io/blogs/actor_critic](https://thanhnguyentang.github.io/blogs/actor_critic)   
[5] [Trust Region Policy Optimization](https://arxiv.org/pdf/1502.05477.pdf)   
[6] [Overcoming catastrophic forgetting in neural networks](https://arxiv.org/pdf/1612.00796.pdf)   
[7] [On Catastrophic Forgetting in Generative Adversarial Networks](https://arxiv.org/abs/1807.04015)

(work in progress)
