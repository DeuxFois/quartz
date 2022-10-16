# Apprentissage non-supervisé
- $\mathbf{x} \in \mathbb{R}^{d}$ est une observation de $d$ variables réelles.
- L'ensemble d'apprentissage est définit par les observations $\lbrace{x}\_{i}\rbrace\_{i=1}^{m}$ où $n$ est le nombre d'exemples d'apprentissages (de points).
- Les exemples sont souvent mis sous la forme d'une matrice $\mathbf{X} \in \mathbb{R}^{n \times d}$ définie par $\mathbf{X}=\left[\mathbf{x}\_{1}, \ldots, \mathbf{x}_{n}\right]^{\top}$  contenant les exemples d'apprentissage en lignes et les variables en colonnes.
- $d$ et $n$ définissent la dimensionnalité du problème d'apprentissage.
### Clustering
> Organiser les objets en des groupes présentant une certaine similarité (taxonomie des espèces animales).
- [kmeans](data-science/machine-learning/unsupervised-learning/kmeans.md)
- Gaussian Mixture Model

<br/>

### Estimation des densité de probabilité
> Estimer la loi de probabilité des données d’apprentissage (estimer la loi d’un bruit).
- parzen window
- Histogram

<br/>
### Réduction de dimension
> Diminuer la dimensionnalité des données pour pouvoir mieux les interpréter/visualiser (recommandation).