---
layout: post
title: Convergences of random variables
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 


A proof to an elementary result in probability theory, just to hone the skills a bit. 

Here all random variables are defined in some probability space $(\Omega, \mathcal{F}, Pr)$. 

**Lemma 1**: If $X_n  \overset{a.s.}{\rightarrow} X$, then 
$\forall \epsilon >0, \lim_{m \rightarrow \infty} Pr( |X_n - X| \leq \epsilon, \forall n \geq m  ) = 1$. 

**Proof**. A wise strategy is to start with some very simple case to catch an idea of how a solution would look like. One such simple case is when $\Omega$ is finte. 
But that's behind the scence, here I directly present the general case of $\Omega$. 

First, let $A := \\{\omega \in \Omega: X_n(\omega) \rightarrow X(\omega) \\}$. Since $X_n  \overset{a.s.}{\rightarrow} X$, we have $Pr(A) = 1$. Fix $\epsilon > 0$ and define 

$$
B_m := \{ \omega \in \Omega: |X_n(\omega) - X(\omega)| \leq \epsilon, \forall n \geq m \}.
$$


We need to prove that $ Pr(B_m) \rightarrow 1 $. 
For any $\omega \in A$, there exists an integer $ N(\omega, \epsilon) $ such that $ |X_n(\omega) - X(\omega)| \leq \epsilon, \forall n > N(\omega, \epsilon)$. 
Define $C_m = \\{  \omega \in A: N(\omega, \epsilon) \leq m \\}$. Apparently, we have $C_m \subseteq B_m$, i.e., $ Pr(C_m) \leq Pr(B_m) \leq 1 $, and $C_m \rightarrow A$, i.e., $\lim_{m \rightarrow \infty} Pr(C_m) = Pr(A) = 1$. Thus, it follows from the squeeze theorem that $ Pr(B_m) \rightarrow 1 $.


**Theorem**. Almost sure convergence implies convergence in probability. 

*Proof*. Using the same notations as in Lemma 1. We also fix $\epsilon > 0$ and additionallydefine 

$$
D_n = \{ \omega \in \Omega: |X_n(\omega) - X(\omega)| \leq \epsilon \}. 
$$ 

We have $$ B_m = \displaystyle \cap_{n \geq m} D_n \subseteq D_m $$. We apply the squeeze theorem again, using $ Pr(B_m) \rightarrow 1 $, to obtain $ Pr(D_m) \rightarrow 1 $.

