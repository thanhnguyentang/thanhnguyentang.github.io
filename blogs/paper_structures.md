---
layout: post 
title:  Some observations on the structure of idea flows in ML research 
date: 2019-05-13
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 


Here I discuss some personal observations on how an idea in ML research is evolved with the goal in mind that I would eventually capture some of the salient structures of how a research idea is evolved through different stages from a naive, seemly trivial intuition to a rigorous piece to a published work. This would have many interesting applications namely to apply and develop my own idea evolution pipeline, and to understand how novelty and knowledge is born. 

Some cognitive tasks for generating ML novelty that will be discussed shortly: 
* Creative application of an idea from one domain to another  
* Generalization of several relevent methods
* Reformulation of several problems 
* Proposing new problems 
<!-- * Proposing new testbeds  -->

# 1. Applying a technique/idea from one problem domain to another 
This is a basic form of doing research and perhaps the most general approach because the other approaches (will be presented next) can be more or less thought of as some application of an idea or technique to an problem of interest; it is hardly that an idea or technique come from nowhere. The idea itself is not original and the problem itself might not be original, so where does the novelty come from? The originality might lie at the meta idea of applying the idea to the problem domain and at the way you develop the idea in the problem domain to its fullest. Such an application many times requires non-trivial design choices and novel treatments. Remember that having such an idea is nice but it is for from enough for a publishable work if one stops there; it is a very small step among many other steps required to generate novelty. Here I discuss a proposed pipeline of how to develop an idea from one domain to another. Of course, this should not be a unique pipeline but at least some thing that can be doable. 

* A good *rationale* (e.g., motivation) for why this leveraging would make sense and have potential advantage. If possible, provide some *simple analytical example* where the existing methods fail but leveraging the idea would overcome it is very convincing and powerful. 
* It is a good sign if when one applies an idea from one domain to another nontrivial design choices and novel treatments are required. At this point, some theorem or preposition would be very rigorous and convincing.
* Design a good algorithimc solution and experimental prototype to demonstrate the soundness of the proposed idea.  

<!-- * Identify any **unique non-trival problem** (even if it is not too hard) in leveraging the techniques to the existing algorithms. At this stage, seeing a unique problem arise along the way of the idea (i.e., of leveraging the techniques to the existing algorithms) should be a good news. Why? Because this means that the idea is not trivial; the unique and non-trivial problem might weight the contribution of the idea more and helps in convincing reviewers. Then try to solve that unique problem. At this point, some **theorem** or **preposition** is very interesting and convinving. -->

<!-- * Design a good **algorithmic solution** and **experimental prototype** to demonstrate that the idea actually works and meaningful. I would like to express more at this point that while proposing an idea and solving its unique arising problem are important steps in this direction, **this does not make the step of desigining good experimental prototype any less important**. Because after all, what matters is something that actually works and works better. If there is no good experimental result to support the idea (except theoretical papers that do not really need experiment, but I am focusing on empirical AIML here), the ideas proposed seem like a **wheelchair science**. Plus, this is also a good motivation on the idea because it is always fun to see the idea actually works in practice. Thus, never underestimate this step.   -->

Some examples: 
* [1] applies Stein's operator as control variates to reduce variance in policy gradient class 
* [2] applies Stein's variational inference to the maximum entropy policy optimization

