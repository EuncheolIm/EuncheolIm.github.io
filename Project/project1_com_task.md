---
layout: base
title: Project
permalink: /project/project1/com-task
---

# Implement add COM TASK

### How can Base COM TASK add ?
For adding COM TASK in Whole Body Control (WBC), we have to add $$\ddot{q}$$ in decision variables.
So, QP form of GRF problem can be changed as follows:

<p>
$$
\begin{align}
\mathbf{f}^d = \arg \underset{\mathbf{\ddot{q}}, \mathbf{f}}{\min}
 \underbrace{(\mathbf{A}_c\mathbf{X} - \mathbf{b})^\top \mathbf{S}_1 (\mathbf{A}_c\mathbf{X} - \mathbf{b})}_{C_1} + \underbrace{\mathbf{X}^T \mathbf{S}_2 \mathbf{X}}_{C_2} \\
\end{align}
$$
$$
\text{s.t.} \quad 
\mathbf{M}\mathbf{\ddot{q}} - \mathbf{J}^T_c\mathbf{f} = \mathbf{Sh}, \\
\mathbf{\underline{d}} \leq \mathbf{C}\mathbf{f} \leq \mathbf{\bar{d}},\\
\mathbf{\underline{c}} \leq \mathbf{f_z} \leq \mathbf{\bar{c}}
$$
where,
$$
\begin{align*}
\underbrace{\begin{bmatrix}
  0_{n+6} && I_3 && 0_3 && I_3 && 0_3 \\  
  0_{n+6} && S(r_{1,{com}})_3 && I_3 && S(r_{2,{com}})_3 && I_3
\end{bmatrix}}_{A_c}
\underbrace{\begin{bmatrix}
  \ddot{q}_{n+6} \\
  f_1 \\  
  f_2
\end{bmatrix}}_X & =
\underbrace{\begin{bmatrix}
  m(\ddot{x}_{com} + g) \\  
  I_G \dot{\omega}_b
\end{bmatrix}}_b
\end{align*} \\
\begin{align*}
\qquad \qquad \mathbf{M}\mathbf{\ddot{q}} - \mathbf{J}^T_c\mathbf{f} = \mathbf{Sh} \qquad \leftarrow \qquad   \text{floating base dynamics constraints}\\
\begin{cases}
\mathbf{\underline{d}} \leq \mathbf{C}\mathbf{f} \leq \mathbf{\bar{d}}, \qquad \leftarrow \qquad \text{Friction cone constraints}\\
\mathbf{\underline{c}} \leq \mathbf{f_z} \leq \mathbf{\bar{c}}
\end{cases}
\end{align*}
$$
</p>
$$n$$: Total joint degree of freedoms <br>

### Cost function  <br>
- C_1 : Centroidal dynamics  <br>
- C_2 : GRF, $$\ddot{q}$$ regularization

### Details
- Status: Finished
