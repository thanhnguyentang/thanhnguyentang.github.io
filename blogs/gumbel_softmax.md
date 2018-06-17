---
layout: post
title: Gumbel-Softmax to Approximate Samples for a Categorical Distribution  
---   
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 

## Abstract 
In this post, we review the two workaround approaches to backpropagation in stochastic operation and specifically present the Gumbel-Softmax trick for sample approximation to categorical distributions. 

## 1. Introduction 
Sampling operation precludes the backpropagation in a computation graph because it does not have a well-defined gradient. For example, take a look at the computation graph (1) in Figure 1. The deterministic and differential functions on each node in that computation graph allow the chain rule to be applied smoothly. Applying the chain rule to the computation graph (2) in Figure 1 however encounters a difficulty: the sampling 'function' in the graph - the link from node $P_{\theta}(Z)$ to node $Z$ - is not differential. In general, there are two ways we can work around this problem ([1]): (i) Score function-based gradient estimators and (ii) Path derivative gradient estimators. To highlight the general ideas of these two workaround techniques, we first establish a common scenario where a random variable $\pmb{z}$ depends on parameter $\pmb{\theta}$ via $\pmb{z} \sim p_{\pmb{\theta}}(\pmb{z})$ and a scalar cost function $f(\pmb{z})$ depends on $\pmb{z}$. The objective to be minimized is then the expected cost function on $\pmb{\theta}$: 

$$L(\pmb{\theta}) = \mathbb{E}_{p_{\pmb{\theta}}(\pmb{z})} \left[ f(\pmb{z}) \right]$$

{% include image.html url="/blogs/figs/comp_graph_with_sampling.png" description="Figure 1. Examples of a deterministic and stochastic computation graph (Figure from [1])." %}

### 1.1. Score function-based gradient estimators 
The score function estimator (SF) derives the following unbiased estimator: 

$$\nabla_{\pmb{\theta}} \mathbb{E}_{p_{\pmb{\theta}}(\pmb{z})} \left[ f(\pmb{z}) \right] = \mathbb{E}_{p_{\pmb{\theta}}(\pmb{z})} \left[ f(\pmb{z}) \nabla_{\pmb{\theta}} \log p_{\pmb{\theta}}(\pmb{z})  \right] $$ 

The main advantage the SF offers is that it only requires $p_{\pmb{\theta}}(\pmb{z})$ to be differential in $\pmb{\theta}$. Although a SF estimator is unbiased, it has high variance and slow convergent rate. 

### 1.2. Path derivative gradient estimators
Path derivative gradient estimators, on the other hand, approach the backpropagation in sampling operation by representing the sampling operation as a deterministic function, therefore, allowing the gradients to flow through all nodes of a computation graph as usual (we usally refer to this approach as a reparameterization technique ([2])). The deterministic representation of sampling operation might be either equivalent or approximate to the original sampling operation. In particular, we rewrite sampling operation $\pmb{z} \sim p_{\pmb{\theta}}(\pmb{z})$ as: 

$$\pmb{z} = g(\pmb{\theta}, \pmb{\epsilon})$$ 

where $g(.,.)$ is a deterministic function and $\pmb{\epsilon} \sim p(\pmb{\epsilon})$ which does not depdend on $\pmb{\theta}$. It can be noticed in this approach that the main bottleneck is to find such deterministic function $g$. A well-known example of this category is the rewriting of sampling from Gaussian distribution in [2].

## 2. Gumbel-Softmax distribution 
A Gumbel-Softmax distribution is used to approximate samples from a categorical distribution ([1]) and also falls into the paradiam of path derivative gradient estimators. Let us consider a categorical distribution $\pmb{\pi} = (\pi_1, ..., \pi_K)$ where $0 \leq \pi_i \leq 1$ and $\sum_{i=1}^K \pi_i = 1$. Then, the approximate samples from the categorical distribution $\pmb{\pi}$ can be computed using Gumbel-Softmax distribution: 

$$y_i = \frac{ e^{ (\log(\pi_i) + g_i) / \tau }}{ \sum_{i=1}^K e^{  (\log(\pi_i) + g_i) / \tau }  } $$

where $g_i \sim Gumbel(0,1)$ (i.e., $g_i = - \log ( -\log u_i) $ for $u_i \sim U(0,1)$), $1 \leq i \leq K$. Notice that the approximate sample $\pmb{y} = (y_1, ..., y_K)$ is a **deterministic** function of the categorical distribution $\pmb{\pi}$, and this is exactly how approximations of this sort come into play in defeating the undefined backprobagation of stochastic operations.
## References 
[1] E. Jang, S. Gu, and B. Poole. *Categorical reparameterization with gumbel-softmax*. arXiv
preprint arXiv:1611.01144, 2016.  
[2] D. P. Kingma and M. Welling. *Auto-encoding variational bayes*. arXiv preprint arXiv:1312.6114, 2013.
