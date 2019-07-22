---
layout: post 
title: Lagrangian  
date: 2019-07-22
---  
[[Back Home]](/)  [[Back to Blog]](/blogs/post) 

# Constrained minimization  
Consider the constrained minimization: 

$$
\label{eq:primal}
\min_{x \in \mathcal{X} \subset \mathbb{R}^n } f(x) \\
\text{s.t. } h_i(x) \leq 0, \forall 1 \leq i \leq m, \\
l_j(x) = 0, \forall 1 \leq j \leq k. 
$$

* (Definition) The **Lagrangian**: 

$$
L(x,u,v) = f(x) + \sum_{i=1}^m u_i h_i(x) + \sum_{j=1}^k v_j l_j(x).
$$

* (Definition) The **Lagrangian dual function**: 

$$
g(u,v) := \min_{x \in \mathcal{X}} L(x,u,v).
$$

* (Definition) The **dual problem**: 

$$
\label{eq:dual}
\max_{u \in \mathbb{R}_{+}^n, v \in \mathbb{R}^m} g(u,v).
$$

* (Definition & Theorem) The **duality gap**:  Given primal feasible $x$ and dual feasible $u,v$, the duality gap $\Delta(x,u,v)$ (defined below) between $x$ and $u,v$ satisfies: 

$$
\Delta(x,u,v) := f(x) - g(u,v) \geq f(x) - f^* \geq 0 
$$

where $f^*$ is the primal optimal value of problem $(\ref{eq:primal})$. 

*Proof*: 

$$
g(u, v) = \min_{x' \in \mathcal{X}} L(x', u, v) \leq L(x^*, u, v) \leq f(x^*) \leq f(x). 
$$

where $x^*$ is a primal optimal solution of $(\ref{eq:primal})$.

* (Theorem) Slater's condition (for **strong duality** ):  

$$ \label{eq:Slater}
(f \text{ is convex}) \land (\exists x : h(x) < 0, l(x) = 0) \implies f^* = g^*
$$

where $g^*$ is the dual optimal value of problem $(\ref{eq:dual})$. 

* (Theorem) Zero duality gap implies strong duality: 

$$
\Delta(x,u,v) = 0 \implies f^* = g^*. 
$$

* (Definition) The Karush-Kuhn-Tucker conditions or **KKT conditions** for the primal problem $(\ref{eq:primal})$: 
    * *Stationality*: 

        $$ 0 \in \partial_{x} L(x,u,v) $$

    * *Complementary slackness*: 
    
        $$ u_i h_i(x) = 0, \forall i$$

    * *Primial feasibility*: 
    
        $$ h_i(x) \leq 0, l_j(x) = 0, \forall i,j$$

    * *Dual feasibility*: 
    
        $$u_i(x) \geq 0, \forall i$$  

* (Theorem) Nessesary conditions: if $x^{* }$ and $u^{* }, v^{* }$ are primal and dual solutions, and if the strong duality holds, then $x^{* }, u^{* }, v^{*}$ satisfies the KKT conditions.  

* (Theorem) Sufficient conditions: if $x^{* }, u^{* }, v^{* }$ satisfies the KKT conditions, then $ x^{* }$ and $u^{* }, v^{* }$ are primal and dual solutions. 

* Important implication: If the Slater's condition in $(\ref{eq:Slater})$ is met, instead of directly solving the primal problem in $(\ref{eq:primal})$, we only need to solve the KKT conditions. For example, Tishby uses this technique to derive for a solution of the [Information Bottleneck method](https://arxiv.org/abs/physics/0004057). 

# Reference  
[1] https://www.cs.cmu.edu/~ggordon/10725-F12/slides/16-kkt.pdf


