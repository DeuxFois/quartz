# Regression linéaire
Données d'apprentissage : $\lbrace\mathbf{x}\_{i}, y\_{i} \rbrace\_{i=1}^{m}$  
observations (entrées) $: \mathbf{x}_{i} \in \mathbb{R}^{d}$  

mesure d'intérêt (sortie à prédire) : $y_{i} \in \mathcal{Y}$

Fonction de prédiction : $f: \mathbb{R}^{d} \mapsto \mathcal{Y}$  
## Paramètre
$$\vec{(\beta)}^T = (\beta\_0,\beta\_1,...,\beta\_n) \in \mathbb{R}^{d+1}$$

## Modèle
$$ f\_\beta(x) = \beta\_0 + \Sigma_{i=1}^m(\beta\_ix\_i)$$

- Notation matricielle :
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

## coût 
- fonction qu'on va chercher à minimiser (ou maximiser)
- On veut $\boldsymbol{\beta}$ tel que $f_{\boldsymbol{\beta}}(\mathbf{x})$ soit proche de $y$ pour toutes les données d'apprentissage $\lbrace{x}\_{i}, {y}\_{i}\rbrace\_{i=1}^{m}$ :
$$
\hat{y}\_{i}=f\_{\boldsymbol{\beta}}\left(\mathbf{x}\_{i}\right) \approx y\_{i} \quad \forall i \in\{1, \cdots, m\}
$$
- Trouver le meilleur vecteur $\vec{\beta}$ équivaut à minimiser le coût (quadratique) global des erreurs :
$$
\mathbf{J}(\boldsymbol{\beta})=\frac{1}{2 m} \sum\_{i=1}^{m}\left(f\_{\boldsymbol{\beta}}\left(\mathbf{x}\_{i}\right)-y\_{i}\right)^{2}
$$

$$
➡ \space \underset{\boldsymbol{\beta}}{\operatorname{argmin}}(\mathbf{J}(\boldsymbol{\beta}))
$$
Parmi toutes les droites possibles, on cherche la droite pour laquelle la somme des carrés des écarts verticaux des points à la droite est minimale.
![](_resources/Pasted%20image%2020220630221441.png)

# Descente de gradient
- Objectif : trouver le minimum d'une fonction-coût  
<br/>

1. initialisation : $\boldsymbol{\beta}^{(0)}$
2. à chaque étape $k$, modifier $\boldsymbol{\beta}^{(k-1)}$ pour faire $\operatorname{diminuer} \mathbf{J}\left(\boldsymbol{\beta}^{(k)}\right)$
3. arrêt lorsque le minimum est atteint

Pour chaque variable $\beta_{j}$ :
$$
\beta_{j}^{(k)}:=\beta\_{j}^{(k-1)}-\alpha \frac{1}{m} \sum\_{i=1}^{m}\left(f\_{\boldsymbol{\beta}(k-1)}\left(\mathbf{x}\_{i}\right)-y\_{i}\right) x\_{i j}
$$
Remarque $: \forall i, x_{i 0}=1$
- $\alpha$ : pas d'apprentissage
![](_resources/Pasted%20image%2020220701115156.png)

# Locally weighted linear regression
As evident from the image below, this algorithm cannot be used for making predictions when there exists a non-linear relationship between X and Y. In such cases, locally weighted linear regression is used.

![](https://media.geeksforgeeks.org/wp-content/uploads/Linear-Regression-on-non-linear-data.png)

### Locally Weighted Linear Regression:

The modified cost function is:
$$
\mathbf{J}(\boldsymbol{\beta})= \sum\_{i=1}^{m}w^{(i)}\left(f\_{\boldsymbol{\beta}}\left(\mathbf{x}\_{i}\right)-y\_{i}\right)^{2}
$$