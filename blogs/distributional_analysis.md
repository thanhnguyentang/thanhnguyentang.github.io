---
layout: post
title: A Note on  Amortila et al. "A Distributional Analysis of Sampling-based RL Algorithms" 
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post)   

# 1. Intro 
I have come across this interesting AISTATS2020 paper [1] on Twitter. Since this is possibly relevant to my research (using variational inference rooted in optimal transport to study generalization and robusteness in RL), I think it would be a good idea to dive into this paper to see what's new and what can be applicable to my own current research problem. As a bit of context, Bellmare and his collaborators  have started quite a successful (IMO) RL research direction namely "Distributional RL" [2] back in 2017 where they study the dynamic programming and RL methods on the space of probability measures. This idea opens a venue to apply optimal transport (a mathematically rich framework) to RL. While distributional RL focus more on *algorithmic* design for how to learn as implicit auxiliary tasks the instrinstic randomness from the MDP kernel and reward function, the current paper "A Distributional Analysis of Sampling-based RL Algorithms" brings the distributional idea into designing new *analysis* tool. In this post, I will try to summarize the meat of this interesting paper with a biased goal in mind that to understand the paper better for my own research problem and to hopefully give an intuitive explanation of the paper to make it more accessible to broader readers. 

## 2. Distributional sample-based TD learning  
The paper deals with several sample-based RL updates such as TD learning, Q-learning, SARSA, Expected SARSA and Double Q-learning. Once the distributional idea works out in one update algorithm, its extensions to the other are quite straightforward. Thus, for our educational purpose, it is safe to focus on just the case of TD(0). 


## 2.1 Policy evaluation setting 
As always, the distributional idea starts with this generic approach: we lift the object of interest to its distributions and work on the space of these distributions later on. By doing this, we enrich information of the object of interest into our algorithm and analysis, thus it is likely helpful. In addition, an enriched space would call for more mathematical tools to study it, thus has a potential of bringning new insight that is otherwise overlooked by the unlifted approach. For example in  Distributional RL [2], the object of interest is the sum of discounted rewards; while the traditional RL learn about the expected value of the sum of discounted rewards, distributional RL takes into account their distribution. Similarly for TD(0) learning, instead of taking an update on the value function estimate, we take an update on the *distribution* of the value function estimate (think of making more involved updates for the entire distribution rather than just update its sample). So here comes a natural application of the distributional idea to TD(0) learning: 

