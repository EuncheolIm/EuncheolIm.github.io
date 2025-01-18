---
layout: base
title: Project
permalink: /project/onelegsquat/project1/onelegsquat_task
---

# Implement add COM TASK

### How can Base COM TASK add ?
We have to add swing leg task in WBC to move legs.
So, QP form of GRF problem can be changed as follows:

<p>
$$
\begin{align}
\mathbf{f}^d = \arg \underset{\mathbf{\ddot{q}}, \mathbf{f}}{\min}
 \underbrace{(\mathbf{A}_c\mathbf{X} - \mathbf{b})^\top \mathbf{S}_1 (\mathbf{A}_c\mathbf{X} - \mathbf{b})}_{C_1} + \underbrace{\mathbf{X}^T \mathbf{S}_2 \mathbf{X}}_{C_2} 
 + \underbrace{\mathbf{\ddot{x}_{sw}^T} S_3 \mathbf{\ddot{x}_{sw}^T}}_{C_3}\\
\end{align}
$$
$$
\text{s.t.} \quad 
\mathbf{M}\mathbf{\ddot{q}} - \mathbf{J}^T_c\mathbf{f} = \mathbf{Sh}, \\
\mathbf{\underline{d}} \leq \mathbf{C}\mathbf{f} \leq \mathbf{\bar{d}},\\
\mathbf{\underline{c}} \leq \mathbf{f_z} \leq \mathbf{\bar{c}}, \\
\mathbf{J}\mathbf{\ddot{q}} = -\mathbf{\dot{J}}\mathbf{\dot{q}}
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
\qquad \qquad \mathbf{M}\mathbf{\ddot{q}} - \mathbf{J}^T_c\mathbf{f} = \mathbf{Sh} \qquad \leftarrow \qquad   &\text{floating base dynamics}\\  &\text{constraints} \\
% \begin{cases}
\mathbf{\underline{d}} \leq \mathbf{C}\mathbf{f} \leq \mathbf{\bar{d}}, \qquad \leftarrow \qquad &\text{Friction cone constraints1}\\
 \mathbf{\underline{c}} \leq \mathbf{f_z} \leq \mathbf{\bar{c}}, \qquad \leftarrow \qquad &\text{Friction cone constraints2}\\
% \end{cases} \\
\mathbf{J}\mathbf{\ddot{q}} = -\mathbf{\dot{J}}\mathbf{\dot{q}}  \qquad \leftarrow \qquad   &\text{contact equality constraints}
\end{align*}
$$
</p>
$$n$$: Total joint degree of freedoms <br>

### Cost function  <br>
- C_1 : Centroidal dynamics  <br>
- C_2 : GRF, $$\ddot{q}$$ regularization <br>
- C_3 : Swing leg task

### Details
- Status: Finished
