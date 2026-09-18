---
title: "Control by Shaping Delay Distribution"
excerpt: "A general-purpose framework for spectral-abscissa optimization with discrete and distributed delays. The results improve the current state of analysis and control concerning time-delay systems. Beyond refining existing tools, this work introduces a novel distributed-delay output feedback control law, including regularization."
collection: portfolio
---

# Controller Optimization for Time-Delay Systems

Building upon the methodology of Appeltans, Silm, and Michiels [1], I developed a general-purpose framework for controller optimization based on the **spectral abscissa** of time-delay systems. The framework supports systems of retarded-type with both **discrete and distributed delays**, and allows several classes of output-feedback controllers to be optimized.

## Time-Delay Systems

The framework considers linear time-invariant (LTI) time-delay systems of the form

$$
P \leftrightarrow
\left\{
\begin{aligned}
\dot{x}(t)
&=
\sum_{k=1}^{m_A} A_k x(t-h_{A,k})
+
\sum_{k=1}^{m_B} B_k u(t-h_{B,k})
+
\int_{-h_I}^{0} G(\theta)x(t+\theta)\,d\theta,
\\
y(t)
&=
\sum_{k=1}^{m_C} C_k x(t-h_{C,k}),
\end{aligned}
\right.
$$

where $x\in\mathbb{R}^n$ is the state, $u\in\mathbb{R}^{n_u}$ is the control input, and $y\in\mathbb{R}^{n_y}$ is the measured output.

Inputs typically represent the actuators used to influence the system, while outputs represent measurements available to the controller.

The framework allows the construction and optimization of **static**, **dynamic**, and **distributed-delay output-feedback controllers**.

---

## Controller Classes

### Dynamic Output Feedback

A dynamic output-feedback controller is given by

$$
C_{\mathrm{dyn}} \leftrightarrow
\left\{
\begin{aligned}
\dot{x}_c(t) &= A_c x_c(t) + B_c y(t),\\
u(t) &= C_c x_c(t) + D_c y(t),
\end{aligned}
\right.
$$

where $x_c\in\mathbb{R}^{n_c}$ is the controller state and
$A_c$, $B_c$, $C_c$, and $D_c$ are real-valued matrices of appropriate dimensions.

When $n_c=0$, this reduces to static output feedback,

$$
C_{\mathrm{stat}} \leftrightarrow u(t)=D_c y(t).
$$

### Distributed-Delay Output Feedback

A more general feedback law combines instantaneous and distributed-delay feedback:

$$
C_{\mathrm{dist}} \leftrightarrow
u(t)
=
K_1 y(t)
+
K_2
\int_{-\tau}^{0}
g(\theta)y(t+\theta)\,d\theta,
$$

where $K_1,K_2\in\mathbb{R}^{n_u\times n_y}$.

The kernel $g(\theta)$ is represented using a truncated Chebyshev expansion,

$$
g(\theta)
=
\sum_{k=0}^{K}
c_k
T_k\left(2\frac{\theta}{\tau}+1\right),
$$

where $T_k$ denotes the Chebyshev polynomial of the first kind and
$c_k\in\mathbb{R}$ are the corresponding expansion coefficients.

The parameter $\tau$ is taken as the maximum delay appearing in the plant.

---

## Spectral-Abscissa Optimization

Given a plant $P$ and controller $C$, the resulting closed-loop system is denoted by $CL$.

The objective of the framework is to find controller parameters that minimize the **spectral abscissa**

$$
\sigma_{\max}(CL)
=
\max_{\lambda\in\Lambda(CL)}
\operatorname{Re}(\lambda),
$$

where $\Lambda(CL)$ denotes the spectrum of the closed-loop system.

The controller-design problem can therefore be written as

$$
\boxed{
\min_{\mathbf{p}}
\;
\sigma_{\max}(CL)
}
$$

where $\mathbf{p}$ contains the parameters defining the controller.

For the three controller classes considered above, the optimization variables are respectively

$$
\mathbf{p}_{\mathrm{stat}}
=
\operatorname{vec}(D_c),
$$

$$
\mathbf{p}_{\mathrm{dyn}}
=
\operatorname{vec}
\left(
\begin{bmatrix}
B_c & A_c\\
D_c & C_c
\end{bmatrix}
\right),
$$

and

$$
\mathbf{p}_{\mathrm{dist}}
=
\begin{bmatrix}
\operatorname{vec}(K_1)\\
\operatorname{vec}(K_2)\\
c
\end{bmatrix},
$$

where $\operatorname{vec}(\cdot)$ denotes column-wise vectorization and

$$
c =
\begin{bmatrix}
c_0 & \cdots & c_K
\end{bmatrix}^{T}.
$$

The optimization procedure tunes these parameters in order to obtain a closed-loop system with a reduced spectral abscissa.

---

# Regularization of the Distributed-Delay Kernel

A practical difficulty arises when optimizing the distributed-delay controller. When the Chebyshev expansion degree $K$ is large, direct optimization can produce kernels with strong, undesirable oscillations.

To address this, two regularization approaches were investigated:

1. **Penalty-based regularization**
2. **Chebyshev damping**

## Penalty-Based Regularization

The first approach explicitly penalizes variations in the kernel. The regularization functional is

$$
\operatorname{REG}(c)
=
R[g]
=
\int_{-\tau}^{0}
\left(g'(\theta)\right)^2\,d\theta.
$$

This penalizes rapidly varying kernels and introduces a direct trade-off between controller performance and kernel smoothness.

---

## Chebyshev Damping

The second approach is based on the decay properties of Chebyshev expansions.

For sufficiently regular functions, Chebyshev coefficients decay rapidly, with analytic functions exhibiting exponential coefficient decay under appropriate assumptions [2]. This suggests using the decay of the higher-order coefficients as a simple mechanism for controlling oscillations in the optimized kernel.

The optimized coefficients $\tilde{c}_k$ are therefore modified according to

$$
c_k =
\begin{cases}
\tilde{c}_k,
& 0\leq k\leq5,
\\[4pt]
\dfrac{\tilde{c}_k}{\rho^{k-5}},
& k\geq6,
\end{cases}
$$

where $\rho>1$ controls the amount of damping.

Unlike the penalty-based approach, this does not explicitly measure the global smoothness of $g$. Instead, it suppresses higher-order components of the Chebyshev expansion.

The resulting regularization is therefore simple, interpretable, and directly motivated by Chebyshev approximation theory.

---

# Multi-Objective Optimization

Regularization introduces a trade-off.

Increasing the amount of regularization generally constrains the controller and can therefore change the achievable spectral abscissa. In the penalty-based formulation, this can be viewed explicitly as a multi-objective optimization problem involving

$$
J_1(\mathbf{p}) = \sigma_{\max}(CL)
$$

and

$$
J_2(\mathbf{p}) = \operatorname{REG}(c).
$$

A weighted formulation is

$$
\boxed{
\min_{\mathbf{p}}
\;
\alpha J_1(\mathbf{p})
+
(1-\alpha)J_2(\mathbf{p}),
\qquad
\alpha\in[0,1].
}
$$

For convex multi-objective problems, varying $\alpha$ can be used to trace the Pareto front. Here, however, the spectral-abscissa objective is non-convex. Consequently, the procedure may instead trace a **local Pareto front** associated with a particular local minimum.

One way to investigate this is to solve the weighted optimization problem for a sequence of values

$$
\alpha_1,\alpha_2,\ldots,\alpha_N.
$$

The resulting local minima can then be used to visualize how the trade-off between spectral performance and regularization evolves.

Because of the non-convexity, the local minimum can change discontinuously as $\alpha$ varies [3]. Numerical experiments demonstrate that such transitions can indeed occur.

For the damping approach, there is no explicit second objective analogous to $\operatorname{REG}(c)$ because the regularization is imposed implicitly through the damping parameter $\rho$. Nevertheless, the same type of trade-off can be visualized by varying $\rho$ and observing the resulting changes in the spectral abscissa and kernel.

---

# Numerical Experiments

The following experiments illustrate the effect of regularization on the spectrum of time-delay systems and on the resulting optimized controllers.

## Experiment 1 — Scalar System

Consider the scalar time-delay system

$$
\begin{aligned}
\dot{x}(t)
={}&
(1-e^{-1})x(t)
+\frac{1}{2}x(t-1)
+\frac{e}{2}x(t-2)
+\frac{e^2}{2}x(t-3)
\\
&+
\int_{-1}^{0}
\theta x(t+\theta)\,d\theta
+
u\left(t-\frac{1}{32}\right).
\end{aligned}
$$

Both penalty-based and damping-based regularization are considered for this system.

### Penalty and Damping Regularization

<video width="100%" controls>
  <source src="images/first_experiment.mp4" type="video/mp4">
</video>

<video width="100%" controls>
  <source src="images/second_experiment.mp4" type="video/mp4">
</video>

### Evolution with the Regularization Parameter

<video width="100%" controls>
  <source src="images/system_1_alpha_animation.mp4" type="video/mp4">
</video>

---

## Experiment 2 — Two-Dimensional System

A second experiment considers the two-dimensional system

$$
\begin{aligned}
\dot{x}(t)
={}&
(1-e^{-1})
\begin{bmatrix}
1 & 2\\
2 & 1
\end{bmatrix}
x(t)
+
\frac{1}{2}
\begin{bmatrix}
1 & 0\\
0 & 1
\end{bmatrix}
x(t-1)
\\
&+
\frac{e}{2}
\begin{bmatrix}
-2 & 1\\
1 & -2
\end{bmatrix}
x(t-2)
\\
&+
\int_{-1}^{0}
\begin{bmatrix}
2e^\theta & 0\\
0 & e^\theta
\end{bmatrix}
x(t+\theta)\,d\theta
+
\begin{bmatrix}
1\\
0
\end{bmatrix}
u\left(t-\frac{1}{2}\right).
\end{aligned}
$$

The evolution of the spectrum during controller optimization is shown below.

<video width="100%" controls>
  <source src="images/system_2_experiment.mp4" type="video/mp4">
</video>

---

# References

1. P. Appeltans, H. Silm, and W. Michiels,  
   *TDS-CONTROL: A MATLAB package for the analysis and controller-design of time-delay systems*,  
   IFAC-PapersOnLine, vol. 55, pp. 272–277, 2022.

2. L. N. Trefethen,  
   *Approximation Theory and Approximation Practice*, Extended Edition, 2019.

3. K. Miettinen,  
   *Nonlinear Multiobjective Optimization*, 1998.