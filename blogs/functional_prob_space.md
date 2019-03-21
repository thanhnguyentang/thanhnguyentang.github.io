---
layout: post
title: A property of functionals on probability space
---   
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 

In this post, we will make use of the basic properties of RKHS presented in [1] to prove a nice property stated in [2]. 

Let $\mathcal{H}_k \in \mathbb{R}^{\mathcal{X}}$ be some RKHS with reproducing kernel $k$ over $\mathcal{X} \in \mathbb{R}^d$, $\mathcal{P}$ be the space of all probability measures on $\mathcal{X}$. Define the following mapping from the space of all probability measures the RKHS: 

$$\psi: \mathcal{P} \rightarrow \mathcal{H}_k, P \mapsto \psi_P(\cdot) = \int_{\mathcal{X}} k(\cdot, x) dP(x).$$


Let $\mathcal{H}_{\hat{k}}$ be a RKHS over $\mathcal{P}$ with the reproducing kernel $\hat{k}$ defined as: 

$$\hat{k}(P, P') := \int_{\mathcal{X}} \int_{\mathcal{X}} k(x, x') dP(x) dP'(x').$$

Then, the mapping  


$$M: \mathcal{H}_k \rightarrow \mathcal{H}_{\hat{k}}, f \mapsto Mf \text{ where } Mf(P) := \mathbb{E}_P[f] := \langle \psi_P, f \rangle_k, \forall P \in \mathcal{P}$$

is isometrically isomorphic. 

**Proof**:   
It is easy to see that $M$ is a bounded linear operator (thus, it is continuous). 

**Part 1**: **Isometry** (i.e., norm perserving property)   

Let $\mathcal{H}_k^0$ be the pre-RKHS of $\mathcal{H}_k$, i.e., 

$$\mathcal{H}_k^0 = span\{ k(\cdot, x): x \in \mathcal{X}\}.$$

Consider any $f = \sum_{i=1}^n \alpha_i k(\cdot, x_i)$ in the pre-RKHS, we will prove that $\| Mf \|_{\hat{k}} = \| f \|_k.$

Indeed, we have 

$$ \| f\|_k^2 = \langle f ,f \rangle = \sum_{i,j} \alpha_i \alpha_j k(x_i, x_j).$$

$$  Mf(P) = \langle \psi_P, f \rangle_k = \sum_i \alpha_i \psi_P(x_i) = \sum_i \alpha_i \hat{k}(P, \delta_{x_i}).$$

Thus, 

$$ \| Mf \|_{\hat{k}}^2 = \langle Mf, Mf \rangle_{\hat{k}} = \sum_{i,j} \alpha_i \alpha_j \hat{k}(\delta_{x_i}, \delta_{x_j}) = \sum_{i,j} \alpha_i \alpha_j k(x_i, x_j) = \| f\|_k^2. $$

Since $\mathcal{H}_k^0$ is dense in $\mathcal{H}_k$ (Moore-Aronszajn theorem, see e.g., [1]), the isometry in the former generalizes to that in the latter. 

**Part 2**: **Isomorphy** (bijective mapping between two spaces)  
Since $M$ is a bounded linear operator, it is injective. What's left is to prove that $M$ is surjective.  Indeed, we can prove that using a similar technique as in part 1. Again, we work on the pre-RKHS of the RKHS before generalizing the result to the entire RKHS using Moore-Aronszajn theorem. Consider any $\hat{f} = \sum_{i=1}^n \alpha_i \hat{k}(\cdot, P_i)$ from the pre-RKHS with reproducing kernel $\hat{k}$, we will first prove that $\exists f \in \mathcal{H}_k, Mf = \hat{f}$. Indeed, simply choose: 

$$f := \sum_i \alpha_i \int_{\mathcal{X}} k(\cdot, x) dP_i(x).$$

Then, we have: 

$$Mf = \sum_{i,j} \alpha_i \alpha_j \int_{\mathcal{X}}\int_{\mathcal{X}} k(x_i, x_j) dP_i(x_i) dP_j(x_j) = \sum_{i,j} \alpha_i \alpha_j \hat{k}(P_i, P_j) = \hat{f}.$$


Since $\mathcal{H}_k^0$ is dense in $\mathcal{H}_k$, the isomormy in the former generalizes to that in the latter.
## References  
[1]  Thanh's blog. [*Basics of LKHS and beyond*](http://thanhnguyentang.github.io/blogs/rkhs.pdf).  
[2]  Oliveira, R., Ott, L., and Ramos, F. (2019). *Bayesian optimisation under uncertain inputs*.