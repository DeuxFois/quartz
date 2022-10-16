---
tags:
  - supervised-learning
  - discriminative
  - discrete-value
  - classification
---
**⚠️ this article presume you know about the [linear-regression](data-science/machine-learning/supervised-learning/linear-regression.md)**

Logistic Regression uses a **sigmoid function**, also known as the 'logistic function'.  
This model makes the following assumption : $0 \leq h\_\theta(x) \leq 1$


## Parameter
**same as linear regression**
$$\vec{(\beta)}^T = (\beta\_0,\beta\_1,...,\beta\_n) \in \mathbb{R}^{d+1}$$
## Model
$$ f\_\beta(x) =  \frac{\mathrm{1} }{\mathrm{1} + e^{-x\beta^T} }  $$ 
 <details>
<summary> that's the sigmoid function </summary>

![](_resources/Screenshot%20from%202022-08-13%2010-34-25.png)

</details>

## Cost Function 
$$
\mathbf{J}(\boldsymbol{\beta})=-\frac{1}{m} \sum\_{i=1}^{m}\left[y\_{i} \log \left(f\_{\boldsymbol{\beta}}\left(\mathbf{x}\_{i}\right)\right)+\left(1-y\_{i}\right) \log \left(1-f\_{\boldsymbol{\beta}}\left(\mathbf{x}\_{i}\right)\right)\right]
$$
## Gradient descent 
**same as linear regression**
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

![|650](_resources/1_PQ8tdohapfm-YHlrRIRuOA.gif)