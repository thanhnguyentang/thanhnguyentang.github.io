---
layout: post
title: Research Proposals 
---  
[[Back Home]](/) 

> # Project 1: Offline Reinforcement Learning with Provable Statistical Efficiency


Reinforcement Learning (RL) is one of the most ubiquitous learning paradigms and of the most active research area in machine learning. An RL agent interactively and simultaneously learns the underlying dynamics and produces a policy to make decisions in this environment.  However, in practical settings, an interaction with such environment is often expensive or even prohibited, making the RL agent difficult to obtain any online data. Instead, an offline dataset from historical interactions (e.g., demonstration data from human experts or data from any previous policies) is redundant. This research proposal aims at developing new algorithms to leverage offline data with provable statistical efficiency. The significance of this project is that it will constitute fundamental understanding (of the statistical limits and benefits) of offline RL [1] and provide practical algorithms to efficiently and reliably accelerate real-life decision-making problems such as audit, marketing, dynamic pricing, personalized medicines, recommender systems, matching markets, and new material discoveries. 

The fundamental challenges of offline RL are embodied into two following questions: (i) What is a minimal, realistic condition that guarantees sample efficiency in offline RL? and (ii) How can we design a practical algorithm that can efficiently leverage various offline dataset scenarios with provable guarantee? This project proposal will significantly advance the current literature of offline RL by investigating into these fundamental questions. Our initial effort toward answering these questions include our recent works [2,3] where in [2], we have established a sample complexity of offline policy evaluation - an instance of offline RL, under deep ReLU network function approximation – a commonly encountered scenarios in real applications, while in [3], we have proposed a novel way to leverage offline data and combine it with online learning to overcome the problem of support deficiency in offline learning. 


### References
[1] Sergey Levine, Aviral Kumar, George Tucker, Justin Fu. *“Offline Reinforcement Learning: Tutorial, Review, and Perspectives on Open Problems”*, 2020.   
[2] Thanh Nguyen-Tang, Sunil Gupta, Hung Tran-The, Svetha Venkatesh. *“On Finite-Sample Analysis of Offline Reinforcement Learning with Deep ReLU Networks”*, 2021.    
[3] Hung Tran-The, Sunil Gupta, Thanh Nguyen-Tang, Santu Rana, Svetha Venkatesh. *“Combing Offline Learning and Online Learning for Contextual Bandits with Deficient Support”*, 2021. 

--------------

> # Project 2: Provable Generalization in Deep Learning with Structural Inductive Biases    


Generalization – the ability to generalize from training datasets or scenarios to unseen ones, is one of the ultimate goals for AI systems. Deep learning (DL) – a revolutionized machine learning method that adapts deep neural networks with stacked computation modules to datasets, presents a stronger generalization ability than classical methods in many strongly supervised learning tasks or in environments with strong and densely rewards. However, in real-life scenarios, the new data might come from different (but relevant) distributions or new tasks, and the current DL systems generally are not robust in such changes in distribution. How can we provably improve the generalization ability of current DL systems? The no-free lunch theorem for machine learning suggests that any learning algorithm that generalizes well on several distributions or tasks can fail arbitrarily in some other distributions or tasks. Thus, for generalization, it is necessary to introduce structural assumptions about the solutions or the problems at hand. Inductive biases (e.g., compossibility in convolutional neural networks) – a set of cognitive structures that prioritizes the learning toward several desirable properties, are a promising direction to improve DL generalization. This research proposals aim at systematically developing new structural inductive biases that provably guarantee robust generalization in DL. The significance of this project is that it will fundamentally and algorithmically advance the generalization ability of DL systems which in turns benefit vast downstream DL application tasks that require stronger forms of cognitive abilities (e.g., sequential decision making and reasoning) rather than large-scale pattern recognition as in many current DL systems. 

The fundemental challenges in DL generalization are embodied into two following questions: (i) What inductive biases can improve generalization in deep neural networks?, and (ii) In particular, how much do the structures induced by inductive biases improve the generalization bound? For (i), we will draw inspirations from cognitive science and information theory to design new scalable deep neural networks with inductive biases. Currently, we focus on exploiting the causality structure for deep representation learning to make DL systems generalize beyound the i.i.d. settings. For (ii), a new generalization theory is required as the capacity-based generalization bounds obtained by classical statistical learning (e.g., via uniform convergence with VC dimension and Rademacher complexity) are vacuous for overparametrized models such as deep neural networks. This research proposal will investigate into understanding and improving DL generalization guided by these two questions. As our initial effort toward answering these questions, we have introduced an inductive bias inspired by information theory into the learning process of deep neural networks to optimally reduce its redundant information within each network layers and showed a link to the generalization ability [3]. 


### References 
[1] Anirudth Goyal, Yoshua Bengio. *“Inductive Biases for Deep Learning of Higher-Level Cognition”*, 2021.    
[2] Chiyuan Zhang, Samy Bengio, Moritz Hardt, Benjamin Recht, Oriol Vinyals. *“Understanding deep learning requires rethinking generalization”*, 2017.    
[3] Thanh Nguyen-Tang, Jaesik Choi. *“Markov Information Bottleneck to Improve Information Flow in Stochastic Neural Networks”*, 2019.    
[4] Hoang Thanh-Tung, Truyen Tran, Svetha Venkatesh. *"Improving Generalization and Stability of Generative Adversarial Networks"*, 2019.  
[5] Bernhard Schölkopf, Francesco Locatello, Stefan Bauer, Nan Rosemary Ke, Nal Kalchbrenner, Anirudh Goyal, Yoshua Bengio. *"Towards Causal Representation Learning"*, 2021. 
