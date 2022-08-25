---
tags:
  - supervised-learning
  - discriminative
  - discrete-value
  - generative
  - classification
---
**⚠️ this article presume you know about the [normal law and multivariate normal law](data-science/statistic/gaussian.md)**

## Bayes Law

$$
\underbrace{\mathbb{P}(Y \mid X)}_{\text {Posterior probability }}=\frac{\overbrace{\mathbb{P}(Y)}^{\text {Prior probability}} \cdot \overbrace{\mathbb{P}(X \mid Y)}^{\text {Likelihood}}}{\mathbb{P}(X)}
$$
the denominator is given by
$p(x)={p(x|y=1)}p(y=1)+{p(x|y=0)}p(y=0)$  
however, to calculate $p(y \mid x)$ in order to make a prediction we don’t need to calculate
the denominator, since
$$
\begin{aligned}
\arg \max _{y} p(y \mid x) &=\arg \max _{y} \frac{p(x \mid y) p(y)}{p(x)} \\
&=\arg \max _{y} p(x \mid y) p(y)
\end{aligned}
$$

## Naive Bayes

### assumptions :
- all features are independent:
$$
P(x|y=c)=\prod\_{d=1}^D P(x\_d | y=c)
$$

$\Rightarrow$ Let $X=\left[X^{1}, X^{2}, \ldots X^{d}\right]$ the explanatory variables, with $d$ the number of explanatory variables
$$
\begin{aligned}
{p}(X=\mathbf{x} \mid Y=y) &={p}\left(X^{1}=x^{1}, X^{2}=x^{2}, \cdots, X^{d}=x^{d} \mid Y=y\right) \\
&={p}\left(X^{1}=x^{1} \mid Y=y\right) \cdot {p}\left(X^{2}=x^{2} \mid Y=y\right) \cdots {p}\left(X^{d}=x^{d} \mid Y=y\right) \\
&=\prod_{j=1}^{d} {p}\left(X^{j}=x^{j} \mid Y=y\right)
\end{aligned}
$$

-   The probability ${p}\left(X^{j}=x^{j} \mid Y=y\right)$ follow a normal distribution
$$
{p}\left(X^{j}=x^{j} \mid Y=y\right) \sim \mathcal{N}\left(\mu_{y}^{j}, \sigma_{y}^{j^{2}}\right)
$$
$\mu_{y}^{j}\left(\sigma_{y}^{j^{2}}\right)$ is the mean (variance) of the values taken by the training data belonging to the class $y$ for the $j$-th explanatory variable
$$
{p}\left(X^{j}=x^{j} \mid Y=y\right)=\frac{1}{\sqrt{2 \pi} \cdot \sigma_{y}^{j}} e^{-\frac{1}{2 \sigma_{y}^{j^{2}}}\left(x^{j}-\mu_{y}^{j}\right)^{2}}
$$

### Prediction
  
For a new observation $x$, we predict its class $\hat{y}$ such that:
$$
\begin{aligned}
\hat{y} &=\underset{y}{\operatorname{argmax }} (\mathbb{ P}(Y=y \mid X=\mathbf{x})) \\
&=\underset{y}{\operatorname{argmax }}( \mathbb{ P}(Y=y) \cdot \prod_{j=1}^{d} {p}\left(X^{j}=x^{j} \mid Y=y\right))
\end{aligned}
$$
with ${p}(Y=y) = \dfrac{m\_y}{m}$  
![](_resources/Pasted%20image%2020220812164733.png)


## Linear Discriminant Analysis (LDA)
- ${p}(Y=y)=\frac{m_{y}}{m}$ like the naive bayes classifier
- ${p}(X=\mathbf{x} \mid Y=y)$   is modeled by a **multivariate normal distribution law**

$$
\begin{aligned}
{p}(X=\mathbf{x} \mid Y=y) & \sim \mathcal{N}\left(\boldsymbol{\mu}\_{y}, \boldsymbol{\Sigma}\right)
&=\frac{1}{(2 \pi)^{\frac{d}{2}}|\boldsymbol{\Sigma}|^{\frac{1}{2}}} e^{-\frac{1}{2}\left(\mathbf{x}-\boldsymbol{\mu}\_{y}\right)^{T} \boldsymbol{\Sigma}^{-1}\left(\mathbf{x}-\boldsymbol{\mu}\_{y}\right)}
\end{aligned}
$$

avec
- $\boldsymbol{\mu}_{y} \in \mathbb{R}^{d}$  the mean vector for the $d$ explanatory variables for the training data that belong to the class $y$
- $\boldsymbol{\Sigma} \in \mathbb{R}^{d \times d}$ the covariance matrix calculated from all the training data (regardless of their class)

Pictorially, what the algorithm is doing can be seen in as follows:    
 ![|500](_resources/Pasted%20image%2020220812112326.png)  
Note that the two Gaussians have contours that are the same shape and orientation, since they share a covariance matrix $Σ$, but they have different means $µ_0$ and $µ_1$.
### LDA and logistic regression
The GDA model has an interesting relationship to logistic regression. If we view the quantity $p\left(y=1 \mid x ; \phi, \mu_{0}, \mu_{1}, \Sigma\right)$ as a function of $x$, we'll find that it can be expressed in the form
$$
p\left(y=1 \mid x ; \phi, \Sigma, \mu_{0}, \mu_{1}\right)=\frac{1}{1+\exp \left(-\theta^{T} x\right)},
$$
This is exactly the form that logistic regression-a discriminative algorithm-used to model $p(y=$ $1 \mid x)$

When would we prefer one model over another? GDA and logistic regression will, in general, give different decision boundaries when trained on the same dataset. Which is better?
Watch more : [LDA-vs-LogisticRegression](data-science/machine-learning/supervised-learning/bayes/LDA-vs-LogisticRegression.md)

## QDA 
Analyse discriminante quadratique ou Quadratic Discriminant Analysis (QDA)
- similaire à l'Analyse Discriminante Linéaire
- mais une matrice de covariance $\boldsymbol{\Sigma}_{y}$ est calculée pour chaque classe $y$
$$
\begin{aligned}
{p}(X=\mathbf{x} \mid Y=y) & \sim \mathcal{N}\left(\boldsymbol{\mu}\_{y}, \boldsymbol{\Sigma}\_{y}\right) \\
&=\frac{1}{(2 \pi)^{\frac{d}{2}}\left|\boldsymbol{\Sigma}\_{y}\right|^{\frac{1}{2}}} e^{-\frac{1}{2}\left(\mathbf{x}-\boldsymbol{\mu}\_{y}\right)^{T} \boldsymbol{\Sigma}\_{y}^{-1}\left(\mathbf{x}-\boldsymbol{\mu}\_{y}\right)}
\end{aligned}
$$
![](_resources/Pasted%20image%2020220704160714.png)