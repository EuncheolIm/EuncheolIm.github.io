---
layout: base
title: Project
permalink: /project/project1/grf
---

# Implement GRF using qpOASEE

### Ground Reaction Force (GRF)
Ground reaction force (GRF) is the force exerted by the ground on a body in contact with it. The component of the GRF parallel to the surface is the frictional force. When slippage occurs the ratio of the magnitude of the frictional force to the normal force yields the coefficient of static friction.(Wikipedia)

###  Centroidal dynamics-based foot force
<div class="publication-list" style="display: flex; align-items: center;">
  <div class="image-container">
    <img src="{{ '/assets/img/Project1/SingleRigidBody.png' | relative_url }}" alt="SingleRigidBody" style="width: 200px; height: 200px; border: 1px solid #ccc; border-radius: 8px;">
  </div>
  <div class="text-container" style="margin-left: 30px;">
    <p>
      The simpler linear dynamic equations are expressed in the following form:
      $$ m(\ddot{x}_{com} + g) = \sum^c_{i=1} f_i $$ 
      $$ I_G \dot{\omega}_b \simeq \sum^c_{i=1}(p_i \times f_i)$$
    </p>
  </div>
</div>
<p>
where:
- \( f_i \): ground reaction force at the \( i \)-th contact point, <br>
- \( r_i \): position of the \( i \)-th contact point relative to the center of mass,  
- \( m \): mass of the robot, <br>  
- \( g \): gravitational force,
- \( I_G \): centroidal rotational inertia,
- \( p \): i-th contact point, <br>
- \( w_b \): body's angular velocity;
</p>

<p>
The desired acceleration of the \( \ddot{x}^d_{com}\) is computed by PD control law:
$$
\begin{align} \ddot{x}^d_{com} = K_{p,{com}}(x^d_{com} - x_{com}) + K_{d,{com}}(\dot{x}^d_{com} - \dot{x}_{com})\end{align}$$

The desired angular acceleration of the \( \dot{\omega}^d_b\) is computed by PD control law:
$$\begin{align} \dot{\omega}_d = K_{p,{base}}e(R^d_bR^T_b) + K_{d,{base}}(\omega^d_b - \omega_b)\end{align}$$

The simpler linear dynamic equations can be rewritten in the following matrix form:
$$
\begin{align}
\begin{bmatrix}
  I && I \\  
  S(r_{1,{com}}) && S(r_{2,{com}})
\end{bmatrix}
\begin{bmatrix}
  f_1 \\  
  f_2
\end{bmatrix} & =
\begin{bmatrix}
  m(\ddot{x}_{com} + g) \\  
  I_G \dot{\omega}_b
\end{bmatrix}
\end{align}
$$
Given desired state of COM, GRFs can be computed. And solving the GRFs without non-solvable cased, friction cone can be used in inequality constraints.

$$
\begin{align}
\mathbf{f}^d = \arg\min_{\mathbf{f} \in \mathbb{R}^k} 
\left( (\mathbf{A}\mathbf{f} - \mathbf{b})^\top \mathbf{S} (\mathbf{A}\mathbf{f} - \mathbf{b}) + \alpha \mathbf{f}^\top \mathbf{W} \mathbf{f} \right)
\end{align}
$$
$$
\text{s.t.} \quad 
\mathbf{\underline{d}} \leq \mathbf{C}\mathbf{f} \leq \mathbf{\bar{d}}, \\
\mathbf{\underline{c}} \leq \mathbf{f_z} \leq \mathbf{\bar{c}}
$$
</p>

### Details
- Status: Finished