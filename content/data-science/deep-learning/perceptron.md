# Introduction
the perceptron is a mathematical model. Originally, he was inspired by the human brain and was constitued by, what we might call, an artificial neuron. This one is composed with **inputs with different weights**, who represent the synapse. Each one are summed and this sum is multiply by an activation function who will take a value between 0 and 1.
![](_resources/Pasted%20image%2020221012153349.png)
# Mathematical model
## Neuron
Consider a neuron with 2 input, we have the following
![](_resources/Pasted%20image%2020221015201527.png)
## Activation function
So, we have the function ${Z} = \mathbf{w}^{T}\mathbf{x}+b$, and we want to have the probability of the output to be 0 or 1.
That's the purpose of the **activation function**, which is the **sigmoid function**. It's a function that takes any real number and maps it to a value between 0 and 1. So, we have the sigmoid function, which is $${\sigma}({Z}) = \frac{1}{1 + e^{-Z}}$$
![](_resources/Pasted%20image%2020221015202650.png)
That's logistic regression who have the  [same model as linear discriminant analysis](data-science/deep-learning/LDA%20and%20Sigmoid.md) 
$$P(Y=1|{\bf x})=\sigma\left({\bf w}^{T}{\bf x}+b\right)$$
But 
- **ignore model assumption** (Gaussian class populations, homoscedasticity)
- instead find **w**, b that maximizes the likelihood of the data.

We have,
$$\begin{aligned}
&\arg \max \_{\mathbf{w}, b} P(\mathbf{d} \mid \mathbf{w}, b)\\
&=\arg \max \_{\mathbf{w}, b} \prod\_{\mathbf{x}\_i, y\_i \in \mathbf{d}} P\left(Y=y\_i \mid \mathbf{x}\_i, \mathbf{w}, b\right) \\
&=\arg \max \_{\mathbf{w}, b} \prod\_{\mathbf{x}\_i, y\_i \in \mathbf{d}} \sigma\left(\mathbf{w}^T \mathbf{x}\_i+b\right)^{y\_i}\left(1-\sigma\left(\mathbf{w}^T \mathbf{x}\_i+b\right)\right)^{1-y\_i} \\
&=\arg \min \_{\mathbf{w}, b} \underbrace{\sum\_{\mathbf{x}\_i, y\_i \in \mathbf{d}}-y\_i \log \sigma\left(\mathbf{w}^T \mathbf{x}\_i+b\right)-\left(1-y\_i\right) \log \left(1-\sigma\left(\mathbf{w}^T \mathbf{x}\_i+b\right)\right)}\_{\mathcal{L}(\mathbf{w}, b)=\sum\_i \ell\left(y\_i, \hat{y}\left(\mathbf{x}\_i ; \mathbf{w}, b\right)\right)}
\end{aligned}$$

This loss is an instance of the cross-entropy
$$
H(p, q)=\mathbb{E}_p[-\log q]
$$
for $p=Y \mid \mathbf{x}_i$ and $q=\hat{Y} \mid \mathbf{x}_i$.

When Y takes values in {−1, 1}, a similar derivation yields the **logistic loss**
$$\mathcal{L}(\mathbf{w},b)=-\sum\_{{\bf x}\_{i},y\_{i}\in{\bf d}}\log\sigma\left(y\_{i}(\mathbf{w}^{T}\mathbf{x}\_{i}+b)\right)$$

- In general, the cross-entropy and the logistic losses do not admit a minimizer that can be expressed analytically in closed form. 
- However, a minimizer can be found numerically, using a general minimization technique such as gradient descent.
## Gradient descent
Let ${\mathcal{L}}(\theta)$ denote a loss function dened over model parameters (e.g., $\mathbf{w}$ and b ).  

To minimize ${\mathcal{L}}(\theta)$, gradient descent uses local linear information to iteratively move towards a (local) minimum.

For $\theta_{0}\in\mathbb{R}^{d}$, a first-order approximation around $\theta_{0}$ can be dened as $$\hat{\mathcal{L}}(\theta_{0}+\epsilon)=\mathcal{L}(\theta_{0})+\epsilon^{T}\nabla_{\theta}\mathcal{L}(\theta_{0})+\frac{1}{2\gamma}\vert\vert\epsilon\vert^{2}.$$A minimizer of the approximation ${\hat{\mathcal{L}}}(\theta_{0}+\epsilon)$

$$\begin{aligned}
{r l}{\nabla_{\epsilon}{\hat{\cal Z}}(\theta_{0}+\epsilon)}&=0 \\
&={\nabla_{\theta}{\mathcal Z}(\theta_{0})+{\frac{1}{\gamma}}\epsilon,}\end{aligned}$$
Therefore, model parameters can be updated iteratively using the update rule
$$\theta_{t+1}=\theta_{t}-\gamma\nabla_{\theta}\mathcal{Z}(\theta_{t}),$$
with : 
- $\theta_0$, the initial parameters of the model
- $\boldsymbol{\gamma}$ is the **learning rate**
- both are criticalfor the convergence of the update rule