# Neural ODE

## 1. Introduction

<img src="/Users/jung0202/Library/Application Support/typora-user-images/image-20250330201551076.png" alt="image-20250330201551076" style="zoom:50%;" />

- Models such as residual networks, recurrent neural network decoders, and normalizing flows  can be seen as an Euler discretization of a continuous transformation :
  $$
  \mathbf{h}_{t+1}=\mathbf{h}_t+f\left(\mathbf{h}_t, \theta_t\right)
  $$
  where $t \in\{0 \ldots T\}$ and $\mathbf{h}_t \in \mathbb{R}^D$.

- we parameterize the continuous dynamics of hidden units using an ordinary differential equation (ODE) specified by a neural network:
  $$
  \frac{d \mathbf{h}(t)}{d t}=f(\mathbf{h}(t), t, \theta)
  $$

- ODE solvers has several benefits:

  - **Memory efficiency** : compute gradients of a scalar-valued loss with respect to all inputs of any ODE solver, without backpropagating through the operations of the solver.
  - **Adaptive computation** : allows the cost of evaluating a model to scale with problem complexity.
  - **Scalable and invertible normalizing flows** : the change of variables formula becomes easier to compute
  - **Continuous time-series models** : continuously-defined dynamics can naturally incorporate data which arrives at arbitrary times.

------

## 2. Reverse-mode automatic differentiation of ODE solutions

- The main technical difficulty in training continuous-depth networks is performing reverse-mode differentiation (also known as backpropagation) through the ODE solver
  - **Compute gradients using the adjoint sensitivity**
    - This approach computes gradients by solving a second, augmented ODE backwards in time, and is applicable to all ODE solvers.
    - This approach scales linearly with problem size, has low memory cost, and explicitly controls numerical error.