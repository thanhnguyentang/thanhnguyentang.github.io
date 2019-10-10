---
layout: post
title: A cheatsheet of distribution divergence and measure theory
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 



* Wasserstein distance 
* MMD 
* $\chi^2$-distance 
* $\phi$-divergence 
* Jensen-Shannon distance 
* Jensen-Tsallis distance
* Cramer distance 
* Stein discrepancy: 

$$
D(P,Q) = \max_{\phi \in \mathcal{F}} \mathbb{E}_{Q(x)}[trace(\mathcal{A}_P \phi(x))]
$$  

where $\mathcal{A}_{P}(x) = \nabla_x \log P(x) \phi(x)^T + \nabla_x \phi(x)$



**Pushforward measure** is a measure obtained by transforming a measure from one measurable space to another through a measurable mapping. Formally, consider two measurable spaces $(\Omega_1, \mathcal{F}_1)$ and $(\Omega_2, \mathcal{F}_2)$, $\mu: \mathcal{F}_1 \rightarrow \mathbb{R}^{+}$ is a measure on $\mathcal{F}_1$, $f: \Omega_1 \rightarrow \Omega_2$ is a measurable function. Then, the pushforward measure by $f$ on $\mu$ is given by

$$
(f_{\#} \mu)(A) := \mu(f^{-1}(A)), \forall A \in \mathcal{F}_2.
$$

**Change-of-variables formula**: 

$$
\int_{\Omega_2} g d(f_{\#} \mu) = \int_{\Omega_1} g \circ f d \mu.
$$

(Work in progress)