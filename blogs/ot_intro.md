---
layout: post 
title: A Naive Introduction to Optimal Transport
date: 2019-05-13
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 

# 1. There are many divergences, why optimal transport? 

Different from KL divergence, optimal transport (OT) is a legimate distance. And unlike $L_1$-norm, OT has a continuity property: as probability measures is that as one probability measure weakly 
converges to another probability measure, their OT distance also converges to zero (Slide 9/47 in [1]). More formally, this "continuity" property can be stated as follows. 

Consider the OT distance denoted by: 

$$W_p(\alpha, \beta) := \min_{\pi \in \mathcal{M}^1_{+}(\mathcal{X}^2)} \left\{ \int_{\mathcal{X}^2} d(x,y)^p d\pi(x,y): \pi_1 = \alpha, \pi_2 = \beta \right\}$$

Then, we have: 
$$ \alpha_n \rightharpoonup \beta \iff W_p(\alpha_n, \beta) \rightarrow 0 $$ where $\alpha_n \rightharpoonup \beta$ denotes weak convergence, i.e., $\forall f \in \mathcal{C}(\mathcal{X}), \int_{\mathcal{X}} f d \alpha_n \rightarrow \int_{\mathcal{X}} f d \beta$.

# 2. Reducing the computationality of OT 
One major drawback of OT is its expensive computation, which is $O(n^3 \log(n))$ in general. In some special cases, the computation can be less, e.g., $O(n^3)$ if $\alpha$ and $\beta$ have the same weights, $O(n \log(n))$ for 1-D case, and $O(n^2 \log(n))$ for $p=1$. 

One of the major research direction for OT that I am aware is to reduce the computation of OT. A common approach is to introduce regularization into the OT objective so that we relax some constraints and solve for an approximation optimization. Interestingly, regularization in some particular way does not give the optimazation more difficult, it makes it more tracable. For example, in Sinkhorn divergence, an entropy regularization is added, resulting a tractable optimization. Specifically, the regularized OT objective in Sinkorn is: 

$$\min_{P \in U(a,b)} \sum_{i,j} d(x_i, y_j)^p P_{i,j} + \epsilon P_{i,j} \log \frac{P_{i,j}}{a_i b_j}$$

The solution to the regularized OT optimization above is tractable, as given by: $P_{i,j} = u_i K_{i,j} v_j, K_{i,j} = e^{-\frac{d(x_i, y_j)^p}{\epsilon}}$.

Which rationale that allows an entropic regularization to make a more tractable optimization even if adding an entropic regularization seems to make the objective more complex? Introducing an entropy constraint into the OT objective can be thought of as relaxing the optimization, thus the resulting optimization can be easier, not nessasarily harder because of having more terms. Indeed, the regularized optimization is easier in this case. In particular, the regularized OT is a constraint optimization that can be solved with Lagrangian method because the strong duality holds for this optimization (the entropy term is convex). Thus, we instead solve the KKT conditions for the Lagrangian, resulting in a closed-form solution which can be efficiently iteratively updated using Sinkorn's update [1]. 

# 3. Discussion  
It strikes me that Lagrangian method is really helpful to regularize hard optimization and by softening it, the resulting optimization becomes more tractable. I have encountered its usage at least in Information Botllteneck [3], Distributional Robustness [4] and now regularized OT. It would be interesting to be able to apply OT into sequential decision making problems, especially Bayesian optimization and Reinforcement Learning. How likely and fruitly it would be? 

# 4. Some additions on the topic  

* When Wasserstein divergence is better than KL divergence?  
    * Wasserstein distance works even for two dimensions that do not have common support. KL divergence fails in such scenario.  
    * Wasserstein distance is a valid distance and is weakly continuous. 


# References: 
[1] Grabriel Peyre, [Optimal Transport for Machine Learning](https://portal.klewel.com/watch/webcast/optimal-transport-for-machine-learning/talk/1/).   
[2] Marco Cuturi, [Sinkhorn Distances: Lightspeed Computation of Optimal Transport](https://arxiv.org/abs/1306.0895).  
[3] Naftali Tishby, Fernando C. Pereira, and William Bialek, [Information Bottleneck Method](https://arxiv.org/abs/physics/0004057).   
[4] John Duchi, and Hongseok Namkoong, [Variance-based regularization with convex objectives](https://arxiv.org/abs/1610.02581).