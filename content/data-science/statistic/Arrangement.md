### Arrangement sans répétition
Un arrangement sans répétition est noté:  
$$A_n^{k}$$

Un arrangement avec répétions c'est prendre k billes parmi n dans un sac de billes, en tenant compte de l'ordre dans lequel on sort chacune d'elles. L'ensemble des issues (qui sont de longueur k) correspond aux arrangements sans répétition.

Par exemple j'ai un sac avec une boule verte (V), une bleue (B) et une rouge(R) (n=3), dans lequel je prend deux billes (k=2) l'ensemble des issues possibles est:  
$${VB, VR, BV, BR, RV, RB}$$   
Ce sont des tuples, l'ordre des éléments est importants.
  
**Le nombre d'issues possibles est**:  
$$\frac{n!}{(n-k)!}$$

Pour notre exemple, cela correspond à: 

$$\frac{3}{(3-2)!} = 6$$

En effet, pour prendre la première bille j'ai n possibilités, pour la deuxième, n-1, etc. Si on prend toutes les billes (k=n), on se retrouve avec n! possibilités, mais sinon, il faut diviser par (k-1)! pour éliminer les possibilités des billes restantes dans le sac après en avoir tiré k.



### Arrangement avec répétition
Un arrangement avec répétition c'est tirer successivement k billes parmi n, en remettant chacune des billes tirées dans le sac au fil des tirages, et en tenant compte de l'ordre dans lequel les billes sortent.

Pour le sac exemple, les issues possibles sont:

$${VV, VB, VR, BB, BV, BR, RR, RB, RV}$$

**Le nombre d'issues possibles est**:  
$$n^k$$

Pour notre exemple, cela fait:

$$3^2 = 9$$

### Nombre de permutations
Une permutation est une suite ordonné de n éléments.
Pour un arrangement $BRVV$, une permuation possible est $RVBV$.

**Une permutation correspond à un arrangement avec k=n**
Si les billes sont toutes disctinctes, on a $n!$ permutations possibles.

Dans le cas où les il y a plusieurs billes du même type, notons $n_B$ le nombre de billes bleues, $n_R$ et $n_V$ le nombre de billes rouges et vertes. Notons que $n_R + n_V + n_B = n$. On a alors un nombre de permutations possibles de:  
$$ \frac{n!}{n_A!n_B!n_C!}$$

On divise par le produit des factoriels des nombre de billes de chaque type pour éliminer les permutations redondantes (avec la formule $n!$ on répète plusieurs fois la combinaison $BRVV$ par exemple, puisque les deux billes V peuvent échanger de place et donner le même résultat).

### Combinaison sans répétition
Une combinaison sans répétition est notée:  
$$C_n^{k}$$
Une combinaison sans répétition correspond au fait de tirer k billes parmi n, d'un seul coup. C'est un arrangement sans répétition dont on ignore l'ordre.

Pour le sac exemple, ça nous donne:  
$$\{VR, RB, BV\}$$

**Le nombre d'issues possibles est**:  
$$\frac{n!}{k!(n-k)!} = \frac{A_n^{k}}{k!}$$

Ici on trouve:
$$\frac{3!}{2!(3-1)!} = 3$$

**NB**: Ceci est dû au fait qu'il y a une "correspondance" entre combinaison avec répétition et arrangement sans répétition. En effet, l'ensemble des arrangements qu'on peut générer à partir d'une combinaison (sans répétition) vaut k! : k possibilités de choix pour le premier élément, k-1 pour le deuxième etc.

Ce sont des ensembles, il n'y a pas d'ordre des éléments.
  
### Combinaison avec répétition
Une combinaison avec répétition est une combinaison dans laquelle les éléments peuvent apparaître plusieurs fois.
Par exemple faire 2 tirage de boules avec remise, si on s'intéresse uniquement aux résultats et pas à l'ordre dans lequel elles apparaissent, est une combinaison avec répétition.

Pour le sac exemple, ça nous donne:
$$\{VR, RB, BV, VV, RR, BB\}$$

**Le nombre d'issues possibles est**:  
$$\frac{(n+k-1)!}{k!(n-1)!}$$

Ca correpond à une combinaison sans remise de k éléments parmi n + k - 1.

Ici on trouve:
$$\frac{4!}{2!(3-1)!} = 6$$

**NB**: On pourrait se dire qu'il suffirait de diviser les arrangements avec répétitions par k! pour trouver les combinaisons avec répétitions, comme on avait fait dans le cas sans répétition. Ca ne fonctionne pas car il y a des cas où les combinaisons et les arrangements se confondent. Par exemple l'ensemble {RB} correspond bien à 2! tuples (BR et RB), mais l'ensemble {RR} ne génère qu'un seul tuple RR !
