---
layout: post
title: Concentration Inequalities 
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post)    

An unofficial collection of concentration inequalities that I find very helpful (and probably very common) despite of being simple. A motivation for this collection is to provide a cheatsheet for analyzing an algorithm's convergence rate and bounding some quantity of interest (specially in the context of sequential decision making under uncertainty): 
### Notation  
$$Pr(A) := Pr\{X \in A \} = P(\{ \omega \in \Omega: X(\omega) \in A \in \mathcal{B} \})$$ where $X$ is a random variable that maps the probability space $(\Omega, \mathcal{F}, P)$ to the probability space $(\mathcal{X}, \mathcal{B}, Pr).$
# Concetration on possitive values  

$$
\mathbb{E}[X] \leq C Pr\{ X \geq 0\}
$$

where $C = \max_{X\in \mathcal{X}} X.$

# Union bound 

$$
Pr( \cup_{i=1}^N A_i ) \leq \sum_{i=1}^{N} Pr( A_i)
$$

where $1 \leq N \leq \infty$. The equality occurs when $A_i$ are mutually non-overlapping. 

### Auxilary:  

$$
A \subset B \implies Pr(A) \leq Pr(B)
$$


# References 
[1] Russo and Van Roy. *Learning to Optimize via Posterior Sampling*.  

(work in progress)
