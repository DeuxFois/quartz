[Generative vs Discriminative](MLIA/Generative%20vs%20Discriminative.md)
# Apprentissage non-supervisé
- $\mathbf{x} \in \mathbb{R}^{d}$ est une observation de $d$ variables réelles.
- L'ensemble d'apprentissage est définit par les observations $\lbrace{x}\_{i}\rbrace\_{i=1}^{m}$ où $n$ est le nombre d'exemples d'apprentissages (de points).
- Les exemples sont souvent mis sous la forme d'une matrice $\mathbf{X} \in \mathbb{R}^{n \times d}$ définie par $\mathbf{X}=\left[\mathbf{x}\_{1}, \ldots, \mathbf{x}_{n}\right]^{\top}$  contenant les exemples d'apprentissage en lignes et les variables en colonnes.
- $d$ et $n$ définissent la dimensionnalité du problème d'apprentissage.
### Clustering
Organiser les objets en des groupes présentant une certaine similarité  
[Introduction](MLIA/Clustering/Introduction.md)
 
- [Kmeans](MLIA/Clustering/Kmeans.md)
> taxonomie des espèces animales

<br/>

### Estimation des densité de probabilité
Estimer la loi de probabilité des données  d’apprentissage
> estimer la loi d’un bruit

<br/>

### Reduction de dimension
Diminuer la dimensionnalité des données pour pouvoir mieux les interpréter/visualiser  
[Introduction](MLIA/Réduction_dimension/Introduction.md)    
[Covariances et corrélations](MLIA/Réduction_dimension/Covariances%20et%20corrélations.md)
- [ACP](MLIA/Réduction_dimension/ACP.md)  


> interpréter/visualiser

<br/>
<br/>

# Apprentissage supervisé
- On associe à chaque observation $\mathbf{x}\_{i}$ une valeur à prédire $y\_{i} \in \mathcal{Y}$.
- Tout comme pour les observation les valeurs à prédire (label) peuvent être concaténées en un vecteur $\mathbf{y} \in \mathcal{Y}^{n}$
- L'espace des valeurs à prédire $\mathcal{Y}$ sera :
- $\mathcal{Y}=\lbrace\{-1,1\}\rbrace$ pour la classification binaire ou $\mathcal{Y}=\{1, \ldots, m\}$ pour la classification multi-classes ( $m$ classes).
- $\mathcal{Y}=\mathbb{R}$ pour la régression.
### Classification
Affecter une classe à une observation 

> (reconnaissances de  caractères, météo, pluie)  

[Bayes](MLIA/Classification/Bayes.md)
<br/>

### Régression
Prédire une valeur réelle à partir d’une observation
>prédire une valeur réelle (météo, temperature)  
- [Linear Regression](MLIA/Regression/Linear%20Regression.md)
- [Logistic Regression](MLIA/Regression/Logistic%20Regression.md)  
<br/>  
<br/>
- [Batch vs stohastic](MLIA/Regression/Batch%20vs%20stohastic.md)
<br/>
<br/>

# Apprentissage par renforcement
Apprendre pour maximiser un gain
> conduite autonome, jeux, systèmes de contrôle