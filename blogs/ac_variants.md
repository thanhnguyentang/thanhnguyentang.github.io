---
layout: post 
title: Actor-Critic Variants 
date: 2019-05-13
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 


* Monte Carlo Policy Gradient (REINFORCE): 
    * For each episode
        * Sample $\tau = (s_0, a_0, r_0, ..., s_T, a_T, r_T)$ from $p(a\|s, \theta)$   
        * For $t = 0:T$ do
            * $G_t \leftarrow \sum_{i=0}^{T-t} \lambda^i r_{t+i} $  
            * $ \theta \leftarrow \theta + \alpha \gamma^t G_t \nabla_{\theta} \log \pi(a_t \| s_t, \theta) $



* Monte Carlo Policy Gradient with baseline: 
    * For each episode
        * Sample $\tau = (s_0, a_0, r_0, ..., s_T, a_T, r_T)$ from $p(a\|s, \theta)$   
        * For $t = 0:T$ do
            * $G_t \leftarrow \sum_{i=0}^{T-t} \lambda^i r_{t+i} $  
            * $\delta_t = G_t - \hat{V}(s_t, w)$  
            * $w \leftarrow w + \beta \delta_t \nabla \hat{V}_{w}(s_t, w)$
            * $ \theta \leftarrow \theta + \alpha \gamma^t \delta_t \nabla_{\theta} \log \pi(a_t \| s_t, \theta) $

* Actor-Critic (1-step): 
    * For each episode
        * For $t = 0:T$ do
            * Sample $a_t \sim p(a \| s_t), r_t = R(s_t, a_t), s_{t+1} \sim p(s \|s_t, a_t)$
            * $\delta = r_t + \gamma \hat{V}(s_{t+1}, w) - \hat{V}(s_t, w)$  
            * $w \leftarrow w + \beta \delta \nabla \hat{V}_{w}(s_t, w)$
            * $ \theta \leftarrow \theta + \alpha \gamma^t \delta \nabla_{\theta} \log \pi(a_t \| s_t, \theta) $

# References  
[1] [Deep Reinforcement Learning and Control Fall 2018, CMU 10703](http://www.andrew.cmu.edu/course/10-703/)