$$
V_{k+1}(s) \overset{D}{=} (1- \alpha) V_k(s) + \alpha (R(s,A) + \gamma V_k(S'))
\label{distributional_TD}
$$

where $(A, R, S')$ is the random tuple of action, reward and next state conditioned on the curent state $s$, and $\overset{D}{=}$ denotes equality in distribution. Intuitively, you start with an arbitrary value distribution estimate $V_0$ and evole this initial distribution according to the update equation (\ref{distributional_TD}). Unlike the standard TD(0) learning where you get a value function estimate $v_k \in \mathbb{R}^{\mathcal{X}}$ at every step, you get a value function estimate distribution  $V_k \in \mathcal{P}(\mathbb{R})^{\mathcal{X}}$. This is pretty much all the necessary work to lift the standard TD(0) to distributional TD(0). Now, the real questions are: 
* (i) Does the evolution process in Equation (\ref{distributional_TD}) converge? 
* (ii) And if it does converge to a stationary distribution, what is interesting about this stationary distribution? 

Before discussing these two questions, I discuss one thing regarding Equation (\ref{distributional_TD}) that possibly looks not very natural to the sampling-based sense. To see this, let us write the standard TD(0) update: 

$$
v_{k+1}(s) = (1- \alpha) v_k(s) + \alpha (r(s,a) + \gamma v_k(s'))
\label{standard_TD}
$$

where $r(s,a) \sim R(s,a), a \sim A, s' \sim S'$, and we denote little $v$ for value function estimates (and big $V$ for value funtion estimate distribution). Here, the standard TD(0) update in Equation (\ref{standard_TD}) only requires a sample $(r,a,s')$ to construct its evolution process while the distributional TD(0) update in Equation (\ref{distributional_TD}) requires the complete description of $R,A,S'$ to construct its evolution. That's why  Equation (\ref{distributional_TD}) is in fact not very much like a sampling-based update. However, Equation (\ref{distributional_TD}) is used to characterize (e.g. the weak convergence of) Equation (\ref{standard_TD}) because Equation (\ref{standard_TD}) is an empirical realization drawn from Equation (\ref{distributional_TD}). 

Now if being familiar with the arguments in the distributional RL [2], one can easily show that the evolution (\ref{distributional_TD}) converges to a fixed distribution $\psi_{\alpha}$ in Wasserstein metric by showing that the operator governing the evolution is a contraction. This is Proposition 4.1 in [1]. Especially and obviously if $\alpha=1$, the fixed distribution $\psi_{\alpha}$ is the *return distribution* in the distributional RL [2]. 

There are several interesting characteristics of the fixed distribution $\psi_{\alpha}$ presented in Theorem 5.1 and 5.2 of [1]. First, the mode of  $\psi_{\alpha}$ is the true value function $f^{\pi}$ under the sample policy $\pi$ in policy evaluation setting (i.e., the fixed point of the Bellman operator $T^{\pi}$). Second, we can relate the covariance (across states) of the zero-mean noise $\hat{T}^{\pi}f - T^{\pi}f$ (arising due to sample-based update) to the covariance (accross states) of the fixed distribution $\psi_{\alpha}$. Especially the operator norm of the covariance (accross states) of the fixed distribution $\psi_{\alpha}$ is monotonically decreasing (to $0$) with $\alpha$. Recall that the operator norm of a matrix $A$, denoted by $$\|A\|_{op}$$, is the maximum norm of all the vectors linearly transformed by $A$ from the unit ball, i.e., $$\sup\{ \| Ax \| : \|x\| \leq 1, x \in \mathbb{R}^d \}$$. Intuitively, as $\alpha \rightarrow 1$, $\psi_{\alpha}$ is "maximally stochastic" that resembles the return distribution in [2]; as $\alpha \rightarrow 0$, $\psi_{\alpha}$ becomes less stochastic and more centered around its mode $f^{\pi}$. So while it is not yet rigorous, it is quite reasonable to pose a question that: 

> As $\alpha$ smoothly changes from $1$ to $0$ with $k \rightarrow \infty$, does the intermedia value distribution $V_k$ in Equation (\ref{distributional_TD}) interpolate from the return distribution to a posterior distribution over the value function estimate? In other words, since $V_k$ contains both intrinsic and parametric uncertainty, does the intrinsic uncertainty in $V_k$ is gradually swearing off as $\alpha$ smoothly changes from $1$ to $0$ with $k \rightarrow \infty$? Another question is, as the value distribution $V_k$ in Equation (\ref{distributional_TD}) can be reframed as a Wasserstein gradient flow, does this Wasserstein gradient flow reveal new characteristics of the behaviour of $V_k$?  

The caveat is however that if $\alpha = 0$, the contraction factor of the distributional TD(0) which is $1 - \alpha + \alpha \gamma$ becomes $1$, thus the distributional TD(0) is not a contraction anymore.  

## 2.1 Control setting 

In the control setting (Q-learning update, i.e., replacing the Bellman operator in TD(0) by the optimality Bellman operator), the expectation of the fixed distribution $\psi_{\alpha}$ is greater than the optimal value function. This is understandable because $\mathbb{E} \circ \max \geq \max \circ \mathbb{E}$ in $\mathcal{P}(\mathbb{R}^d)^{\mathcal{X} \times \mathcal{A}}$. 

## 3. Conclusion 
So basically, the goal is still doing the analysis of TD(0) update in Equation (\ref{standard_TD}) but in an indirect way: via a lifted distributional version in Equation (\ref{distributional_TD}). Intuitively, one can think of the Equation (\ref{standard_TD}) is simply a sample drawn from the distributional Equation (\ref{distributional_TD}). So, by changing (lifting) the working space from $\mathbb{R}^{\mathcal{X} \times \mathcal{A}}$ to $\mathcal{P}(\mathbb{R})^{\mathcal{X} \times \mathcal{A}}$, we relax the constraints in the original space $\mathbb{R}^{\mathcal{X} \times \mathcal{A}}$ thus can possibly reveal more interesting properties from the perspective of the new space $\mathcal{P}(\mathbb{R})^{\mathcal{X} \times \mathcal{A}}$ by leveraging more rigorous mathametical tools (especically from Optimal Transport and Information Geometry). An intermediate implication of the analysis from this paper [1] is that constant stepsize RL can still converge (though in distribution). 


## References 
[1] Amortila et al. "[A Distributional Analysis of Sampling-based RL Algorithms](https://arxiv.org/abs/2003.12239)".   
[2] Bellmare et al. "[A distributional perspective in RL](https://arxiv.org/abs/1707.06887)".