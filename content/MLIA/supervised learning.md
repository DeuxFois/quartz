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
