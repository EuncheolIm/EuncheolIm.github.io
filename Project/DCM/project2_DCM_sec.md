---
layout: base
title: Project
permalink: /project/DCM/project2/dcm_basic
---

# Implement DCM

### Divergent Component of Motion (DCM)
Center of mass (COM) dynamics can split into stable and unstable part.
The state variable related to the unstable part of the dynamics has been referred to as as <span class="bold-text">"(instantaneous) Capture Point"(CP)</span> by Pratt and Koolen et al., <span class="bold-text">"Extrapolated Center of Mass"</span> by Hof et al., and <span class="bold-text">"Divergent Component of Motion"(DCM)</span> by Takenaka et al. <br>
<span class="bold-text">CP</span> point on the ground where the robot has to step to come to a stop asymptotically in 2D plane <br>
However, <span class="bold-text">DCM</span> is a point in 3D. <span class="bold-text">CP</span> and <span class="bold-text">DCM</span>(projected to floor) are equivalent, but not for 3D.

###  DCM definition 
<div class="publication-list" style="display: flex; align-items: center;">
  <div class="image-container">
    <img src="{{ '/assets/img/Project2/DCM_in_3Dspace.png' | relative_url }}" alt="DCM_in_3Dspace" style="width: 200px; height: 200px; border: 1px solid #ccc; border-radius: 8px;">
  </div>
  <div class="text-container" style="margin-left: 30px;">
    <p>
      The DCM is defined as a point at a certain distance in front of the COM
      $$ \mathbf{\xi} = \mathbf{x} + b\mathbf{\dot{x}}$$
      $$ \mathbf{\dot{x}} = -\frac{1}{b}(\mathbf{x}-\mathbf{\xi})$$
      $$ \dot{\xi} = -\frac{1}{b}\mathbf{x} + \frac{1}{b}\mathbf{\xi} \frac{b}{m}\mathbf{F}$$
    </p>
  </div>
</div>


###  Enhanced Centroidal Moment Pivot Point (eCMP)
Genarally, a robot is subject to gravity and external forces. We design external forces being appropriate for the locomotion task while fullfilling the feasibility constraint (CoP in base of support).
To simplify this design, make use of a force-to-point transformation similar as <span class="bold-text">Linear Inverted Pendulum (LIP)</span> model. 
External forces in a simple repelling force law encode based on the difference of the COM and the so-called eCMP <br> : $$\mathbf{F}_{ext}=s(x-r_{ecmp})$$.
$$ \rightarrow $$ Total force $$\mathbf{F}$$ = $$\mathbf{F}_{ext}+\mathbf{F}_{g} = s(x-r_{ecmp}+mg)$$. <br>

Using Total Force,  DCM dynamics can be changed as follows:<br>
$$\mathbf{\dot{\xi}} = (\frac{bs}{m}-\frac{1}{b})\mathbf{x} + \frac{1}{b}\mathbf{\xi} - \frac{bs}{m}\mathbf{r}_{ecmp}+b\mathbf{g}$$ <br>
$$\mathbf{\dot{\xi}} = \frac{1}{b}\mathbf{\xi} - \frac{1}{b}\mathbf{r}_{ecmp}+b\mathbf{g}$$ <br>
This equation means DCM dynamics decoupled from COM dynamics. It is crucial only stabilizes the unstable DCM dynamics while leaving the natually stable COM dynamics unaffected.

###  Virtual Repellent Point (VRP)
Virtual Repellent Point is added gravity affect in $$\mathbf{r}_{ecmp}$$. $$\mathbf{r}_{vrp} = \mathbf{r}_{ecmp} + [0,  0, b^2g]^T = \mathbf{r}_{ecmp} + [0, 0, \Delta z_{vrp}]^T$$.
So, DCM dynamics can represent $$\mathbf{\dot{\xi}} = \frac{1}{b}(\mathbf{\xi} - \mathbf{r}_{vrp})$$.
And, in the whole paper, $$b = \sqrt{\Delta z_{vrp}/g}$$. <br>

