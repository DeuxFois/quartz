Soit une **urne contenant des boules rouges et des boules blanches** pour un total de n boules, on s’intéresse à la variable aléatoire **K : nombre de boules rouges tirées parmi les n boules** .  On se demande alors quelle est la probabilité p(k) pour que K soit égale à un nombre donné k.  
Il y a deux façons de procéder au tirage des n boules :
- **tirage non exhaustif** : on prélève une boule, puis une autre après **remise** de la première dans l’urne, puis une
troisième après remise de la seconde, etc. Les prélèvements successifs sont donc indépendants puisque la
composition de l’urne est la même avant chaque prélèvement ; on utilisera la **loi binomiale**
- **tirage exhaustif** : on prélève chacune des n boules **sans remise** des précédentes. Les prélèvements ne sont
plus indépendants, la composition de l’urne étant modifiée après chaque prélèvement, et cela d’autant plus
que la taille n de l’échantillon prélevé est élevée devant celle de la population des boules contenues dans
l’urne ; on utilisera la **loi hypergéométrique**

voir aussi [Arrangement et combinaison](data-science/statistic/Arrangement.md)

## loi Binomiale  
Dans le cas du **tirage non exhaustif**, à chaque boule rouge correspond la probabilité ϖ d’être prélevée, à
chaque boule blanche la probabilité (1 - ϖ).  Par application du théorème des probabilités composées, la probabilité de l’ensemble de
l’échantillon s’écrit alors $ϖ^{k} (1 - ϖ)^{n-k}$ .


Cela étant posé, il faut noter que le résultat obtenu se réfère arbitrairement à un certain ordre d’arrivée des
boules rouges et blanches. Or, quand on se propose de calculer la probabilité d’avoir k boules rouges parmi les
n boules extraites, l’ordre d’arrivée est indifférent. Autrement dit, on n’est pas intéressé par la probabilité
d’une combinaison particulière de k boules rouges parmi les n boules, mais par la probabilité de l’ensemble
n
des $\binom{n}{k} = \frac{n!}{k!(n-k)!}$combinaisons possibles. Par application du théorème des probabilités totales, cette probabilité s’écrit
k
alors : 

$p(k)=\binom{n}{k} \varpi^{k}(1-\varpi)^{n-k}$,  parfois notée $C_{n}^{k} \varpi^{k}(1-\varpi)^{n-k}$


Ainsi, la loi binomiale dépend de **deux paramètres $n$ et $\varpi$**
$$
P(k)=p(0)+p(1)+\ldots+p(k)=\sum_{i=0}^{k}\left(\begin{array}{c}
n \\
i
\end{array}\right) \varpi^{i}(1-\varpi)^{n-i}
$$
**L'intérêt pratique de la loi binomiale** est très grand. Au lieu de parler d'une urne contenant une certaine proportion $\sigma$ de boules rouges, on peut parler **d'une population** contenant une certaine proportion $\varpi$ **d'individus présentant une certaine qualité ou ayant un certain avis**, pour constater que ce modèle théorique permet de définir la probabilité du nombre d'individus ayant cette qualité ou cet avis, et susceptibles de figurer dans un échantillon de $n$ individus tirés au hasard dans la population en question.   
La loi binomiale joue ainsi un rôle important dans un grand nombre de problèmes de jugement sur échantillon : sondages d'opinion ou contrôle du nombre de pièces défectueuses dans une fabrication.

Notons à ce stade que nous avons utilisé la majuscule $P$ pour désigner une somme de probabilités. Il lui correspond la fonction de répartition de la variable $K$. Par contre, nous avons utilisé la minuscule $p$ pour désigner une probabilité simple qui correspond à la densité de probabilité de la variable $K$.


Avec les notations précédentes, une variable aléatoire $K$ qui suit une loi binomiale sera notée $K \sim \mathcal{B}(n, \varpi)$.


