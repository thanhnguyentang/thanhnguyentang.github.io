---
layout: post 
title: A naive introduction to Optimal Transport. 
date: 2019-05-13
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 

# 1. There are many divergences, why optimal transport? 

The most intriguting property of optimal transport (OT) as probability measures is that as one probability measure weakly 
converges to another probability measure, their OT distance also converges to zero. $L_1$-norm does not have that property (Slide 9/47 in [1]). More formally, this "continuity" property can be stated as follows. 

Consider the OT distance denoted by: 

$$W_p(\alpha, \beta) := \min_{\pi \in \mathcal{M}^1_{+}(\mathcal{X}^2)} \left\{ \int_{\mathcal{X}^2} d(x,y)^p d\pi(x,y): \pi_1 = \alpha, \pi_2 = \beta \right\}$$

Then, we have: 
$$ \alpha_n \rightharpoonup \beta \iff W_p(\alpha_n, \beta) \rightarrow 0 $$ where $\alpha_n \rightharpoonup \beta$ denotes weak convergence, i.e., $\forall f \in \mathcal{C}(\mathcal{X}), \int_{\mathcal{X}} f d \alpha_n \rightarrow \int_{\mathcal{X}} f d \beta$.

# 2. Reducing the computationality of OT 
One major drawback of OT is its expensive computation, which is $O(n^2 \log(n))$ in general. In some special cases, the computation can be less, e.g., $O(n^3)$ if $\alpha$ and $\beta$ have the same weights, $O(n \log(n))$ for 1-D case, and $O(n^2 \log(n))$ for $p=1$. 

One of the major research direction for OT that I am aware is to reduce the computation of OT. A common approach is to introduce regularization into the OT objective so that we relax some constraints and solve for an approximation optimization. Interestingly, regularization in some particular way does not give the optimazation more difficult, it makes it more tracable. For example, in Sinkhorn divergence, an entropy regularization is added, resulting a tractable optimization. Specifically, the regularized OT objective in Sinkorn is: 

$$\min_{P \in U(a,b)} \sum_{i,j} d(x_i, y_j)^p P_{i,j} + \epsilon P_{i,j} \log \frac{P_{i,j}}{a_i b_j}$$

The solution to the regularized OT optimization above is tractable, as given by: $P_{i,j} = u_i K_{i,j} v_j, K_{i,j} = e^{-\frac{d(x_i, y_j)^p}{\epsilon}}$.

I am more curious about which rationale that allows an entropic regularization to make a more tractable optimization even if adding an entropic regularization seems to make the objective more complex. I will try to answer this question in the next update. 

# References: 
[1] Grabriel Peyre, [Optimal Transport for Machine Learning](https://portal.klewel.com/watch/webcast/optimal-transport-for-machine-learning/talk/1/). 


*Note: Post in progress. 