## Probabilité conditionnelle:  
Si l’événement A se réalise, les événements possibles deviennent en effet l’ensemble des parties de A, et non plus l’ensemble des parties de Ω. Dans ce cas, pour tout événement X de l’ensemble Ω, seule la partie X ⋂ A reste alors un événement possible
$$P(A|B) = \frac{P(A\cap B)}{P(B)}$$ si P(B) != 0
conséquence directe du [Théorème des probabilités composées](data-science/statistic/intro.md)

## Théorème des probabilités composées:  
La définition même des probabilités conditionnelles permet d'écrire que:
$$
p(A \cap B)=p(A) \times p(B \mid A)
$$
et aussi que $p(A \cap B)=p(B) \times p(A \mid B)$
C'est le théorème des probabilités composées, que l'on peut énoncer ainsi : si un événement résulte du concours de deux événements, sa probabilité est égale à celle de l'un d'eux multipliée par la probabilité conditionnelle de l'autre sachant que le premier est réalisé.

Soit, par exemple, à calculer la probabilité pour que, tirant successivement deux cartes d'un jeu de 32 cartes, ces deux cartes soient des valets. Appelons $A$ et $B$ les deux événements suivants :
- A : la première carte est un valet ( $A$ désigne tous les tirages possibles dont la lère carte est un valet)
- $B$ : la deuxième carte est un valet ( $B$ désigne tous les tirages possibles dont la 2e carte est un valet)
La probabilité cherchée est $p(A \cap B)$ qui est aussi égale à $p(A) \times p(B \mid A)$.
Lors du premier tirage, il y a 32 cartes et 4 valets dans le jeu, d'où $p(A)=\frac{4}{32}$.
Lors du second tirage, il reste 31 cartes et seulement 3 valets, puisque l'événement $A$ est réalisé, d'où $p(B \mid A)=\frac{3}{31}$.
Le résultat est donc : $p(A \cap B)=\frac{4}{32} \times \frac{3}{31}=\frac{3}{248} \simeq 0.012$.

### Théorème de Bayes  
$$P(A|B) = \frac{P(B|A)*P(A)}{P(B)}$$


### Base rate fallacy
On peut utiliser le théorème de Bayes pour l'*oubli de la fréquence de base* (**Base rate fallacy**).  
**Enoncé** 0.5% de la population est malade. On a un test de détection de la maladie, qui a un taux de [faux positifs](http://vulgairedev.fr/blog/article/faux-positifs) (= gens détectés mais non malades) de 5% et un taux de faux négatifs (= gens non-détectés mais malades) de 10%. On teste quelqu'un, le test est positif, quelle est la probabilité qu'il soit vraiment malade ?     **Réponse**: 8.3%

Ce résultat paraît très surprenant à première vue. On a un classifieur qui a des taux d'erreur de l'ordre de 5 à 10%, et pourtant quand il détecte qu'on est malade, il y a très peu de chances qu'on le soit vraiment ! En fait il est très important de discerner precision, FPR (False Positive Rate) et FNR (False Negative Rate) pour ne pas faire d'erreur.

(je mets les notations en anglais, la plupart des articles sont en anglais donc ça permet moins de confusion)


![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/statistiques/table_FP.png) 

Verticalement on a la réalité, et horizontalement ce qui est prédit.
En language courant, la traduction de ces sigles est la suivante:

- TP: Malades detectés comme étant malades
- FP: Non-malades detectés comme étant malades (erreur)
- FN: Malades non detectés (erreur)
- TN: Non-malades non detectés.

D'après l'énoncé et la définition du FPR (False Positive Rate), on a:  
$$FPR = \frac{FP}{FP+TN}$$  
C'est donc l'ensemble des non-malades détectés comme étant malades, divisé par l'ensemble des non-malades, càd la **proportion d'erreur parmi les non-malades**.

De même, le FNR (False Negative Rate):  
$$FNR = \frac{FN}{FN+TP}$$  
C'est donc l'ensemble des malades non-détectés, divisé par l'ensemble de malades, càd la **proportion d'erreurs parmi les malades**.

La précision, quand à elle, est définie comme suit:  
$$Precision = \frac{TP}{TP+TN}$$  
C'est l'ensemble des malades détectés sur l'ensemble des détections, càd la **proportion de détections justes**.  

Ceci étant dit, revenons au problème. Si on me dit que le test est positif, cela signifie que je suis soit dans la catégorie des non-malades détectés (erreur), soit dans la catégorie des malades détectés. **C'est là qu'il ne faut pas se tromper** et dire que la répartition dans ces deux catégories est 5% et 95%, puisque c'est **faux** (contraire à la définition du dessus). Puisque la probabilité d'être malade est très faible (0.5%), le nombre de personnes détectées comme étant malades alors qu'elles ne le sont pas est très élevé (99.5% \* 5% \* n), en tous cas bien supérieur au nombre de personnes detectées comme étant malades et l'étant rééllement (0.5% \* 90% \* n). C'est pour cela qu'on trouve finalement une probabilité d'être effectivement malade faible, bien que le test soit positif.

### Démo
Notation:
M: malade  
!M: non-malade  
D: détecté  
!D: non-détecté  

On veut la probabilité d'être malade sachant qu'on est détecté, càd P(M|D).
On sait aussi que:
  
$$p(M) = \frac{0.5}{100}$$  
$$p(D|\neg M) = \frac{5}{100}$$  
(définition du faux positif)

$$p(\neg D|M) = \frac{10}{100}$$  
(définition du faux négatif)

$$p(\neg D|\neg M) = \frac{95}{100}$$  
(complémentaire du faux positif)  


$$p(D| M) = \frac{90}{100}$$  
(complémentaire du faux négatif)
  
En appliquant le théorème de Bayes, on a:
$$p(M|D) = \frac{p(D|M)*p(M)}{p(D)} $$  

Or, d'après la loi des probabilités totales, on a:

$$p(D) =  p(D\cap M) + p(D\cap \neg M)$$  
  
$$p(D) = 0.90*0.005 + 0.995*0.05$$  

Donc on trouve:  
$$p(M|D) = \frac{p(D|M)*p(M)}{p(D)} \approx 0.083$$


**Se souvenir**:
  
$$P(A|B)+P(\neg A|B) = 1$$