<div class="publication-list" style="display: flex; align-items: center;">
  <div class="image-container" style="display: flex; gap: 0px;">
    <img src="{{ '/assets/img/Project2/Fig_2.png' | relative_url }}" alt="Fig2" style="width: 220px; height: 220px; border: 1px solid #ccc; border-radius: 8px;">
    <img src="{{ '/assets/img/Project2/Fig_3.png' | relative_url }}" alt="Fig3" style="width: 220px; height: 220px; border: 1px solid #ccc; border-radius: 8px;">
  </div>
    <p>
      $$ \qquad \text{DCM definition : } \mathbf{\xi} = \mathbf{x} +\sqrt{\Delta z_{vrp}/g} $$ 
      $$ \qquad \text{DCM dynamics : } \mathbf{\dot{\xi}} = \sqrt{\Delta g/z_{vrp}} (\mathbf{\xi} - \mathbf{r}_{vrp}) $$ 
      $$ \qquad \text{COM dynamics : } \mathbf{\dot{x}} = -\sqrt{\Delta g/z_{vrp}} (\mathbf{x} - \mathbf{\xi}) $$ 
      $$ \qquad \text{External forces : } \mathbf{F}_{ext} = \frac{mg}{\Delta z_{vrp}}(\mathbf{x}-\mathbf{r}_{ecmp})$$
    </p>
  <div class="text-container" style="margin-left: 30px;">
  </div>
</div>

### Gerneration of Nominal Divergent Component of Motion Reference
4 assumptions
1) A1: robot’s feet are point feet (corresponding to foot centers if robot has finite-sized feet).
2) A2: changes in angular momentum $$L$$ are zero ($$\dot{L} = 0$$).
3) A3: instantaneous transitions between left and right SS phases [no double support (DS)].
4) A4: no impacts during support transitions.
To comply with the assumptions A1 and A2, we choose $$r_{f,i}$$, eCMP and CoP (see Fig. 4) to be coincident for planning. <br>
<br>

the desired DCM locations at the end of each step (see Fig. 5) via recursion: <br>
$$ \xi_{d, \text{eos}, i-1} = \xi_{d, \text{ini}, i} = \mathbf{r}_{\text{vrp}, d, i} + e^{-\sqrt{\frac{g}{\Delta z_{\text{vrp}}}} t_{\text{step}, i}} \cdot  \left( \xi_{d, \text{eos}, i} - \mathbf{r}_{\text{vrp}, d, i} \right)$$, <br>
where, $$\xi_{d, \text{ini}, i}$$ is $$i$$ th initial desired DCM.<br>
 For planning, we assume that the DCM will come to a stop over the final previewed foot position, i.e.,$$\xi_{d, \text{ini}, N-1} = r_{vrp,d,N}$$. Now, based this equation the reference trajectories for the DCM position (bold blue lines in Fig. 5) can be computed as <br>
$$\xi_{d, i}(t) = \mathbf{r}_{\text{vrp}, d, i}  + e^{\sqrt{\frac{g}{\Delta z_{\text{vrp}}}} (t - t_{\text{step}, i})} \cdot  \left( \xi_{d, \text{eos}, i} - \mathbf{r}_{\text{vrp}, d, i} \right)$$. <br>
The corresponding DCM velocity trajectories are found via derivation of above equation.
$$\dot{\xi}_{d, i}(t) = \sqrt{\frac{g}{\Delta z_{\text{vrp}}}} \cdot e^{\sqrt{\frac{g}{\Delta z_{\text{vrp}}}} (t - t_{\text{step}, i})} \cdot  \left( \xi_{d, \text{eos}, i} - \mathbf{r}_{\text{vrp}, d, i} \right)$$

<div class="publication-list" style="display: flex; align-items: center;  justify-content: center;">
  <div class="image-container" style="display: flex; gap: 5px;  justify-content: center;">
    <img src="{{ '/assets/img/Project2/Fig_4.png' | relative_url }}" alt="Fig4" style="width: 310px; height: 310px; border: 1px solid #ccc; border-radius: 8px;">
    <img src="{{ '/assets/img/Project2/Fig_5.png' | relative_url }}" alt="Fig5" style="width: 310px; height: 310px; border: 1px solid #ccc; border-radius: 8px;">
  </div>
  <div class="text-container" style="margin-left: 30px;">
  </div>
</div>

### Implement Result
When 6 steps are given,
$$\Delta Z_{vrp} = 0.6 (m)$$ and step length $$= 0.2$$, dt $$=0.001$$
<div class="publication-list" style="display: flex; align-items: center;  justify-content: center;">
  <div class="image-container" style="display: flex; gap: 10px;  justify-content: center;">
    <img src="{{ '/assets/img/Project2/DCM_result.png' | relative_url }}" alt="DCM_result" style="width: 700px; height: 300px; border: 1px solid #ccc; border-radius: 8px;">
  </div>
  <div class="text-container" style="margin-left: 30px;">
  </div>
</div>

### Details
- Status: Finished.

### Reference
- Englsberger, Johannes, Christian Ott, and Alin Albu-Schäffer. "Three-dimensional bipedal walking control based on divergent component of motion." Ieee transactions on robotics 31.2 (2015): 355-368.
