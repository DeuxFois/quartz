---
tags:
  - supervised-learning
  - discriminative
  - continuous value
---
## Linear Regression
### Pros

-   Quick to compute and can be updated easily with new data
-   Relatively easy to understand and explain

Regularization techniques can be used to prevent **_overfitting_**

### Cons

-   Unable to learn complex relationships
-   Difficult to capture **_non-linear relationships_** (without first transforming data which can be complicated)
---


dataset : $\lbrace\mathbf{x}\_{i}, y\_{i} \rbrace\_{i=1}^{m}$  with $\mathbf{x}_{i} \in \mathbb{R}^{d}$  

output : $y_{i} \in \mathcal{Y}$

$f: \mathbb{R}^{d} \mapsto \mathcal{Y}$  
## Parameter
$$\vec{(\beta)}^T = (\beta\_0,\beta\_1,...,\beta\_n) \in \mathbb{R}^{d+1}$$

## Model
$$ f\_\beta(x) = \beta\_0 + \Sigma_{i=1}^m(\beta\_ix\_i)$$

- Matrix notation :
$$
f_{\boldsymbol{\beta}}(\mathbf{X}) \approx \tilde{\mathbf{X}} \boldsymbol{\beta}
$$
$$
\left(\begin{array}{c}
y_{1} \\
y_{2} \\
\vdots \\
y_{m}
\end{array}\right) \approx\left(\begin{array}{ccccc}
1 & x_{11} & x_{12} & \ldots & x_{1 d} \\
1 & x_{21} & x_{22} & \ldots & x_{2 d} \\
\vdots & \vdots & \vdots & \vdots & \\
1 & x_{m 1} & x_{m 2} & \ldots & x_{m d}
\end{array}\right) \quad\left(\begin{array}{c}
\beta_{0} \\
\beta_{1} \\
\vdots \\
\beta_{d}
\end{array}\right)
$$
$$\mathbf{X} \in \mathbb{R}^{m \times d}, \tilde{\mathbf{X}} \in \mathbb{R}^{m \times(d+1)} \space \space \space \space  \space \boldsymbol{\beta} \in \mathbb{R}^{d+1}$$

## Cost Function
- the function that we will try to minimize
- Since we want $\boldsymbol{\beta}$ such that $f_{\boldsymbol{\beta}}(\mathbf{x})$ is close to $y$ for all training data $\lbrace{x }\_{i}, {y}\_{i}\rbrace\_{i=1}^{m}$:
$$
\hat{y}\_{i}=f\_{\boldsymbol{\beta}}\left(\mathbf{x}\_{i}\right) \approx y\_{i} \quad \forall i \in\{1, \cdots, m\}
$$
- Finding the best vector $\vec{\beta}$ is equivalent to minimizing the global (quadratic) error cost:
$$
\mathbf{J}(\boldsymbol{\beta})=\frac{1}{2 m} \sum\_{i=1}^{m}\left(f\_{\boldsymbol{\beta}}\left(\mathbf{x}\_{i}\right)-y\_{i}\right)^{2}
$$

$$
➡ \space \underset{\boldsymbol{\beta}}{\operatorname{argmin}}(\mathbf{J}(\boldsymbol{\beta}))
$$
![|650](_resources/Pasted%20image%2020220630221441.png)


## Gradient descent
- Objective: find the minimum of the cost function
<br/>

1. initialisation : $\boldsymbol{\beta}^{(0)}$
2. at each step $k$, modify $\boldsymbol{\beta}^{(k-1)}$ to make $\operatorname{decrease} \mathbf{J}\left(\boldsymbol{\beta}^{(k )}\right)$
3. stop when the minimum is reached

For each $\beta_{j}$ :
$$
\beta_{j}^{(k)}:=\beta\_{j}^{(k-1)}-\alpha \frac{1}{m} \sum\_{i=1}^{m}\left(f\_{\boldsymbol{\beta}(k-1)}\left(\mathbf{x}\_{i}\right)-y\_{i}\right) x\_{i j}
$$
⚠ $\forall i, x_{i 0}=1$
- $\alpha$ = learning rate (step size)  
![|650](Screenshot%20from%202022-08-13%2010-25-35.png)
![|650](1_eeIvlwkMNG1wSmj3FR6M2g.gif)

## Locally weighted linear regression
As evident from the image below, this algorithm cannot be used for making predictions when there exists a non-linear relationship between X and Y. In such cases, locally weighted linear regression is used.

![](https://media.geeksforgeeks.org/wp-content/uploads/Linear-Regression-on-non-linear-data.png)

The modified cost function is:
$$
\mathbf{J}(\boldsymbol{\beta})= \sum\_{i=1}^{m}w^{(i)}\left(f\_{\boldsymbol{\beta}}\left(\mathbf{x}\_{i}\right)-y\_{i}\right)^{2}
$$
A fairly standard choice for the weights is
$$
w^{(i)}=\exp \left(-\frac{\left(x^{(i)}-x\right)^{2}}{2 \tau^{2}}\right)
$$
By changing the value of $\tau$ we can choose a fatter or a thinner width for circled, in mathematical reprensentation it is the bandwidth of the Gaussian bell-shaped curve of the weighing function.
![|650](_resources/Pasted%20image%2020220813075641.png)