## Loi hypergéométrique
Au **tirage exhaustif** correspond la loi hypergéométrique. Dans ce cas, la composition de l'urne est modifiée après chaque tirage. Il convient donc de préciser la composition initiale de l'urne de la façon suivante :
- $N$ : nombre total de boules dans l'urne,
- $R=N \varpi$ : nombre total de boules rouges,
- $N-R=N(1-\pi)$ : nombre total de boules blanches.
Tirer $n$ boules dont $k$ rouges revient à tirer $k$ boules parmi les $R$ rouges et $(n-k)$ boules parmi les $(N-R)$ blanches.

Si nous individualisons chaque boule, le nombre d'échantillons de $n$ boules que l'on peut tirer de l'urne est égal à $\binom{N}{n}$,   
le nombre d'échantillons de $k$ boules rouges prélevées parmi les $R$ rouges est égal à $\binom{R}{k}$ et celui des échantillons de $(n-k)$ boules blanches prélevées parmi les $(N-R)$ blanches est égal à $\binom{N-R}{n-k}$. Le nombre d'échantillons contenant $k$ boules rouges et $(n-k)$ boules blanches est donc égal à $\binom{R}{k}\binom{N-R}{n-k}$. Tous ces
$$
p(k)=\frac{\left(\begin{array}{c}
R \\
k
\end{array}\right)\left(\begin{array}{c}
N-R \\
n-k
\end{array}\right)}{\left(\begin{array}{l}
N \\
n
\end{array}\right)}
$$
Une variable aléatoire $K$ qui suit une loi hypergéométrique sera notée $K \sim \mathcal{H}(N, n, R)$ avec les notations précédentes.






## Loi de Poisson
À chaque couple de valeurs $n$ et $\varpi$ correspond, dans le cas d'un tirage non exhaustif, une loi binomiale.  
Pour des raisons de commodité de calcul, les statisticiens se sont efforcés de trouver des lois approchées plus facile à utiliser. La loi de Poisson est l'une d'elles.  Elle correspond aux hypothèses : $n$ grand, $\varpi$ petit, le produit $n \varpi=\lambda$ étant fini. On peut écrire dans ces conditions :
$$
\frac{n !}{k !(n-k) !} \varpi^{k}(1-\varpi)^{n-k}=\frac{n(n-1) \cdots(n-k+1)}{k !} \times \frac{\lambda^{k}}{n^{k}} \times \frac{\left(1-\frac{1}{n}\right)^{n}}{\left(1-\frac{1}{n}\right)^{k}}=\frac{n(n-1) \cdots(n-k+1)}{n^{k}} \times \frac{\lambda^{k}}{k !} \times \frac{\left(1-\frac{1}{n}\right)^{n}}{\left(1-\frac{1}{n}\right)^{k}}
$$
Si l'on fait tendre $n$ vers l'infini, $\frac{n(n-1) \cdots(n-k+1)}{n^{k}} \rightarrow 1,\left(1-\frac{\lambda}{n}\right)^{k} \rightarrow 1$ et on peut montrer que $\left(1-\frac{\lambda}{n}\right)^{n} \rightarrow e^{-\lambda}$. À la limite, on obtient alors la loi de Poisson définie par les probabilités :
$$
p(k)=e^{-\lambda} \times \frac{\lambda^{k}}{k !}
$$

Elle dépend du seul paramètre $\lambda$ et il existe des tables qui donnent, pour différentes valeurs de $\lambda$, les probabilités cumulées correspondantes :
$$
P(k)=\sum_{i=0}^{k} e^{-\lambda} \times \frac{\lambda^{i}}{i!}
$$
La loi de Poisson présente donc l'intérêt de simplifier les calculs, puisqu'une seule table poissonnienne se substitue à un grand nombre de tables binomiales. En pratique et en première analyse, on peut utiliser l'approximation de Poisson quand $n \geq 50$ et $\varpi \leq 0.1$. Ceci lui confere un champ d'application très large, en particulier dans l'échantillonnage industriel où les proportions de déchets sont heureusement faibles.
Une variable $K$ suivant une loi de Poisson de paramètre $\lambda$ sera notée $K \sim \mathcal{P}(\lambda)$
