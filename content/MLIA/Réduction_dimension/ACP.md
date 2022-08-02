
# Objectif
Condenser l’information de la matrice des donn´ees afin d’en retirer les relations caractéristiques (ressemblances entre observations et liaisons entre variables) tout en limitant la perte d’information.

# Inertie
Utilisation de l'inertie $\mathcal{I}$ : somme des distances au carré entre les $m$ observations et leur centre de gravité $\boldsymbol{\mu} \in \mathbb{R}^{d}$ :
$$
\begin{aligned}
\mathcal{I} &=\sum_{j=1}^{d} \sum_{i=1}^{m} p_{i}\left(x_{i j}-\mu_{j}\right)^{2} \\
&=\sum_{j=1}^{d} s_{j}^{2}
\end{aligned}
$$
avec $s_{j}^{2}$ la valeur de la variance (carré de l'écart-type) pour la $j$-ème variable $\left(X_{j}\right)$.
En d'autres termes,
- L'inertie est une généralisation de la notion de variance au cadre multivarié. C'est la somme des variances pour les $d$ variables, i.e., la somme des éléments diagonaux de la matrice de variance-covariance
- L'inertie est une information du nuage des observations.
- Si les données sont centrées-réduites : $\mathcal{I}=\sum_{j=1}^{d} s_{j}^{2}=\sum_{j=1}^{d} 1=d$