<!-- * Leverage some techniques into an existing class of algorithms (e.g., , [2] applies Stein's variational inference to the maximum entropy policy optimization. Also take a look at [Qiang Liu's publication page](https://www.cs.utexas.edu/~lqiang/publication.html) for many interesting applications of Stein's operator in variational inference and RL which are by the way the topics of my most interest.)  
* In leveraging some techniques to another, usually need to demonstrate the following things: -->
    


# 2. Reformulation of the existing problems 

Reformulating the existing problems usually gives novel perspectives. By reframing a problem into another class of problem, one can leverage the techniques from the new problem class to solve the existing problem. For example, [3] reframes actor-critic into a minimax framework and solve the resuling problem by leveraging techniques from the resulting problem. [4] reframes RL as variational inferences and gives some interesting insights resulted from the reformulation.  

<!-- * Like the "modification" approach, "reformulation" approach also generally requires clarifying **rationale**, identifying and solving the **unique non-trivial problem**, and desinging **algorithmic solution** and **experimental protype**.   -->

# 3. Generalizing several methods   
Connecting different domains through a unifying framewokr has a great benefit of novelty as it brings what's understood in one domain into solving problems in the other ones. Some examples for this generalization approach:

* Connecting smoothed game optimization with GAN and RL [6]. 
* Lift Stein Variational Gradient Descent from Euclidean space to Riemannian manifold [8] 
* Lift Stein variational inference and black-box variational inference into a unifying framework via gradient flow [9], quite similar in the spirit to the way [10] generalizes GAN, variational inference and policy gradient via a unifying framework called probability functional descent. 

<!-- * This "connection" approach can be generalization of "reformulation" and "modification" approach. The "connection" approach is more fundamental while the "reformulation" and "modification" are two specific ways for realizing the "connection" approach.  -->

# 4. Proposing new problems   

* Proposing new important problems and solving it by probably leveraging existing techniques.   
* Provide a rationale for the new problems, e.g., why are they important and worth solving? 
* Sometimes, the solution to the new problem might be not difficult but as long as the problem itself is novel, this idea is also important. 
* Sometimes, there are also the unique non-trival problems emerged along the way of solving the original problem. This would be interesting. 
* And again, algorithmic and experimental design for the proposed solutions for the problem are always important for empirical research. 

*(not completed yet)*

# 5. Some other practical observations  
* Intuition (or *thought experiment*) $\rightarrow$ simple analytical example $\rightarrow$ Scale the idea in practice 
* Figure 1 is the one that tells everything about the main intuition (aka the mental experiment) of a paper. Quick check: A good figure 1 is the one that if you read only the Figure 1 and its caption, you still have an idea of the main thing in this paper.  A really nice exemplar for this is [5]. While the main contribution of [5] is a technical method for preventing catastrophic forgetting, Figure 1 of this paper presents only the mental experiment that requires not much expertise to understand.  
* Some papers have very simple final derivation which is very intuitive (and looks as if a heuristic), e.g. [5,7]. Despite this, there should be good evidence for this final derivation: (1) *Pre-evidence*: The mental experiment motivation, simple analytical example and mathematical backup that lead to the final derivation; (2) *Post-evidence*: Some good experimental prototype (synthetic or scaled one) to prove why the final derivation works. Without the pre-evidence, the idea would look like very much arbitrary and heuristic. Without the post-evidence, the idea becomes useless.  

* Three pillars for a good contribution: *theoretical, algorithmic, and empirical*. Yes, I feel RL gives a good playground for all these pillars; it also connects classic problems (e.g., bandit) to modern ones (e.g., deep reinforcement learning).  A good contribution needs not be significant equally in all these aspects. Some looks like a natural, incremental modification (simple in algorithmic approach) but can demonstrate it theoretical and/or empirical significance (e.g., [this one](https://arxiv.org/abs/1710.10044)). Some start with empirical comparitive study to figure out some emprical questions (e.g., [this one](https://arxiv.org/abs/1901.11084)). RL is also a good representive problem for general AI in which we aim at learning to solve many general intelligence tasks using little domain knowledge. That said, if there is AGI (Artificial General Intelligence), or at least if we want to make existing learning algorithms sufficiently more general that they are today, RL is a good testbed for such gold. Just a little remark on it: causality and reasoning are two of the important ingredients (or subgoals) in this path. 


# References 

[1] [Action-dependent control variates for policy gradient via Stein's Identity](https://arxiv.org/abs/1710.11198)  
[2] [Stein Variational Policy Gradient](https://arxiv.org/abs/1704.02399)  
[3] [Boosting the actor with dual critic](https://arxiv.org/abs/1712.10282)  
[4] [Reinforcement Learning and Control as Probabilistic Inference: Tutorial and Review](https://arxiv.org/abs/1805.00909)  
[5] [Continual Learning Through Synaptic Intelligence](https://arxiv.org/abs/1703.04200)  
[6] [Smooth games optimization and machine learning workshop](https://sgo-workshop.github.io/index_2018.html)  
[7] [Proximal Policy Optimization Algorithms](https://arxiv.org/pdf/1707.06347.pdf) 
[8] [Riemann Stein Variational Gradient Descent](https://arxiv.org/abs/1711.11216)  
[9] [The equivalence between Stein variational gradient descent and black-box variational inference](https://arxiv.org/abs/2004.01822)  
[10] [Probability Functional Descent: A Unifying Perspective on GANs, Variational Inference, and Reinforcement Learning](https://arxiv.org/abs/1901.10691)

-------------------------
    Update: 
    * 13/04/20: revise the post by making it clearer and more coherent and chaning the title
    * 29/08/19: original post