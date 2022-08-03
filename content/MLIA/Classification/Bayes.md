# Bayes Law
$$
\underbrace{\mathbb{P}(Y \mid X)}_{\text {Posterior probability }}=\frac{\overbrace{\mathbb{P}(Y)}^{\text {Prior probability}} \cdot \overbrace{\mathbb{P}(X \mid Y)}^{\text {Likelihood}}}{\mathbb{P}(X)}
$$
the denominator is given by
$\mathbb{P}(X) = {\mathbb{P}(X|Y=\mathbf{1})}\mathbb{P}(Y=1)+{\mathbb{P}(X|Y=\mathbf{0})}\mathbb{P}(Y=0)$  
then we don’t actually need to calculate
the denominator, since
$$
\begin{aligned}
\arg \max _{y} p(y \mid x) &=\arg \max _{y} \frac{p(x \mid y) p(y)}{p(x)} \\
&=\arg \max _{y} p(x \mid y) p(y)
\end{aligned}
$$
# Naive Bayes
## Assuptions

### independence assumption

$$
P(x|y=c)=\prod\_{d=1}^D P(x\_d | y=c)
$$

i.e., given the label, all features are independent.

### la probabilité $\mathbb{P}\left(X^{j}=x^{j} \mid Y=y\right)$ suit une loi normale (gaussienne) [Gaussian](MLIA/Stats/Gaussian.md)
$$
\mathbb{P}\left(X^{j}=x^{j} \mid Y=y\right) \sim \mathcal{N}\left(\mu_{y}^{j}, \sigma_{y}^{j^{2}}\right)
$$
- la probabilité $\mathbb{P}\left(X^{j}=x^{j} \mid Y=y\right)$ suit une loi normale (gaussienne) de moyenne $\mu_{y}^{j}$ et de variance $\sigma_{y}^{j^{2}}$
- $\mu_{y}^{j}\left(\sigma_{y}^{j^{2}}\right)$ est la moyenne (variance) des valeurs prises par les données d'apprentissage appartenant à la classe $y$ pour la $j$-ième variable explicative
$$
\mathbb{P}\left(X^{j}=x^{j} \mid Y=y\right)=\frac{1}{\sqrt{2 \pi} \cdot \sigma_{y}^{j}} e^{-\frac{1}{2 \sigma_{y}^{j^{2}}}\left(x^{j}-\mu_{y}^{j}\right)^{2}}
$$

## Prédiction
Pour une nouvelle observation $x$, on prédit sa classe $\hat{y}$ tel que :
$$
\begin{aligned}
\hat{y} &=\underset{y}{\operatorname{argmax}} \mathbb{P}(Y=y \mid X=\mathbf{x}) \\
&=\underset{y}{\operatorname{argmax}} \mathbb{P}(Y=y) \cdot \prod_{j=1}^{d} \mathbb{P}\left(X^{j}=x^{j} \mid Y=y\right)
\end{aligned}
$$

# LDA
Analyse discriminante linéaire ou Linear Discriminant Analysis (LDA)
- est un algorithme génératif
- $\mathbb{P}(Y=y)=\frac{m_{y}}{m}$ comme le classifieur bayésien naif
- $\mathbb{P}(X=\mathbf{x} \mid Y=y)$ est modélisé par une loi de distribution normale multivariée
$$
\begin{aligned}
\mathbb{P}(X=\mathbf{x} \mid Y=y) & \sim \mathcal{N}\left(\boldsymbol{\mu}_{y}, \boldsymbol{\Sigma}\right) 
&=\frac{1}{(2 \pi)^{\frac{d}{2}}|\boldsymbol{\Sigma}|^{\frac{1}{2}}} e^{-\frac{1}{2}\left(\mathbf{x}-\boldsymbol{\mu}_{y}\right)^{T} \boldsymbol{\Sigma}^{-1}\left(\mathbf{x}-\boldsymbol{\mu}_{y}\right)}
\end{aligned}
$$
avec
- $\boldsymbol{\mu}_{y} \in \mathbb{R}^{d}$ le vecteur moyenne pour les $d$ variables explicatives pour les données d'apprentissage qui appartiennent à la classe $y$
- $\boldsymbol{\Sigma} \in \mathbb{R}^{d \times d}$ la matrice de covariance calculée à partir de toutes les données d'apprentissage (indépendamment de leur classe)
- |. le déterminant

# QDA 
Analyse discriminante linéaire ou Linear Discriminant Analysis (LDA)
- est un algorithme génératif
- $\mathbb{P}(Y=y)=\frac{m_{y}}{m}$ comme le classifieur bayésien naif
- $\mathbb{P}(X=\mathbf{x} \mid Y=y)$ est modélisé par une loi de distribution normale multivariée
$$
\begin{aligned}
\mathbb{P}(X=\mathbf{x} \mid Y=y) & \sim \mathcal{N}\left(\boldsymbol{\mu}_{y}, \boldsymbol{\Sigma}\right) \\
&=\frac{1}{(2 \pi)^{\frac{d}{2}}|\boldsymbol{\Sigma}|^{\frac{1}{2}}} e^{-\frac{1}{2}\left(\mathbf{x}-\boldsymbol{\mu}_{y}\right)^{T} \boldsymbol{\Sigma}^{-1}\left(\mathbf{x}-\boldsymbol{\mu}_{y}\right)}
\end{aligned}
$$
avec
- $\boldsymbol{\mu}_{y} \in \mathbb{R}^{d}$ le vecteur moyenne pour les $d$ variables explicatives pour les données d'apprentissage qui appartiennent à la classe $y$
- $\boldsymbol{\Sigma} \in \mathbb{R}^{d \times d}$ la matrice de covariance calculée à partir de toutes les données d'apprentissage (indépendamment de leur classe)
- |. le déterminant
![](Pasted%20image%2020220704160714.png)