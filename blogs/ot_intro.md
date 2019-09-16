---
layout: post 
title: A Naive Introduction to Optimal Transport
date: 2019-05-13
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 

# 1. History 
* Originally proposed by Monge in the 18-th century  
* Nobel Prizes for Koopmans and Kantorovich 
* Fields Medals for Villani and Figalli 

# 2. Introduction   
**Primal**: 
* $(M,d)$: a complete separable metric space 
* $p,q$: absolutely continuous Borel probability measure on $M$ 
* $\pi$: a **transport plan**, a joint probability measure on $M \times M$ with marginals $p$ and $q$:   

    $$
        \pi(A \times M) = p(A) \text{ for any Borel subset } A \subseteq M\\
        \pi(B \times M) = q(B) \text{ for any Borel subset } B \subseteq M
    $$
* $d(x,x')$: a distance metric on $M \times M$
* Wasserstein distance between $p$ and $q$:

$$
W(p,q) := \min_{\pi \in \Pi(p,q)} \mathbb{E}_{\pi} d(x,x')
$$

where $\Pi(p,q)$ is the set of all joint probability measure on $M \times M$ with marginals $p$ and $q$.

**Intuition or the primal**:
* $p(x)$: The amount of bread produced in bakery at location $x$ 
* $q(x')$: The amount of bread produced in cafe at location $x'$ 
* $d(x,x')$: The cost to transport unit bread from location $x$ to $x'$ 
* $\pi(x,x')$: The amount of bread to transport from $x$ to $x'$ 
* $W(p,q)$: The minimum cost to transport all bread from bakeries to cafes. 

**Kantorovich duality**: 

$$
W(p,q) = \max_{\alpha, \beta} \{ \sum_{x} \alpha(x) p(x) + \sum_{x'} \beta(x') q(x')  \}  \\
\text{ subject to } \alpha(x) + \beta(x') \leq d(x,x')
$$

**$p$-Wasserstein metric as the $L^p$ metric on the inverse CDF:**

$$
W_p^p(U,V) = \int_{0}^1 | F^{-1}_U(\omega) - F^{-1}_V(\omega) |^p d \omega 
$$


# 3. Many divergences, why optimal transport? 

* Different from KL divergence, Wasserstein distance is a legimate distance. Also, Wasserstein distance works even for two dimensions that do not have common support while KL divergence fails in such scenario. And unlike $L_1$-norm, OT has a continuity property: as probability measures is that as one probability measure weakly converges to another probability measure, their  Wasserstein distance also converges to zero (Slide 9/47 in [1]). More formally, this "continuity" property can be stated as follows. 

$$
p \rightharpoonup q \iff W(p,q) \rightarrow 0
$$

where $p \rightharpoonup q$ denotes weak convergence, i.e., $\forall f \in \mathcal{C}(\mathcal{X}), \int_{\mathcal{X}} f d p \rightarrow \int_{\mathcal{X}} f d q$.

* Applicable in domains where the underlying similarity in the outcome space is more important than exact matching likelihoods. 

<!-- Consider the OT distance denoted by: 

$$W_p(\alpha, \beta) := \min_{\pi \in \mathcal{M}^1_{+}(\mathcal{X}^2)} \left\{ \int_{\mathcal{X}^2} d(x,y)^p d\pi(x,y): \pi_1 = \alpha, \pi_2 = \beta \right\}$$

Then, we have: 
$$ \alpha_n \rightharpoonup \beta \iff W_p(\alpha_n, \beta) \rightarrow 0 $$ where $\alpha_n \rightharpoonup \beta$ denotes weak convergence, i.e., $\forall f \in \mathcal{C}(\mathcal{X}), \int_{\mathcal{X}} f d \alpha_n \rightarrow \int_{\mathcal{X}} f d \beta$. -->

# 4. Maximum Entropy Wasserstein distance 
One major drawback of Wasserstein distance is its expensive computation, which is $O(n^3 \log(n))$ in general. In some special cases, the computation can be less, e.g., $O(n^3)$ if $\alpha$ and $\beta$ have the same weights, $O(n \log(n))$ for 1-D case, and $O(n^2 \log(n))$ for $p=1$. 

One of the major research direction for Wasserstein distance that I am aware is to reduce its computation. A common approach is to introduce regularization into the OT objective so that we relax some constraints and solve for an approximation optimization. Interestingly, regularization in some particular way does not give the optimazation more difficult, it makes it more tracable. For example, in Sinkhorn divergence, an entropy regularization is added, resulting a tractable optimization:

$$
W_{\gamma}(p,q) := \min_{\pi \in \Pi(p,q)} \mathbb{E}_{\pi} d(x,x') - \gamma H(\pi)
$$

where 

* $\pi(x,x') = u(x) v(x') K(x, x')$ 
* $Kv = p./ u$ and $K^Tu = q./ v$ if $x$ and $x'$ take finite values. 

<!-- . Specifically, the regularized OT objective in Sinkorn is: 

$$\min_{P \in U(a,b)} \sum_{i,j} d(x_i, y_j)^p P_{i,j} + \epsilon P_{i,j} \log \frac{P_{i,j}}{a_i b_j}$$

The solution to the regularized OT optimization above is tractable, as given by: $P_{i,j} = u_i K_{i,j} v_j, K_{i,j} = e^{-\frac{d(x_i, y_j)^p}{\epsilon}}$. -->

Which rationale that allows an entropic regularization to make a more tractable optimization even if adding an entropic regularization seems to make the objective more complex? Introducing an entropy constraint into the OT objective can be thought of as relaxing the optimization, thus the resulting optimization can be easier, not nessasarily harder because of having more terms. Indeed, the regularized optimization is easier in this case. In particular, the regularized OT is a constraint optimization that can be solved with Lagrangian method because the strong duality holds for this optimization (the entropy term is convex). Thus, we instead solve the KKT conditions for the Lagrangian, resulting in a closed-form solution which can be efficiently iteratively updated using Sinkorn's update [1]. 

# 5. Discussion  
It strikes me that Lagrangian method is really helpful to regularize hard optimization and by softening it, the resulting optimization becomes more tractable. I have encountered its usage at least in Information Botllteneck [3], Distributional Robustness [4] and now regularized OT. It would be interesting to be able to apply OT into sequential decision making problems, especially Bayesian optimization and Reinforcement Learning. How likely and fruitly it would be? 


# References: 
[1] Grabriel Peyre, [Optimal Transport for Machine Learning](https://portal.klewel.com/watch/webcast/optimal-transport-for-machine-learning/talk/1/).   
[2] Marco Cuturi, [Sinkhorn Distances: Lightspeed Computation of Optimal Transport](https://arxiv.org/abs/1306.0895).  
[3] Naftali Tishby, Fernando C. Pereira, and William Bialek, [Information Bottleneck Method](https://arxiv.org/abs/physics/0004057).   
[4] John Duchi, and Hongseok Namkoong, [Variance-based regularization with convex objectives](https://arxiv.org/abs/1610.02581).   
[5] [Wasserstein Training of Deep Boltzmann Machines](https://www.cs.cmu.edu/~epxing/Class/10708-17/project-reports/project11.pdf)   


*Last update: 16/08/19*