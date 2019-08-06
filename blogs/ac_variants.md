---
layout: post 
title: Actor-Critic Variants and Recursion-Preserving Principle for Incremental Learning
date: 2019-05-13
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 

You might feel overwhelmed by many incremental updates (e.g., TD learning, Q-learning, Monte Carlo gradien policy) in RL and might wonder if these incremental updates are arbitrary. As always,  it is a good idea to seek for something invariant when facing complex systems or ideas with many variants (on this point, check out [3Blue1Brown's video](https://www.youtube.com/watch?v=M64HUIJFTZM&feature=youtu.be) as another example of how the mindset of **"seeking invariance under variance"** is useful). What's invariant here is an implicit principle that I would like to dub as recursion-preserving principle for incremental update: 

# Recursion-preserving principle for incremental learning (RPIL)
> No matter which incremental update (e.g., TD learning, Q-learning, Monte Carlo gradien policy) you use (or wish to design), the principle is to make sure that the incremental update preserves the recursive relationship derived from Bellman equation. This is to make sure the resulting estimate is unbiased; thus, it would likely converge to the Bellman optimality. 

# Monte Carlo Policy Gradient (REINFORCE): 

* For each episode
    * Sample $\tau = (s_0, a_0, r_0, ..., s_T, a_T, r_T)$ from $p(a\|s, \theta)$   
    * For $t = 0:T$ do
        * $G_t \leftarrow \sum_{i=0}^{T-t} \lambda^i r_{t+i} $  
        * $ \theta \leftarrow \theta + \alpha \gamma^t G_t \nabla_{\theta} \log \pi(a_t \| s_t, \theta) $

## Problems with PG: 
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

* Hessian matrix or Fisher Information Matrix can measure the sensitibity of the policy wrt its parameter. 


# Proximal Policy Optimization (PPO) 

* Policy gradient methods have low convergence rate. This can be improved with natural policy gradient. 
* Natural policy gradient however involves an expensive second-order derivative matrix. 
* PPO comes to rescue by relaxing a hard constraint and approximating second-order method. 

# References  
[1] [Deep Reinforcement Learning and Control Fall 2018, CMU 10703](http://www.andrew.cmu.edu/course/10-703/)   
[2] https://medium.com/@jonathan_hui/rl-trust-region-policy-optimization-trpo-explained-a6ee04eeeee9   
[3] https://medium.com/@jonathan_hui/rl-proximal-policy-optimization-ppo-explained-77f014ec3f12   

(work in progress)
