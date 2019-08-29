---
layout: post 
title: On some practical structures of papers in empirical AIML 
date: 2019-05-13
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 



# 1. Modification to an existing class of algorithms    

* Leverage some techniques into an existing class of algorithms (e.g., [1] applies Stein's operator as control variates to reduce variance in policy gradient class, [2] applies Stein's variational inference to the maximum entropy policy optimization. Also take a look at [Qiang Liu's publication page](https://www.cs.utexas.edu/~lqiang/publication.html) for many interesting applications of Stein's operator in variational inference and RL which are by the way the topics of my most interest.)  

* In leveraging some techniques to another, usually need to demonstrate the following things:
    * A good **rationale** (e.g., motivation) for why this leveraging would make sense and have potential advantage. If possible, provide some **simple analytical example** where the existing methods fail but leveraging the techniques would overcome it is very convincing and powerful. 

    * Identify any **unique non-trival problem** (even if it is not too hard) in leveraging the techniques to the existing algorithms. At this stage, seeing a unique problem arise along the way of the idea (i.e., of leveraging the techniques to the existing algorithms) should be a good news. Why? Because this means that the idea is not trivial; the unique and non-trivial problem might weight the contribution of the idea more and helps in convincing reviewers. Then try to solve that unique problem. At this point, some **theorem** or **preposition** is very interesting and convinving.

    * Design a good **algorithmic solution** and **experimental prototype** to demonstrate that the idea actually works and meaningful. I would like to express more at this point that while proposing an idea and solving its unique arising problem are important steps in this direction, **this does not make the step of desigining good experimental prototype any less important**. Because after all, what matters is something that actually works and works better. If there is no good experimental result to support the idea (except theoretical papers that do not really need experiment, but I am focusing on empirical AIML here), the ideas proposed seem like a **wheelchair science**. Plus, this is also a good motivation on the idea because it is always fun to see the idea actually works in practice. Thus, never underestimate this step.  


# 2. Reformulation of the existing problems 

* Reformulating the existing problems usually gives novel perspectives. By reframing a problem into another class of problem, one can leverage the techniques from the new problem class to solve the existing problem. For example, [3] reframes actor-critic into a minimax framework and solve the resuling problem by leveraging techniques from the resulting problem. [4] reframes RL as variational inferences and gives some interesting insights resulted from the reformulation.  

* Like the "modification" approach, "reformulation" approach also generally requires clarifying **rationale**, identifying and solving the **unique non-trivial problem**, and desinging **algorithmic solution** and **experimental protype**.  

# 3. Connecting different seemingly unrelated problem domains  
* Connecting different domains has a great benefit of novelty as it brings what's understood in one domain into solving problems in the other ones. A good exemplar for this "connection" approach is connecting smoothed game optimization with GAN and RL [6]. 

* This "connection" approach can be generalization of "reformulation" and "modification" approach. The "connection" approach is more fundamental while the "reformulation" and "modification" are two specific ways for realizing the "connection" approach. 

# 4. Proposing new problems   

* Proposing new important problems and solving it by probably leveraging existing techniques.   
* Provide a rationale for the new problems, e.g., why are they important and worth solving? 
* Sometimes, the solution to the new problem might be not difficult but as long as the problem itself is novel, this idea is also important. 
* Sometimes, there are also the unique non-trival problems emerged along the way of solving the original problem. This would be interesting. 
* And again, algorithmic and experimental design for the proposed solutions for the problem are always important for empirical research. 

*(not completed yet)*

# 5. Some other practical observations  
* Intuition (or **mental experiment** <sup>1</sup>) $\rightarrow$ simple analytical example $\rightarrow$ Scale the idea in practice 
* Figure 1 is the one that tells everything about the main intuition (aka the mental experiment) of a paper. Quick check: A good figure 1 is the one that if you read only the Figure 1 and its caption, you still have an idea of the main thing in this paper.  A really nice exemplar for this is [5]. While the main contribution of [5] is a technical method for preventing catastrophic forgetting, Figure 1 of this paper presents only the mental experiment that requires not much expertise to understand.  
* Some papers have very simple final derivation which is very intuitive (and looks as if a heuristic), e.g. [5,7]. Despite this, there should be good backup for this final derivation: 
    * (1) **Pre-backup**: The mental experiment motivation, simple analytical example and mathematical backup that lead to the final derivation,   
    * (2) **Post-backup**: Some good experimental prototype (synthetic or scaled one) to prove why the final derivation works.   

    Without the pre-backup, the idea would look like very much arbitrary and heuristic. Without the post-backup, the idea becomes useless.  

* Don't be fool by your small progress results. Chances are they are not siginificant and reportable; thus keep improving on it and making it more significant. 

<sup>1</sup> *I first hear the term **mental experiment** from [a friend of mine]((https://arxiv.org/abs/1807.04015)). This is a nice term that I wish to adopt it though my intended use might not exactly have the same meaning as its originality.* 

# References 

[1] [Action-dependent control variates for policy gradient via Stein's Identity](https://arxiv.org/abs/1710.11198)  
[2] [Stein Variational Policy Gradient](https://arxiv.org/abs/1704.02399)  
[3] [Boosting the actor with dual critic](https://arxiv.org/abs/1712.10282)  
[4] [Reinforcement Learning and Control as Probabilistic Inference: Tutorial and Review](https://arxiv.org/abs/1805.00909)  
[5] [Continual Learning Through Synaptic Intelligence](https://arxiv.org/abs/1703.04200)  
[6] [Smooth games optimization and machine learning workshop](https://sgo-workshop.github.io/index_2018.html)  
[7] [Proximal Policy Optimization Algorithms](https://arxiv.org/pdf/1707.06347.pdf)


*Last update: 29/08/19*